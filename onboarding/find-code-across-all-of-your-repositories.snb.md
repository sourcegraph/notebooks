## Find and reference code across all your repositories

**Problem:** opening multiple repositories in separate IDEs wastes cycles, while command line search tools offer poor syntax highlighting and code browsing.  

We love Sourcegraph because searching all our repositories is incredibly easy and allows us to explore code much faster. To search an org or user’s repos use glob like syntax:

```sourcegraph
repo:^github\.com/sourcegraph/.* Config()
```

## Search a single repository

An easy way to search a single repository is to type the org and repo name and and use the regexp metacharacter `$` to match the exact repository name.

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ Config()
```

## Search branches and tags

The rev filter allows us to search various branches. For example, all Sourcegraph releases tagged 3.2 and higher can be searched with:

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ rev:*refs/heads/3.2* Config()
```

### Next notebook

[Search and review diffs 10x faster than git log and grep](./search-and-review-commits.snb.md)
