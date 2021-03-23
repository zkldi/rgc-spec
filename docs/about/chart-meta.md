# RGC Chart Meta

We'll be disecting this part of the example RGC file:

```json
"chartMeta": {
    "difficulty": "HYPER",
    "genre": "BIG BEAT",
    "level": "2",
    "marquee": "GAMBOL"
},
```

The first thing worth noting about `chartMeta` is that all of these properties are *defined by the `schema`*.

This is **very** important! In short, ``chartMeta`` contains [metadata](../definition) about the chart, and **all of its fields** are defined by the `schema`.

## How do I know what metadata to expect?

Well, if you don't recognise the `schema`, you can't. However, the `std/` library for schemas has definitions (in this documentation) for what to
expect from `chartMeta` given a schema.

Since our example RGC file is for ``std/iidx-SP``, you can refer to its documentation [here](help);

## Why is it like this?

Schemas are allowed to have freedom on what they deem metadata, and what they could have or need as metadata simply cannot be anticipated properly.

Although [RGCMeta](../meta) makes an attempt to standardise `title`, `artist` and `charter`, there's nothing else that we can reasonably anticipate.

