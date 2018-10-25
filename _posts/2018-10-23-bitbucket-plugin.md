---
title:  "Developing Atlassian Bitbucket plugins"
date:   2018-10-23 9:00:00
categories: [general]
---

At work we are using bitbucket server for our source code and slack for our communication.
The following gives you an insight into how to make a bitbucket plugin that talks to Slack.

![marketplace]({{ site.url }}/assets/stash2slack-marketplace.png){: .center-image }

## The Why

My team at InnoGames is responsible for a long list of repositories and is heavily utilizing pull-requests to do code-review.

PRs were lying around a lot unless authors pro-actively asked for reviews. So we wanted to automate this better and found a bitbucket plugin that forwards pull-request reviews to slack: Stash2Slack ([see in Marketplace](https://marketplace.atlassian.com/apps/1213042/slack-notifications-plugin)).

Unfortunately Stash2Slack turned out not to have a rest api to have its settings configured which is a requirement for us since we have automation of settings configuration on our multitude of repositories (we checked the [source code on github](https://github.com/pragbits/stash2slack)).

I decided to add this feature to the plugin and this post is a summary on steps necessary to develop a bitbucket plugin that I wish I had when I started.

The following is a short recap of the (sometimes) not so obvious steps to take to work on an Atlassian Bitbucket plugin.

* find the plugin here: [Stash2Slack on Atlassian Marketplace](https://marketplace.atlassian.com/apps/1213042/slack-notifications-plugin)
* find the source of this plugin here: [Stash2Slack Github](https://github.com/pragbits/stash2slack)

## The Setup (on mac)

Three simple steps to get up and running:

* Install prerequisites
* Run local BB server
* Build/Deploy/Test your plugin

### Install Prerequisites (see [Atlassian Docs](https://developer.atlassian.com/server/framework/atlassian-sdk/set-up-the-atlassian-plugin-sdk-and-build-a-project/))
    
* JDK 
    * check `javac -version` (tested with 1.8.0_121)
    * check `java -version`
* Atlassian SDK using brew `brew install atlassian/tap/atlassian-plugin-sdk`
    * check `atlas-version` (tested with 6.3.10)

### Run local BB server

* Start server process
    * run `atlas-run-standalone --product bitbucket`
    * *this runs a bitbucket server instance in foreground (so you can overlook the stdout)*
* Server state / clean server
    * `cd amps-standalone-bitbucket-LATEST/`
    * *this folder is used to persist the entire servers state, if you want a clean install, just delete this one*
* Debug logs
    * run `less amps-standalone-bitbucket-LATEST/target/bitbucket/home/log/atlassian-bitbucket.log`
    * *now you can search/follow the log for anything you print with log4j*

### Run your plugin

* Build your plugin
    * run `atlas-package` where your plugins `pom.xml` is located
    * *this will run maven build scripts to create the plugins .jar*
* Install plugin in running server   
    * run `atlas-install-plugin` next to the `pom.xml` again
    * *this will install the plugin into the running bitbucket server instance*

## The actual change

Now the actual change was pretty simple:
* add a rest description to the plugin manifest
* add a standard spring style REST endpoint

See the manifest changes here:

```xml
<rest key="slack-rest" path="/stash2slack" version="1.0">
    <description>Stash2Slack Rest API</description>
</rest>
```

See an example REST endpoint here:
```java
@Path("/settings")
public class RestResource {

    ...
    
    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public Response getGlobalSettings() {
        validate(() -> validationService.validateForGlobal(Permission.ADMIN));

        return Response.ok(getDTOFromGlobalSettings(globalSettingsService)).build();
    }
}
```

Find the entire changeset [here on github](https://github.com/pragbits/stash2slack/pull/71/files).

## Outlook

This was obviously just a quick overview but working with atlassian plugins seems fun and I found a couple of possible improvements I would like to tackle.

### Debug local instance

For my testing so far I did not need to connect via debugger right into my plugin but I can see how knowing how is beneficial.

So here is an explanation on how to run your local instance to be able to debug it (on port 8000): [Run in debug mode](https://scriptrunner.adaptavist.com/4.3.5/bitbucket/DevEnvironment.html#_b_new_app_with_a_href_https_developer_atlassian_com_docs_developer_tools_working_with_the_sdk_command_reference_atlas_run_standalone_atlas_run_standalone_a)

And here you find how to configure your IntelliJ to be able to connect to it: [Setup IntelliJ](https://scriptrunner.adaptavist.com/4.3.5/bitbucket/DevEnvironment.html#_3_create_debug_configuration_in_idea)

### Thread support to improve readability

For now the plugin dumps all updates into a single slack channel sequentially. As said we handle a lot of repositories and one thing I would like to integrate is thread support for those messages to have a cleaner overview (one thread per PR).

Unfortunately this is not that easy so it ended up in the backlog: The simple webhooks for sending slack messages does not support threading so this idea was a deadend unless I want to invest into a bigger more sophisticated implementation. 

Although I see a lot of opportunities to cleanup the current code that creates the webrequests manually when switching to [jslack](https://github.com/seratch/jslack) (a very nice java client lib for communicating with slack)

### Server State and Active objects

Another problem I came across is the flawed way settings are saved. This manifests in this plugin being very error prone when used in a bitbucket server setup with multiple nodes. 

It uses `PluginSettings` to store settings which are not synchronized between nodes and it should use `ActiveObjects` which is even stated in this forum thread: [here](https://community.atlassian.com/t5/Answers-Developer-Questions/JiraPluginSettings-with-Data-Center/qaq-p/529283) - too bad the docs for atlassian plugin development are rather lacking.

### Repo is dormant

My addition to the plugin in shape of a [PR](https://github.com/pragbits/stash2slack/pull/71) is completely ignored now, no wonder since the repo was not updated in a year. The author clearly moved on. I would like to dig into how to distribute an Atlassian plugin to maybe supply this updated version to a broader audience. If only I had time...
