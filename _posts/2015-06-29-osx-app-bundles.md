---
title:  "Apple osx app bundles with #dlang"
date:   2015-06-29 21:00:00
categories: [code, osx, dlang]
---

Recently I wanted to package an application that I have written in D so that it would feel like a native osx application bundle. Nothing fancy.. Boy little did I know..
Even though an app bundle is nothing more than a special kind of folder structure that an osx finder interprets in a special way.
Actually it is pretty hairy to create such a bundle by hand without the help of XCode. 
Since I am in dland nothing is normal so here is how you do it:

### "App Bundles with a Makefile"

Joseph Long wrote an excellent blog post about the creation of an app bundle using makefiles and that was the foundation of my work:
[blog post](http://joseph-long.com/writing/app-bundles-with-a-makefile/)

Using these instructions got me to the point where I had an app bundle that started on my system.
Still no app icon or bundled dependencies.

### App Icon

Defining the app logo is 'just' a matter of adding a compatible image file and modifying the Info.plist file of the app:

{% highlight xml %}
...
<key>CFBundleIconFile</key>
<string>iconfile</string>
...
{% endhighlight %}

Unfortunately an ICNS File is needed.
So here I was with my icon PNG and found these instructions to convert it to the appropriate format:
[SO Thread](http://stackoverflow.com/questions/12306223/how-to-manually-create-icns-files-using-iconutil)

Now we have an app logo and still need to fix the dependencies...

### How to find locations inside app bundles

So the last bastion was to use the bundle to bundle up the necessary frameworks and dependencies with it. This allows us to save the user the trouble of installing all kinds of libs and maybe even having to built libs themselves.

Since we use dynamic bindings of libs like SDL the binary of our app looks for those *.dylib files in system folders and near the executable itself. The problem is that even though the bundle is just a folder the current directory maybe total different and in this current working dir it is where the bindings look for the dynlib files.

I found the solution in [this SO Thread](http://stackoverflow.com/a/520951/2458533) explaining how to query the system for bundle paths:

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

The next problem was how to do this in dland? Well actually pretty straight forward with yet another dynamic binding to the CoreFoundation Framework of the osx system itself...

### CoreFoundation library bindings for D

To be able to use the system libs you need to have bindings which I created (with JUST the functionality needed for this case, so please feel free to add and PR):

* [dub package](http://code.dlang.org/packages/derelict-cf)
* [github repository](https://github.com/Extrawurst/DerelictCF)

Using these bindings we can do the exact same as in the c++ example above and change the working directory appropriately after app start.
Now we are finished, we can use whatever content we bundle up in the app and give it an icon image!

### "Bundle Programming Guide"

The rest of the info needed and for reference here are the official docs by Apple: [bundle programming guide](https://developer.apple.com/library/mac/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html)
