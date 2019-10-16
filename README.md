# AppImage Crafters Docs

You wil find here tips and tricks on how to use the AppImageCreadters docker-images to produce high quality AppImages.

## Introducction

The process of producing an AppImage file starts by creating a relocatable and totally standalone installation of your binaries. Then it's compressed into a squashfs file and appended to a runtime. While it's possible to do it by hand this is not a trivial task therefore several tools where created to make your life easier.

This page complements the [AppImage Project Documentation](https://docs.appimage.org/) with build examples based on the docker images maintained by the AppImageCrafters group. If __you are an AppImage creator__ and have had a hard time building some dependency of your app, please consider uploading it to the collection so others can benefit from it. If __you are creating an Appimage__ for the __first time__, we have growing collection of tested recipes for you to use.

## The AppImage build environment

The secret to make your AppImage run on almost any GNU/Linux distribution is simple: build on the oldest and still supported system available (read Centos 6). But that's not allways a trivial task and there are a lot of details to be addressed. Therefore our goal is to provide you the right base system with the tool chains ready to compile and pack your software.

To achieve this goal, we purpose a workflow based on a collection of docker recipes that can be mix to create the right build environment for your app.

In this build environment is possible thanks to the [Red Hat Developer Toolset](https://access.redhat.com/products/Red_Hat_Enterprise_Linux/Developer/#dts=&dev-page=6) which provide modern build toolchains that can be installed on systems like Centos 6 and produce binaries that are totally forward compatible (cosider by example the ABI break between libc++ and libc++-11). Using this configuration was built [`linuxdeploy`](https://github.com/linuxdeploy/) a tool that allows to create an AppDir and pack it into an AppImage. Aditionaly were included cmake, autotools and autoconf.

## Recipes

### Basics

AppImages are meant to pack desktop applications, therefore is mandatory to provide a valid [`Desktop Entry`](https://standards.freedesktop.org/desktop-entry-spec/latest/) and [`icon`](https://specifications.freedesktop.org/icon-theme-spec/icon-theme-spec-latest.html). Additionally it's recommended to include an [`AppStream file`](https://www.freedesktop.org/software/appstream/docs/index.html) to properly document your bundle.

About the __copyrights__, while the tools do their best to include the dependencies copyrights it's your sole resposibility to make shore that all of them are fulfilled properly. To do so you can copy them into the `AppDir` before creating the AppImage bundle.

To __create the `AppDir`__ the app install script must be configured to use `/usr` as prefix. The you must call install into a folder named `AppDir`. _The folder is named that way by convention, you can use the one you like._

To __generate the `AppImage`__ the `linuxdeploy` tool will be used. It receives as paramether the path to the `AppDir` and the plugins to be enabled. So to produce an AppImage the corresponding plugin must be used as follows:

```linuxdeploy --appdir=path/to/AppDir --output appimage```

### Packing C++ Application

Creating an AppImage from a C++ application is simple just create an `appimage-build.sh` script with the follwing content in your project root dir:
```
#!/bin/bash

mkdir -p appimagecraft-build-release
pushd appimagecraft-build-release
  cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr .
  DESTDIR=AppDir make install
  linuxdeploy --appdir=AppDir --output appimage
popd
```
and from your source dir call:
```docker run -v $PWD:/source -w /source appimagecrafters/docker-linuxdeploy bash ./appimage-build.sh```

a nice AppImage file should be found in the `appimagecraft-build-release` folder with the name you gave it on the `Desktop Entry` file.

__A full working example can be found [here](https://appimagecrafters.github.io/#packing-c-application)__
