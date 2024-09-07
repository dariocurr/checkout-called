# checkout-called

Checkout the called repository at the same reference it was invoked with

## Why

Following the discussion [here](https://github.com/actions/toolkit/issues/1264),
since evidently there is not enough [dogfeeding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) happening at @github,
I created `checkout-called` to simplify checking out the called repository in your [reusable workflows](https://docs.github.com/en/actions/sharing-automations/reusing-workflows)

## Usage

To use this action in your called workflows and check out the repository automatically, add the following lines:

```yml
- name: Checkout called repo
  uses: dariocurr/checkout-called@main
```

This action mirrors the arguments of [`actions/checkout`](https://github.com/actions/checkout), except for `ref` and `repository` that are injected automatically based on the call.

For example, if you call: `user/repository/.github/workflows/workflow.yml@branch`, it will checkout the `user/repository` repository using `branch` as reference

> This repository is committed to maintaining compatibility and same version as [`actions/checkout`](https://github.com/actions/checkout)

###  Private repository

According to the GitHub API [documentation](https://docs.github.com/en/rest/actions/workflow-runs?apiVersion=2022-11-28#get-a-workflow-run),
for private repositories, you need to provide a token with `Actions: read` permissions.
To do this, pass the token as an argument:

```yml
- name: Checkout called repo
  uses: dariocurr/checkout-called@main
  with:
    token: ${{ secrets.TOKEN }}
 ```
