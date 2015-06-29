---
title:  "Apple osx app bundles for D"
date:   2015-06-29 21:00:00
categories: [code, osx, dlang]
---

### "App Bundles with a Makefile"

Joseph Long wrote an excellent blog post about the creation of an app bundle using makefiles and that was the foundation of my work:

[blog post](http://joseph-long.com/writing/app-bundles-with-a-makefile/)

### How to find locations inside an app bundle

{% %}
CFBundleRef mainBundle = CFBundleGetMainBundle();
CFURLRef resourcesURL = CFBundleCopyResourcesDirectoryURL(mainBundle);
char path[PATH_MAX];
if (!CFURLGetFileSystemRepresentation(resourcesURL, TRUE, (UInt8 *)path, PATH_MAX))
{
    // error!
}
CFRelease(resourcesURL);

chdir(path);
std::cout << "Current Path: " << path << std::endl;
{% %}

[SO Thread](http://stackoverflow.com/a/520951/2458533)

### CoreFoundation library bindings for D

[dub package](http://code.dlang.org/packages/derelict-cf)
[github repository](https://github.com/Extrawurst/DerelictCF)

### "Bundle Programming Guide"

The rest of the info needed can be found in the official docs by Apple: [bundle programming guide](https://developer.apple.com/library/mac/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html)

