---
layout:     post
title:      Setup Ubuntu 16.04 for ML Research
date:       2016-05-16 00:00:00
summary:    Setup note
categories: ml_practice
thumbnail: jekyll
tags:
 - Ubuntu setup
 - Anaconda
 - Tensorflow
 - Jekyll
---

Today, I screwed up my Ubuntu, again. One thing I learned while I have been being used Ubuntu is that, reinstalling Ubuntu can be much faster than stuking in a difficult problem which might not be able to be solved forever. Today, after realizing all environment variables are screwed, I quickly reinstalled **Ubuntu 16.04** (rather than Ubuntu 14.04!) and set up the environment for machine learning research as follows.

I am leaving my records for myself as well as for those who also need a manual to quickly setup Ubuntu 16.04. Before starting the installations, I would like to remark that the shortcuts of ```copy / paste``` in terminal(```Ctrl+T```) are ```Ctrl+Shift+C``` *or* ```V``` rather than ```Ctrl+C``` *or* ```V```. You can copy and paste the following example codes for your installation except for specific filenames.

All notes are written based on python 2. For installing on python 3 rather than python 2, you may need little changes such as using ```pip3``` and ```python3``` instead of ```pip``` and ```python```.


### Install commands
- pip installation

```bash
$ sudo apt install python-pip
$ sudo pip install --upgrade pip
```
- To keep up your up-to-date system, run the followings evertime you install any packages

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
```

### Anaconda
- Download Anaconda from [here][L_Anaconda]

```bash
$ cd ~/Download
$ chmod +x Anaconda2-4.0.0-Linux-x86_64.sh
# Do not use sudo before ./Ana...
$ ./Anaconda2-4.0.0-Linux-x86_64.sh   # Check the .sh filename
```
- Restart the terminal *or* run ```$ source ~/.bashrc```


### Tensorflow (cpu) on Anaconda
- Follow the instruction of the office [Tensorflow site][L_TF]

```bash
$ conda create -n tf python=2.7
$ source activate tf
# Now, you will see changed prompt as (tf)$
# Do not use sudo before ./pip...
(tf)$ pip install --upgrade pip
(tf)$ pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl   # Check the .whl filename    
```
- Test if your Tensorflow is working

```bash
$ python
>> import tensorflow
>> # No error occurs
```
- To escape from the virtual conda environment, run ```$ source deactivate```

![After finishing Tensorflow installation][S3_TF]

### Troubleshooting
- If you see a failure message in ```import tensorflow``` even though you have successfully installed Tensorflow, it may be because you have used ```sudo``` during the installations of Anaconda or Tensorflow.
- In this case, uninstall Anaconda (simply [delete the Anaconda][L_UnAna] folder with ```$ sudo rm -rf ~/anaconda2```) and correctly follow the instructions again.

### Git
- Git for using [GitHub][L_GitHub] repository

```bash
$ sudo apt-get install git
$ git config --global user.name "GitHub username"
$ git config --global user.email "GitHub email"
```

- Test if your Git is working

```bash
$ git clone https://github.com/terryum/terryum.io.git
```

### R
- R now supports Ubuntu 16.04 LTS
- First, edit ```/etc/apt/sources.list``` file.
```bash
$ sudo gedit /etc/apt/sources.list
```
- Add the following line at the end.

```bash
deb https://cloud.r-project.org//bin/linux/ubuntu xenial/
```

- After modification,

```bash
$ sudo apt-get update
$ sudo apt-get install r-base
```

- You can download and install RStudio from [here][L_RStudio]

### Java, Ruby, and Jekyll

- The belows are thing required for Jekyll

```bash
$ sudo apt-get install default-jdk
$ sudo apt install ruby
$ sudo apt-get install ruby-dev
$ sudo gem install jekyll
$ sudo gem install s3_website  # For easily managing Jekyll posts
```


\* *I will keep updating this post for including, e.g., CUDA installation on Ubuntu 16.04. for GPU computing.*

--------------------------

The belows are additional notes for my Ubuntu setting.

### Language Setting (Korean)
- Language support - Install / Remove Languages - Check ```Korean```  
- Keyboard input method system : fcitx
- Text Entry - click ```+``` - Add Hangul(Fcitx)
- Set a shortcut for switch between EN/KOR as, e.g. Shift+Space
- Reboot if any problem occurs

### Display
- To prevent the sticky mouse cursor on the monitor edge, Screen Display - Sticky Edge Off

### Shortcut
- Keyboard - Shortcut Tab  
- I prefer to set my own shortcuts for Screenshots
- "Semi-maximize the window" is already assigned to "Ctrl+Super+Left/Right"
- Click and hold the super key to see shortcut assignemnets

### Install Software from Ubuntu Software
- Atom editor
- VLC media player



<!---
### Nvidia Setup (Driver, CUDA, cuDNN)
- ERROR!!!
- ```sudo apt-get install nvidia-361``` (Before doing this, check if ```nvidia-361``` is applicable to your graphic card)
- (If you don't mind) Disable UEFI during installation
- Check [this article][L_CUDA_install] for CUDA installation on Ubuntu 16.04
- Ctrl+Alt+F1 - login - ```sudo apt-get purge NVIDA*```
-->

[L_NVidia]: http://www.geforce.com/drivers
[L_CUDA_install]: https://www.pugetsystems.com/labs/articles/NVIDIA-CUDA-with-Ubuntu-16-04-beta-on-a-laptop-if-you-just-cannot-wait-775/
[L_Anaconda]: https://www.continuum.io/downloads
[L_CondaTF]: https://anaconda.org/jjhelmus/tensorflow
[L_TF]: https://www.tensorflow.org/versions/r0.8/get_started/os_setup.html#anaconda-installation
[S3_TF]: {{site.imgurl}}/TF_install.png
[L_UnAna]: https://docs.continuum.io/anaconda/install
[L_GitHub]: https://github.com/
[L_RStudio]: https://www.rstudio.com/products/rstudio/download/
