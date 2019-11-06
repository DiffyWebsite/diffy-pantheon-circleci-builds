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


## Breakdown of steps on CircleCI config

`SITE_ENV` is a variable that is machine name of your Pantheon project and name of your multidev environment.

First we clone the code (checkout) and add ssh keys so we can connect to Pantheon

Next we push our commit's code to Pantheon's repository "pr" branch. For that we add Pantheon's git server to known_hosts so we are not asked to confirm it when push the code.

We have configured Pantheon automatically deploy "pr" branch code to multi-dev environment. You will probably need to set all your deployments steps there.

We install terminus. You can find usage of CircleCI cache there. More on this https://circleci.com/docs/2.0/caching/.

We expect that code already deployed to "pr" branch so we can clear caches on that environment.

As a last step we trigger visual regression testing `diffy-utils/runCheck.sh`.

It is very important to configure your private variables in CircleCI. These are API_KEY -- key from Diffy (can be optained under Account->Keys https://app.diffy.website/#/keys) and TERMINUS_MACHINE_TOKEN (https://pantheon.io/docs/machine-tokens).

## Breakdown of steps to trigger visual regression testing job

Some settings you will need to adjust:
* PROJECT_ID -- this is an ID of the project in Diffy
* environments:
ENV1URL -- test environment
ENVDEVURL -- dev environment
ENVPRURL -- multi-dev environment used for builds

First, script gets an authentication token from your api key. That is POST call to /api/auth/key.

Next, we trigger POST /api/projects/PROJECT_ID/compare call and pass all the variables along with commit sha.
