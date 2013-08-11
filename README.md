# mofo

Everybody seems to be creating serialization formats, so here's mine: Mark's Object FOrmat

## Rationale

JSON is fairly lightweight, but it's still cruftier than it needs to be and doesn't handle dates properly.
MOFO reduces the cruft and does dates.

## RFC

I didn't do CS or anything like that, so I don't know BNF; therefore, the following will be in English.

### Primitives

Five primitive data types are supported: String, Number, Date, Boolean and Binary:

* String elements are surrounded by $, as in `$MOFO is awesome$`.
  * An actual $ symbol in a string is escaped by doubling, as in `$Total: $$599.99$`
  * A null string is represented by `$$`
* Number elements are surrounded by #, as in `42`
  * A null number is represented by `##`
* Dates are surrounded by / and are in ISO 8601, as in `/2013-08-11T15:17:10Z/`
  * A null date is represented by `//`
* Boolean values are not delimited; they are represented by the single characters `^` for true and `!` for false
  * A null boolean is represented by a `?`
* Binary data is represented by non-delimited, lowercase hexadecimal digit pairs surrounded by ampersands, as in `&4d4f464f&`.
  * A null binary value is represented by `&&`

### Structured data

If it ain't broke, don't fix it.

#### Object graphs

* Object graphs are surrounded by `{` and `}`.
* Any text outside the delimiters used for primitive types and the three boolean symbols is considered to be a property name.
  * Property names may not contain the following characters: `$` `#` `/` `^` `!` `?` `&` `{` `}` `[` `]`
  * But they can contain spaces

Example:
````
{First name$Douglas$Last name$Adams$Date of birth/1952-03-11/}
````

#### Lists

* Lists are surrounded by [ and ]
* Elements within lists are naturally delimited
  * Consecutive elements of the same type are split by a single instance of that type's delimiter

Examples:
* Numbers: `[#0#1#1#2#3#5#8#13#21#]`
* Strings: `[$Alice$Bob$Charlie$]`
* Dates: `[/1970-01-01/1601-01-01/]`
* Booleans: `[^^^!!?^!]`
* Mixed (string, string, number, boolean, date): `[$Alice$Bob$#42#^/1752-09-14/]`

### Whitespace

Whitespace outside the `$` string delimiters and not within a property name is ignored.

### Summary

This JSON:

````
{
  "Name": "Phoenix",
  "Chassis": "Titan V",
  "First launch": "5th April 2063",
  "Thumbnail": ???,
  "Real": false,
  "Crew": [ "Cochrane", "Riker", "La Forge" ]
}
````
  
Is equivalent to this MOFO:

````
{Name$Phoenix$Chassis$Titan V$Drive$Warp$First launch/2063-04-05/Real!Thumbnail&94a2f19094213a6f8241a9408266f957&Crew[$Cochrane$Riker$La Forge$]}
````

Which is clearly way better.
