This Wiki page provides a quick-start guide for creating a Debug build of CEF/Chromium using the current master (development) branch.

**Note to Editors: Changes made to this Wiki page without prior approval via the [CEF Forum](http://magpcss.org/ceforum/) or[ Issue Tracker](https://bitbucket.org/chromiumembedded/cef/issues?status=new&status=open) may be lost or reverted.**

***
[TOC]
***

# Overview

This page provides a quick-start guide for setting up a minimal development environment and building the master branch of Chromium/CEF for development purposes. For a comprehensive discussion of the available tools and configurations visit the [BranchesAndBuilding](https://bitbucket.org/chromiumembedded/cef/wiki/BranchesAndBuilding.md) Wiki page.

This guide is NOT intended for:

- Those seeking a prebuilt binary distribution for use in third-party apps. Go [here](http://magpcss.net/cef_downloads/) instead.
- Those seeking to build the binary distribution in a completely automated manner. Go [here](https://bitbucket.org/chromiumembedded/cef/wiki/BranchesAndBuilding.md#markdown-header-automated-method) instead.

Development systems can be configured using dedicated hardware or a [VMware](http://www.vmware.com/products/player), [Parallels](http://www.parallels.com/eu/products/desktop/download/) or [VirtualBox](https://www.virtualbox.org/wiki/Downloads) virtual machine.

The below steps can often be used to develop the most recent release branch of CEF/Chromium in addition to the master branch. Chromium build requirements change over time so review the build requirements listed on the [BranchesAndBuilding](https://bitbucket.org/chromiumembedded/cef/wiki/BranchesAndBuilding.md#markdown-header-release-branches) Wiki page before attempting to build a release branch. Then just add `--branch=XXXX` to the automate-git.py command-line where "XXXX" is the branch number you wish to build.

# File Structure

The same file structure will be used on all platforms. "~" can be any path that does not include spaces or special characters. We'll be building this directory structure for each platform in the following sections.

```
~/code/
  automate/
    automate-git.py   <-- CEF build script
  chromium_git/
    cef/              <-- CEF source checkout
    chromium/
      src/            <-- Chromium source checkout
    update.[bat|sh]   <-- Bootstrap script for automate-git.py
  depot_tools/        <-- Chromium build tools
```

With this file structure you can develop multiple CEF/Chromium branches side-by-side. For example, repeat the below instructions using "chromium_git1" as the directory name instead of "chromium_git".

# Windows Setup

**What's Required**

- Windows 7 or newer, 64-bit OS.
- Visual Studio 2015 Update 3 installed in the default location.
- [Windows 10.0.14393 SDK](https://dev.windows.com/en-us/downloads/windows-10-sdk) installed in the default location.
- At least 8GB of RAM and 40GB of free disk space.
- Approximately 2 hours with a fast internet connection (25Mbps) and fast build machine (2.6Ghz+, 4+ logical cores).

**Step-by-step Guide**

All of the below commands should be run using the system "cmd.exe" and not a Cygwin shell.

1\. Create the following directories.

```
c:\code\automate
c:\code\chromium_git
```

2\. Download [depot_tools.zip](https://storage.googleapis.com/chrome-infra/depot_tools.zip) and extract to "c:\code\depot_tools". Do not use drag-n-drop or copy-n-paste extract from Explorer, this will not extract the hidden ".git" folder which is necessary for depot_tools to auto-update itself. You can use "Extract all..." from the context menu though. [7-zip](http://www.7-zip.org/download.html) is also a good tool for this.

3\. Run "update_depot_tools.bat" to install Python, Git and SVN.

```
cd c:\code\depot_tools
update_depot_tools.bat
```

4\. Add the "c:\code\depot_tools" folder to your system PATH. For example, on Windows 10:

- Run the "SystemPropertiesAdvanced" command.
- Click the "Environment Variables..." button.
- Double-click on "Path" under "System variables" to edit the value.

5\. Download the [automate-git.py](https://bitbucket.org/chromiumembedded/cef/raw/master/tools/automate/automate-git.py) script to "c:\code\automate\automate-git.py".

6\. Create the "c:\code\chromium_git\update.bat" script with the following contents. Remove the GN_DEFINES line if you’re planning to create Release builds instead of Debug builds.

```
set CEF_USE_GN=1
set GN_DEFINES=is_win_fastlink=true
set GN_ARGUMENTS=--ide=vs2015 --sln=cef --filters=//cef/*
python ..\automate\automate-git.py --download-dir=c:\code\chromium_git --depot-tools-dir=c:\code\depot_tools --no-distrib --no-build
```

Run the "update.bat" script and wait for CEF and Chromium source code to download. CEF source code will be downloaded to "c:\code\chromium_git\cef" and Chromium source code will be downloaded to "c:\code\chromium_git\chromium\src". After download completion the CEF source code will be copied to "c:\code\chromium_git\chromium\src\cef".

```
cd c:\code\chromium_git
update.bat
```

7\. Create the "c:\code\chromium_git\chromium\src\cef\create.bat" script with the following contents. Remove the GN_DEFINES line if you’re planning to create Release builds instead of Debug builds.

```
set CEF_USE_GN=1
set GN_DEFINES=is_win_fastlink=true
set GN_ARGUMENTS=--ide=vs2015 --sln=cef --filters=//cef/*
call cef_create_projects.bat
```

Run the "create.bat" script to generate Ninja and Visual Studio project files.

```
cd c:\code\chromium_git\chromium\src\cef
create.bat
```

This will generate a "c:\code\chromium_git\chromium\src\out\Debug_GN_x86\cef.sln" file that can be loaded in Visual Studio for debugging and compiling individual files. Replace “x86” with “x64” in this path to work with the 64-bit build instead of the 32-bit build. Always use Ninja to build the complete project. Repeat this step if you change the project configuration or add/remove files in the GN configuration (BUILD.gn file).

8\. Create a Debug build of CEF/Chromium using Ninja. Replace “x86” with “x64” in the below example to generate a 64-bit build instead of a 32-bit build. Edit the CEF source code at "c:\code\chromium_git\chromium\src\cef" and repeat this step multiple times to perform incremental builds while developing.

```
cd c:\code\chromium_git\chromium\src
ninja -C out\Debug_GN_x86 cef
```

9\. Run the resulting cefclient sample application.

```
cd c:\code\chromium_git\chromium\src
out\Debug_GN_x86\cefclient.exe
```

See the [Windows debugging guide](https://www.chromium.org/developers/how-tos/debugging-on-windows) for detailed debugging instructions.

# Mac OS X Setup

**What's Required**

- OS X 10.10 or newer.
- Xcode 8.3.
- At least 8GB of RAM and 40GB of free disk space.
- Approximately 2 hours with a fast internet connection (25Mbps) and fast build machine (2.6Ghz+, 4+ logical cores).

**Step-by-step Guide**

In this example "~" is "/Users/marshall". Note that in some cases the absolute path must be used. Environment variables described in this section can be added to your "~/.bash_profile" file to persist them across sessions.

1\. Create the following directories.

```
mkdir ~/code/automate
mkdir ~/code/chromium_git
```

2\. Download "~/code/depot_tools" using Git.

```
cd ~/code
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

3\. Add the "~/code/depot_tools" directory to your PATH. Note the use of an absolute path here.

```
export PATH=/Users/marshall/code/depot_tools:$PATH
```

4\. Download the [automate-git.py](https://bitbucket.org/chromiumembedded/cef/raw/master/tools/automate/automate-git.py) script to "~/code/automate/automate-git.py".

5\. Create the "~/code/chromium_git/update.sh" script with the following contents.

```
#!/bin/bash
export CEF_USE_GN=1
python ../automate/automate-git.py --download-dir=/Users/marshall/code/chromium_git --depot-tools-dir=/Users/marshall/code/depot_tools --no-distrib --no-build --x64-build
```

Give it executable permissions.

```
cd ~/code/chromium_git
chmod 755 update.sh
```

Run the "update.sh" script and wait for CEF and Chromium source code to download. CEF source code will be downloaded to "~/code/chromium_git/cef" and Chromium source code will be downloaded to "~/code/chromium_git/chromium/src". After download completion the CEF source code will be copied to "~/code/chromium_git/chromium/src/cef".

```
cd ~/code/chromium_git
./update.sh
```

6\. Create the "~/code/chromium_git/chromium/src/cef/create.sh" script with the following contents.

```
#!/bin/bash
export CEF_USE_GN=1
./cef_create_projects.sh
```

Give it executable permissions.

```
cd ~/code/chromium_git/chromium/src/cef
chmod 755 create.sh
```

Run the "create.sh" script to create Ninja project files. Repeat this step if you change the project configuration or add/remove files in the GN configuration (BUILD.gn file).

```
cd ~/code/chromium_git/chromium/src/cef
./create.sh
```

7\. Create a Debug build of CEF/Chromium using Ninja. Edit the CEF source code at "~/code/chromium_git/chromium/src/cef" and repeat this step multiple times to perform incremental builds while developing.

```
cd ~/code/chromium_git/chromium/src
ninja -C out/Debug_GN_x64 cef
```

8\. Run the resulting cefclient sample application.

```
cd ~/code/chromium_git/chromium/src
open out/Debug_GN_x64/cefclient.app
```

See the [Mac OS X debugging guide](https://www.chromium.org/developers/how-tos/debugging-on-os-x) for detailed debugging instructions.

# Linux Setup

**What's Required**

- [Ubuntu 14.04 LTS 64-bit](http://www.ubuntu.com/download/desktop) is recommended. To build on Debian 7 64-bit see the [BuildingOnDebian7](https://bitbucket.org/chromiumembedded/cef/wiki/BuildingOnDebian7.md) Wiki page for additional instructions. Building with other versions or distros has not been tested and may experience issues.
- At least 6GB of RAM and 40GB of free disk space.
- Approximately 2 hours with a fast internet connection (25Mbps) and fast build machine (2.6Ghz+, 4+ logical cores).

**Step-by-step Guide**

In this example "~" is "/home/marshall". Note that in some cases the absolute path must be used. Environment variables described in this section can be added to your "~/.profile" or "~/.bashrc" file to persist them across sessions.

1\. Create the following directories.

```
mkdir ~/code/automate
mkdir ~/code/chromium_git
```

2\. Download and run "~/code/install-build-deps.sh" to install build dependencies. Answer Y (yes) to all of the questions.

```
cd ~/code
curl 'https://chromium.googlesource.com/chromium/src/+/master/build/install-build-deps.sh?format=TEXT' | base64 -d > install-build-deps.sh
chmod 755 install-build-deps.sh
sudo ./install-build-deps.sh
```

3\. Install the "libgtkglext1-dev" package required by the cefclient sample application.

```
sudo apt-get install libgtkglext1-dev
```

4\. Download "~/code/depot_tools" using Git.

```
cd ~/code
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

5\. Add the "~/code/depot_tools" directory to your PATH. Note the use of an absolute path here.

```
export PATH=/home/marshall/code/depot_tools:$PATH
```

6\. Download the "~/automate/automate-git.py" script.

```
cd ~/code/automate
wget https://bitbucket.org/chromiumembedded/cef/raw/master/tools/automate/automate-git.py
```

7\. Create the "~/code/chromium_git/update.sh" script with the following contents.

```
#!/bin/bash
export CEF_USE_GN=1
python ../automate/automate-git.py --download-dir=/home/marshall/code/chromium_git --depot-tools-dir=/home/marshall/code/depot_tools --no-distrib --no-build
```

Give it executable permissions.

```
cd ~/code/chromium_git
chmod 755 update.sh
```

Run the "update.sh" script and wait for CEF and Chromium source code to download. CEF source code will be downloaded to "~/code/chromium_git/cef" and Chromium source code will be downloaded to "~/code/chromium_git/chromium/src".  After download completion the CEF source code will be copied to "~/code/chromium_git/chromium/src/cef".

```
cd ~/code/chromium_git
./update.sh
```

8\. Create the "~/code/chromium_git/chromium/src/cef/create.sh" script with the following contents.

```
#!/bin/bash
export CEF_USE_GN=1
./cef_create_projects.sh
```

Give it executable permissions.

```
cd ~/code/chromium_git/chromium/src/cef
chmod 755 create.sh
```

Run the "create.sh" script to create Ninja project files. Repeat this step if you change the project configuration or add/remove files in the GN configuration (BUILD.gn file).

```
cd ~/code/chromium_git/chromium/src/cef
./create.sh
```

9\. Create a Debug build of CEF/Chromium using Ninja. Edit the CEF source code at "~/code/chromium_git/chromium/src/cef" and repeat this step multiple times to perform incremental builds while developing. Note the additional "chrome_sandbox" target required by step 10.

```
cd ~/code/chromium_git/chromium/src
ninja -C out/Debug_GN_x64 cef chrome_sandbox
```

10\. Set up the [Linux SUID sandbox](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_suid_sandbox_development.md).

```
# This environment variable should be set at all times.
export CHROME_DEVEL_SANDBOX=/usr/local/sbin/chrome-devel-sandbox

# This command only needs to be run a single time.
cd ~/code/chromium_git/chromium/src
sudo ./build/update-linux-sandbox.sh BUILDTYPE=Debug_GN_x64
```

11\. Run the cefclient sample application.

```
cd ~/code/chromium_git/chromium/src
./out/Debug_GN_x64/cefclient
```

See the [Linux debugging guide](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_debugging.md) for detailed debugging instructions.

# Next Steps

- If you're seeking a good code editor on Linux check out the [Eclipse](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_eclipse_dev.md) and [Emacs](https://chromium.googlesource.com/chromium/src/+/master/docs/emacs.md) tutorials.
- Review the [Tutorial](https://bitbucket.org/chromiumembedded/cef/wiki/Tutorial.md) and [GeneralUsage](https://bitbucket.org/chromiumembedded/cef/wiki/GeneralUsage.md) Wiki pages for details on CEF implementation and usage.
- Review the Chromium debugging guide for [Windows](https://www.chromium.org/developers/how-tos/debugging-on-windows), [Mac OS X](https://www.chromium.org/developers/how-tos/debugging-on-os-x) or [Linux](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_debugging.md).
- When you’re ready to contribute your changes back to the CEF project see the [ContributingWithGit](https://bitbucket.org/chromiumembedded/cef/wiki/ContributingWithGit.md) Wiki page for instructions on creating a pull request.
