## Search and review commits

Sourcegraph allows you to search code and code history in a single interface.  To find all TODOs in commit messages, add `type:commit` to your query.

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ type:commit TODO
```

## Search code in commits

You can also search the code in commits by setting the type to `diff`.

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ type:diff TODO
```

## Find code removed in a timeframe

By combining `before:` and `after:` filters, you can search for a range of time the code may have existed.

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ type:diff before:yesterday after:"last week" TODO
```

## Search commits for ONLY removed code

Try this example search which returns commits where todos were removed:

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ type:diff select:commit.diff.removed TODO
```

### Next notebook

[Refining queries to code you care about](./filter-by-file-language-and-other-elements-of-code.snb.md)
