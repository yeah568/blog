---
title: A Beginner's Guide to Video Encoding, Part 1 - YUV
summary: 
tags:
- yuv
- h264
---

Last year, I did an internship at Broadcom, where I learned a lot about the way modern video formats work. However, that took a *lot* of time to acquire, since seemingly ever article I found required some level of pre-existing experience. The aim of this series is just to introduce you to the concepts needed to get your foot in the door. For the first post, I'll start with YUV - the foundation of video.






YUV exploits the fact that the human eye is more sensitive to changes in luminance, or brightness, than it is to changes in colour. 




![YUV422 planar vs. packed](/assets/img/posts/video-encoding-part-1/packed_planar.png)
*An example of YUV420 planar on the left, and packed (YUYV, YUY2) on the right.*