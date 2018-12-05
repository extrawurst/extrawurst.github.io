---
title:  "golang and gitlab ci"
date:   2018-12-05 18:00:00
categories: [general]
---

This is a quick writeup of how to setup a simple ci pipeline for a `go` project on gitlab using golang's 1.11 modules.

## The Why

I was working on an [Alexa Skill](https://www.amazon.com/dp/B07L4W8D7C) written in go and in fact did my first steps in go. I am still a noob when in comes to the go-world, so I googled on how to setup continous integration for a go project...

![meme]({{ site.url }}/assets/go-gitlab-meme.jpg)

Turns out using go in a CI Job was not as easy as I thought. Let's see what different approaches the top hits on the google search for 'golang gitlab ci' propose:

1. [about.gitlab.com/2017/11/27/go-tools-and-gitlab-how-to-do-continuous-integration-like-a-boss](https://about.gitlab.com/2017/11/27/go-tools-and-gitlab-how-to-do-continuous-integration-like-a-boss/)
	* ✅ shows usage of lots of nice tools: code coverage, linter ...
	* ⛔️ uses crude bash scripts
	* ⛔️ uses copy-code-to-gopath hack (read below)
	* ⛔️ spreads wrong information: [my rant tweet](https://twitter.com/Extrawurst/status/1070363661687029760)
	* ⛔️ pretty sad for an official blog post by gitlab itself

2. [blog.boatswain.io/post/build-go-project-with-gitlab-ci](https://blog.boatswain.io/post/build-go-project-with-gitlab-ci/)
	* ✅ short and sweet read
	* ⛔️ uses 3rd party tool [glide](https://glide.sh/)
	* ⛔️ same old copy-code-to-gopath hack like the rest of the bunch

3. [blog.netways.de/2018/06/07/continuous-integration-with-golang-and-gitlab](https://blog.netways.de/2018/06/07/continuous-integration-with-golang-and-gitlab/)
	* ✅ [dep](https://github.com/golang/dep) feels more official being part of go
	* ⛔️ still dep does not ship with go
	* ⛔️ same old copy-code-to-gopath hack like the rest of the bunch

## A single workspace

As you can see it does not turn out to be an easy topic.
Most hastle in this area comes from go having this concept of a workspace. Workspaces were a design decision that go took on day 1. They enforce a certain mindset on you where in a central folder all go projects on your hard drive have to live (including the downloaded dependencies). This is for some (or most) a very unnatural way of structuring their projects. It also comes with lots of strings attached: Like ci jobs copying their code on the fly into the $HOME/go folder to build and resolve 🤢.

Since I am not an early adopter (lucky me 🥳) go by now released version 1.11 with a first version of module support: [golang modules](https://github.com/golang/go/wiki/Modules)

Modules allow you to have your projects independently structured much like most of the third party package managers like glide/godep did. Since this concept is so new most of the sources online do not mention it yet as being an easy alternative to build go projects, hence my post here.

## The Setup with modules

lets look at our source, go files' import-statements act as depdencies right away, some of them renamed to protect name collisions but go deeply connects to git using git repos as the definition of a dependency:
```
package main

import (
	"github.com/aws/aws-lambda-go/lambda"
	"github.com/aws/aws-sdk-go/service/dynamodb"
	"github.com/aws/aws-sdk-go/service/dynamodb/dynamodbattribute"
	"github.com/aws/aws-sdk-go/aws"
	awssession "github.com/aws/aws-sdk-go/aws/session"
	alexa "github.com/ericdaugherty/alexa-skills-kit-golang"
)
...
```

Now in your prject to initialise the usage of modules you simply run:
```
go mod init alexa-prost
```

This generates a `go.mod` file looking like this:
```
module alexa-prost

require (
	github.com/aws/aws-lambda-go v1.8.0
	github.com/aws/aws-sdk-go v1.15.90
	github.com/ericdaugherty/alexa-skills-kit-golang v0.0.0-20181003210505-70580a479839
)
```

This file contains the versions of the dependencies that got downloaded so that a reliable deterministic build can be reproduced everywhere.

Now this allows us to leave our repository wherever we want and makes our gitlab-ci script super simple:
```
test:
  image: golang:1.11
  script:
    - go test
```

## The caveats

1. 👎 **mod is not final** - We are using a very early feature of go here and it might change and contain bugs.
2. 👎 **gitlab ci caching not supported** - if you want to use gitlab-ci's caching you still need to use the same ol' copy-around trick

### Regarding gitlab ci caching

Even with go 1.11's modules the dependencies will live in the `$GOPATH` - so no `node_modules` like folder in your project's path.
This can still be solved a little more elegant than copying your project code around. Since you just want to cache the dependencies we copy the dependencies around and that is way less verbose:
```
test:
  image: golang:1.11
  cache:
    paths:
      - .cache
  script:
    - mkdir -p .cache
    - export GOPATH="$CI_PROJECT_DIR/.cache"
    - make test
```

**Note:** my OCD felt better once i had caching of dependencies working - but the build was still not faster. That meybe due to it currently being too small to be limited in the dependency download/build process.