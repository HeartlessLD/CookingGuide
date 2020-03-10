# CookingGuide
个人做菜攻略
学习王刚教学视频做一些简单的总结

This Wiki page provides information about CEF branches and instructions for downloading, building and packaging source code.

**Note to Editors: Changes made to this Wiki page without prior approval via the [CEF Forum](http://magpcss.org/ceforum/) or[ Issue Tracker](https://bitbucket.org/chromiumembedded/cef/issues?status=new&status=open) may be lost or reverted.**

***
[TOC]
***

# Background

The CEF project is an extension of the Chromium project hosted at http://www.chromium.org. CEF maintains development and release branches that track Chromium branches. CEF source code can be built manually or with automated tools.

# Development

Ongoing development of CEF occurs in the [CEF master branch](https://bitbucket.org/chromiumembedded/cef/src?at=master). This location tracks the current [Chromium master branch](https://chromium.googlesource.com/chromium/src.git) and is not recommended for production use.

Current CEF master branch build requirements are as follows. See the [MasterBuildQuickStart](https://bitbucket.org/chromiumembedded/cef/wiki/MasterBuildQuickStart.md) Wiki page for a development build quick-start guide.

Windows Build Requirements | macOS Build Requirements | Linux Build Requirements |
|:---------------------------|:----------------------------|:-------------------------|
Win 7+, VS2015u3, Win10.0.14393 SDK, Ninja | macOS 10.9-10.12, 10.9+ build system, 10.9+ deployment target, 10.10 base SDK, Xcode 8.3, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |

The following URLs should be used for downloading development versions of CEF.

  * CEF3 - https://bitbucket.org/chromiumembedded/cef/src?at=master

CEF1 is no longer actively developed or supported. See the [CEF1 Retirement Plan](http://magpcss.org/ceforum/viewtopic.php?f=10&t=10647) for details.

# Release Branches

CEF branches are created to track each Chromium release milesone (MXX) branch. Users developing applications for production environments are encouraged to use release branches for the following reasons:

  * Binary CEF builds are tied to specific Chromium releases.
  * Release versions of CEF/Chromium are better tested and more appropriate for release applications.
  * Within a release branch the CEF API is "frozen" and generally only security/bug fixes are applied.
  * CEF release branches can include patches to Chromium/Blink source if necessary.
  * CEF master development won't interfere with consumer release schedules.

Modern CEF release version numbers have the format X.YYYY.A.gHHHHHHH where:

  * "X" is the CEF major version (currently 3).
  * "YYYY" is the Chromium branch.
  * "A" is an incremental number representing the number of commits in the current branch. This is roughly equivalent to the SVN revision number but on a per-branch basis and assists people in quickly determining the order of builds in the same branch (for bug reports, etc).
  * "gHHHHHHH" is the 7-character abbreviation for the Git commit hash. This facilitates lookup of the relevant commit history in Git.

Detailed Chromium and CEF version information is available in the include/cef\_version.h header file that will be created during the build process or by loading the “about:version” URL in a CEF-derived application.

CEF release branches and associated platform build requirements are as follows.

## Current Release Branches (Supported)

Support for newer branches begins when they enter the Chromium beta channel. Support for older branches ends when they exit the Chromium stable channel. The [Spotify automated builder](http://opensource.spotify.com/cefbuilds/index.html) provides CEF builds for the current Chromium stable channel and will switch to the next Chromium branch when that branch is promoted to the stable channel. Updating CEF branches is currently a manual process so there will likely be a delay between [Chromium release announcements](http://googlechromereleases.blogspot.com/) and the availability of associated CEF builds. See the [Chromium release calendar](https://www.chromium.org/developers/calendar) for estimated Chromium release dates and versions.

| Branch Date | Release Branch | Chromium Version | Windows Build Requirements | macOS Build Requirements | Linux Build Requirements |
|:------------|:---------------|:-----------------|:---------------------------|:----------------------------|:-------------------------|
| Mar 2017    | [3029](https://bitbucket.org/chromiumembedded/cef/src/3029?at=3029) | 58               | Win 7+, VS2015u3, Win10.0.14393 SDK, Ninja | macOS 10.9-10.12, 10.9+ build system, 10.9+ deployment target, 10.10 base SDK, Xcode 8.3, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |
| Jan 2017    | [2987](https://bitbucket.org/chromiumembedded/cef/src/2987?at=2987) | 57               | Win 7+, VS2015u3, Win10.0.14393 SDK, Ninja | macOS 10.9-10.12, 10.9+ build system, 10.9+ deployment target, 10.10 base SDK, Xcode 7.3.1, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |

## Legacy Release Branches (Unsupported)

Legacy CEF builds are available from the [Spotify automated builder](http://opensource.spotify.com/cefbuilds/index.html) back to 2704 branch and from the [Adobe automated builder](https://cefbuilds.com/) back to 1453 branch. Building legacy branches is not supported. If you choose to build a legacy branch you will need to solve any build errors on your own. Newer legacy branches (within the last few months) can often be built using the same tooling as current branches. Older legacy branches can potentially be built by downloading a CEF source archive at the desired branch from [here](https://bitbucket.org/chromiumembedded/cef/downloads?tab=branches) and a Chromium source archive at the associated/required version from [here](https://gsdview.appspot.com/chromium-browser-official/), and then combining them to create the required directory structure.

| Branch Date | Release Branch | Chromium Version | CEF1 | CEF3 | Windows Build Requirements | macOS Build Requirements | Linux Build Requirements |
|:------------|:---------------|:-----------------|:-----|:-----|:---------------------------|:----------------------------|:-------------------------|
| Nov 2016    | [2924](https://bitbucket.org/chromiumembedded/cef/src/2924?at=2924) | 56               | No   | Yes | Win 7+, VS2015u3, Win10.0.10586 SDK, Ninja | macOS 10.9-10.12, 10.9+ build system, 10.9+ deployment target, 10.10 base SDK, Xcode 7.3.1, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |
| Oct 2016    | [2883](https://bitbucket.org/chromiumembedded/cef/src/2883?at=2883) | 55               | No   | Yes | Win 7+, VS2015u3, Win10.0.10586 SDK, Ninja | macOS 10.9-10.12, 10.9+ build system, 10.7+ deployment target, 10.10 base SDK, Xcode 7.3.1, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |
| Aug 2016    | [2840](https://bitbucket.org/chromiumembedded/cef/src/2840?at=2840) | 54               | No   | Yes | Win 7+, VS2015u2 or VS2015u3, Win10.0.10586 SDK, Ninja | macOS 10.9-10.12, 10.9+ build system, 10.7+ deployment target, 10.10 base SDK, Xcode 7.3.1, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |
| Jul 2016    | [2785](https://bitbucket.org/chromiumembedded/cef/src/2785?at=2785) | 53               | No   | Yes | Win 7+, VS2015u2 or VS2015u3, Win10.0.10586 SDK, Ninja | macOS 10.9-10.11, 10.9+ build system, 10.7+ deployment target, 10.10 base SDK, Xcode 7.3.1, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |
| May 2016    | [2743](https://bitbucket.org/chromiumembedded/cef/src/2743?at=2743) | 52               | No   | Yes  | Win 7+, VS2015u2 or VS2015u3, Win10.0.10586 SDK, Ninja | macOS 10.9-10.11, 10.9+ build system, 10.7+ deployment target, 10.10 base SDK, Xcode 7.1.1-7.3.1, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |
| Apr 2016    | [2704](https://bitbucket.org/chromiumembedded/cef/src/2704?at=2704) | 51               | No   | Yes  | Win 7+, VS2015u2, Win10.0.10586 SDK, Ninja | macOS 10.9-10.11, 10.9+ build system, 10.7+ deployment target, 10.10 base SDK, Xcode 7.1.1, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |
| Jan 2016    | [2623](https://bitbucket.org/chromiumembedded/cef/src/2623?at=2623) | 49               | No   | Yes  | WinXP+, VS2013u4 or VS2015u1 (experimental), Win10 SDK, Ninja | macOS 10.6-10.11, 10.7+ build system, 10.6+ deployment target, 10.10 base SDK, Xcode 7.1.1, Ninja, 64-bit only | Ubuntu 14.04+, Debian Wheezy+, Ninja |
| Oct 2015    | [2526](https://bitbucket.org/chromiumembedded/cef/src/2526?at=2526) | 47               | No   | Yes  | WinXP+, VS2013u4 or VS2015u1 (experimental), Win8.1 SDK, Ninja | macOS 10.6-10.11, 10.6+ deployment target, 10.10 base SDK, Xcode 6.1, Ninja, 64-bit only | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Jul 2015    | [2454](https://bitbucket.org/chromiumembedded/cef/src/2454?at=2454) | 45               | No   | Yes  | WinXP+, VS2013u4, Win8.1 SDK, Ninja | macOS 10.6-10.10, 10.6+ deployment target, 10.9 base SDK, Xcode 6.1, Ninja, 64-bit only | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Apr 2015    | [2357](https://bitbucket.org/chromiumembedded/cef/src/2357?at=2357) | 43               | No   | Yes  | WinXP+, VS2013u4, Win8.1 SDK, Ninja | macOS 10.6-10.10, 10.6+ deployment target, 10.9 base SDK, Xcode 6.1, Ninja, 64-bit only | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Jan 2015    | [2272](https://bitbucket.org/chromiumembedded/cef/src/2272?at=2272) | 41               | No   | Yes  | WinXP+, VS2013u4, Win8.1 SDK, Ninja | macOS 10.6-10.10, 10.6+ deployment target, 10.9 base SDK, Xcode 6.1, Ninja, 64-bit only | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Oct 2014    | [2171](https://bitbucket.org/chromiumembedded/cef/src/2171?at=2171) | 39               | No   | Yes  | WinXP+, VS2013u4, Win8.1 SDK, Ninja | macOS 10.6-10.9, 10.6+ SDK, Xcode 5.1.1, Ninja | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Aug 2014    | [2062](https://bitbucket.org/chromiumembedded/cef/src/2062?at=2062) | 37               | No   | Yes  | WinXP+, VS2013, Win8 SDK, Ninja | macOS 10.6-10.9, 10.6+ SDK, Xcode 5.1.1, Ninja | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Apr 2014    | [1916](https://bitbucket.org/chromiumembedded/cef/src/1916?at=1916) | 35               | No   | Yes  | WinXP+, VS2013, Win8 SDK, Ninja | macOS 10.6-10.9, 10.6+ SDK, Xcode 5.1.1, Ninja | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Jan 2014    | [1750](https://bitbucket.org/chromiumembedded/cef/src/1750?at=1750) | 33               | No   | Yes  | WinXP+, VS2010-2013, Win8 SDK, Ninja | macOS 10.6-10.9, 10.6+ SDK, Xcode 5.1.1, Ninja | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Oct 2013    | [1650](https://bitbucket.org/chromiumembedded/cef/src/1650?at=1650) | 31               | No   | Yes  | WinXP+, VS2010-2012, Win8 SDK, Ninja (optional) | macOS 10.6-10.9, 10.6+ SDK, Xcode 5.1.1, Ninja | Ubuntu 12.04+, Debian Wheezy+, Ninja |
| Jul 2013    | [1547](https://bitbucket.org/chromiumembedded/cef/src/1547?at=1547) | 29               | No   | Yes  | WinXP+, VS2010-2012, Win8 SDK, Ninja (optional) | macOS 10.6-10.8, 10.6+ SDK, Xcode 3.2.6-4.x, Ninja (optional) | Ubuntu 12.04+, Debian Squeeze+, Ninja |
| Apr 2013    | [1453](https://bitbucket.org/chromiumembedded/cef/src/1453?at=1453) | 27               | Yes  | Yes  | WinXP+, VS2010, Win8 SDK, Ninja (optional) | macOS 10.6-10.8, 10.6+ SDK, Xcode 3.2.6-4.x, Ninja (optional) | Ubuntu 12.04+, Debian Squeeze+, Ninja (optional) |
| Jan 2013    | [1364](https://bitbucket.org/chromiumembedded/cef/src/1364?at=1364) | 25               | Yes  | Yes  | WinXP+, VS2010, Win8 SDK, Ninja (optional) | macOS 10.6-10.8, Xcode 3.2.6-4.x, Ninja (optional) | Ubuntu 12.04+, Debian Squeeze+, Ninja (optional) |
| Oct 2012    | [1271](https://bitbucket.org/chromiumembedded/cef/src/1271?at=1271) | 23               | Yes  | Yes  | WinXP+, VS2010, Win7 SDK   | macOS 10.6-10.8, 10.6+ SDK, Xcode 3.2.6-4.x | Ubuntu 12.04+, Debian Squeeze+ |
| Aug 2012    | [1180](https://bitbucket.org/chromiumembedded/cef/src/1180?at=1180) | 21               | Yes  | Yes  | WinXP+, VS2010, Win7 SDK   | macOS 10.6-10.7, 10.5+ SDK, Xcode 3.2.6-4.x | Ubuntu 12.04+, Debian Squeeze+ |
| Apr 2012    | [1084](https://bitbucket.org/chromiumembedded/cef/src/1084?at=1084) | 19               | Yes  | No   | WinXP+, VS2008, Win7 SDK   | macOS 10.6-10.7, 10.5+ SDK, Xcode 3.2.6-4.x | Ubuntu 10.04+, Debian Squeeze+ |
| Feb 2012    | [1025](https://bitbucket.org/chromiumembedded/cef/src/1025?at=1025) | 18               | Yes  | No   | WinXP+, VS2008, Win7 SDK   | macOS 10.6-10.7, 10.5+ SDK, Xcode 3.2.6-4.x | Ubuntu 10.04+, Debian Squeeze+ |
| Dec 2011    | [963](https://bitbucket.org/chromiumembedded/cef/src/963?at=963) | 17               | Yes  | No   | WinXP+, VS2008, Win7 SDK   | macOS 10.6-10.7, 10.5+ SDK, Xcode 3.2.6 | Ubuntu 10.04+, Debian Squeeze+ |

The following URL should be used for downloading release versions of CEF where YYYY is the release branch number.

  * https://bitbucket.org/chromiumembedded/cef/src/YYYY?at=YYYY

Note that 1025 and older branches contain only CEF1 source code and that 1547 and newer branches contain only CEF3 source code.

# Building from Source

Building from source code is currently supported on Windows, Mac macOS and Linux platforms. Use of the Automated Method described below is recommended. Building the current CEF/Chromium master branch for local development is described on the [MasterBuildQuickStart](https://bitbucket.org/chromiumembedded/cef/wiki/MasterBuildQuickStart.md) Wiki page. Building the current CEF/Chromium stable branch automatically for production use is described on the  [AutomatedBuildSetup](https://bitbucket.org/chromiumembedded/cef/wiki/AutomatedBuildSetup.md#markdown-header-linux-configuration) Wiki page. For other branches see the build requirements listed in the “Release Branches” section above and the “Build Notes” section below.

## Automated Method

CEF provides tools for automatically downloading, building and packaging Chromium and CEF source code. These tools are the recommended way of building CEF locally and can also be integrated with automated build systems as described on the [AutomatedBuildSetup](https://bitbucket.org/chromiumembedded/cef/wiki/AutomatedBuildSetup.md) Wiki page. See the [MasterBuildQuickStart](https://bitbucket.org/chromiumembedded/cef/wiki/MasterBuildQuickStart.md) Wiki page for an example of the recommended workflow for local development builds.

These steps apply to the Git workflow only. The Git workflow is recommended for all users and supports CEF3 master and newer CEF3 release branches (1750+).

1\. Download the [automate-git.py script](https://bitbucket.org/chromiumembedded/cef/raw/master/tools/automate/automate-git.py). Use the most recent master version of this script even when building release branches.

2\. On Linux: Chromium requires that certain packages be installed. You can install them by running the [install-build-deps.sh script](https://bitbucket.org/chromiumembedded/cef/wiki/MasterBuildQuickStart.md#markdown-header-linux-setup) or by [explicitly running](https://bitbucket.org/chromiumembedded/cef/wiki/AutomatedBuildSetup.md#markdown-header-linux-configuration) the necessary installation commands.

3\. Run the automate-git.py script at whatever interval is appropriate (for each CEF commit, once per day, once per week, etc).

To build master:

```
python /path/to/automate/automate-git.py --download-dir=/path/to/download
```

To build a release branch:

```
python /path/to/automate/automate-git.py --download-dir=/path/to/download --branch=2785
```

By default the script will download depot\_tools, Chromium and CEF source code, run Debug and Release builds of CEF, and create a binary distribution package containing the build artifacts in the “/path/to/download/chromium/src/cef/binary\_distrib” directory. Future runs of the script will perform the minimum work necessary (unless otherwise configured using command-line flags). For example, if there are no pending CEF or Chromium updates the script will do nothing.

If you run the script and CEF or Chromium updates are pending the “/path/to/download/chromium/src/cef” directory will be removed and replaced with a clean copy from “/path/to/download/cef\_(branch)” (specify the `--no-update` command-line flag to disable updates). Make sure to back up any changes that you made in the “/path/to/download/chromium/src/cef” directory before re-running the script.

The same download directory can be used for building multiple CEF branches (just specify a different `--branch` command-line value). The existing “/path/to/download/chromium/src/out” directory will be moved to “/path/to/download/out\_(previousbranch)” so that the build output from the previous branch is not lost. When you switch back to a previous branch the out directory will be restored to its original location.

The script will create a 32-bit build on Windows by default. To create a 64-bit build on Windows, Mac macOS or Linux specify the `--x64-build` command-line flag. 32-bit builds on Mac macOS are [no longer supported](https://groups.google.com/a/chromium.org/d/msg/chromium-dev/sdsDCkq_zwo/yep65H8Eg3sJ) starting with 2272 branch so this flag is now required when building 2272+ on that platform.

If you receive Git errors when moving an existing checkout from one branch to another you can force a clean Chromium Git checkout (specify the  `--force-clean` command-line flag) and optionally a clean download of Chromium dependencies (specify the `--force-clean-deps` command-line flag). Any build output that currently exists in the “src/out” directory will be deleted. Re-downloading the Chromium dependencies can take approximately 30 minutes with a reasonably fast internet connection.

Add the `--help` command-line switch to output a complete list of supported command-line options.

## Manual Downloading

Chromium and CEF can be downloaded and built as a manual process. This is more complicated and is not recommended for most users. See the [MasterBuildQuickStart](https://bitbucket.org/chromiumembedded/cef/wiki/MasterBuildQuickStart.md) Wiki page for an example of the recommended developer workflow.

### Development

These instructions apply only to the development (master) version of CEF3 using the Git workflow. Complete instructions for using the Git workflow in Chromium are available [here](https://sites.google.com/a/chromium.org/dev/developers/how-tos/get-the-code).

1\. Install depot\_tools as described [here](http://www.chromium.org/developers/how-tos/install-depot-tools). To avoid potential problems make sure the path is as short as possible and does not contain spaces or special characters.

2\. Create a chromium checkout directory (for example, /path/to/chromium). To avoid potential problems make sure the path is as short as possible and does not contain spaces or special characters.

3\. View the CHROMIUM\_BUILD\_COMPATIBILITY.txt file in the CEF top-level directory to identify the Chromium Git commit hash that you need. This will change over time as CEF is updated to work with newer Chromium revisions.

4\. Download Chromium source code using the fetch tool included with depot\_tools. This step only needs to be performed the first time Chromium code is checked out.

```
cd /path/to/chromium
fetch --nohooks chromium
```

5\. Update the Chromium checkout to the required Git commit hash. DO NOT use 'git checkout commit\_hash' directly because the sub-project dependencies will not be updated.

```
cd /path/to/chromium
gclient sync --revision <commit_hash> --jobs 16
```

6\. Download CEF source code from the CEF Git repository to a "cef" directory inside the Chromium "src" directory. If Chromium code was downloaded to "/path/to/chromium/src" then CEF code should be downloaded to "/path/to/chromium/src/cef". Note that the directory must be named "cef".

```
cd /path/to/chromium/src
git clone https://bitbucket.org/chromiumembedded/cef.git
```

### Release Branch

These instructions apply only to release branches of CEF3 using the Git workflow. The Git workflow supports newer CEF3 release branches (1750+). Complete instructions for using the Git workflow in Chromium are available [here](https://sites.google.com/a/chromium.org/dev/developers/how-tos/get-the-code).

1\. Install depot\_tools as described [here](http://www.chromium.org/developers/how-tos/install-depot-tools). To avoid potential problems make sure the path is as short as possible and does not contain spaces or special characters.

2\. Create a chromium checkout directory (for example, /path/to/chromium). To avoid potential problems make sure the path is as short as possible and does not contain spaces or special characters.

3\. View the CHROMIUM\_BUILD\_COMPATIBILITY.txt file in the CEF top-level directory to identify what Chromium release version that you need. This will change over time as CEF is updated to work with newer Chromium release versions.

4\. Download Chromium source code using the fetch tool included with depot\_tools. This step only needs to be performed the first time Chromium code is checked out.

```
cd /path/to/chromium
fetch --nohooks chromium
```

5\. Download additional branch and tag information.

```
cd /path/to/chromium/src
gclient sync --nohooks --with_branch_heads
git fetch
git fetch --tags
```

6\. Check out the release version and update the third-party dependencies.

```
cd /path/to/chromium/src

# Check out the release version (“53.0.2785.89” in this example).
git checkout refs/tags/53.0.2785.89

# Update third-party dependencies.
gclient sync --jobs 16
```

7\. Download CEF source code from the CEF Git repository to a "cef" directory inside the Chromium "src" directory. If Chromium code was downloaded to "/path/to/chromium/src" then CEF code should be downloaded to "/path/to/chromium/src/cef". Note that the directory must be named "cef".

```
cd /path/to/chromium/src
git clone https://bitbucket.org/chromiumembedded/cef.git
cd cef

# Create a local branch tracking the remote branch (“2785” in this example).
git checkout -t origin/2785
```

## Manual Building

See the [MasterBuildQuickStart](https://bitbucket.org/chromiumembedded/cef/wiki/MasterBuildQuickStart.md) Wiki page for an example of the recommended developer workflow.

## Manual Packaging

After building both Debug and Release configurations you can use the make\_distrib tool (.bat on Windows, .sh on macOS and Linux) to create a binary distribution.

```
cd /path/to/chromium/src/cef/tools
./make_distrib.sh --ninja-build
```

If the process succeeds a binary distribution package will be created in the /path/to/chromium/src/cef/binary\_distrib directory.

See the [make\_distrib.py](https://bitbucket.org/chromiumembedded/cef/src/master/tools/make_distrib.py) script for additional usage options.

The resulting binary distribution can then be built using CMake and platform toolchains. See the README.txt file included with the binary distribution for more information.

## Build Notes

This section summarizes build-related requirements and options.

  * Building on most platforms will require at least 8GB of system memory.
  * [Ninja](https://code.google.com/p/chromium/wiki/NinjaBuild) must be used when building newer CEF/Chromium branches.
  * [GYP](https://gyp.gsrc.io/docs/UserDocumentation.md) is supported by 2785 branch and older. [GN](http://www.chromium.org/developers/gn-build-configuration) is supported by 2785 branch and newer, and required starting with 2840 branch. Set `CEF_USE_GN=1` to build 2785 branch with GN instead of GYP.
  * CEF does not support component builds (see [issue #1617](https://bitbucket.org/chromiumembedded/cef/issues/1617)).
  * To perform a 64-bit build on Windows (any branch) or macOS (branch 2171 or older) set `GYP_DEFINES=target_arch=x64` (GYP only) or build the `out/[Debug|Release]_GN_x64` target (GN only). To perform a 32-bit Linux build on a 64-bit Linux system see instructions on the [AutomatedBuildSetup](https://bitbucket.org/chromiumembedded/cef/wiki/AutomatedBuildSetup.md#markdown-header-linux-configuration) Wiki page.
  * To perform an “official” build set `GYP_DEFINES=buildtype=Official` (GYP only) or `GN_DEFINES=is_official_build=true` (GN only). This will disable debugging code and enable additional link-time optimizations in Release builds. See instructions on the [AutomatedBuildSetup](https://bitbucket.org/chromiumembedded/cef/wiki/AutomatedBuildSetup.md) Wiki page for additional official build recommendations.
  * Windows -
    * If multiple versions of Visual Studio are installed on your system you can set the GYP\_MSVS\_VERSION environment variable to create project files for that version. For example, set the value to "2013" for VS2013 or "2015" for VS2015. Check the Chromium documentation for the correct value when using other Visual Studio versions.
    * If you wish to use Visual Studio for debugging and compiling in combination with a Ninja build you can set `GYP_GENERATORS=ninja,msvs-ninja` (GYP only) or `GN_ARGUMENTS=--ide=vs2015 --sln=cef --filters=//cef/*` (GN only) to generate both Ninja and VS project files. Visual Studio is supported only for debugging and compiling individual source files -- it will not build whole targets successfully. You must use Ninja when building CEF/Chromium targets.
  * Mac macOS -
    * The combination of deployment target and base SDK version will determine the platforms supported by the resulting binaries. For proper functioning you must use the versions specified under build requirements for each branch.
    * 32-bit builds are no longer supported with 2272 branch and newer. See [here](https://groups.google.com/a/chromium.org/d/msg/chromium-dev/sdsDCkq_zwo/yep65H8Eg3sJ) for the Chromium announcement.
  * Linux -
    * CEF is developed and tested using Ubuntu 14.04 and Debian 7.8. It should be possible to build and run CEF on other compatible Linux distributions but this is untested.
    * The libgtkglext1-dev package is required in branch 1547 and newer to support the off-screen rendering example in cefclient. This is only a requirement for cefclient and not a requirement for other applications using CEF.
