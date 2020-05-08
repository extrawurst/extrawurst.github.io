---
title:  "github actions + homebrew = ❤️"
date:   2020-04-14 12:00:00
categories: [general]
---

So in this story our hero sets out to do something that seemed trivial: Adding continuous deployment to a github repo automating deployment of a little macOS binary to homebrew.

![gh-actions]({{ site.url }}/assets/github-actions.png)

# Marketplace Actions

The beauty of github actions is that there is a marketplace to share the ones you have written with others and simplify reuse in your own projects.

Naturally I was expecting to be able to just use one of these and found: [dawidd6/action-homebrew-bump-formula](https://github.com/dawidd6/action-homebrew-bump-formula)

Looks simple enough, so I added it and realized that under the hood it spins up a docker container based on some ubuntu and would complain because I was building my pipeline on macOS machines - afterall I want to build and deploy a macOS binary.

After some digging I realised that I am supposed to use [actions/upload-artifact](https://github.com/actions/upload-artifact) to build my stuff on a mac machine and than upload the artifacts to be downloaded back into a followup job that would distribute them to homebrew using the aforementioned marketplace action. Sounds complicated and overkill? I agree.

# Custom Action
Turns out the marketplace github actions of others do not work with my macOS builder. So next I implemented it myself. How hard can it be?

The first step is to try brew locally to create a PR bumping a formula version:
```
brew bump-formula-pr -f --version=<version> --no-browse --no-audit --sha256=<hash> --url=<url> formula
```
This can be run on macOS github workers ✅

Let's step through the workflow file:

## Step 1: Checkout and build

```yaml
name: CD
on:
  push:
   tags:
     - '*'
jobs:
  release-osx:
    runs-on: macos-latest
    steps:
    - uses: actions/checkout@v2
    - name: Build Release
      run: make build-release
```

Here we see the head of the file, we run the workflow whenever a tag is pushed and use a macOS machine to run-on. Then we actually build the binary which leaves the build binary in `./target/gitui-mac.tar.gz`

## Step 2: Store info using set-output

```yaml
    - name: Get version
      id: get_version
      run: echo ::set-output name=version::${GITHUB_REF/refs\/tags\//}
    
    - name: Set SHA
      id: shasum
      run: |
        echo ::set-output name=sha::"$(shasum -a 256 ./target/gitui-mac.tar.gz | awk '{printf $1}')"
```

Here you see 2 more steps: First we remove the `refs/tags/` from the ref name to get something like `v0.1.0` that we need for the brew formula versioning. Then we calculate the sha256 hash of the bundle to pass on to homebrew aswell for validation. We store this information using the `::set-output` to allow using it in later steps of the workflow

## Step 3: Create Release

```yaml
    - name: Create Release
      id: create_release
      uses: actions/create-release@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        tag_name: ${{ github.ref }}
        release_name: ${{ github.ref }}
        draft: false
        prerelease: true
```

This actually creates a github release based on the git tag. It is important to **note**: it should not be a draft — drafts will not be downloadable for the public and therefore the next steps will fail. This can be confusing to debug because pasting the download link into the browser might work when you have a valid github session that has access to that release🤯.

## Step 4: Upload Archive

```yaml
    - name: Upload Release Asset
      uses: actions/upload-release-asset@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        upload_url: ${{ steps.create_release.outputs.upload_url }}
        asset_path: ./target/gitui-mac.tar.gz
        asset_name: gitui-mac.tar.gz
        asset_content_type: application/gzip
```

This step attaches the archive to the release. It uses an output variable from a previous step as you can see in line 6.

## Step 5: Bump Brew Formula

```yaml
    - name: Bump Brew
      env: 
        HOMEBREW_GITHUB_API_TOKEN: ${{ secrets.BREW_TOKEN }}
      run: |
        brew tap extrawurst/tap
        brew bump-formula-pr -f --version=${{ steps.get_version.outputs.version }} --no-browse --no-audit \
        --sha256=${{ steps.shasum.outputs.sha }} \
        --url="https://github.com/extrawurst/gitui/releases/download/${{ steps.get_version.outputs.version }}/gitui-mac.tar.gz" \
        extrawurst/tap/gitui
```

This is the final bit. Here we actually bump the homebrew formula and create a PR to the tap repository. Note that we have to set a specific env variable in line 3. For this we have to create a new access token here: https://github.com/settings/tokens
Then all the work from above comes together: The output variable containing the extracted version string and the sha256 hash.

# TL;DR

* use *outputs* to transfer data between steps (see 1 and 5)
* create new access *token* for brew tap pull-request creation (see 5)
* github release cannot be *draft* for download link to be public (see 3)
* find the entire workflow file here: [extrawurst/gitui/cd.yml](https://github.com/extrawurst/gitui/blob/master/.github/workflows/cd.yml)

# Conclusion

Github-Actions rock! Homebrew is great and together its a match made in heaven. It’s unfortunate that the existing marketplace actions for homebrew are all using docker and therefore linux even though homebrew is primarly a tool for macOS. I need to look into whether there’s an alternative to publishing an action not based on Docker to the marketplace.