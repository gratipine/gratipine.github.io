---
title: "How to do a python CI pipeline on Github"
date: 2021-11-22 19:59:00 -0000
categories: how-to
---

## Overview
CI pipelines are a part of the famous duo CI/CD. The CI part stands for continuous intergation, meaning that there are automatic ways to integrate code from different developers in a single software product. This leads to faster development and a more robust code at the end of it. 

The way it works is that as soon as you push (or try to commit) your code automatic tests and checks are run on it. The checks could be:
- unit tests
- linting
- checks there are no passwords in the code
- mainainability checks
- part of a larger suite like [SonarQube](https://www.sonarqube.org/), for example.

## Why we do this
On an organizational scale we are doing this because writing code and deploying it on time can be hard. It can be made harder by having multiple people working on a project who need to communicate and ensure that changes to the source code do not lead to a crash when combined.

On a personal scale I do this even when working alone because automatic checks make life easier and allow me to focus on an analysis or an architecture, rather than on whether the code is following the right standards. If you are using a bigger tool, it will allow you to check for things like security risks and duplications in logic, which is helpful as well.

You can read a bit more on the philosophy of continuous integration [here](https://www.atlassian.com/continuous-delivery/continuous-integration).

## The CI part on github
### The UI way
In github the CI / CD actions can be performed using a tab called Actions. The tab is visible when you are at the root of your repository. Once you click on it there are several categories of actions you can perform. You are looking for Continuous integration. Depending on the structure of your repo and what languages you use you might have to click on View all to be able to see the one that fits your purposes. Assuming you are working with Python, you could do a Pylint action. If you click on Configure you will be taken to a page where a  

### The script way

## project showing how it works

## parting words
- this is just the integration prt, does not mean that we are done