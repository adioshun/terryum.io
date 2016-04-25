---
layout:     post
title:      Let's have Ubuntu installed
date:       2016-04-23 07:02:00
summary:    How to install Ubuntu
categories: Ubuntu
thumbnail: android
tags:
 - Ubuntu
 - Ubuntu install
---

The first post is about **how to set up** the machine learning environment in **Ubuntu**. I don't want to explain the details of Ubuntu installation process because I believe you can find lots of good articles about it in other pages as well.

I think there are three ways to enjoy linux environment as a Windows user: installing Ubuntu *(i)* in the dual boot system, *(ii)* in a virtual machine, *(iii)* and in [Docker][Docker].

# Using virtual machines
I first tried *(ii)* because I thought it's a good way to enjoy both Windows and Linux in parallel. Many friends recommended [VirtualBox][VM1] as a virtual machine, but you can also use [VMware][VM2] or [Hyper-V][VM3] as well. (You can find Hyper-V in Windows 8 or later. If it's not searchable, [enable Hyper-V from Programs and Features][VM3_Find]) A drawback what I found from using Hyper-V is that it doesn't allow full screen mode and the maxium resolution you can use is 1920x1080 (which is much lower than my Surface Book resolution, 3000x2000).

[![VirualBox and VMware][Img_VM1]][Img_VM2]

# Using Docker

Using [Docker][Docker]is also a good option if you want to have a ready-to-use environment in which all applications are pre-installed by others. For example, if you take a [deep learning course in Udacity][DL], the course provides a docker image in which everything you need for running [Tensorflow][TF] is already pre-installed for Window users. It's convenient but I don't think it's a way to be completely immerged in Linux environment.

[![Docker][Img_Docker1]][Img_Docker2]

# Ubuntu in dual boot system

I'd like to recommend you to have Ubuntu installed in your **dual boot** system. There are numerous benefits for doing it. For me, the biggest benefit is that I'm **no longer distracted** by miscellaneous temptations (like games) as in Windows environment because my Ubuntu is so clean. ~~Please anyone blocks facebook from my computer for me.~~ The **real benefit** is that I can use **full of my computing resources** such as memory, cpu, and gpu, for learning a large amount of data. That's why we install Ubuntu: applying machine learning to a large amount of data.

![I have Ubuntu now][Img_Ubuntu]

OK. I hope you wouldn't have difficulties to have Ubuntu installed in your computer. One thing I'd like to mention is that, please use a **English name as your computer name** even if your mother language is not English (like me). Korean name, for example, may occurs errors in some applications due to the directory path which includes non-English. We will see how to install essential packages for machine learning in the next post.


  [Docker]: https://www.docker.com/
  [VM1]: https://www.virtualbox.org/
  [VM2]: http://www.vmware.com/
  [VM3]: https://www.microsoft.com/en-us/server-cloud/solutions/virtualization.aspx
  [VM3_Find]: https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/quick_start/walkthrough_install
  [Img_VM1]: {{site.imgurl}}/VMs.jpg
  [Img_VM2]: https://www.maketecheasier.com/convert-virtual-machines-vmware-virtualbox/
  [DL]: https://www.udacity.com/course/deep-learning--ud730
  [TF]: https://www.tensorflow.org/
  [Img_Docker1]: {{site.imgurl}}/Docker.png
  [Img_Docker2]: https://meteorhacks.com/docker-container-war-and-meteor/
  [Img_Ubuntu]: {{site.imgurl}}/MyUbuntu.png
