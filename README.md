# bazel-contrib CodeRabbit configuration

[.coderabbit.yaml](.coderabbit.yaml) holds the shared CodeRabbit defaults for
`bazel-contrib` repositories.

## Setup

Following [CodeRabbit's central configuration documentation](https://docs.coderabbit.ai/configuration/central):

Note, Central configuration requires schema V2. Do not migrate this file to V3.

## Repository configuration

Repositories without their own configuration file use these shared defaults.
A repository with its own configuration replaces the central file unless it
sets `inheritance: true` to merge the shared defaults with its local settings.
These are shared defaults, not enforced organization overrides.

### Configuration priority

For review settings, the priority order is highest first:

1. The repository's own `.coderabbit.yaml` (or TypeScript configuration).
2. The shared configuration in `bazel-contrib/coderabbit`.
3. The repository's settings in the CodeRabbit web UI.
4. The organization's defaults in the CodeRabbit web UI.
5. CodeRabbit's built-in defaults.

By default, the highest available layer supplies the whole configuration;
missing values use built-in defaults. Set `inheritance: true` to fill missing
values from lower layers. Explicit values in higher layers win. Inheritance
stops after a layer with inheritance off, so enabling it in a repository file
does not automatically include every web UI setting. This shared file currently
leaves inheritance off.

Repository web UI settings participate only when **Use organization settings**
is off. Saving the UI form stores default values too; use its YAML editor to
save only the settings you intend. Enforced global overrides take precedence
over these layers. See [inheritance and merging](https://docs.coderabbit.ai/configuration/inheritance-and-merging).

### Disable CodeRabbit completely for your repository

Ask a `bazel-contrib` organization owner to remove your repository from the
CodeRabbit GitHub App's repository access:

1. Open the organization's GitHub **Settings → GitHub Apps → CodeRabbit → Configure**.
2. Under **Repository access**, remove your repository from the selected list
   and save. If access is set to **All repositories**, the owner must switch to
   **Only select repositories**, preserving access for the other participating
   repositories, including `bazel-contrib/coderabbit`.

Do not suspend or uninstall the organization's app to disable a single repo;
those actions affect the whole organization. See
[GitHub's repository access instructions](https://docs.github.com/en/apps/using-github-apps/reviewing-and-modifying-installed-github-apps#modifying-repository-access).

Setting `reviews.auto_review.enabled: false` only disables automatic reviews;
it does not remove app access or disable all CodeRabbit interactions.

### Extend the shared configuration

Add `.coderabbit.yaml` at your repository root with `inheritance: true`. For
example, this keeps the shared defaults while adding draft reviews and
Bazel-specific review guidance:

```yaml
# yaml-language-server: $schema=https://coderabbit.ai/integrations/schema.v2.json
inheritance: true
reviews:
  auto_review:
    drafts: true
  path_instructions:
    - path: "**/*.bzl"
      instructions: >-
        Check that rule inputs and toolchains are declared explicitly and
        actions do not depend on undeclared host tools or environment variables.
```

See the [V2 configuration reference](https://docs.coderabbit.ai/reference/configuration)
for path filters, review profiles, automatic review controls, tool settings,
and other options. Keep this example on V2 when inheriting our V2 shared config.
After a review, comment `@coderabbitai configuration` on the PR to check the
effective settings and their sources.

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
