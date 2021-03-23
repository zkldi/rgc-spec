# RGC Events

We'll be disecting this part of the example RGC file:

!!! note
    For legibility's sake, I have truncated the data a bit here.

```json
"events": {
    "BGM": [
        {
            "ms": 0,
            "pan": 0,
            "sound": 1
        },
        {
            "ms": 0,
            "pan": 0,
            "sound": 2
        }
    ],
    "BPM": [
        {
            "ms": 0,
            "bpm": 144.95
        }
    ],
    "BSS": [],
    "HCN": [],
    "CN": [],
    "HBSS": [],
    "NOTE": [
        {
            "ms": 4144,
            "col": 6
        },
        {
            "ms": 5631,
            "col": 0
        }
    ],
    "SAMPLE": [],
    "METER": [],
    "TIMINGWINDOW": [],
    "MEASUREBAR": [],
    "EOC": [],
    "SCRATCH": [
        {
            "ms": 3012
        },
        {
            "ms": 5731
        }
    ]
}
```

As mentioned previously, an event is simply something that happens in a chart at a given time. You can re-read what RGC considers an event [here](../definition);

RGC's events are defined by the schema - that is to say that **this is an empty object by default**.
Everything you see defined above has been defined that way by the ``std/iidx-SP`` schema.

But, what's happening with these events? Why are there empty arrays, and what do the keynames mean?

## Event Types

The `events` property in RGC is defined as an empty object. Schemas can declare types of objects, which must then be declared as `events.[TYPE-NAME] = Array`.
So, if a schema declares type FOO, `events.FOO` **MUST** be an array.

What goes inside the array is also determined by the schema. An event array **MUST** contain identical types, so you cant have `events.FOO = [fooObject, barObject]`.

!!! warning
    If a schema defines a type, that type **MUST** have atleast an empty array! Event Types are **NOT** optional.

## Neutral Event

All events **MUST** extend the neutral event.

The neutral event is simply an object with one property - `ms`, which defines the miliseconds an event occurs at. This ensures that all events are on the same
single, easy-to-understand timeline.

`ms` is defined as a float. It may be positive or negative.

## Custom Events

Often, we will want to store some properties about our event. Lets look at the ``NOTE`` type from the above example. It looks like the NOTE event defines one additional
property, ``col``.

To find out what this does, we need to look at the documentation for our schema, and see what it says about that type.

``std/iidx-SP`` defines a NOTE event as an event with one additional property - ``col``, which **MUST** be an integer between 0 and 6.

This simple extensible way of representing events means rather complex things can be represented in an easily parseable way.

## Why not just a single array?

This was a topic of hot-debate when designing RGC. We decided on an object of `[type]: type[]` syntax, because it allows for programs to easily separate their logic
in handling different types.

As a common example, code may want to interact with ``BPM`` different than it does ``NOTE``. Although it is just as possible with a single array, it becomes hard to separate
all BPM changes from ``NOTE`` events. With this approach, you can trivially only target types you want to work with.

The other advantages involves static typing. With the single-array approach, you are working with a type of `Array<(type1 | type2 | type3)>`. This proves immediately painful, even
in more lenient-typed languages such as TypeScript.

If you find yourself wanting to convert to a single array, it can trivially be achieved with the following code:

```ts
function FlattenRGC(rgc) {
    let arr = [];
    for (const type in rgc.events) {
        for (const event of rgc.events[key]) {
            event._type = type; // monkeypatch _type on so you can logically branch based on type.
            arr.push(event);
        }
    }

    return arr;
}
```

!!! note

    A lot of rhythm game operations require events to be properly sorted on when they occur. However, the RGC format does not check the sortedness of events.

    It is legal for RGC files to have their events in any order, However, RGC parsers **SHOULD** sort all events "ms then lexicographically" on parse.

    This is because some operations upon RGC are only well defined against sorted files (such as hashing).

    For more information on this, see [bar](foo)

## Do Nots

If you find yourself writing your own schema, make sure you don't do the following!

- Events where `ms` doesn't matter.

If `ms` doesn't matter, you have metadata. It should be in chartMeta.

- Events with booleans

You **MUST NOT** define events that use booleans **to discriminate types**. For example:

```json
// CN
{
    "ms": 123,
    "col": 4,
    "isHCN": false
}
```

**MUST NOT** be done. Instead, you should opt for separate types.

```json
// CN
{
    "ms": 123,
    "col": 4
},
// HCN
{
    "ms": 123,
    "col": 4
}
```

This reduces the complexity of parsers, and avoids constant branching. Remember, an event type should point to an array of **identical event types**.

Note that its perfectly valid RGC to use booleans in an event, they should just not be used to discriminate types.

- Optional Properties

Events **MUST** not have optional properties. Nullable properties are completely okay, but all keys **MUST** be present at all times.
