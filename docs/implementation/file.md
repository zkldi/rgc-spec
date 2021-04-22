# RGC File Info Specification

As mentioned in [Core Information](../core-notes), RGC is a subset of JSON.

As such, all rules of [RFC8259](https://tools.ietf.org/html/rfc8259) apply.

## File Extension

RGC files **SHOULD** use the `.rgc` file extension. They **MAY** use `.json` as a file extension.

## MIME Type

RGC files **MUST** use ``application/json`` as their MIME type.

## Encoding

As is specified by JSON, RGC files **MUST** be in UTF-8, and **MUST NOT** have a Byte-Order-Mark before content.

If converting from files that prefer another encoding (such as `.bms`), data **MUST** be converted to UTF-8.

## Comments

As is specified by JSON, RGC files **MUST NOT** contain comments. RGC-interacting code **MUST NOT** accept RGC files containing comments.

