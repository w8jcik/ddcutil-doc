
## Installing **ddcutil** and **ddcui** From Prebuilt Packages

Prebuilt packages fall into 2 groups, those maintained as part of the **ddcutil** project (upstream), and those in Linux distribution repositories (downstream).

If no prebuilt package is available for your distribution and architecture, **ddcutil** can be [built from source](building.md).

### Prebuilt Packages Maintained by the **ddcutil** Project

These packages are rebuilt with each **ddcutil** release.

#### openSUSE Build Service

**ddcutil** packages for some Debian, openSUSE, and Ubuntu releases are available for download from the openSUSE Build Service, including official openSUSE builds.

The graphical user interface, **ddcui** is not available from OBS. 

There's not one clean link for downloading OBS packages, so here are several.  

- [Add repository and download package ddcutil](http://software.opensuse.org/download.html?project=home%3Arockowitz&package=ddcutil).
This is all you will need if just installing the command line version of ddcutil.  
- [All ddcutil packages](http://software.opensuse.org/search?utf8=%E2%9C%93&q=ddcutil&search_devel=true&baseproject=ALL)
Packages for both the command line and shared library version of **ddcutil**.  Note that the development package is named 
**libddcutil-devel** on RPM based distributions (SUSE, Fedora) and **libddcutil-dev** on dpkg based distributions (Debian, Ubuntu).  
- [File system view](http://download.opensuse.org/repositories/home:/rockowitz/)  

#### Launchpad

The [**ddcutil** Launchpad PPA](https://launchpad.net/~rockowitz/+archive/ubuntu/ddcutil)
contains builds of both **ddcutil** and **ddcui** for recent Ubuntu releases. These builds are also suitable for other recemt Debian derivative distributions.
Builds exist for the following architectures: amd64, arm64, armhf, i386, ppc64el. 

To use this PPA repository, issue the following commands:
~~~
$ sudo add-apt-repository ppa:rockowitz/ddcutil
$ sudo apt-get update
~~~

Alternatively, you can manually add lines like the following to /etc/apt/sources.list, or to a file in /etc/apt/sources.list.d
~~~
deb http://ppa.launchpad.net/rockowitz/ddcutil/ubuntu cosmic main 
deb-src http://ppa.launchpad.net/rockowitz/ddcutil/ubuntu cosmic main 
~~~

#### Fedora COPR

Fedora COPR contains **ddcutil** builds for recent Fedora and openSuse releases. It also contains **ddcuti** builds for Fedora.  Builds for both **ddcutil** and **ddcui** are found in repository [rockowitz/ddcutil](https://copr.fedorainfracloud.org/coprs/rockowitz/ddcutil/). 
 
To use this COPR repository, issue the following command:
~~~
$ sudo dnf copr enable rockowitz/ddcutil
~~~
Note: ***dnf-plugins-core*** must be installed. 

Alternatively, you can download a repository file found on the [ddcutil copr page](https://copr.fedorainfracloud.org/coprs/rockowitz/ddcutil/) and save it in /etc/yum.repos.d. 


### Prebuilt Packages Maintained by Linux Distributions

**ddcutil** is included in many Linux distribution.  In particular, it is included in Arch, Debian (and hence its derivatives such as Ubuntu), Fedora, Gentoo, Manjaro, openSUSE, and Rapbian.
For an up to date list of Linux distributions that include **ddcutil**, see the [Repology](https://repology.org/metapackage/ddcutil/versions) web site. 

Bear in mind that the version of **ddcutil** found in Linux distributions is often several releases behind the "upstream" release, or may only contain the standalone 
command-line version. 

