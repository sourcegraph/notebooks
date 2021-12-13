# Find everywhere log4j is used across all your code

Run these queries on Sourcegraph to quickly determine which projects directly depend on vulnerable versions of log4j.
The following links show results on Sourcegraph Cloud across 2M public repositories.

## How to use this notebook on your Sourcegraph instance

- _Ensure your Sourcegraph instance version is at least 3.31 and enable notebooks in experimental settings `experimentalFeatures.showSearchNotebook`_
- <a href="https://sourcegraph.com/github.com/sourcegraph/notebooks@rn/log4j-notebook/-/raw/log4j.snb.md" target="_blank">Download</a> this notebook as a Markdown file
- Add the Markdown file to a branch in your repository and open it in Sourcegraph
- Customize the searches below to target specific repositories or keep them as-is to search across all repositories
- Keep a list of file blocks to track your progress on the files you need to update

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

## Example: a list of files to update

https://sourcegraph.com/github.com/SonarSource/sonarqube@601e7fbb0ca7cd323b69742e15cd016dac46cf62/-/blob/build.gradle?L367

https://sourcegraph.com/github.com/projectlombok/lombok@37d548c7fa1db5284227a861089857fc20de06ce/-/blob/buildScripts/ivy.xml?L48
