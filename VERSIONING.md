# Versioning Policy

> This document describes the Versioning Policy for [@workloads](https://github.com/workloads).

## Table of Contents

<!-- TOC -->
* [Versioning Policy](#versioning-policy)
  * [Table of Contents](#table-of-contents)
  * [Terraform Core](#terraform-core)
  * [Terraform Plugins](#terraform-plugins)
    * [Providers under the `hashicorp` Namespace:](#providers-under-the-hashicorp-namespace)
    * [Providers *not* under the `hashicorp` Namespace:](#providers-not-under-the-hashicorp-namespace)
  * [GitHub Actions](#github-actions)
  * [Notes](#notes)
<!-- TOC -->

The [@workloads](https://github.com/workloads) project aims to deliver code that results in a predictable and reproducible outcome.

Version-pinning is a foundational aspect of this goal.

## Terraform Core

> **Note**
>
> This section of the versioning policy applies to Terraform Core (e.g.: the `terraform` binary).

The versioning policy _must_ adhere to the following requirements:

* starts with the latest (generally) available minor version release (e.g.: `>= 1.9.0`)
* contains all possible patch-level releases under this range (e.g.: `1.9.1`)
* ends with excluding the next available minor version release (e.g.: `< 1.10.0`)

As an example, if the currently available version is `1.9.1`, then the following configuration is the _only_ acceptable version constraint for `terraform`:

```hcl
required_version = ">= 1.9.0, < 2.0.0"
```

Define this configuration item in the [`terraform` stanza](https://developer.hashicorp.com/terraform/language/settings) inside `terraform.tf`.

## Terraform Plugins

> **Note**
>
> This section of the version policy applies to Terraform Plugin (e.g.: the provider-specific binaries).

This section contains subsections outlining first-party (e.g.: providers under the `hashicorp/` namespace) and third-party providers (e.g.: providers _not_ under the `hashicorp/` namespace).

### Providers under the `hashicorp` Namespace:

First-party providers are eligible for _ranged upgrades_ inside the currently available major-version range.

The versioning policy _must_ adhere to the following requirements:

* starts with the latest (generally) available patch-level version release (e.g.: `>= 5.0.1`)
* contains all possible patch-level releases under this range (e.g.: `5.0.2`)
* ends with excluding the next available major version release (e.g.: `< 6.5.0`)

As an example, if the currently available version is `5.0.1`, then the following configuration is the _only_ acceptable version constraint for a provider:

```hcl
provider = {
  source  = "hashicorp/provider"
  version = ">= 5.0.1, < 6.0.0"
}
```

Define this configuration item in the `required_providers` block in the [`terraform` stanza](https://developer.hashicorp.com/terraform/language/settings) inside `terraform.tf`.

### Providers *not* under the `hashicorp` Namespace:

Third-party providers, whether partner- or community-maintained aren't considered as _acceptable_ for ranged upgrades.

The versioning policy _must_ adhere to the following requirements:

* starts with a specific patch-level version release (e.g.: `1.2.3`)
* contains no other configuration

As an example, if the currently available version is `1.2.3`, then the following configuration is the _only_ acceptable version constraint for a provider:

```hcl
provider = {
  source  = "third-party/provider"
  version = "1.2.3"
}
```

Define this configuration item in the `required_providers` block in the [`terraform` stanza](https://developer.hashicorp.com/terraform/language/settings) inside `terraform.tf`.

## GitHub Actions

> **Note**
>
> This section of the versioning policy applies to GitHub Actions.

The versioning policy _must_ adhere to the following requirements:

* starts with a specific patch-level version release (e.g.: `1.2.3`)
* contains no other configuration

As an example, if the currently available version is `1.2.3`, then the following configuration is the _only_ acceptable version constraint for `terraform`:

```hcl
action = {
  owner      = "action-owner"
  repository = "action-repo"
  version    = "v1.2.3"
}
```

Define this configuration item in the `actions_config` variable inside [`github-organization/variables.tf`](https://github.com/workloads/github-organization/blob/main/variables.tf).

Terraform [processes](https://github.com/workloads/github-organization/blob/main/actions.tf) the version constraint and retrieves the associated _committish_ to generate repository-specific Workflow templates.

## Notes

For tooling that's not explicitly described in this versioning policy, operate with the strictest constraint possible.
