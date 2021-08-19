# _conveyal/analysis-backend_ [v5.10.1](https://github.com/conveyal/analysis-backend/tree/v5.10.1)

**TL;DR** This effort was abandoned.

## Preface

At the time of writing, August 19, 2021, the latest tagged version of the
_analysis-ui_, [v4.12.0](https://github.com/conveyal/analysis-ui/tree/v4.12.0),
appears unable to interact with recent versions of the R5 backend.
This is not the case with recent untagged versions of the UI.

In an attempt to deploy a full Conveyal stack with official releases,
we built _conveyal/analysis-backend v5.10.1_
as it is from the same time _conveyal/analysis-ui v4.12.0_.

However, it seems running these legacy versions of the Conveyal projects
is unwise for the following reasons:

1. The _conveyal/analysis-backend_ README has the following [warning](https://github.com/conveyal/analysis-backend#this-repository-is-deprecated-it-has-been-merged-into-conveyals-main-routing-component-r5):

   > This repository is deprecated. It has been merged into Conveyal's main
   > routing component R5.

2. This version of the _conveyal/analysis-backend_ requires AWS S3 access.
   Recent versions of [R5](https://github.com/conveyal/r5) do not.

3. It seems likely Conveyal deploys non-tagged versions of the UI and tagged versions
   of the backend analysis tools for the following reasons:

   a. _conveyal/analysis-ui v4.12.0_ is from September 2019 while there have
   been steady commits to the project until July 2021.

   b. The latest tagged version of _R5_, [v6.4](https://github.com/conveyal/r5/tree/v6.4),
   is from May 2021.

   c. _conveyal/analysis-ui v4.12.0_ does not seem to work with recent _R5_ versions.

## Checking out the source code

```sh
git clone --branch v5.10.1 https://github.com/conveyal/analysis-backend.git analysis-backend.v5.10.1
cd analysis-backend.v5.10.1
checkout -b dev
```

See [conveyal/r5/issues/627](https://github.com/conveyal/r5/issues/627) for
an explanation of why the above is necessary.

## Building the Java JAR

### The build environment

```sh
$ git status .
On branch dev
nothing to commit, working tree clean
```

```sh
$ java -version
openjdk version "11.0.12" 2021-07-20
OpenJDK Runtime Environment 18.9 (build 11.0.12+7)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.12+7, mixed mode)
```

```sh
$ gradle --version
------------------------------------------------------------
Gradle 6.9
------------------------------------------------------------

Build time:   2021-05-07 07:28:53 UTC
Revision:     afe2e24ababc7b0213ccffff44970aa18035fc0e

Kotlin:       1.4.20
Groovy:       2.5.12
Ant:          Apache Ant(TM) version 1.10.9 compiled on September 27 2020
JVM:          11.0.12 (Oracle Corporation 11.0.12+7)
OS:           Linux 4.15.0-23-generic amd64
```

### The build command

```sh
gradle build shadowJar -x test
```

Running the compiled code results in errors because AWS S3 configuration is required.
We did not pursue this further.
