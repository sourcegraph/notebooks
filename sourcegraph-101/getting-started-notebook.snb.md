# Sourcegraph Introduction for XXXXX


What is Sourcegraph? Sourcegraph is a code intelligence platform. It enables developers to:

* find any code regardless of repository or language
* navigate symbol definitions and references across the entire codebase
* track and visualize migrations, code smells and versions with fully customizable dashboards
* automate and manage large scale code changes 

This is a Sourcegraph Notebook. It combines markdown with code search, symbol definitions and code snippets. The purpose of this notebook is to help get you started searching with Sourcegraph. We will use two of our search modes and use some of the most frequently used search filters.

## Admitted hacks and TODOs in our code

In the first instance we will use a literal search to find TODOs in our code.

```sourcegraph
-file:\.(json|md|txt)$ hack OR todo OR kludge OR fixme patterntype:literal
```

In this case we are excluding some file types from our search.


## Who else is using a particular package or library

Let us assume we are going to use Spring Boot security framework and we want to see where else this is used in our organization. The `select:repo` filter will return the only the repos that reference the Spring framework.

```sourcegraph
context:global import spring security select:repo patterntype:regexp
```

## Find Functions / Classes across the codebase

Let us try and find code connected to web authentication by searching for the pattern 'web auth' using a *fuzzy* match.

```sourcegraph
context:global web auth type:file patterntype:regexp
```

Let us limit our search to Java code only.

```sourcegraph
context:global  web auth lang:Java patterntype:regexp
```

And rather than search any content let us limit our search to symbols

```sourcegraph
context:global  web auth type:symbol lang:Java patterntype:regexp
```

### Code navigation

Select one of the symbols to load the source code. Then hover over the symbol with your mouse and hopefully you should get the option to find references and definitions of this symbol. Select one and Sourcegraph will search for that symbol across all our repos.

## Find private key and access tokens

```sourcegraph
context:global  (-----BEGIN [A-Z ]*PRIVATE KEY------)|(("gh|'gh)[pousr]_[A-Za-z0-9_]{16,}) case:yes patterntype:regexp
```

## Find recent dependencies

```sourcegraph
repo:github\.com/sourcegraph/ file:package.json type:diff after:"1 week ago"
```

## See all recent commits authored by "Carl"

We can search not only content but also commit messages and code diffs. Let us take a look at the code diffs authored by Carl. It uses regular expression to match Carl with author names.

```sourcegraph
type:diff author:"carl"
```
We can also specify an interval for this query as well

```sourcegraph
type:diff author:"carl" after:"6 months ago"
```
Finally let us search the commit messages authored by Carl which include the pattern bug fix

```sourcegraph
type:commit author:"carl" fix bug patterntype:regexp
```
