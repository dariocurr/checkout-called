# checkout-called

Checkout the called repository at the same reference it was invoked with

## Why

Following the discussion [here](https://github.com/actions/toolkit/issues/1264),
since evidently there is not enough [dogfeeding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) happening at @github,
I created `checkout-called` to simplify checking out the called repository in your [reusable workflows](https://docs.github.com/en/actions/sharing-automations/reusing-workflows)

## Usage

> This action requires `id-token: write` permissions

To use this action in your called workflows and check out the repository automatically, add the following lines:

```yml
- name: Checkout called repo
  uses: dariocurr/checkout-called@main
```

This action mirrors the arguments of [`actions/checkout`](https://github.com/actions/checkout), except for `ref` and `repository` that are injected automatically based on the call.

For example, if you call: `user/repository/.github/workflows/workflow.yml@branch`, it will checkout the `user/repository` repository using `branch` as reference

> This repository is committed to maintaining compatibility and same version as [`actions/checkout`](https://github.com/actions/checkout)

## Limitations

This action relies on the OIDC token (`ACTIONS_ID_TOKEN_REQUEST_TOKEN`) to read
the `job_workflow_ref` claim, which is the only reliable way to discover the
repository and ref of the *called* reusable workflow at runtime.

GitHub does **not** mint an OIDC token for workflows triggered by
[`pull_request` from a forked repository](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect),
for the same security reason that secrets are not exposed to those runs.
As a result, this action cannot work in that scenario and will exit with a
clear error.

Possible workarounds (need to be tested properly):

- Use [`pull_request_target`](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#pull_request_target)
  instead of `pull_request`, with the usual fork-safety caveats — do not check
  out untrusted PR code with elevated permissions.
- Use [`workflow_run`](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#workflow_run)
  to react to fork PR builds from the upstream repository's context.

See [#54](https://github.com/dariocurr/checkout-called/issues/54) for context.

## Versioning and tags

Tags (for example `v6.0.2`) may move to stay aligned with
[`actions/checkout`](https://github.com/actions/checkout)
for the same release line.
This is intentional and not an error.

If you need a fully immutable reference for automation,
pin to a commit SHA instead of a tag.
