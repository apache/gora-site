Title: Gora Compiler-CLI Overview

# Introduction

The Gora compiler-cli is a simple utility dependency which provides a command line interface used to invoke the [Gora Compiler](./compiler.html)
It exists separate from the Gora Compiler enabling us to distinguish between usability and functionality. It does however depend upon the Gora Compiler.

The compiler-cli is trivial to invoke but also provides a useful usage statement when invoked incorrectly

# Usage

     $ Usage: GoraCompilerCLI 
     or
     $ Uage: GoraCompilerCLI -h

results in:

     $ Usage: gora-compiler ( -h | --help ) | (<input> [<input>...] <output>)

so for example, if you wished to compile one schema file, you could enter:

     $ bin/gora gora-compiler gora-tutorial/src/main/avro/pageview.json gora-tutorial/src/main/java/

if however for example you wished to compile more than one schema file, you could enter:

     $ bin/gora gora-compiler gora-tutorial/src/main/avro/pageview.json gora-core/src/examples/avro/webpage.json gora-tutorial/src/main/avro/metricdatum.json gora-tutorial/src/main/java/

The schema file is a single JSON file or a string array of JSON files. 

The output directory is the destination for the generated Java source files. For example, if you specific <code>src/main/java</code> then the 
generate source is placed into <code>src/main/java/</code> under the package naming convention used within the JSON schema.

