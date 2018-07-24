---
id: register-resource
title: Register resource
url: /resources/register-resource/
status: wip
---

* Endpoint: `GET /register`

Response attributes:

* `domain`: (String) The Internet domain the register is available from.
* `total-entries`: (Integer) The number of entries in the log.
* `total-records`: (Integer) The number of records in the register.
* `register-record`: A copy of the Register register Record resource entity describing this register.
* `custodian`: (Optional String) The custodian of the register.
* `last-updated`: (Timestamp) The date the register was last updated.

The register resource summarises the register statistics.

***
TODO: The `register-record` field doesn't show a full record, just the item
part of it.
***

***
**EXAMPLE:**

```http
GET /register HTTP/1.1
Host: country.register.gov.uk
Accept: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "domain": "register.gov.uk",
  "total-records": 199,
  "total-entries": 208,
  "register-record": {
    "fields": [
      "country",
      "name",
      "official-name",
      "citizen-names",
      "start-date",
      "end-date"
    ],
    "registry": "foreign-commonwealth-office",
    "text": "British English names and descriptive terms for countries as recognised by the UK government",
    "phase": "beta",
    "register": "country"
  },
  "custodian": "**REDACTED**4b0d7e9f250bfe7844571bc1c1a4eb23294ac264cf0e27690cc043b0ac779ab6",
  "last-updated": "2018-06-13T13:54:40Z"
}
```
***