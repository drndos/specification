---
id: v2-rest-api
title: REST API
url: /v2/rest-api
version: v2
---

The REST API exposes a set of read-only resources to interact with a register
via HTTP.
These resources can be split into high-level (“porcelain”) and low
level (“plumbing”) depending on the intention of the resource.

Porcelain resources:

* [Records](/v2/rest-api/records): The latest data available.
* [Context](/v2/rest-api/context): Contextual information such as the schema.

Plumbing resources:

* [Entries](/v2/rest-api/entries): The entries of the [log](/v2/glossary/log).
* [Blobs](/v2/rest-api/blobs): The raw blobs of data.
* [Proofs](/v2/rest-api/proofs): The proofs to [audit](/v2/data-model/audit) the integrity of the register.


## Pagination

Collections MUST offer pagination through the [`Link` header](@rfc8288).
Use a `rel="next"` to link to the next page. You MAY offer a previous page
link with `rel="previous"` if you expect iterating backwards.

## Codes

The list below describes the essential HTTP codes and its meaning for any
Registers API. You SHOULD use other codes when necessary as long as they don't
collide with the ones defined in this specification.

|Code|Status|Description|
|-|-|-|
|200|Success|The resource exists and has been delivered.|
|301|Moved Permanently|The resource exists but it has a new permanent location.|
|404|Not Found|The resource doesn't exist.|
|405|Method Not Allowed|The request method is not allowed.|
|406|Not Acceptable|The format requested is not available.|
|500|Internal Server Error|Generic server error.|

The payload MUST be in the requested [serialisation format](#serialisation)
when available.


## Canonical URL

_This section is non-normative._

An implementation MAY implement [the canonical link](@rfc6596) to consolidate
URLs for the same resource with and without a trailing slash.


## Serialisation

There are two mechanisms to declare the preferred serialisation format: the
`Accept` header or the suffix. When both are provided the suffix MUST take
preference. When none are provided, JSON MUST be the fallback serialisation
format.

JSON is the canonical serialisation format but CSV is encouraged to be
implemented as well.


### JSON

* Suffix: `.json`
* Content type: `application/json`
* Reference: [JSON](@rfc8259)

All attribute values MUST be encoded as JSON strings. When JSON needs to be in
a canonical form, use the procedure defined in the [hash
datatype](/v2/datatypes/hash).

JSON can have missing attributes. These have the same semantics as an
attribute with a `null` value or an empty string or empty array.  See the
[forward compatibility section](/v2/data-model/evolve#forwards-compatibility).

### CSV

* Suffix: `.csv`
* Content type: `text/csv`
* Reference: [Tabular data model](@tabular-data-model)

CSV can have empty (blank) values. These have the same semantics as if
the attributes were missing.
See the [forward compatibility section](/v2/data-model/evolve#forwards-compatibility).

#### List of values

Generic CSV parsers typically treat all values as strings ([RFC4180](@rfc4180)).
This specification allows to express multiple values of the same type when the
[attribute](/v2/glossary/attribute) is of [cardinality
n](/v2/datatypes#cardinality). The syntax extension for CSV to allow this is:

```abnf
cell = value [";" value]
```

Where `value` is any valid string conforming to the attribute datatype.

***
**EXAMPLE:**

For example, a record such as:

```csv
id, name, flavours
1, Foo, pistachio;chocolate;vanilla
```

Where the attribute “flavours” is of cardinality n, is equivalent to the
following JSON:

```json
{
  "id": "1",
  "name": "Foo",
  "flavours": ["pistachio", "chocolate", "vanilla"]
}
```
***

***
**NOTE:**

The [tabular data model](@tabular-data-model) effort from the W3C provide a
way to describe what how a value is expected to be parsed. The Registers
specification aims to be compatible with it.

In that context, a value for an attribute of type string with cardinality n
can be defined in the tabular data model as:

```
"datatype": "string",
"separator": ";",
"valueUrl": "{?values}"
```
***


### Extensions

A register MAY provide additional, possibly domain specific, representations.

<!-- A register containing attributes with Points or Polygons values may also serve
a list of blobs as [GML](@gml), [KML](@kml) or other geographical 
serialisation format. -->


## Security

_This section is non-normative._

A register should only be available over HTTPS and it should redirect from
HTTP to HTTPS.

Registers aim to be auditable and tustworthy data sources. Any mechanism that
helps minimise attacks able to compromise the integritiy of the data in
transit or at rest should be considered favourably.

***
NOTE: You should consider enabling [HTTP Strict Transport Security](@rfc6797)
and use [Content Security Policy Level 2](@csp2) when appropriate.
***
