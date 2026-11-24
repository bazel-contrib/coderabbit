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

### Opt out while keeping GitHub App access

Commit this V2 `.coderabbit.yaml` at your repository root. Replace any existing
configuration with it; `inheritance: false` prevents shared and web UI settings
from filling in values. No GitHub App access changes are needed.

```yaml
# yaml-language-server: $schema=https://coderabbit.ai/integrations/schema.v2.json
inheritance: false
reviews:
  auto_review:
    enabled: false
    auto_incremental_review: false
    description_keyword: ""
    labels: []
  path_filters:
    - "!**/*"
  disable_cache: true
  high_level_summary: false
  review_status: false
  review_progress: false
  commit_status: false
  in_progress_fortune: false
  suggested_labels: false
  auto_apply_labels: false
  suggested_reviewers: false
  auto_assign_reviewers: false
  auto_approvals:
    enabled: false
  slop_detection:
    enabled: false
  post_merge_actions: []
  pre_merge_checks:
    title:
      mode: "off"
    description:
      mode: "off"
    issue_assessment:
      mode: "off"
    custom_checks: []
  finishing_touches:
    unit_tests:
      enabled: false
    simplify:
      enabled: false
    autofix:
      enabled: false
    fix_ci:
      enabled: false
    resolve_merge_conflict:
      enabled: false
    custom: []
chat:
  auto_reply: false
  allow_non_org_members: false
  integrations:
    jira:
      usage: disabled
    linear:
      usage: disabled
knowledge_base:
  opt_out: true
  automatic_linking_mode: disabled
  linked_repositories: []
  code_guidelines:
    enabled: false
  jira:
    usage: disabled
  linear:
    usage: disabled
  web_search:
    enabled: false
  mcp:
    usage: disabled
issue_enrichment:
  auto_enrich:
    enabled: false
  planning:
    enabled: false
    auto_planning:
      enabled: false
  labeling:
    auto_apply_labels: false
```

This disables automatic reviews, excludes all review files, suppresses review
status/summary output, and disables the listed chat, issue, and code-writing
features. Knowledge base opt-out also permanently erases this repository's
stored learnings after the next default-branch merge on GitHub Cloud.
See the [V2 settings reference](https://docs.coderabbit.ai/reference/configuration).

Merge the file into your default branch. Existing PRs may need their branches
updated, depending on the repository's
[configuration source](https://docs.coderabbit.ai/configuration/file).
If CodeRabbit is a required merge check, arrange to remove that requirement
before disabling its status output.

YAML has no master off-switch: explicit mentions/commands can still receive
responses, app access remains, and separately configured products or automations
are outside this file's scope. Enforced global overrides can also override
these settings.

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
