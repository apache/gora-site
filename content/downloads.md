Title: Gora Releases 

# Download

[TOC] 

## Introduction

Download the newest release of Apache Gora. <b>Please Note</b> Gora is ONLY released
as source code and NOT binary. This is because you will most likely want to recompile 
various aspects of the codebase during or as a prerequisite to using Gora in your stack.

See the [CHANGES](https://github.com/apache/gora/blob/apache-gora-0.9/CHANGES.md#apache-gora-09-release---120819-ddmmyyyy)
file for more information on the list of updates in this release.

Gora is always distributed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).

## Prerequsites

You require [Apache Maven](http://maven.apache.org) to build the Gora source code. 
Maven can either be downloaded and installed manually or alternatively via command
line via your operating system package manager.

<b>N.B.</b> Gora is NOT tested against the Windows platform.

## Downloads

<a href="http://www.apache.org/dyn/closer.lua/gora/0.9/apache-gora-0.9-src.tar.gz" class="btn btn-primary btn-large">Download (0.9 src.tar.gz)</a>
<a href="http://www.apache.org/dyn/closer.lua/gora/0.9/apache-gora-0.9-src.zip" class="btn btn-primary btn-large">Download (0.9 src.zip)</a>

## Mirrors

The link in the Mirrors column below should display a default mirror selection 
based on your inferred location. If (when you browse to it) you do not see that page, try a different browser. 
The checksum and signature are links to the originals on the main distribution server.

<table class="table">
  <thead>
  <tr>
    <th align="left">Version</th> 
    <th align="left">Mirrors</th> 
    <!--   <th align="left">MD5 Checksum</th> -->
    <th align="left">ASCII Signature</th> 
    <th align="left">SHA512 Checksum</th> 
  </tr>
  </thead>
  <tbody>
  <tr>
    <td>Apache Gora 0.9 (tar.gz)</td>
    <td><a href="http://www.apache.org/dyn/closer.cgi/gora/0.9/apache-gora-0.9-src.tar.gz">apache-gora-0.9-src.tar.gz</a></td> 
    <!-- <td><a href="http://www.apache.org/dist/gora/0.9/apache-gora-0.9-src.tar.gz.md5">apache-gora-0.9-src.tar.gz.md5</a> </td>  -->
    <td><a href="http://www.apache.org/dist/gora/0.9/apache-gora-0.9-src.tar.gz.asc">apache-gora-0.9-src.tar.gz.asc</a> </td> 
    <td><a href="http://www.apache.org/dist/gora/0.9/apache-gora-0.9-src.tar.gz.sha512">apache-gora-0.9-src.tar.gz.sha512</a> </td>
  </tr>
  <tr>
    <td>Apache Gora 0.9 (zip)</td>
    <td><a href="http://www.apache.org/dyn/closer.cgi/gora/0.9/apache-gora-0.9-src.zip">apache-gora-0.9-src.zip</a></td>
    <!-- <td><a href="http://www.apache.org/dist/gora/0.9/apache-gora-0.9-src.zip.md5">apache-gora-0.9-src.zip.md5</a></td> -->
    <td><a href="http://www.apache.org/dist/gora/0.9/apache-gora-0.9-src.zip.asc">apache-gora-0.9-src.zip.asc</a></td>
    <td><a href="http://www.apache.org/dist/gora/0.9/apache-gora-0.9-src.zip.sha512">apache-gora-0.9-src.zip.sha512</a></td>
  </tr>
  </tbody>
</table>

## Verify Releases
It is essential that you verify the integrity of the downloaded files using the PGP and SHA512 signatures. 
published with every Gora release.

Please read [Verifying Apache HTTP Server Releases](http://httpd.apache.org/dev/verification.html) 
for more information on why you should verify our releases.

We strongly recommend you verify your downloads with at least both PGP Signature and SHA512 Checksum. Guidance
for doing so is provided below.

## PGP Signature
The PGP signatures can be verified using PGP or GPG. First download the 
[KEYS](http://www.apache.org/dist/gora/KEYS) as well as the asc signature file 
for the relevant distribution. 

<b>N.B.</b>Make sure you get these files from the 
[main distribution directory](http://www.apache.org/dist/gora/), rather than from a 
mirror. Then verify the signatures using the following

    $ gpg --import KEYS
    $ gpg --verify gora-X.Y.Z-src.tar.gz.asc
    
The files in the most recent release are signed by Kevin Ratnasekera (CODE SIGNING KEY) <djkevincr@apache.org> A3E66AC7
    
## SHA512 Checksum
Alternatively, you can verify the SHA512 checksum on the files. 
A unix program called sha512sum is included in many unix distributions.

    $ sha512sum apache-gora-X.Y.Z-src.tar.gz

output should match the string in apache-gora-X.Y.Z.tar.gz.sha512

## Previous Releases
If you are looking for previous releases of Apache Gora, have a look in the 
[Apache Archives](http://archive.apache.org/dist/gora/), or alternatively 
for even older releases check out the [Incubator archives](http://archive.apache.org/dist/incubator/gora/).

Subscribe to the dev@ [mailing list](./mailing_lists.html) if you want to 
get notified about future releases. You can check out the Gora 
[Quick Start Guide](./current/quickstart.html) to get up and running in no time. 
    
All Apache Gora releases are available under the Apache License, Version 2.0. 
See the NOTICE.txt file contained in each release artifact for applicable 
copyright attribution notices.
