RGC Specification
=============================================

This page serves as the specification (and documentation) for the .rgc (Rhythm Game Chart) file format.

The current version of RGC is ``v0.1.0``. As of writing this, this is in development, and is not stable for any purpose.

This specification is expected to be followed exactly by implementers, to avoid incompatibilities or spec fragmentation.

RGC is a JSON schema for rhythm game charts. It is designed to be simple to interact with, and works as an interchange format between rhythm games.

RGC does not replace any existing formats, and does not have to be implemented by any existing rhythm games.

.. toctree::
   :maxdepth: 2
   :caption: Overview:
   
   overview/motivation
   overview/definition
   overview/format
   overview/formatversion

.. toctree::
   :maxdepth: 2
   :caption: Specification

   rgc/outline
   rgc/meta
   rgc/rgcMeta
   rgc/events
   rgc/event
   
.. toctree::
   :maxdepth: 2
   :caption: Standard Formats

   std/iidx-SP
   std/iidx-DP

.. meta::
    :title: RGC Specification
    :description: The official specification for RGC.
    :keywords: Docs, Rhythm Game, rgc, json, pulse, backbeat