# RGC Schema

RGC files **MUST** have the following properties.

- `meta`
- `chartMeta`
- `events`

They **MUST** all be defined as objects.

!!! note
    The order in which *any* keys appear is not a specification requirement.

---

## ``meta``

The `meta` field for RGC **MUST** have the following properties, and they **MUST** match the below description.

### `schema`

The schema field **MUST** be a string that matches the following regex:

```regex
/^[a-zA-Z0-9-]{3,32}\/[a-zA-Z0-9-]{3,32}$/
```

!!! info
    The regex enforces that `schema` is in the `namespace/name` form, where
    namespace and name are between 3 and 32 characters.

    Namespace and name **MUST** also be alphanumeric characters only. As such, this regex implements that.

Parsers **SHOULD** reject schema values they do not recognise, but **MAY** reduce that to a warning.

### `schemaVersion`

The schemaVersion field **MUST** be a positive non-zero integer.

Parsers **SHOULD** reject schemaVersion values they do not recognise, but **MAY** reduce that to a warning.

### `rgcVersion`

The rgcVersion field **MUST** be a string that matches the following regex:

```regex
/^(?:0|[1-9]\d*)\.(?:0|[1-9]\d*)\.(?:0|[1-9]\d*)$/
```

!!! info
    The regex enforces that `rgcVersion` is in a `MAJOR.MINOR.PATCH` form,
    where MAJOR, MINOR and PATCH are all 0 or greater.

### `title`

The title field **MUST** be either a string or null.

### `artist`

The artist field **MUST** be either a string or null.

### `charter`

The charter field **MUST** be either a string or null.

---

## ``chartMeta``

The ``chartMeta`` field for RGC **MUST** be defined as an object.

Parsers **SHOULD** validate this against the `schema`'s expected values for ``chartMeta``.

If validating against the `schema`'s expected values, parsers **MUST** reject excess properties.

---

## ``events``

The ``events`` field for RGC **MUST** be defined as an object.

Parsers **SHOULD** validate this against the `schema`'s expected values for `events`.

Parsers **MUST NOT** ignore event types if validating against the schema.

---

## Validating Schemas

Parsers **SHOULD** validate RGC files against the schemas they claim to be.

### ``chartMeta``

If validating RGC against a schema, Parsers **MUST NOT** allow excess properties in this field.

Validation **MUST** involve checking **all** expected properties, and they all **MUST** be defined.

### ``events``

If validating RGC against a schema, Parsers **MUST NOT** allow excess properties in this field.

Validation **MUST** check **all** event types. They all **MUST** be defined as an array of the expected type.

All events **MUST** extend the Neutral Event - they must have an ``ms`` property which is a number.

## Neutral Event

The neutral event is the base event that all events must extend from. It is defined as follows.

| Property | Type | Description |
| :: | :: | :: |
| `ms` | Number | The absolute miliseconds this event occurs at. This can be positive or negative. |