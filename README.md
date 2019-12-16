# Notes on Darkvater fork #

Armbian officially pulled the plug on debian jessie (and cubieboard) support back in November 2019. Unfortunately all the legacy images were also pruned with that so it is not possible to download sunxi-legacy kernel for the cubieboard. This is required if one wants to use on-board MMC card. This fork uses the last known version that supports the cubieboard and patches the build to support cubian.
Usage is the same as the official build (see below), with two additional changes.
* the private key for the local apt repo needs to be added. This is done with ```gpg --allow-secret-key-import --import packages/extras-buildpkgs/buildpkg.gpg```
* as the armbian packages for jessie no longer exist they need to be compiled, use the ```EXTERNAL_NEW="compile"``` flag. Note that the docker flow doesn't support this, so you have to build it on the host

**My Config**

I did the following to compile my cubieboard with debian jessie
```./compile.sh  BOARD="cubieboard" BRANCH="default" RELEASE="jessie" BUILD_MINIMAL="yes" BUILD_DESKTOP="no" KERNEL_CONFIGURE="no" KERNEL_ONLY="no" INSTALL_HEADERS="yes" COMPRESS_OUTPUTIMAGE="7z" SEVENZIP="yes" PROGRESS_LOG_TO_FILE="yes" BSPFREEZE="yes" EXTERNAL="yes" CLEAN_LEVEL="images" EXTERNAL_NEW="compile" EXTERNAL="yes"```

Once this is done, the docker flow can be used as long as ```CLEAN_LEVEL``` is not set to "extras" which deletes the armbian packages that were just built previously. To use the docker flow, see for example below (assumes rebuild of kernel)
```./compile.sh docker BOARD="cubieboard" BRANCH="default" RELEASE="jessie" BUILD_MINIMAL="yes" BUILD_DESKTOP="no" KERNEL_CONFIGURE="no" KERNEL_ONLY="no" INSTALL_HEADERS="yes" COMPRESS_OUTPUTIMAGE="7z" SEVENZIP="yes" PROGRESS_LOG_TO_FILE="yes" BSPFREEZE="yes" CLEAN_LEVEL="make,debs,oldcache"```

# Armbian #

Debian based Linux for ARM based single-board computers
  
[https://www.armbian.com](https://www.armbian.com "Armbian")


# How to build an image or a kernel?

Supported build environment is **Ubuntu Bionic 18.04 x64** ([minimal iso image](http://archive.ubuntu.com/ubuntu/dists/bionic/main/installer-amd64/current/images/netboot/mini.iso)).

- guest inside a [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or other virtualization software,
- guest managed by [Vagrant](https://docs.armbian.com/Developer-Guide_Using-Vagrant/). This uses Virtualbox (as above) but does so in an easily repeatable way,
- inside a [Docker](https://docs.armbian.com/Developer-Guide_Building-with-Docker/), [systemd-nspawn](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) or other container environment [(example)](https://github.com/armbian/build/pull/255#issuecomment-205045273),
- running natively on a dedicated PC or a server (**not** recommended),
- **25GB disk space** or more and **2GB RAM** or more available for the VM, container or native OS,
- superuser rights (configured `sudo` or root access).

**Execution**

	apt-get -y install git
	git clone https://github.com/armbian/build
	cd build
	./compile.sh

Make sure that full path to the build script does not contain spaces.

You will be prompted with a selection menu for a build option, a board name, a kernel branch and an OS release. Please check the documentation for [advanced options](https://docs.armbian.com/Developer-Guide_Build-Options/) and [additional customization](https://docs.armbian.com/Developer-Guide_User-Configurations/).

Build process uses caching for the compilation and the debootstrap process, so consecutive runs with similar settings will be much faster.

# How to report issues?

Please read [this](https://github.com/igorpecovnik/lib/blob/master/.github/ISSUE_TEMPLATE.md) notice first before opening an issue.

# How to contribute?

- [Fork](https://help.github.com/articles/fork-a-repo/) the project
- Make one or more well commented and clean commits to the repository. 
- Perform a [pull request](https://help.github.com/articles/creating-a-pull-request/) in github's web interface.

If it is a new feature request, don't start the coding first. Remember to [open an issue](https://guides.github.com/features/issues/) to discuss the new feature.

If you are struggling, check [this detailed step by step guide on contributing](https://www.exchangecore.com/blog/contributing-concrete5-github/).

## Where to get more info?

- [Documentation](https://docs.armbian.com/Developer-Guide_Build-Preparation/ "Developer resources")
- [Prebuilt images](https://www.armbian.com/download/ "Download section")
- [Support forums](https://forum.armbian.com/ "Armbian support forum")
