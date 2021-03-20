.. _rgc_meta:

RGC: Meta
==================================

Meta (field name ``meta``) **MAY** be extended by formats.

As such, everything listed below is a required field, but other fields may be present depending on ``rgcMeta.format``.

.. list-table:: Contents
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``title``
        - String | null
        - The song title for this chart. If this is not known, this **SHOULD** be null.
    *   - ``artist``
        - String | null
        - The song artist for this chart. If this is not known, this **SHOULD** be null.
    *   - ``charter``
        - String | null
        - The charter behind this song. If this is not known, this **SHOULD** be null.