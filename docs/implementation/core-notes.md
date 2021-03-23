# Core Information

If present, and in capitalised bold, words are to be interpreted as declared in [RFC2119](https://tools.ietf.org/html/rfc2119).

RGC is a subset of [JSON](https://www.json.org)
As such, all rules of [RFC8259](https://tools.ietf.org/html/rfc8259) apply.

## Types

The implementation details often discuss that properties have types.

This documentation uses the exact following definition of types.

!!! warning
    Types are **NOT** implicitly nullable.

### Prefixes

The prefix "Positive" means >= 0.

The prefix "Positive Non-zero" means > 0.

Similarly, "Negative" means <= 0, and "Negative Non-zero" < 0.

The prefix "Optional" means the property is allowed to not exist at all.

### Union

Types may be declared as a union of primitives or other types, such as 0 | 1
meaning 0 OR 1.

Similarly, 0 | string (while useless), indicates a union of the value 0 and any string.

If something is nullable, it will be unioned with "null", like String | null.

### Primitives

A String refers to a string.

A Number refers to a **decimal/float** value.

An Integer refers to a **whole** positive or negative value.

A Boolean refers to a boolean - true | false.

Null refers to the exact primitive `null`.

### Arrays

Arrays are to be interpreted as dynamic-length lists. This is identical to terms
such as ArrayList in C# or Java.

Arrays of types are declared as Array&lt;T&gt;, where T is the type.

### Other Objects

Any other declaration will refer to an object. This may be a hyperlink to the
object declaration, or may be included relatively nearby.

### Example Declaration

The below tables show an example declaration of a user object with this syntax.

| Property | Type | Description |
| :: | :: | :: |
| `name` | String \| null | The users name. |
| `age` | Positive Integer | The users age. |
| `friends` | Array&lt;String \| Positive Integer&gt; | The users friends, this can be a string or a user ID. |
| `permissions` | Permissions Object | The permissions this user has. |

#### Permissions Object

| Property | Type | Description |
| :: | :: | :: |
| `admin` | Boolean | Whether this user is an admin or not. |
| `moderator` | Boolean | Whether this user is a moderator or not. |