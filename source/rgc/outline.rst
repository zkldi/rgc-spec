RGC: Main
==================================

This defines the entire JSON body inside RGC.

.. warning::
    Unless otherwise specified, fields are NOT nullable, and must be defined.

.. list-table:: Body
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``meta``
        - :ref:`rgc_meta`
        - Contains information about the RGC file, including the schema this RGC adheres to.
    *   - ``chartMeta``
        - ``rgcMeta.schema`` extended :ref:`rgc_chartmeta`
        - Meta information about the song and chart.
    *   - ``events``
        - ``rgcMeta.schema`` extended :ref:`rgc_events`
        - A dictionary containing arrays of all events in the chart.

#########
Example
#########

.. code-block:: ts

    {
        "meta": {
            "schema": "std/iidx-SP",
            "schemaVersion": 1,
            "rgcVersion": "0.1.0",
            "title": "Song Title",
            "artist": "Song Artist",
            "charter": "zkldi"
        },
        "chartMeta": {
            "genre": null,
            "level": "12",
            "marquee": "SONG TITLE",
            "difficulty": "HYPER"
        },
        "objects": {
            "NOTE": [
                {
                    "ms": 4900,
                    "col": 6
                }
            ],
            // All these must be present, even if empty!
            "BGM": [],
            "BSS": [],
            // "BPM", "SAMPLE", etc...
        }
    }