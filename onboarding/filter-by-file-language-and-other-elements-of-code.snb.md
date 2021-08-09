## Quickly filter by file path and language

Code search often returns files that aren’t relevant. Soucegraph allows users to filter by file path and language to narrow down results:

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ Config() lang:Go
```

There are many other filters available, documented [here](https://docs.sourcegraph.com/code_search/reference/queries#keywords-all-searches)

## Removing unwanted files

Filters like `repo`, `lang` and `file` can be negated to remove files in directories unrelated to your query. *Tip:* the dynamic filters in the search sidebar often auto generate these filters for you. You can open the query in a new tab to discover dynamic filters.

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ Config() lang:go -file:/tests/* -file:/docs/*
```

## OR expressions + filters

Try this example search to span the frontend and backend divide:

```sourcegraph
repo:^github\.com/sourcegraph/sourcegraph$ Config() (lang:Go OR lang:TypeScript) -file:/tests/* -file:/docs/*
```
