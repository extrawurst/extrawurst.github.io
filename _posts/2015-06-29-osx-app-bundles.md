---
title:  "Apple osx app bundles with #dlang"
date:   2015-06-29 21:00:00
categories: [code, osx, dlang]
---

Recently I wanted to package an application that I have written in D so that it would feel like a native osx application bundle. Nothing fancy.. Boy little did I know..
Actually it is pretty hairy to create such a bundle by hand without the help of XCode. 
Since I am in dland nothing is normal so here is how you do it:

### "App Bundles with a Makefile"

Joseph Long wrote an excellent blog post about the creation of an app bundle using makefiles and that was the foundation of my work:
[blog post](http://joseph-long.com/writing/app-bundles-with-a-makefile/)

Using these instructions got me to the point where I had an app bundle that started on my system.
Still no app icon or bundled dependencies.

### App Icon

Defining the app logo is 'just' a matter of adding an compatible image file and configurating the Info.plist of the app.
Unfortunately an ICNS File is needed.
So here I was with my icon PNG File and found this instruction to convert it to the appropriate format:
[SO Thread](http://stackoverflow.com/questions/12306223/how-to-manually-create-icns-files-using-iconutil)

Now we have an app logo and still need to fix the dependencies...

### How to find locations inside an app bundle

{% highlight c %}
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
{% endhighlight %}

[SO Thread](http://stackoverflow.com/a/520951/2458533)

### CoreFoundation library bindings for D

[dub package](http://code.dlang.org/packages/derelict-cf)
[github repository](https://github.com/Extrawurst/DerelictCF)

### "Bundle Programming Guide"

The rest of the info needed can be found in the official docs by Apple: [bundle programming guide](https://developer.apple.com/library/mac/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html)

