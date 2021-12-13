# Find everywhere log4j is used across all your code

Run these queries on Sourcegraph to quickly determine which projects directly depend on vulnerable versions of log4j.
The following links show results on Sourcegraph Cloud across 2M public repositories.

## Gradle

```sourcegraph
org\.apache\.logging\.log4j' 2\.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14)(\.[0-9]+))
lang:gradle patterntype:regexp
```

## Maven

```sourcegraph
<log4j\.version>2\.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14)(\.[0-9]+))</log4j\.version>
file:pom\.xml patterntype:regexp
```

## Ivy

```sourcegraph
org="org\.apache\.logging\.log4j".*rev="2\.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14)(\.[0-9]+))"
file:ivy\.xml patterntype:regexp
```

## SBT (Scala)

```sourcegraph
"org.apache.logging.log4j" % "2.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14)(\.[0-9]+))
file:\.sbt$ patterntype:regexp
```

## Bazel

```sourcegraph
org\.apache\.logging\.log4j: 2.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14)(\.[0-9]+))
lang:bazel patterntype:regexp
```

## Any file containing `org.apache.logging.log4j` followed by a vulnerable version number

```sourcegraph
org\.apache\.logging\.log4j 2.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14)(\.[0-9]+))
patterntype:regexp
```
