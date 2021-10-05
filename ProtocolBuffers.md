# Data comparisons:
1. CSV
2. Relational data
  Cons
  * Can't be transmitted directly
3. JSON
  Pros
  * Simple
  * Network transmittable
  Cons
  * 
4. Protocol Buffers
  Pros
  * Fully typed data
  * Compressed automatically
  * Schema required to read or generate code
  * Documentation embeded in data
  * Schema evolution possible
  * 10x smaller 100x faster than XML
  * Code generated
  Cons
  * Requires a library
  * Data can't be read without schema

```proto
syntax="proto3";
message Message {
  string arg1 = 1;
  string arg2 = 2;
}
```

`.proto` -> GoLibrary Encode / Decode -> Data

Some databases support protocol buffer format for direct storage


Proto2 vs Proto3
2016 google released proto3
Easiest to learn

```proto

/*
 * Use imported packages with dot access syntax like Java.
 */
package [DOT SEPERATED TYPE NAME]
import [FILENAME RELATIVE TO CALLER];

enum Flavour {
  UNKNOWN = 0; // Defaulkt
  CHOCOLATE = 1;
  VANILLA = 1;
}
/*
 * int32
 * int64
 * float
 * double
 * bool 
 * string
 * bytes
 *
 * Type can be an enum, message or primitive
 */
message [ NAME ] {
  [? MODIFIER ] [ TYPE ] [ LABEL ] = [ TAG ];
  int32 age = 1;

/*
 * 1-15 primary use tags
 * 16+ secondary, less populated tags
 */
  repeated int32 ages = 2;
  Flavour flavour = 3;
}

```


## Default fields
All fields if not specified or known take a default value
```
bool = false
number = 0
string = ""
byes = empty
enum = first value
repeated empty list
```

