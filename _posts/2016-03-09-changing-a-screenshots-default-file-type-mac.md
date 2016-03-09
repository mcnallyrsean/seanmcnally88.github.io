---
layout: post
title: Changing a Screenshot's Default File Type (Mac)
modified:
categories: 
excerpt:
tags: [mac, screenshot, jpg]
image:
  feature:
date: 2016-03-09T11:10:49-06:00
---

This is a short post for those of you who would like to change the default file type of a Macbook screenshot.

By default, a screenshot is saved as a PNG file. However, as you'd imagine, there are other file types that a screenshot can be saved as (JPG, TIFF, PDF, GIF, et cetera).

To change the default, open up Terminal and type this code into the command line 

```
defaults write com.apple.screencapture type jpg;killall SystemUIServer
```

To use another file type as the default, simply replace jpg in the command with the file type of your choice (assuming its an image file type, of course).

##### Helpful Shortcuts

To take a full screenshot -
```
command + shift + 3
```

To capture a selection from the screen -
```
command + shift + 4
```

Happy screenshotting!