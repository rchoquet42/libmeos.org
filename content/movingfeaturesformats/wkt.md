---
title: "Well-Known Text (WKT)"
date: 2022-07-29T14:22:43+02:00
draft: false
---

“Well-known text” is a scheme for writing moving features into a standard text string. It is an extension of the scheme for writing a [simple features ](https://en.wikipedia.org/wiki/Simple_Features)geometry into a [standard text string](https://libgeos.org/specifications/wkt/).



### WKT BNF

``` 
<at> ::= @
<comma> ::= ,
<equal> ::= =
<semicolon> ::= ;
<left paren> ::= (
<right paren> ::= )
<left bracket> ::= [
<right bracket> ::= ]
<left brace> ::= {
<right brace> ::= }
<srid> ::= SRID
<z> ::= Z

<b> ::= <boolean>
<f> ::= <float>
<i> ::= <integer>
<s> ::= <string>
<t> ::= <timestamp>

<bool representation> ::= <b>
<int representation> ::= <i>
<float representation> ::= <f>
<text representation> ::= <s>

<x coord> ::= <f>
<y coord> ::= <f>
<z coord> ::= <f>
<coords> ::= <x coord> <y coord> [ <z coord> ]
<srid representation> ::= <srid> <equal> <i> <semicolon>
<point representation> ::=
    [ <srid> ] POINT [ <z> ] <left paren> <coords> <right paren>

<base representation> ::=
    <bool representation> |
    <int representation> |
    <float representation> |
    <text representation> |
    <point representation> |

<tinstant> ::=
    <base representation> <at> <t>
<tinstant set> ::=
    <left brace> <tinstant> {<comma> <tinstant>} ... <right brace>
<tsequence set> ::=
    <left bracket> <tinstant> {<comma> <tinstant>} ... <right bracket>
<tsequence set> ::=
    <left brace> <tsequence> {<comma> <tsequence>} ... <right brace>

<wkt representation> ::=
    <tinstant> |
    <tinstant set> |
    <tsequence> |
    <tsequence set>
```

### Writing WKT

Because WKT is a decimal text output, it will never be able to exactly represent the underlying machine representation of the coordinates. In general, WKT is used for display to humans, or for transport when exact coordinate representation is not required (some web applications).

Accordingly, MEOS allows you to customize the coordinate precision and rounding behaviour, as well as the dimensionality of WKT output.


```
/* Read an instant */
const char *wkt_in = "Point(1 1 1,2 2 2)@2022-01-01";
TInstant *inst = tgeompoint_from_text(inst_str);

/* Generate the output WKT */
* The first argument sets the number places after the decimal to output in WKT. Default 16.
* The second argument sets the number of dimension to output in WKT. Default is as in the data.
  */
  unsigned char *wkt = tgeompoint_as_text(inst, 6, 2);

/* do something ... */

/* Free the allocated memory */
free(wkt_in);
free(wkt_out);
free(inst);
```


## Examples

- true@2022-01-01 08:00:00
- 1@2022-01-01 08:00:00
- 2.5@2022-01-01 08:00:00
- walking@2022-01-01 08:00:00
- POINT(0 0)@2022-01-01 08:00:00
- SRID=5676;Point(1 1)@2022-01-01 08:00:00
- {1@2022-01-01 08:00:00, 2@2022-01-01 09:00:00}
- [POINT(1 1)@2022-01-01 08:00:00, POINT(2 2)@2022-01-01 09:00:00]
- {[driving@2022-01-01 08:00:00, driving@2022-01-01 09:00:00], [walking@2022-01-01 10:00:00, walking@2022-01-01 11:00:00]}
- {[POINT(1 1)@2022-01-01 08:00:00, POINT(2 2)@2022-01-01 09:00:00], [POINT(3 3)@2022-01-01 10:00:00, POINT(3 3)@2022-01-01 11:00:00]}

