# Anirniq Avionics

Super repository for all Anirniq modules.

---

## Table of Contents

- [Module requirements](#module-requirements)
  - [LEDs](#leds)
- [Configuring your development environment](#configuring-your-development-environment)
  - [Windows](#windows)
    - [Installing MSys](#installing-msys)
    - [Cloning the project folder](#cloning-the-project-folder)
      - [Adding SSH keys to your GitHub account](#adding-an-SSH-Key-to-your-GitHub-account)
      - [Cloning the super-repo and submodules](#cloning-the-super-repo-and-submodules)
    - [Configuring Atollic](#configuring-atollic)
  - [Linux](#linux)

---

## Module requirements

### LEDs

Each Anirniq module has 5 LEDs, laid out as follows:

```
 [V] [1] [2] [3] [4]
```

| LED | Usage |
|-----|-------|
| V   | Power indicator |
| 1   | Module-specific |
| 2   | Module-specific |
| 3   | SD write indicator |
| 4   | Heartbeat |


---

## Configuring your development environment

### Windows

Required tools:

- Atollic TrueStudio for STM32 ([link](https://atollic.com/truestudio/))
- Gitkraken ([link](https://www.gitkraken.com/)), or any other Git client
- MSys

#### Installing MSys

Download the MinGW-get setup from the following [link](https://osdn.net/projects/mingw/downloads/68260/mingw-get-setup.exe) and install it on your machine.

Once installed, launch MinGW-get and check 'msys-base-bin'. Then go to `File > Apply changes` and wait for files to be downloaded and installed.

![Installing msys-base](https://i.imgur.com/F0EWX5u.png)

This should've installed MSys in `C:/MinGW/msys/1.0/`.

In the start menu, search for "environment" and select "Edit environment variables".

![Finding the 'Edit environment variable' tool](https://i.imgur.com/LbQXOl5.png)

Select "Path" and then "Edit".

![Environment variables screen](https://i.imgur.com/BdShwxC.png)

"Browse" and navigate to the bin folder in Msys (should be `C:/MinGW/msys/1.0/bin`) and then Ok.

![Adding a directory to PATH](https://i.imgur.com/FudcLtM.png)

To test that everything is working as expected, open a new CMD. Press `Windows-R` and type `CMD` in the text field that appears.

In the terminal, type `make`. If you get an error that "'make' is not recognized", something is wrong.  
If you see `make: *** No targets specified and no makefile found.  Stop.`, everything is working normally.

![Launching make in the cmd](https://i.imgur.com/N1AM4o7.png)

#### Cloning the project folders

This repository is a super-repo composed of various git submodules. There is one submodule per project, plus a "shared" repo for shared code. Doing this ensures that if you clone the top-level repo (`anirniq`), all submodules will be placed and named correctly relatively to one another. This simplifies `#including` code from other repositories.

This submodules are defined in the .gitmodules file using SSH URLs, so you will need to add an SSH key to your GitHub account.

##### Adding an SSH Key to your GitHub account

(These instructions are only valid for GitKraken.)

Press the "hamburger icon" in the top right corner and select "Preferences". On the menu to the left, select "Authentification".

Press the "Generate" button, and select the folder in which you wish to save the SSH key pair. You can choose any folder you want, but make sure to save it somewhere where you wont accidentally delete it, as this would break many things. (The recommended, standard folder is `C:/Users/<username>/.ssh`).

![Generating an SSH Key in GitKraken](https://i.imgur.com/wnWjVfo.png)

Once the SSH key pair is generated, press the clipboard icon next to the public key path.

Go to your GitHub account, and in the top right corner select "Settings". On the left menu, select "SSH and GPG keys" and finally press "Add SSH Key".

Paste the content of your clipboard in the "SSH key" text area, and give your key a name (in the "title" text input) that will help you identify this computer.

![Adding an SSH key to GitHub](https://i.imgur.com/HDaf7b4.png)

(Your ssh key should look something like `ssh-rsa ...`. If it doesn't, make sure the keys are generated correctly, and that you press the clipboard icon in GitKraken.)

##### Cloning the super-repo and submodules

In the GitKraken home screen, select "clone". In the URL field, put `git@github.com:club-rockets/anirniq.git`, and change the "Where to clone to" to the folder of your choice (this is where the code will sit in your hard drive, I recommend `C:/Users/<username>/Documents/RockETS/`)

![Cloning a repo with GitKraken](https://i.imgur.com/vLizV3b.png)

Once the clone operation is complete, open the repo. If GitKraken prompts you to "Initialize submodules", press "Yes". Otherwise, you will find a list of submodules in the left sidebar. For each submodules, click on it's name, and select "Initialize this submodule" in the slideout menu that appears.

![Initializing a submodule with GitKraken](https://i.imgur.com/ZlZLEex.png)

These submodules can be used as normal Git repositories. Before starting to code, make sure you open them ("Open this submodule") and checkout the correct branch. Don't forget that you will need to checkout the correct branch/commit in your board's repo **AND** in the shared repo.

#### Configuring Atollic

If you have not restarted Atollic since editing the environment variables, do so now.

In Atollic, go to `File > Import` and then `C/C++ > Existing Code as Makefile Project`.

In the "Source code folder" field, browse to your board's repo (e.g.: `C:/Users/<username>/Documents/RockETS/anirniq/mission`)  
In the "Language group" check only `C` (not `C++`)  
In the "Toolchain for Indexer Settings" menu, select "Atollic ARM tools"  

You should have something like this (except for the path that should begin with `C:/`, the path shown here is a Unix path):

![Importing a project in Atollic](https://i.imgur.com/SaJlEfE.png)

Press "Finish".

Once the project is imported in your workspace, right-click on the project name and select "Properties".

Expand the "C/C++ Build" element and select "Toolchain Editor".

"Current toolchain" should already be set to "Atollic ARM Tools"
Change "Current builder" from "CDT Internal Builder" to "GNU Make Builder"

![Changing the builder in Atollic](https://i.imgur.com/AuYcBcY.png)

Next, select "Settings" (still under "C/C++ Build") and in the Target device menu, select `STM32F4 > MCUs > STM32F407VG.

![Selecting the right compilation target](https://i.imgur.com/b35EWYM.png)

In the menu on the left, go back to "C/C++ Build" and uncheck the "Generate Makefiles automatically option.

![Telling Atollic to not generate makefiles](https://i.imgur.com/DSi1wZd.png)

Under the "Behavior" tab, check "Enable Parallel Build" and "Use optimal jobs". This will make compilation much faster.

![Enabling parallel build](https://i.imgur.com/P9CMMaE.png)

Ok. You should now be able to press the build button (hammer icon in the top toolbar), and get a succesful build. Your CDT Build console should look something like this:

![Example of a successful build](https://i.imgur.com/BdRLchi.png)

### Linux

If you do not have a SSH key: `ssh-keygen`  
Otherwise just `cat ~/.ssh/id_rsa.pub` and add it to your GitHub account.  

To clone the repo: `git clone git@github.com:club-rockets/anirniq`  
To initialize all submodules: `git submodules update --init --recursive`

To compile: make sure you have `arm-none-eabi-gcc` or `arm-atollic-eabi-gcc` (comes with Atollic) in your `$PATH`.  

`make` or `make -jX` where X is the number of cores your computer has.