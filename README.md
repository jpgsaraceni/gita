# Gita

![Gita](https://user-images.githubusercontent.com/80982137/155199653-32391000-56e1-4c2f-90a5-ff9afc9ec558.jpg)

GitHub Actions practice repository.

Basic concepts. Built following [GitHub's tutorial](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions).

## Index

* [What are GitHub Actions](#whats-this)
* [Key Terms](#key-terms)
  * [Runner](#runner)
  * [Workflow](#workflow)
  * [Job](#job)
  * [Actions](#actions)
* [Workflow YAML File](#workflow-yaml-file)
* [Customizing Workflows](#customizing-workflows)
  * [Env Variables](#env-variables)
  * [Scripts](#scripts)
  * [Artifacts](#artifacts)

## What's this?

GitHub Actions is a CI/CD tool, that allows you to set up workflows to be run automagically, whenever a trigger you set goes off (like whenever you push to the repository). Its main use is to automate builds, tests and deployments of your app.

## Key Terms

### Runner

A virtual machine that is used to run the *things* (jobs) defined in your workflow.

### Workflow

A workflow is the set of *things* (jobs) you want to happen when *other things* (triggers) happen.

### Job

Workflows define one or more jobs, which contain shell script or an *Action*. They are basically commands that will run in your runner.

### Actions

Predefined workflow, by you or by the community, for common, repetitive workflows.

## Workflow YAML file

The workflow is configured in a YAML file. The basic syntax is as follows:

```yaml
name: name-to-appear-in-repository-Actions-tab
on: [push] # on creates a trigger to run the workflow. [push] means the action should run on every push to every branch
jobs: # groups all jobs in this workflow
  name-of-a-job-you-are-defining:
    runs-on: ubuntu-latest # defines the runner for the job. In this case, ubuntu
    steps: # groups all steps (actions or shell scripts) for the job
      - uses: actions/checkout@v2 # calls the v2 of actions/checkout. Should be used whenever a workflow runs against repository code
      # the @{version} accepts a version tag, a version SHA or a branch name.
      - uses: actions/setup-node@v2 # node setup community defined action.
      - uses: ./.github/actions/directory-with-user-defined-actions # user defined action.
      # note that ./ refers to the root of the repository.
      - uses: docker://alpine3.8 # to get an Action defined on a Docker Hub image
        with:
          node-version: '14' # installs node on the runner
      - run: npm install -g bats # run a command in runner. In this case, install a dependency
      - run: bats -v # checks the version of bats installed, to check if it was successfully installed.
```

## Customizing Workflows

### Env Variables

You can add environment variables to your workflow by adding an `env` field in the `steps` of a job:

```yaml
jobs:
  example-job:
      steps:
        - name: Connect to PostgreSQL
          run: node client.js
          env:
            POSTGRES_HOST: postgres
            POSTGRES_PORT: 5432
```

### Scripts

Some scripts you can run directly (like the npm script in the example). To use shell scripts for example, add use the `run` keyword followed by the path to the script, and `shell` followed by the name of the shell you wish to run on:

```yaml
jobs:
  example-job:
    steps:
      - name: Run build script
        run: ./.github/scripts/build.sh
        shell: bash
```

### Artifacts

Artifacts are files created during builds or tests in a workflow, and are accessible by all jobs within a same workflow. Actions and workflows called inside a run have write access to its artifacts.

The following example creates an artifact (output.log) using a shell command and accesses it from another action in the same job (upload-artifact):

```yaml
jobs:
  example-job:
    name: Save output
    steps:
      - shell: bash
        run: |
          expr 1 + 1 > output.log
      - name: Upload output file
        uses: actions/upload-artifact@v2
        with:
          name: output-log-file
          path: output.log
```
