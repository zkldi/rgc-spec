# RGC Specification

This page serves as the specification (and documentation) for the .rgc (Rhythm Game Chart) file format.

The current version of RGC is ``v0.2.0``. As of writing this, this specification is in development, and is not stable for any purpose.

RGC is a JSON schema for rhythm game charts. It is designed to be simple to interact with, and doubles up as an interchange format between rhythm games.

RGC does not replace any existing schemas, and does not have to be implemented by any existing rhythm games.

## Working With RGC

This is the site for the RGC format. For tools to appropriately work with it, the offical TypeScript/JS library for interacting with RGC can be found [todo]().

## About

The about pages contain informal explainations of the RGC format, intended to explain what problems this solves
and why it exists.

These are **NOT** a substitute for the technical documentation, and the technical documentation is the only proper reference for RGC implementations.

* [Why RGC?](about/motivation)
* [What is a chart?](about/definition)
* [What is a chart?](about/definition)
* [Example RGC File](about/structure)
* [RGC ``meta``](about/meta)
* [RGC ``chartMeta``](about/chart-meta)
* [RGC ``events``](about/events)

## Implementation

If you are interested in implementing or working with RGC, the
below pages contain exhaustive technical information about implementation.

## Schemas

Alongside RGC we maintain a set of standardised schemas to work with. This page should be used as the official reference for those schemas.