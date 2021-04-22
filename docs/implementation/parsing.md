# RGC Parsing Specification

RGC can be safely parsed with any compliant JSON parser.

There are some methods that **SHOULD** be applied to RGC files after parsing,
but you need not do them.

## Sorting

Code working with RGC will frequently want ``events`` in RGC to be sorted properly.

RGC Parsers **SHOULD** apply the [RGC Consistent Event Sorting Algorithm (CESA)](../sorting) after parsing.

RGC Parsers **MAY** choose to not do this, if they do not need the sortedness of data.

!!! warning
    Functions such as [Hashing RGC](../hashing) are only defined for properly-sorted RGC. By not sorting, you **MUST NOT** perform these algorithms, as they are not defined for unsorted data. 

## Schema

If an RGC file does not match the [RGC Schema](../schema), it **MUST** be rejected by the parser.