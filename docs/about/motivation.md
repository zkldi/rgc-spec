# Why RGC?

## Why make a rhythm game chart format?

The primary motivation for RGC is that existing rhythm game schemas are difficult to work with.

Almost every rhythm game has invented their own way of storing chart information. Furthermore, there are often not specifications for how these files should be formated,
or what even constitutes a well-formed chart under some specifications.

What results from this is that, if someone wants to interact with format ``.xyz``, they would have to write their own parser, their own serialiser, and their own tools to work with it.

If they ever want to migrate to a different language, they have to write these tools again, if the format for ``.xyz`` changes underfoot as time goes on, they're screwed.

A lot of existing rhythm games also have implementation-as-specification issues, where how a format works
is not defined by a specification, but rather by what the most popular interpreter does. A key example of this is LR2's ``.bms`` parser.

## What makes RGC easy to parse?

It's JSON. Almost every programming language (yes, [almost every]("https://github.com/paulohenriquesn/brainfuck-json")) has JSON support either built in or with libraries. This means that parsing RGC in any language is as simple as parsing JSON, which is pretty simple.

Similarly, other design decisions have been made to make RGC as simple to work with as possible. For example, events are defined at a given ``ms``, instead of using ticks, measures or the alignment of the sun.

## But you're proposing another format? Doesn't that make things worse?

![](../img/standards.png)

Under normal circumstances, I'd agree that this would contribute to a mess of more and more schemas, except, there aren't actually existing standards for a rhythm game chart.

While there *are* lots of existing schemas for rhythm game charts for **specific games**, there isn't actually a game-agnostic one.

In fact, RGC could be more suitably described as an interchange format for existing chart schemas. Of which, there are none to compete with!