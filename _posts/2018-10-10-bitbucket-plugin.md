---
title:  "Developing Atlassian Bitbucket plugins"
date:   2018-10-10 12:00:00
categories: [general]
---

## The Why

At work we are using bitbucket server for our sourcecode and slack for our communication.
My team at InnoGames is responsible for a long list of repositories and is heavily utilizing pull-requests to do code-review.
PRs were lying around a lot unless authors pro-actively asked for reviews.

We wanted to automate this better and found a bitbucket plugin that forwards pullrequest reviews to slack: Stash2Slack ([see in Marketplace](https://marketplace.atlassian.com/apps/1213042/slack-notifications-plugin)).

Unfortunately Stash2Slack turned out not to have a rest api to have its settings configured which is a requirement for is since we have automation of settings configuration on our multitude of repositories (we checked the [sourcecode on github](https://github.com/pragbits/stash2slack)).

I decided to add this feature to the plugin and this post is a summary on steps necessary to develop a bitbucket plugin that I wish I had when I started.

## The Setup

* Prerequesists (see [Atlassian Docs](https://developer.atlassian.com/server/framework/atlassian-sdk/set-up-the-atlassian-plugin-sdk-and-build-a-project/))
    * JDK
    * Atlassian SDK using brew `brew install atlassian/tap/atlassian-plugin-sdk`
* `atlas-run-standalone --product bitbucket`
    * runs in FG
    * uses folder to persist
* `atlas-package`
* `atlas-install-plugin`
* follow log file

## Next Steps

* thread support (jslack)
* active objects
