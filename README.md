# Test your github pull requests with Pantheon Multidev
Scripts that allow to build sites for your Pull Requests by using Pantheon Multidev as an environment for builds.

## Prerequisites

You will need:
* Pantheon subscription with one multi-dev environment
* Code is hosted with Github
* Account with CircleCI connected to your Github repo

## CircieCI set up

We have two main jobs in CircleCI configuration:
* Deploy every pull request to multi-dev environment
* Deploy master branch to dev environment of Pantheon

After deployment of pull request we trigger a job with Diffy. See /diffy-utls/runCheck.sh script for that.
