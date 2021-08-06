An interactive version of driftctl's [contributor guide](https://github.com/cloudskiff/driftctl/blob/main/docs/README.md).

## Developer guide
This directory contains some documentation about the driftctl codebase, aimed at readers who are interested in making code contributions.

- [Add new resources](https://github.com/cloudskiff/driftctl/blob/main/docs/new-resource.md)
- [Testing](https://github.com/cloudskiff/driftctl/blob/main/docs/testing.md)

## Core concepts

driftctl uses Terraform providers besides cloud providers SDK to retrieve data.

Resource listing is done using cloud providers SDK. Resource details retrieval is done by calling terraform providers with gRPC.

## Terminology

`Scanner` is used to scan multiple cloud providers and return a set of resources, it calls all declared Supplier

```sourcegraph
repo:^github\.com/cloudskiff/driftctl$ Scanner
```

`Remote` is a representation of a cloud provider

```sourcegraph
repo:^github\.com/cloudskiff/driftctl$ Remote
```

`Resource` is an abstract representation of a cloud provider resource (e.g. S3 bucket, EC2 instance, etc ...)

```sourcegraph
repo:^github\.com/cloudskiff/driftctl$ Resource
```

`ResourceSupplier` is used to list resources from a given type on a given remote and return a resource list, it should exist only one ResourceSupplier per resource

```sourcegraph
repo:^github\.com/cloudskiff/driftctl$ ResourceSupplier
```
