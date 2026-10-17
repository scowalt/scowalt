# CI

## Native Linux routing

The workflow selects `ci_os: linux`, matching the supported Woodpecker 3.19
agent setting `WOODPECKER_AGENT_LABELS=ci_os=linux`. Before merging this change,
the parent must configure and read back that label and actual Linux ARM64
platform on the current agent, then verify the label and Linux AMD64 platform
on its replacement. Include no other OS or unsupported CPU; retain native platform
reporting. This routing label is not a security boundary or hardware attestation.
The original image index supports both targets, without a matrix or emulation.
ARM64 rollout guidance below is historical, not native AMD64 proof. Commands,
pins and status layout stay unchanged; actual AMD64 acceptance is separate.

This profile repository's `.woodpecker.yaml` ports the two existing comment-policy
commands from `.github/workflows/no-comments.yml`, in the same order. It retains
uv 0.11.32, Python 3.14, and the pinned `.comment-policy/requirements.txt`
dependencies in a digest-pinned image with Linux ARM64 and AMD64 manifests. Woodpecker's
standard clone supplies the Git index used by the checker. Local Lefthook checks
and policy exclusions are unchanged; there is no application build or publishing.

Unfiltered `push`, `tag`, and `pull_request` events cover the original Actions
branch pushes, tag pushes, and default PR events. There is no branch or path
filter. External fork code still requires operator approval; jobs need no secrets,
host mounts, privileged execution, or trust changes.

This is source preparation only, not evidence of hosted success. The Actions
workflow is retained byte-for-byte for rollback and must remain enabled until the
replacement is independently reviewed and verified on a PR and default `main`.
Activation and required-check changes belong to the operator. Preserve the original
10-minute job limit using Woodpecker's repository timeout setting: Woodpecker 3.19
does not expose a workflow/step timeout YAML field.

For local policy commands and hook installation, see
[the comment-policy guide](.comment-policy/README.md).
