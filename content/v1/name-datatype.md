---
id: v1-name-datatype
title: Name
url: /v1/datatypes/name
version: v1
---

A **name** is a [String](/v1/datatypes/string) restricted to lower case ASCII
letters, digits and hyphens with the following pattern:

```abnf
name = ALPHA *(ALPHA / DIGIT / "-")
```

***
**EXAMPLE:**

For example, the following values are valid attribute names:

```
name
start-date
end-date
key-01
```

And the following are invalid:

```
0x
42-date
-boo
```
***
