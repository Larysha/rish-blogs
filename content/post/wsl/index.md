---
title: Getting started with WSL for Bioinformatics
date: 2023-08-22
---

A beginner-friendly, high-level overview and comprehensive walk-through of the set up of your bioinformatics terminal environment in Windows with the Windows Subsystem for Linux (WSL2)

Pre-requisites: a basic understanding of navigating a Linux Bash terminal file structure and an interest in getting started with bioinformatics.

***This tutorial will outline the following:***

- Why WSL? 
- Set-up WSL and the Fluent Terminal with optional pretty-fication. 
- How to navigate your Windows files through the Linux shell 
- Installing software and adding your programs to $PATH to access on both Linux and Windows systems 
- Setting up your tools and software for bioinformatics analysis including:
	- Conda, Python and Git
	- R and DevTools
	- Integrating VSCode with WSL
	- Nextflow and Docker 

### Why do we need WSL?

The Unix command line is truly the foundational tool for bioinformaticians - an interface or 'shell' that allows you to access other servers, manipulate large files with ease and create and run your scripts. The shell enables the most efficient way to analyse data, by allowing automation and batch processing of scripts with a high degree of flexibility by stringing together multiple, customisable commands. It is for this reason that most tools and third-party bioinformatics software is designed to run in this environment.

Typically, the command line works through a shell program or interface (like Bash), which is the build-in terminal on a Linux Debian OS. Alternatively, you could use the 'close-enough' terminal on iOS, which provides a subset of Unix commands and functionalities that are sufficient for bioinformatics tasks. 

This has left Windows users in a bit of pickle when they want to perform bioinformatics analysis because the Windows PowerShell works very differently to a Unix shell. In the past, Windows users have had to split their hard-drives, install virtual boxes, use third-party tools like Cygwin or buy secondary machines - none of which provide a smooth work experience. 

Enter the Windows Subsystem for Linux or WSL, first released in 2016. Since then, its capabilities have improved to WSL2, which boasts a seamless and powerful combination of Unix command line tools on a Windows machine, with full access to the file systems of both OSs. It offers a wide variety of Linux OS distributions, such as Kali and Arch Linux, but if you're here to run bioinformatics programs, you'll probably want Ubuntu, since this is a Debian distribution that is compatible with almost all third-party BiX tools. 

