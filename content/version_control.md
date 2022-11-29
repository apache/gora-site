Title: Gora Version Control System

## Overview
For code development on Gora trunk, we use the official Apache Git repository of 
the Apache Software Foundation.

## Git Repository

### Anonymous Access (read-only)
The Apache git repository can be used for accessing development trunk code.
The URL for anonymous read-only access is [https://git.apache.org/gora.git/](https://git.apache.org/gora.git/). 
Alternatively the Github mirror at [https://github.com/apache/gora](https://github.com/apache/gora) can also be used. 
The repository can be cloned by:

    $ git clone https://git.apache.org/gora.git/ 

More instructions for setting up git access can be found [here](https://www.apache.org/dev/git.html).

### Committer Access (read-write)
Committers should always clone the read-write codebase as this is the latest codebase
that the development team is working on. It will also give you access to development 
branches at any given time. 

The code can be cloned as follows

    $ git clone https://git-wip-us.apache.org/repos/asf/gora.git

Committers should also be aware that the committers area and website are still hosted in
the official Apache SVN. More information can be found [here](https://svn.apache.org/repos/asf/gora/README). 
