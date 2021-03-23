# RGC Meta

We'll be disecting this part of the example RGC file:

```json
"meta": {
    "schema": "std/iidx-SP",
    "rgcVersion": "0.2.0",
    "schemaVersion": 1,
    "title": "GAMBOL",
    "artist": "SLAKE",
    "charter": null
}
```

## `title`, `artist`, `charter`

These three are the simplest to explain.

The reason these are in `meta` is because these are *guaranteed* properties of an RGC file.
That is to say that, every chart belongs to a song, and has to be created by someone (or something).

### `title`

**Type**: String | null

The title for the song this RGC file belongs to. Sometimes this is not known, or not available. As such, this field can be null.

### `artist`

**Type**: String | null

The artist for the song this RGC file belongs to. This does not have to be a proper indexable string, and can be a pseudonym or similar.

Sometimes this is not known, or not available. As such, this field can be null.

### `charter`

**Type**: String | null

The charter who created this chart. This does not have to be a proper indexable string, and can be a pseudonym or similar.

Unlike the previous two, this field is *much* more likely to not be found in a file.

Like the others, it can be null where this is not known.

## `rgcVersion`

**Type**: Version String (see ref)

This field indicates the version of RGC this RGC file is for. It is **not** likely that this will ever change.

This **MUST NOT** be null, and **MUST** be a valid version string.

As mentioned above, it's likely this will never significantly change. It only exists to give us that option in the future.

---

## `schema`

**Type**: String

The `schema` string indicates what "schema" other parts of this RGC belong to. This allows for RGC to be extended, and natively support
multiple games.

### Working for multiple games

You might be confused as to how one single format can possibly anticipate everything a rhythm game could throw at it.

The answer is simple: It doesn't try to.

RGC works as a skeleton schema. It requires certain properties to be defined, then uses this parameter to define a ``schema``.

This ``schema`` defines an extension to the spec, for what that specific game may want.

With this, we can enforce sensible properties for games, while giving the schema free reign to extend where necessary.

### Avoiding Fragmentation

This is cool and all, but surely this suffers the standard fragmentation mentioned before? What if two people define different schemas for one game?
Do I have to make my own schema?

This has also been thought through. The ``schema`` string property in RGC **MUST** follow a ``namespace/name`` format.

As an example, if I wanted to define my own schema for CHUNITHM, I would use the schema name ``zkldi/chunithm``, or similar.

The point of the namespacing is to vastly reduce chances of collision between users.

And as for avoiding initial fragmentation, the RGC spec also defines a standard library of schemas for popular games. These are prefixed with the ``std/`` namespace.

The RGC spec reserves the ability to define anything under the ``std/`` namespace, and as such, you **MUST** not implement custom schemas inside of it.

As an example, if I wanted to work with Single Player IIDX, I would use the ``std/iidx-SP`` schema.

!!! note
    There are other restrictions on what characters can go inside the ``schema`` field. These restrictions can be found at :ref:`rgc_meta`

## `schemaVersion`

**Type**: Positive Non-zero Integer

This field indicates the "version" of the above schema. This is used to account for future changes in what the schema allows, without always creating new schemas.

This is not a perfect solution, but you can read a justification below.

### Schemas and Time

It's no secret that games add features over time. The standard schema library has to handle this one way or another, but we run into a pretty irritating issue.

To use a real world example, let's consider the changing of certain ``chartMeta`` properties from IIDX 26 ROOTAGE to IIDX 27 HEROIC VERSE.

In ROOTAGE, the legal values for ``chartMeta.difficulty`` were NORMAL, HYPER and ANOTHER. In HEROIC VERSE, a fourth difficulty was added - LEGGENDARIA.

Now, nothing really significant has changed to the schema here, but we would have to create a new schema ``"schema": "std/iidx-SP-heroicverse"`` to indicate this is now a possible value.

But, that sucks; we'd have loads and loads of schemas, and we'd have to backtrack among all IIDX games in order to create separate schemas for each.

The work on serialisers would be immense, and they'd have to update/repeat a lot of code a lot of the time. But schema versioning doesn't magically solve this problem. We'd just be shifting the problem form "loads of schemas" to "loads of schemaVersions".

The selling point of RGC - that it's easy to parse and work with - would be completely neutered if parsers and serialisers had to account for 15 different schemas in different ways.

### Rolling Supersets

~~help~~
