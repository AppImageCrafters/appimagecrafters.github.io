---
layout: post
title:  "appimage-builder v0.7.5 files based recipes"
date:   2020-11-02 00:00:00 +0400
categories: AppImage AppImageBuilder
comments: true
---

appimage-builder v0.7.5 brings an really interesting feature: *full file based recipes*.
Unlike traditional recipes that uses apt-get and a list of Debian packages to fulfill
the dependencies of a given application the new approach uses a list of files. Yes, you
read it well, all the files that should be included in the final bundle will be listed
in your `AppImageBuilder.yml` file.

The file list is built using the outputs of `strace` and the linker in debug mode. Therefore
all the files that were accessed by the application at runtime are add into the list.  

>> Are you nuts? The recipe files will be huge!

Indeed they will, that's why we will keep recommending the package based recipes. But let's
dig a bit on this idea.

This kind of recipes are focussed on those GNU/Linux distributions that doesn't have apt-get
or other package manager supported by appimage-builder. To use it just run 
`appimage-builder --generate` on a system without apt-get. It will prompt the same questions
and run the application. Once it exits instead of finding which packages provide the files
and libraries accessed at runtime it will proceed to list them in the 
`AppDir > files > include` section.

Once the recipe is generated you can run appimage-builder again to create the bundle. Notice 
that not only libraries are included but also other data files. You may want to take a look 
at them before considering your job done. Some of them may be icons from the system icon theme
or fonts. Those can be safely excluded.

While this approach has its drawbacks it's an important step on bringing appimage-builder
to every GNU/Linux distribution. If you want to see a full recipe generated using this 
check out the following [link](https://github.com/AppImageCrafters/appimage-builder/blob/master/examples/kcalc-files/AppImageBuilder.yml)


Thanks for readings, see you in the comments section.


