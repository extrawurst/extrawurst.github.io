---
title:  "Developing Atlassian Bitbucket plugins"
date:   2018-10-23 12:00:00
categories: [general]
---

## The Why

I wanted to use a slack-notifications plugin form the marketplace for our bitbucket server. Since we deal with a lot of repositories at work I needed it to expose its per-repository-settings via rest api. This allows us to programatically set settings for batches of repositories. Unfortunately this feature was not supported. So I forked the plugin and wanted to add this myself. 

The following is a short recap of the (sometimes) not so obvious steps to take to work on an Atlassian Bitbucket plugin.

* find the plugin here: [Stash2Slack on Atlassian Marketplace](https://marketplace.atlassian.com/apps/1213042/slack-notifications-plugin)
* find the source of this plugin here: [Stash2Slack Github](https://github.com/pragbits/stash2slack)

## The Setup (on mac)

### Install Prerequesists (see [Atlassian Docs](https://developer.atlassian.com/server/framework/atlassian-sdk/set-up-the-atlassian-plugin-sdk-and-build-a-project/))
    
* JDK 
    * check `javac -version` (tested with 1.8.0_121)
    * check `java -version`
* Atlassian SDK using brew `brew install atlassian/tap/atlassian-plugin-sdk`
    * check `atlas-version` (tested with 6.3.10)

### Run local BB server

* Start server process
    * run `atlas-run-standalone --product bitbucket`
    * *this runs a bitbucket server instance in FG (so you can overlook the output on the stdout)*
* Server state / clean server
    * `cd amps-standalone-bitbucket-LATEST/`
    * *this folder is used to persist the entire servers state, if you want a clean install, just delete this one*
* Build your plugin
    * run `atlas-package` where your plugins `pom.xml` is located
    * *this will run maven build scripts to create the plugins .jar*
* Install plugin in runnin server   
    * run `atlas-install-plugin` next to the `pom.xml` again
    * *this will install the plugin into the running bitbucket server instance*
* Debug logs
    * run `less amps-standalone-bitbucket-LATEST/target/bitbucket/home/log/atlassian-bitbucket.log`
    * *now you can search/follow the log for anything you print with log4j*

## Outlook

This was obviously just a quick overview but working with atlassian plugins seems fun and I found a couple of possible improvements I would like to tackle.

### Thread support to improve readability

For now the plugin dumps all updates into a single slack channel sequentially. As said we handle a lot of repositories and one thing I would like to integrate is thread support for those messages to have a cleaner overview (one thread per PR).

Unfortunately this is not that easy so it ended up in the backlog: The simple webhooks for sending slack messages does not support threading so this idea was a deadend unless I want to invest into a bigger more sophisticated implementation. 

Although I see a lot of opportunities to cleanup the current code that creates the webrequests manually when switching to [jslack](https://github.com/seratch/jslack) (a very nice java client lib for communicating with slack)

### Server State and Active objects

Another problem I came across is the flawed way settings are saved. This manifests in this plugin being very error prone when used in a bitbucket server setup with multiple nodes. 

It uses `PluginSettings` to store settings which are not synchronized between nodes and it should use `ActivityObjects` which is even stated in this forum thread: [here](https://community.atlassian.com/t5/Answers-Developer-Questions/JiraPluginSettings-with-Data-Center/qaq-p/529283) - too bad the docs for atlassian plugin development are rather lacking.

### Repo is dormant

My addition to the plugin in shape of a PR is completely ingnored now, no wonder since the repo was not updated in a year. The author clearly moved on. I would like to dig into how to distribute an Atlassian plugin to maybe supply this updated version to a broader audience. If only I had time...
