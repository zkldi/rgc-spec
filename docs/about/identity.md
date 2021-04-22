# Identifying RGC Files

Identifying charts that have been played is *very* important for other tools to interact with RGC.

Lets consider a very common use case of a user getting a score on a chart.
How do we POST that data off to a server, such that it can identify what chart was played?

Well, we're not the first person to have this issue. The solutions devised by existing games are not well defined, and possibly *very* broken.

## The LR2, osu and beatoraja Approach

These games approach identifying charts by hashing the file. This has a couple
advantages, but also has a couple of issues.

The main advantage is that this is easy to do and easy to replicate.

The main disadvantage is that **ANY** change to the file now results in a completely different ID.
This includes things that dont affect gameplay, such as adding comments or whitespace!

We could simply pipe RGC files into a hashing algorithm, and that would be simple,
but it's vulnerable to any change having a catastrophic effect.

!!! info
    Fun fact, this is why old `.bms` files have to stay in SHIFT-JIS encoding, and cant be converted to UTF-8!

## The Etterna Approach

Etterna approaches the hash by looking at the notedata for the chart, and generating a lenient hash depending on its content.

This implementation has the right idea, the ID for the chart is dependent on how it plays to the user, and not on *anything* inside the file.

However, Etternas approach is not directly portable to JSON, so we will have to create our own with a similar idea.

!!! info
    Etterna's hashing implementation is also broken, as it only hashes the order that notes appear, rather than the order and *when* they appear.

## Implementing RGC Hashing

Okay, so we want to hash how the chart *plays* to the end user. This means we can quite safely ignore ``meta`` in the hash for our files.

### Chart Metadata

Some games may have "gameplay-significant" fields in ``chartMeta``, such as osu!'s OD value, or bms's #JUDGE declaration. So we have to include ``chartMeta``.

RGC doesn't come with a generic way of separating those "gameplay-significant" fields from "gameplay-insignificant" ones, which means that ``chartMeta`` clips some metadata
in the hash.
We could define something in the RGC spec to do this, such as ``chartMeta.gameplay`` and ``chartMeta.meta`` (ew), but that falls victim to an even more irritating issue,
which is that defining what is and isnt "gameplay-affecting" is quite difficult. Furthermore, **different game clients can have different opinions on that metadata**.

We could try to standardise all of this, but it only brings more issues if a game decides to make something gameplay affecting in the future.
As it stands, clipping some metadata (such as genres) is just an accepted consequence.

So we must include *all* of `chartMeta` for a standardised algorithm. Now what? Well, we obviously need `events`, so lets look into that part.

### Events

Events need to be sorted. We cannot have an issue where `[event2, event1]` generates a different hash to `[event2, event1]`.

We have [CESA](../implementation/sorting.md) to solve this, so we establish that RGC **MUST** be sorted according to CESA in order to calculate a hash.

### Stable-Stringify

Now we need to avoid an issue with hashing JSON. We cannot just stringify chartMeta + events, because of JSON's order-of-keys.

Semantically, { a:1, b:2 } and { b:2, a:1 } are the same JSON object - But they would hash to different values!

To fix this, we sort all keys lexicographically before stringifying, this resolves the issue of semantically identical objects!

!!! info 
    We have an MIT licensed implementation of Stable-Stringify available at [NPM](https://npmjs.org/fast-json-stable-hash), if you would need one.

### Sync-Dependent Hash

!!! warning
    The below implementation is **NOT** the default hashing method for RGC. Read this whole page.

To calculate the final hash, we omit ``meta`` from the object, leaving us with the following JSON:

```json
{
    "chartMeta": {
        "stuff": 1,
        "moreStuff": 123
    },
    "events": {
        "TYPE": []
    }
}
```

Now, we stable-stringify this JSON.

```
'{"chartMeta":{"moreStuff":123,"stuff":1},"events":{"TYPE":[]}}'
```

Now, we pipe this into **SHA256**, and get our output in **HEX lowercase**.

```
"7d3a5af77dc5beb8482010fdc02a90171f894ee3765c6298726ac5118474edb4"
```

All good, right?

Well, we have one more irritating issue, which means that we have to create *two* separate hashing methods.

The above described method is the RGC *Sync-Dependent* hash. To distinguish this from the following hash methods, the data **MUST** be prepended with "RD" (RGC Dependent).

```
"RD7d3a5af77dc5beb8482010fdc02a90171f894ee3765c6298726ac5118474edb4"
```

### One Last Issue

The last issue comes in the form of "Sync". In our case, we define "Sync" to be the synchronisation of the chart with the audio in the game.

Our issue is that a chart, and a chart with every event shifted forward by 100ms, is played *identical*, and the only difference is in the synchronisation to the audio file.

Normally, we could ignore this, right? I mean, who's creating multiple files with different sync values -- surely the game can do that?

Well, turns out StepMania doesn't support this at all, and StepMania *edits .sm files* to update their sync. Furthermore, pack releasers in the arcade scene release *two* packs
for their charts, one for home use, and one for arcade use, where all the offsets are +14-15ms!

If we didn't account for this, we would cause massive fragmentation, and we can't exactly fix this issue, since it's gone on for years.

This brings us to a final implementation of the hashing algorithm, the Sync-Agnostic hash. **This is the default hash for RGC files**.

### Sync-Agnostic Hash

We follow the same steps as above, but before we stringify the object, we first find the *first* event in the RGC file. This can be achieved with the following logic:

```js
let firstEventMS = null;

for (const type in rgc.events) {
    // since the hashing algorithm is only defined for sorted RGC
    // we can assume index 0 is the first event for this type.
    let thisEvent = rgc.events[type][0];
    
    if (!thisEvent) {
        continue; // this type has no events
    }

    // if this is the first event we've saw
    if (firstEventMS === null) {
        firstEventMS = thisEvent.ms;
    }
    // if this is earlier than our earliest event
    else if (firstEventMS > thisEvent.ms) {
        firstEventMS = thisEvent.ms;
    }
}

if (firstEventMS === null) {
    // this chart has no events??
    firstEventMS = 0;
}
```

Now that we have the location of the first event, we *take away* that value from every event in the chart.

```js
for (const type in rgc.events) {
    for (const event in rgc.events[type]) {
        event.ms -= firstEventMS; // JS is reference-based, so this mutates the RGC file.
    }
}
```

Now we just perform the same stable-stringify into SHA256 lhex!

To differentiate from the other hash, we prefix this one with "RA" (RGC Agnostic).

Other than that, that's how we hash RGC files.

## Applications

So to answer the first question, lets say a server has a concept of chart "RAf12...", Our client could also perform the RGC Sync-Agnostic hash, and therefore post RAf12... alongside
the score data! This also allows applications to have a common point of reference for charts, that is **safe** and **immutable**.

As a more obvious example, lets look at something like a real world game. We might approach the issue where, although we know the in game IDs for charts, they might change as
time goes on! Furthermore, they might have duplicates (I believe one of the PARANOiA series has this issue) under the same ID, or worse, they might not have IDs at all (CS series
of games!)

With RGC's SDH and SAH, we can provide unique identifiers for charts that multiple services can use independently. This makes creating APIs trivial, and means everyone's talking
about the same thing!
