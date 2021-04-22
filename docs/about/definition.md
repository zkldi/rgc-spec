# What is a chart?

So that we're all on the same page - this defines how RGC interprets a "chart".

## Events

RGC interprets a chart as being made of two parts, Events and Metadata.

An event, in RGC, is simply any event that happens at a certain time. Notes, for example, would be an event.

Similarly, non-playable events are also events, such as BPM changes or measure changes.

In short, **anything that happens at a given time is an event to RGC.**

To store events in RGC, RGC uses an Event Type. For example, Notes in ``std/iidx-SP`` have the type of ``NOTE``, BPM events have the type of ``BPM`` and so on.

## Metadata

Metadata, on the other hand, is anything that the chart has stored that **does not** happen at a given time.

For example, the title, artist and charter of the song are all metadata, as they are timestamp agnostic.

In short, anything that is stored about the chart that does not happen at a given time, is metadata.