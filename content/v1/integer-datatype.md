---
id: v1-integer-datatype
title: Integer
url: /v1/datatypes/integer
version: v1
---

A base 10 number with no fractional component.

```abnf
integer = "0" / (["-"] non-zero *digit)
digit =  "0" / positive-digit
non-zero =  "1" / "2" / "3" / "4" / "5" / "6" / "7" / "8" / "9"
```

Leading zeros are not allowed, except for the integer 0, which is
represented as the string `0`. Negative values are marked with a leading “-”
character ([UNICODE](@unicode) `0x2D` HYPHEN-MINUS).

Numeric values such as Infinity and NaN are not permitted.

***
**EXAMPLE:**

The following examples are all valid integer values:

```
100
0
-200
```
***

***
**NOTE:** The integer maximum size is dependent on the implementation.
***
