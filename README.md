# bazel-contrib CodeRabbit configuration

[.coderabbit.yaml](.coderabbit.yaml) holds the shared CodeRabbit defaults for
`bazel-contrib` repositories: English reviews, the `chill` review profile, and
automatic reviews enabled. Other settings retain CodeRabbit's defaults.

## Setup

Following [CodeRabbit's central configuration documentation](https://docs.coderabbit.ai/configuration/central):

1. Keep this repository named exactly `bazel-contrib/coderabbit`.
2. Keep `.coderabbit.yaml` at the root of the default branch (`main`).
3. Grant the CodeRabbit GitHub App access to this repository so it can read the
   configuration. CodeRabbit also needs access to repositories it reviews.

Central configuration requires schema V2. Do not migrate this file to V3.

## Repository configuration

Repositories without their own configuration file use these shared defaults.
A repository with its own configuration replaces the central file unless it
sets `inheritance: true` to merge the shared defaults with its local settings.
These are shared defaults, not enforced organization overrides.

CodeRabbit also takes code guideline files from the central repository when a
repository uses central configuration. Keep repository-specific guidance in
repositories with their own configuration.

## Verify activation

On a pull request in a repository without its own configuration, comment:

```text
@coderabbitai configuration
```

Check that the reported configuration source is this repository's file.
Publishing this repository alone does not verify GitHub App access or activation.

## Changes

Edit `.coderabbit.yaml` through a pull request. Its first line enables YAML
schema validation in compatible editors.
