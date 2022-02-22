# Gita
![Gita](https://user-images.githubusercontent.com/80982137/155199653-32391000-56e1-4c2f-90a5-ff9afc9ec558.jpg)

GitHub Actions practice repository.

Basic concepts. Built following [GitHub's tutorial](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions).

## What's this?

GitHub Actions is a CI/CD tool, that allows you to set up workflows to be run automagically, whenever a trigger you set goes off (like whenever you push to the repository). Its main use is to automate builds, tests and deployments of your app.

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
      - uses: actions/setup-node@v2
        with:
          node-version: '14' # installs node on the runner
      - run: npm install -g bats # run a command in runner. In this case, install a dependency
      - run: bats -v # checks the version of bats installed, to check if it was successfully installed.
