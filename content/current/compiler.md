Title: Gora Compiler Overview

# Introduction

The Gora compiler converts JSON files (the schema(s)) into persistent Java classes/data beans. 
You can then use those classes to interact with a variety of data storage software e.g. the Gora datastore implementations. 

The compiler is very simple to run. But first you should add the Gora installation directory to your path. 

# Usage

     $ bin/gora goracompiler

results in:

     $ Usage: GoraCompiler <schema file> <output dir> [-license <id>]
       <schema file>     - individual avsc file to be compiled or a directory path containing avsc files
       <output dir>      - output directory for generated Java files
       [-license <id>]   - the preferred license header to add to the
               generated Java file. Current options include; 
        ASLv2   (Apache Software License v2.0) 
        AGPLv3  (GNU Affero General Public License)
        CDDLv1  (Common Development and Distribution License v1.0)
        FDLv13  (GNU Free Documentation License v1.3)
        GPLv1   (GNU General Public License v1.0)
        GPLv2   (GNU General Public License v2.0)
        GPLv3   (GNU General Public License v3.0)
        LGPLv21 (GNU Lesser General Public License v2.1)
        LGPLv3  (GNU Lesser General Public License v2.1)

so for example, one would typically enter:

     $ bin/gora goracompiler gora-tutorial/src/main/avro/pageview.json gora-tutorial/src/main/java/


The schema file is a single JSON file or a directory containing JSON files. 

The output directory is the destination for the generated Java source files. For example, if you specific src/main/java then the 
generate source is placed into src/main/java/generated. It's generally a good idea to ignore the generate directory in whatever 
version control software you're using. You are using version control, right?

Finally, the license parameter tells the compile to add a license header to each generated file. Current header options include:

* ASLv2   (<a href="http://www.apache.org/licenses/LICENSE-2.0.html">Apache Software License v2.0</a>)
* AGPLv3  (<a href="http://www.gnu.org/licenses/agpl.html">GNU Affero General Public License</a>)
* CDDLv1  (<a href="http://opensource.org/licenses/CDDL-1.0">Common Development and Distribution License v1.0</a>)
* FDLv13  (<a href-"http://www.gnu.org/copyleft/fdl.html">GNU Free Documentation License v1.3</a>)
* GPLv1   (<a href="http://www.gnu.org/licenses/gpl-1.0.html">GNU General Public License v1.0</a>)
* GPLv2   (<a href="http://www.gnu.org/licenses/gpl-2.0.html">GNU General Public License v2.0</a>)
* GPLv3   (<a href="http://www.gnu.org/licenses/gpl-3.0.html">GNU General Public License v3.0</a>)
* LGPLv21 (<a href="http://www.gnu.org/licenses/lgpl-2.1.html">GNU Lesser General Public License v2.1</a>)
* LGPLv3  (<a href="http://www.gnu.org/licenses/lgpl-3.0.html">GNU Lesser General Public License v3</a>)

It should be noted that if no license header argument is passed, by default the ASLv2 license profile is selected.
