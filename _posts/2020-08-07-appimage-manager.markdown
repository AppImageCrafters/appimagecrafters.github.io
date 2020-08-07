---
layout: post
title:  "Introducing appimage-manager"
date:   2020-08-07 00:00:00 -0500
categories: AppImage
---

`appimage-manager` is a tool for managing your AppImage collection. It works quite similar to a traditional package
manager but without the fuss of managing dependencies.

# Installation

To install appimage-manager go to the releases page at Github, download the AppImage file into `/usr/local/bin` and
give it execution permissions. This is basically what the command below does: 
```
sudo wget https://github.com/AppImageCrafters/appimage-manager/releases/download/v0.1.2/app-0.1.0-x86_64.AppImage -O /usr/local/bin/app; 
sudo chmod +x /usr/local/bin/app
```

# Usage

## Looking for AppImages

AppImage binaries are usually published in the author releases page. So if you know what application do you need then
go to the releases author releases page and download the binary right from there.

If the author uses Github releases you can use: 

`app install https://github.com/lbryio/lbry-desktop`

### Discovering AppImage

[AppImage Index](https://appimage.github.io) is an index of projects that publish AppImages. You can also use 
`https://www.srevinsaju.me/get-appimage/` for a better user interface.

[AppImageHub](https://www.appimagehub.com) is a software store were you can find many original and third party packaged 
applications. It's recommended to always download binaries from the original author to avoid malware. 

AppImageHub is also integrated into the app, so in order to look for an application you can use: `app search <appname>`

AppImage files will be downloaded to `$HOME/Applications` and will be also added to your system menu on installation.


## Updating AppImages

`appimage-manager` support delta updates using the zsync algorithm. This algorithm requires `.zsync` file to be published
by the author. In the cases were this file is not provided a regular download will be performed. Updates work also for
manually downloaded applications.
 
 
# Preview

See it working in this video:
<iframe id="lbry-iframe" width="560" height="315" src="https://lbry.tv/$/embed/app_preview/cdd35353aefe047a8115c95a994ea63bfb728501" allowfullscreen></iframe>