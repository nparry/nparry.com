---
layout: post
date: 2012-01-15
title: Deploying Maven artifacts with SBT 0.11
---

I recently wanted to deploy some artifacts to a Maven repository via SSH
using SBT 0.11 but couldn't find a complete answer in one spot. This
probably has more to do with my sub-par Googling skills than lack of
answers in the world, but for future reference I got it to work by adding
the following to `build.sbt`:

<pre>
publishMavenStyle := true

publishTo <<= (version) { version: String =>
  val repoInfo = if (version.trim.endsWith("SNAPSHOT"))
      ( "nparry snapshots" -> "/home/nparry/repository.nparry.com/snapshots" )
    else
      ( "nparry releases" -> "/home/nparry/repository.nparry.com/releases" )
  val user = System.getProperty("user.name")
  val keyFile = (Path.userHome / ".ssh" / "id_rsa").asFile
  Some(Resolver.ssh(
    repoInfo._1,
    "repository.nparry.com",
    repoInfo._2) as(user, keyFile) withPermissions("0644"))
}
</pre>

With this addition in place, invoking `sbt publish` uploads the
project's JAR file along with a POM, source and javadoc bundles. Note
that I'm using a SSH key for authentication rather than having SBT
prompt me for a password.

This isn't quite as nice a solution as [what I found to work
with SBT 0.7](/2011/02/18/blamo.html), as extra tidbits like the
`maven-metadata.xml` files are not generated. However, the maven-sbt
plugin I used before seems not to have made the jump beyond SBT 0.7.

You can see the [change in
context](https://github.com/nparry/orderly4jvm/commit/d1482ceda30b99554c040a7cd9e1d9b7b956948a#diff-1)
at the Github page for the project in question. I pieced this together
from the [Publishing](https://github.com/harrah/xsbt/wiki/Publishing)
and [Resolvers](https://github.com/harrah/xsbt/wiki/Resolvers) pages of
the SBT wiki.