Okay, sounds great - you can use a Linux terminal in Windows - but how do we do it and how do we optimise it for a BiX set-up? (to save some syllables, I'll be referring to bioinformatics as BiX from here on 🥱)

### Installing WSL Ubuntu 

First off - make sure you're using a Windows 10 (build 19041 and up) or Windows 11 OS. 

- Open the windows command prompt:  `Win Key` + search “command prompt” + select “run as administrator”. Say “Yes” when Windows asks if you will allow the App to make changes.
- In the command prompt, type:  `wsl.exe --install`
- Wait for the installation to finish and type “Yes” if/when required.
- Restart your computer.

It's as simple as that.

The Ubuntu distribution will be installed by default - open it up:
- Once it has finished its initial setup, you will need to create a username and password (this does not need to match your Windows user credentials).
- Remember your password because you will be asked for it every time you want to do anything in your root (`/`) directory with the `sudo` command. Note you will not see anything when you type a password in terminal, but it does recognise what you type.

Now run the following commands:
`sudo apt update` then
`sudo apt full-upgrade` to make sure you're up to date.

The `update` command downloads new library/package changes while the `upgrade` command implements them.

Congrats! Your WSL is set up. Now for the fun part.

### Fluent Terminal and pretty-fications 

***Fluent Terminal (recommended)***
WSL runs from the Ubuntu terminal – this works perfectly and you can use it as-is. 
However, you may prefer to use an alterative terminal emulator - the ["Fluent Terminal"](https://github.com/felixse/FluentTerminal), which is also available for download from the [MS Store](https://apps.microsoft.com/detail/fluent-terminal/9P2KRLMFXF9T?hl=en-us&gl=ZA).  

To use WSL in Fluent Terminal, download and open the application, select the Menu in the top left corner -> hover over “new tab” -> select “WSL”
You can set WSL as you default terminal with Menu -> Settings -> Profiles -> select your default profile. 

The power of this terminal is that you can open multiple tabs for different terminals (Command Prompt, WSL, PowerShell etc. all within one terminal window!). It also looks prettier than the Ubuntu default 😌.

***Pretty-fications (optional)***
This section is optional and for those who want to "play" and explore some advanced features of the terminal - it has no effect on functionality, but might make you feel kinda cool when your prompt flashes at you in bright pink 💃 The `settings` -> `themes` section of Fluent Terminal has some colours and aesthetics you can easily change if you don't want to go too far down the rabbit hole.

If you would *really* like add some flair you can customise the wording and colour of your prompt by editing your `~/.bashrc`  file and changing your prompt string variable `PS1=`. You can read more about that [here](https://phoenixnap.com/kb/change-bash-prompt-linux) . 

Alternatively, you may be interested in simply using a pre-built theme, in which case I would recommend [oh-my-bash](https://github.com/ohmybash/oh-my-bash) - this will take you to a GitHub page where you can download the tool. *Note that downloading the tool will create a new `.bashrc` file!*  Your old `.bashrc` will still be there - but it'll have a suffix that looks something like this: `omb-backup-202...` you can copy and paste any important aliases, etc. from this file into the new one. To use a theme, open up the `.bashrc` file with something like `nano .bashrc` and edit the `OSH_THEME` variable to a theme of your choice, which you can explore [here](https://github.com/ohmybash/oh-my-bash/wiki/Themes).

### How do I access my Windows Files?
The WSL system is integrated, but still separate from you Windows files. If you type `cd` you will find yourself in the `/home` Linux directory, which is generally where you will be working in your Linux system. However, if you're working in a Windows directory you can access easily by "mounting" into you C: drive with `cd /mnt/c/Users/windows_usrname/...`

If you're using the Fluent Terminal, it may set the Windows path as your default automatically.

This means you can easily move around your Windows file systems, provided you include the `/mnt/` before the full file path, and run Linux commands and software in this system. Should you want to access your Linux files, you can use `cd ~/file/path/` as normal. 

The Linux root directory (`/`), which contains the OS system directories, contains a `mnt` folder that you can access with `cd /mnt/` - here to can see the systems, including external hard drives, mapped network drives etc, that you can access through the Unix terminal. 

Of course, you can always make things a little easier and create an alias or "shortcut" by editing your `.bashrc` file (a bash system configuration file) as follows:
```bash
cd ~
nano .bashrc
# add this to the end of the file - change path to your directory of interest
alias docs="cd /mnt/c/Users/user_name/dir_of_interest"
# cntrl+ x -> Y -> enter -> restart terminal or type `source .bashrc`
# now whenever you type `docs`, you will be taken to the folder you specified without having to type out the full path
```
Note that all files beginning with a `.` are hidden in bash if you try and see them with `ls`, however if you list all files with `ls -a` you will see the `.bashrc` file in your home directory.


**Take away points?**
- Access Windows with `/mnt/c`
- The `/` in the beginning is necessary and represents `root` 
- Aliases or shortcuts are a useful way to navigate the system quickly
 
### What if my data and files are on an external hard drive?

It is possible that the external is automatically detected and you can access it with `cd /mnt/d/path/in/external/drive`
but if this is not the case you can mount the drive yourself:

to see if there is an entry in the disk 
```bash
# lists the partitions on your system using `fdisk` with superuser (sudo) privileges to see disk partitions, their sizes, and file systems.
sudo fdisk -l
# then mount with:
sudo mount -t drvfs d: /mnt/d
# assuming your external drive is called "d" - edit as needed
```
### To permanently add a program to $PATH

It is common to download third-party software into you ***home directory***: `~/home/` and want the software available in any working directory (without have to specify the full path to the executable files each time you use it). 
To do this, you can add these file paths to your $PATH variable, which is essentially a variable that contains a list of paths telling Bash where to look for executables. It is known as an 'environment variable', which controls the systems behaviour and how it accesses resources and programmes.  You can edit this variable to include you home directory by editing your [.bashrc file](https://phoenixnap.com/kb/bashrc) located in the **_home_** directory:

```bash 
cd ~
nano .bashrc
# paste this at the end of the file, replacing everything before :$PATH with the directory path of your executables. You can seperate the list of different paths with a colon ":" as follows 

export PATH="/usr/local/sbin:/home/bin/path/to/soft:/another/path:$PATH" 
# save and exit and executre the script / reboot system to implement
# can verify with
echo $PATH
# to see where an execuatable is:
which ls # replace ls with the name of the software
```

Once you have added the software to `PATH` (or downloaded the executables directly into `/usr/bin` with the `sudo` command) you can use your software in any directory, including Windows or external folders. 

### Setting up Conda

If you work in Python and want to create virtual environments (or use a package by someone that uses it), you may want to use [Anaconda](https://www.anaconda.com/) or the "light" version, [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/) - this [link](https://docs.conda.io/projects/miniconda/en/latest/) contains the list of available installer versions. Note the Linux version that is applicable and install on your WSL system with:
```bash
cd ~
# download the relevent version of miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh

# after running this command you will be asked if you want to install, accept the license, confirm the installation location etc. simply follow the prompts and conda will install.
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3

# clean up after set up
rm -rf ~/miniconda3/miniconda.sh
# initialise for a bash shell
~/miniconda3/bin/conda init bash
```

You should now see `(base)` in front of your command prompt, which means you're in the base conda environment and can use the `conda` command to manage packages and environments - you can learn more about the basics of using conda if you're new to it [here](https://learning.anaconda.cloud/conda-basics)

If you prefer using Python environments with `pip` instead of `conda`, then you can set that up with the following:

```bash
sudo apt update && upgrade  
sudo apt install python3 python3-pip python3-venv ipython3
```

### Git and version control
Note that Git comes pre-installed with WSL2 - however you can install it with `sudo apt-get install git`
You can set your global git credentials by entering:
```bash
git config --global user.name "your name"
git config --global user.email "you email" # use the email from your GitHub account if you have one
```
your GitHub account (if you have one)
	- git config --global core.editor "code" 
	- this makes VS Code the defaul text editor  - full list of options here: https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config
	- git config --list

### Installing R and DevTools
Besides Python, R is the next language you'll be using in BiX pipelines and analysis. You can easily install it through WSL with:

```bash
sudo apt update
sudo apt install r-base
# once R is installed, you can also install some additinal R dependancies that may come in useful (optional):

# note that the backslash "\" allows you press the enter key and add code to a new line without executing it (actaully the \ lets the terminal ignore whatever character follows it) so you can enter the code above as-is and it will run correctly

sudo apt install \
r-base-core \
r-recommended \
r-base-dev \
gdebi-core \
build-essential \
libcurl4-gnutls-dev \
libxml2-dev libssl-dev
```
Once R is installed, you can run R through terminal by simply typing `R` - this will open up an R terminal and you can then install DevTools by typing:
```R
install.packages("devtools")
```
You can return your WSL terminal by typing `q()` and `n` 

### Setting up Docker through WSL
Docker is a popular container application that lets you manage and run isolated applications without having to rely on the host machine - this means that headaches around dependencies or code reproducibility are navigated by putting every requirement into one container that works the same way for everyone. There is a bit of a learning curve to get started with this tool, but it is incredibly useful and many bioinformatic tools are available as Docker containers. You can learn more about them [here](https://docs.docker.com/get-started/)

To set it up in WSL2 you can read through their more detailed instructions [here](https://docs.docker.com/desktop/wsl/) but if you want to set it up quickly you can do the following:

1. Download and install the latest version of [Docker Desktop for Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe).
2. During installation, it may suggest using the WSL feature - agree to this (it is currently the recommended selection)
3. Start Docker Desktop in **Windows** -> navigate to settings -> general tab -> make sure the "use WSL2 based engine" selection is turned on

To make sure it is working in terminal you can type `docker --version` - note that docker in WSL only works when the desktop application is active.

### Setting up Nextflow
If you're conducting BiX analysis, at some point you may want to create a pipeline - that is, a scalable, reproducible workflow so that you can run your scripts again and again in different environments and only have to change the inputs. This is where something like Nextflow is really useful - and you can check out their page [here](https://www.nextflow.io/index.html#GetStarted). 

it's real easy to set up:
```bash
# check that you have Java v 11 or later install with 
java --version

# if you don't have Java or the correct version, run:
curl -s "https://get.sdkman.io" | bash
source "/home/username/.sdkman/bin/sdkman-init.sh"
sdk install java 17.0.6-amzn
sdk install java 17.0.6-tem

# if you have Java v 11 or greater, you can go straight to the installation:
wget -qO- https://get.nextflow.io | bash
# a nextflow file should appear in your current directory, make it executable with
chmod +x nextflow
```
To find out more about Nextflow pipelines, you can visit the [nf-co.re site](https://nf-co.re/) 


### Integrating VSCode
Everyone has their favourite place to write and edit code - Atom, Sublime, Nano and even Vim (those folks are wild) - but VSCode is a favourite for a reason and you can use it to edit directly through WSL by installing VSCode on your Windows machine and then installing the [Remote Extension Development Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension through the VSCode extensions tab. In WSL, run `sudo apt update && updrade` and you can now run `code .` to open your current directory in VSCode. VSCode also integrates well with the [Fluent Terminal Extension](Xherdi.fluent-terminal) that allows you to go in the opposite direction and open any directory open in VSCode in Fluent Terminal. 

________________________________

Utilising  a graphics card to take advantage of libraires like TensorFlow is another useful tool to have set up in your environment and GPU Nvidia Drivers and CUDA will be covered in a separate post... For now you should have a foundational and well-integrated environment set up and you are ready to start downloading BiX tools, writing scripts and analysing data! 



