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

### Nvidia Driver

- *[Warning]* This is the most dangeous part in Ubuntu setting. Installing a wrong driver can make fatal errors for your system, e.g., infinite login loop. Thus, make sure to backup all your important files before installation.
- *[Warning]* Don't install Nvidia graphic driver by using, e.g., ```sudo apt-get install nvidia-361```

- I followed this [article][L_NvidiaInstall] for Nvidia driver installation.
- First, download a Nvidia driver file which is compatible with your graphic card from [here][L_Nvidia].
- You need to block *nouveau* before installing the driver. Make a new .config file as follows.

```bash
$ sudo gedit /etc/modprobe.d/blacklist-nouveau.conf
(Add the followings)
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
(Save and quit)
$ echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
sudo update-initramfs -u
```
- Now, you need to reboot and log into tty mode. You can access tty mode via Ctrl+Alt+F1.

```bash
$ sudo stop lightdm
$ sudo apt-get update
$ sudo apt-get upgrade
$ cd ~/Downloads
$ chmod +x NVIDIA-Linux-x86_64-361.42.run # Check the .run filename   
$ sudo sh ./NVIDIA-Linux-x86_64-361.42.run
```
- If you face with fatal errors (e.g. infinite login loop), try to login tty mode(Ctrl+Alt+F1) and delete the all nvidia files by ```sudo apt-get purge nvidia*```

### CUDA and cuDNN for Ubuntu 14.04
- CUDA for Ubuntu 16.04 is not released yet. I found an [article][[L_CUDA_16.04]] for installing CUDA on Ubuntu 16.04, but I don't recommend it because it is too dangerous (or challenging).
- You may refer to the [CUDA installation guide][L_TF_CUDA] given by the official Tensorflow website.
- First, download a CUDA installation file(.deb) from [here][]

```bash
$ cd ~/Download
$ sudo dpkg -i cuda-repo-ubuntu1404-7-5-local_7.5-18_amd64.deb
$ sudo apt-get update
$ sudo apt-get install cuda
```

- Go to [cuDNN website][L_cuDNN] and download cuDNN(v4) library files after registration.
- uncompress the files and copy files as follows

```bash
$ cd ~/Download (to the folder where you can see "cuda" folder)
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

- Open *bashrc* file and add these lines

```bash
$ sudo gedit ~/.bashrc
(Add the followings)
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64"
export CUDA_HOME=/usr/local/cuda
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

- If you have installed CUDA and cuDNN, then you may be able to install Tensorflow(gpu)

```bash
(tf)$ pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl
```
- Test if your Tensorflow is working

```bash
$ python
>> import tensorflow
>> # No error occurs
```
- To escape from the virtual conda environment, run ```$ source deactivate```

![After finishing Tensorflow installation][S3_TF]
![Tensorflow gpu version][S3_TF2]

### Troubleshooting
- If you see a failure message in ```import tensorflow``` even though you have successfully installed Tensorflow, it may be because you have used ```sudo``` during the installations of Anaconda or Tensorflow.
- In this case, uninstall Anaconda (simply [delete the Anaconda][L_UnAna] folder with ```$ sudo rm -rf ~/anaconda2```) and correctly follow the instructions again.
- If you have installed Tensorflow(gpu) but fail to load ```libcudart.so.7.5```, it may be because you didn't change the permission of the lib files or edit .bashrc file. Please check the instruction for CUDA installation again.

### Python packages on Conda
- You can manage each Conda virtual environment, independently. In other words, you can install and use different versions of python packages in different virtual environment.
- After running the virtual environment by ```source activate tf```, you can check the list of installed packages by ```conda list```
- Here is some packages you may want to have installed. Note that we are going to use ```conda install``` instead of ```pip install```.

```bash
$ conda install jupyter numpy scipy matplotlib pandas scikit-learn scikit-image
```

- For loading ```.RData``` to python.  

```bash
$ conda install -c r rpy2=2.7
```

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
deb https://cloud.r-project.org//bin/linux/ubuntu xenial/
```
- After modification,

```bash
$ sudo apt-get update
$ sudo apt-get install r-base
```
- You can download and install RStudio from [here][L_RStudio]

### Java, Ruby, and Jekyll

- The followings are thing required for Jekyll

```bash
$ sudo apt-get install default-jdk
$ sudo apt install ruby
$ sudo apt-get install ruby-dev
$ sudo gem install jekyll
$ sudo gem install s3_website  # For easily managing Jekyll posts
```

### Troubleshooting

- If you are using Ubuntu 14.04, you may get ruby v1.9 instead of v2.3. To get ruby v2.3, follow the instructions bellow.

```bash
$ sudo apt-add-repository ppa:brightbox/ruby-ng
$ sudo apt-get updat
$ sudo apt-get install ruby2.3 ruby2.3-dev
```



\* *I will keep updating this post for including, e.g., CUDA installation on Ubuntu 16.04. for GPU computing.*

--------------------------

The followings are additional notes for my Ubuntu setting.

### Language setting (Korean)
- Language support - Install / Remove Languages - Check ```Korean```  
- Keyboard input method system : fcitx
- Reboot
- Text Entry - click ```+``` - Add Hangul(Fcitx)
- Set a shortcut for switch between EN/KOR as, e.g. Shift+Space
- Reboot if any problem occurs

### Display
- To prevent the sticky mouse cursor on the monitor edge, Screen Display - Sticky Edge Off

### Shortcuts
- Keyboard - Shortcut Tab  
- I prefer to set my own shortcuts for Screenshots
- "Semi-maximize the window" is already assigned to "Ctrl+Super+Left/Right"
- Click and hold the super key to see shortcut assignemnets

### Software from Ubuntu Software
- Atom editor
- VLC media player


[L_Anaconda]: https://www.continuum.io/downloads
[L_CondaTF]: https://anaconda.org/jjhelmus/tensorflow
[L_TF]: https://www.tensorflow.org/versions/r0.8/get_started/os_setup.html#anaconda-installation
[S3_TF]: {{site.imgurl}}/TF_install.png
[S3_TF2]: {{site.imgurl}}/TF_install2.png
[L_UnAna]: https://docs.continuum.io/anaconda/install
[L_GitHub]: https://github.com/
[L_RStudio]: https://www.rstudio.com/products/rstudio/download/

[L_NvidiaInstall]: http://ubuntuhandbook.org/index.php/2015/01/install-nvidia-346-35-ubuntu-1404/
[L_NVidia]: http://www.geforce.com/drivers
[L_CUDA_16.04]: https://www.pugetsystems.com/labs/articles/NVIDIA-CUDA-with-Ubuntu-16-04-beta-on-a-laptop-if-you-just-cannot-wait-775/
[L_CUDA_down]: https://developer.nvidia.com/cuda-downloads
[L_TF_CUDA]: https://www.tensorflow.org/versions/r0.8/get_started/os_setup.html#optional-install-cuda-gpus-on-linux
[L_cuDNN]: https://developer.nvidia.com/cudnn
