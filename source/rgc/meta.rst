.. _rgc_rgcmeta:

RGC: RGCMeta
==================================

RGCMeta (field name ``rgcMeta``) **MUST NOT** be extended by formats. Format Metadata should go inside ``meta``.

.. list-table:: Contents
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``format``
        - Format String
        - Contains the format the RGC adheres to.
    *   - ``rgcVersion``
        - Version String
        - Contains the version of RGC this adheres to. This is not expected to change in the future, but is defined anyway.
    *   - ``formatVersion``
        - Positive Integer
        - Contains the version of the format this is for. This is a single integer, and is intended for parsers/serialisers to know what they're doing.

####################################
Format String
####################################

The value of this field **MUST** be a string, and **MUST** match the following criteria:

- The namespace and name must be between 3 and 32 characters (inclusive).
- The namespace and name can ONLY include :abbr:`Alphanumeric characters and the dash character. (abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-)`.
- The namespace and name must be separated with a single /.

.. list-table:: Examples:
    :widths: 50 50
    :header-rows: 1
 
    * - Value
      - Validity
    * - ``namespace/game``
      - Valid
    * - ``name-space/kebab-case``
      - Valid
    * - ``nameSpace/camelCase``
      - Valid (But kebab-case is prefered.)
    * - ``a-thirty-two-character-namespace/game``
      - Valid
    * - ``ab/game``
      - Invalid: Namespace must be atleast 3 characters.
    * - ``namespace/ab``
      - Invalid: Name must be atleast 3 characters.
    * - ``a-really-really-really-long-character-namespace/game``
      - Invalid: Namespace must be less than 32 characters.
    * - ``namespace/a-really-really-really-long-name``
      - Invalid: Name must be less than 32 characters.
    * - ``name space/game``
      - Invalid: Space is not a legal character.
    * - ``ぽげ/game``
      - Invalid: ぽ and げ are not legal characters.

.. note::
    This can be implemented with the following regex: ``/^[a-zA-Z0-9-]{3,32}\/[a-zA-Z0-9-]{3,32}$/``.

####################################
Version String
####################################

A Version String is a semver-like string.

Similarly to semver, the string follows MAJOR.MINOR.PATCH format, number wise.

This means that changes in MINOR may add new features, but not break existing code,
changes in PATCH will not happen and changes in MAJOR assert incompatibility with other major versions.

.. warning::
    As the name of ``rgcVersion`` implies; this is for distinguishing between possible schema changes RGC may have in the future.
    It's incredibly unlikely that a major schema change will ever happen, but if it does, the MAJOR part of the version string will change.

The value of a Version String **MUST** match the following criteria:

- Contain three valid numbers, separated by ``.`` and nothing else.

.. list-table:: Examples:
    :widths: 50 50
    :header-rows: 1
 
    * - Value
      - Validity
    * - ``1.0.0``
      - Valid
    * - ``0.0.0``
      - Valid (But nonsensical.)
    * - ``13.14.15``
      - Valid
    * - ``v1.1.0``
      - Invalid: Cannot have anything other than three separated numbers.
    * - ``1.1.0-alpha``
      - Invalid: Cannot have anything other than three separated numbers. (Note that this *is* valid semver, though.)
    * - ``01.0.0``
      - Invalid: 01 is not a valid number.
    * - ``1.1.x``
      - Invalid: x is not a valid number
    * - ``1.0``
      - Invalid: Must have three separated numbers.
    * - ``1.0.``
      - Invalid: Must have three separated numbers.

.. note::
    This can be implemented with the following regex: ``/^(?:0|[1-9]\d*)\.(?:0|[1-9]\d*)\.(?:0|[1-9]\d*)$/``.