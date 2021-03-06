Title: Automated Nightly Builds

We're using the Apache Jenkins build server for continuous builds.

[Trunk build](http://builds.apache.org/job/Gora-trunk/)
* This build is set up to check SVN for updates every 60 mins but also 
  builds nightly regardless.
* This builds SNAPSHOT's which can be obtained [here](https://repository.apache.org/content/repositories/snapshots/org/apache/gora/). 

Depending on the nature of the ongoing development, it is common to 
have other branch builds ongoing. You can see all of the Gora builds
[here](https://builds.apache.org/view/G-L/view/Gora/).

Email notifications to commits@gora.apache.org are generated by Jenkins
for build failures (and when builds return to normal).

If you're interested in the Jenkins services or would like to request an
account on the server, check out the
[Jenkins wiki](http://wiki.apache.org/general/Hudson) for more information.
