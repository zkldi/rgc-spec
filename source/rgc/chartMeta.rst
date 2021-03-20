.. _rgc_chartmeta:

RGC: ChartMeta
==================================

Chart Meta (field name ``chartMeta``) **MAY** be extended by schemas.

By default, however, it is an empty object. Schemas will extend this field to include metadata about their chart.

#########
Example
#########

.. code-block:: ts

    {
        "meta": {
            "schema": "std/iidx-SP",
            "title": "5.1.1."
            // etc...
        },
        "chartMeta": {
            "genre": "PIANO AMBIENT",
            // etc...
        }
    }

