# Find log4j dependencies across all your code

Run these queries on Sourcegraph to quickly determine which projects directly depend on vulnerable versions of log4j.
The following notebook contains search queries that identify vulnerable dependencies on Sourcegraph Cloud across 2M public repositories.

## How to use this notebook on your Sourcegraph instance

- Ensure your Sourcegraph instance version is at least 3.31 and enable notebooks in global settings with `{ "experimentalFeatures": { "showSearchNotebook": true }}`
- Copy the Markdown formatted notebook and save it to a file with a `.snb.md` extension (<a href="?view=code">Markdown code</a>)
- Customize the searches below to target specific repositories or keep them as-is to search across all repositories
- You can keep a <a href="#example-a-list-of-files-to-update">list of file blocks</a> to track your progress on the files you need to update
- Add the notebook file to a branch in your repository and open it in Sourcegraph
- Share the link to the notebook with your team
- Update found log4j dependencies until all search queries return no results!

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

https://sourcegraph.com/github.com/lagom/lagom@12e8ee61f0f4c3a3658fd569b5562c0de2b292c5/-/blob/docs/manual/java/guide/logging/code/build-log-lang.sbt?L27

### Writing Sourcegraph Notebook Markdown

You can write regular Markdown-formatted text with interleaved interactive queries and file blocks.
Query blocks are written as code blocks with the following format:

````
```sourcegraph
org\.apache\.logging\.log4j' 2\.((0|1|2|3|4|5|6|7|8|9|10|11|12|13|14)(\.[0-9]+))
lang:gradle patterntype:regexp
```
````

File blocks are represented as simple Sourcegraph links to files:

```
https://sourcegraph.com/github.com/SonarSource/sonarqube/-/blob/build.gradle?L367
```
