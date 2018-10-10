---
title:  "Developing Atlassian Bitbucket plugins"
date:   2018-10-10 12:00:00
categories: [general]
---

## The Why

* [Stash2Slack Github](https://github.com/pragbits/stash2slack)
* [Stash2Slack on Atlassian Marketplace](https://marketplace.atlassian.com/apps/1213042/slack-notifications-plugin)

## The Setup

* Prerequesists (see [Atlassian Docs](https://developer.atlassian.com/server/framework/atlassian-sdk/set-up-the-atlassian-plugin-sdk-and-build-a-project/))
    * JDK
    * Atlassian SDK using brew `brew install atlassian/tap/atlassian-plugin-sdk`
* `atlas-run-standalone --product bitbucket`
    * runs in FG
    * uses folder to persist
* `atlas-package`
* `atlas-install-plugin`
* `less amps-standalone-bitbucket-LATEST/target/bitbucket/home/log/atlassian-bitbucket.log`

## Next Steps

* thread support (jslack)
* active objects
