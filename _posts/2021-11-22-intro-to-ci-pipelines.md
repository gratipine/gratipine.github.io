---
title: "How to create a python CI pipeline on Github"
date: 2022-03-12 19:59:00 -0000
categories: how-to
# https://www.pexels.com/photo/person-using-silver-macbook-pro-1181467/
---

## Overview
CI pipelines are a part of the famous duo CI/CD. The CI part stands for continuous integration, meaning that there are automatic ways to integrate code from different developers in a single software product. This leads to faster development and code that is more robust. 

It works by running automatic tests and checks on the code as soon as you push. This can be configured so that the checks are run before you commit as well. The checks could be:
- unit tests
- linting
- checks there are no passwords in the code
- maintainability checks
- part of a larger suite like [SonarQube](https://www.sonarqube.org/), for example.

## Why we do this
On an organizational scale we are doing this because writing code and deploying it on time can be hard. It can be made harder by having multiple people working on a project who need to communicate and ensure that changes to the source code do not lead to a crash when combined.

On a personal scale I do this even when working alone because automatic checks make life easier and allow me to focus on an analysis or an architecture, rather than on whether the code is following the right coding standards or if all tests still pass. If you are using a bigger tool, it will allow you to check for things like security risks and duplications in logic, which is helpful as well.

You can read a bit more on the philosophy of continuous integration [here](https://www.atlassian.com/continuous-delivery/continuous-integration).

## CI on github
### The UI way
In github the CI / CD actions can be performed using a tab called Actions. The tab is visible when you are at the root of your repository. Once you click on it there are several categories of actions you can perform. You are looking for Continuous integration. Depending on the structure of your repo and what languages you use you might have to click on View all to be able to see the one that fits your purposes. Assuming you are working with Python, you could do a Pylint action. If you click on Configure you will be taken to a page where you can edit a yml file like the one below.

### The script way
To use a workflow github expects a file like below at this address - .github/workflows/filename.yml .

The file below specifies that an action called Pylint will run every time you push to the directory. For example, this one will run against the latest ubuntu image available on github. It will also do tests against three versions of Python - 3.8, 3.9 and 3.10. For each of the versions it will install pylint and run it on any python file within the repo.

#### Specifying what triggers the workflow
There are various settings for when the checks (and other actions) can run:
- on push
- when opening or editing a pull request
- when deploying

You can read the full documentation [here](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows). Below you can see the config for running something whenever a pull request is opened or reopened. 
```bash
on:
  pull_request:
    types: [opened, reopened]
```
#### A script for linting
You can also see it as a github gist [here](https://gist.github.com/gratipine/ff13a64bc884669179b24488c06ce4dd).

```yml
# Name of the action. This will show up when it is being run as a part of a workflow.
name: Pylint

# When it will get run. 
on: [push]

jobs:
  build:
  # what platform the workflow runs on
    runs-on: ubuntu-latest
    strategy:
      matrix:
        # list of Python versions we will test against
        python-version: ["3.8", "3.9", "3.10"]
    steps:
    - uses: actions/checkout@v2
    - name: Set up Python ${{ matrix.python-version }}
        # set up the python environment
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install dependencies
      # install the python packages needed
      run: |
        python -m pip install --upgrade pip
        pip install pylint
    - name: Analysing the code with pylint
     # lint all the the python files
      run: |
        pylint $(git ls-files '*.py')
```
## Example project
You can find an example project with a pipeline [here](https://github.com/gratipine/ci_example). It contains a similar pipeline, with a slightly different environment set up and an example of how to do tests. In [this page](https://github.com/gratipine/ci_example/actions) you can see it took me 7 tries of doing silly things before the pipeline finally passed. 

## Conclusion
This is just an example of running some automatic checks on the code. There are many more things that you can do with the code, including integrate it with more repos and automatically deploy to a place of your choice. 

## Sources
- [Github docs](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
- [Atlassian explanations of github](https://www.atlassian.com/continuous-delivery/continuous-integration)
- [Example Python repo with workflow set up](https://github.com/gratipine/ci_example)