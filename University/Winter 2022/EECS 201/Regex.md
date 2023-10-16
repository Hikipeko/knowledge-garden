A pattern that matches a set of strings.

Provide a standardized way to perform matches on text.

**Online regex tester**

https://regex101.com/

Patterns are composed of smaller regexes that are concatenated.

E.g., h, e, l, l, o concatenated to form the overall regex "hello"

```regex
. for any single character
| for an OR hello|world matches hello and world
\ for special expressions
(,) enclose a whole expression as a subexpression
(A|B) (C|D) matches AC, AD, BC, BD
// Quantifiers
?: <= 1 time
*: >= 0 times
+: >= 1 times
{n}: n times
{n,}: >= n times
{,m} <= m times
{n,m} n <= x <= m
// Brackets
[,] enclose a set to match
[A-Za-z0-9] alphanumeric characters
[^ab] everything not 'a' or 'b'
[:alnum:] alphanumeric characters
[:alpha:]
[:blank:] space and tab characters
// Anchors: perform positional matching
^hello hello must be at the beginning of a line
world$ world must be at the end of a line
// Backreference: match previous parenthesized () subexpression
\n matches nth parenthesized subexpression
(123)testing\1 matches "123testing 123"
```
