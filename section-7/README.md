# Section 7 — Continuous Integration and Deployment with AWS

[Back to the repository guide](../README.md)

## Exercise

[frontend-gh-actions](frontend-gh-actions) contains frontend and GitHub Actions exercises associated with the course's CI/CD material. Its files are learning examples, not evidence of a completed AWS deployment.

## Starting Git after several exercises

It is fine to initialize Git after writing some code. The first commit records the current snapshot; subsequent commits track changes from that point onward.

Before committing, check ignored files and staged changes. Exclude dependencies, build output, `.env` files, and credentials. If the GitHub repository already has a README or license commit, reconcile that history rather than overwriting it.

## Workflow location in this repository

GitHub automatically discovers workflows only under **`.github/workflows/` at the repository root**.

The existing workflow inside `frontend-gh-actions/.github/workflows/` is nested in an exercise directory and is not automatically active from that location. The Vite exercise in section 6 also has a nested workflow example.

A root workflow can target one exercise:

```yaml
name: Fibonacci Client CI

on:
  push:
    paths:
      - "section-8/fibonacci-app/client/**"
      - ".github/workflows/fibonacci-client.yml"
  pull_request:
    paths:
      - "section-8/fibonacci-app/client/**"
      - ".github/workflows/fibonacci-client.yml"

jobs:
  test:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: section-8/fibonacci-app/client
    steps:
      - uses: actions/checkout@v4
      # Add Node setup, dependency installation, and tests here.
```

This is a location/filter example, not a complete test pipeline.

- `paths` controls which changed files trigger the workflow.
- `working-directory` selects the directory for shell `run` steps.
- Action inputs, such as a Docker build context, require their own paths; `working-directory` does not automatically change them.

Separate root workflows can target different sections while keeping the entire course in one repository.
