---
layout: post
title:  "Path mapping in AppImages"
date:   2020-08-20 00:00:00 +0400
categories: AppImage AppImageBuilder APPRUN
comments: true
---

Fixed file paths in binaries have been a nightmare for AppImage creators since the format was created. The most effective
solution to this issue is to modify the source code and add a way of setting those paths at runtime. But, this is not
always possible.

The developer may not have access, permissions, knowledge or time to modify the source code. This is a big stop if you
plan to pack an applications as an AppImage. Obscure binary patches are used to workaround this leading to complex 
packaging recipes.


# System calls hooking to the rescue

With the help of ld-linux.so and its preload feature we can hook every system call that takes a file path as an argument
and edit it to point to the bundled files. This allows running binaries containing fixed file paths without modifying
them.

For its use in AppImage bundle we added this feature into the [AppRun](https://github.com/appimagecrafters/AppRun) project.
This is will be deployed as default entry point for AppImages created with [appimage-builder 0.6](https://github.com/AppImageCrafters/appimage-builder).

# Limitations

Preloading libraries is only possible for dynamic linked binaries, we will need something different for static linked ones.
Luckily the major part of the modern software is dynamically linked.

