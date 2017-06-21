# user-sync.py: User Sync Tool from Adobe

The User Sync Tool is a command-line tool that automates the creation and management of Adobe user accounts.  It
does this by reading user and group information from an organization's enterprise directory system or a file and 
then creating, updating, or removing user accounts in the Adobe Admin Console. The key goals of the User Sync 
Tool are to streamline the process of named user deployment and automate user management for all Adobe users and products.

This application is open source, maintained by Adobe, and distributed under the terms
of the OSI-approved MIT license.  See the LICENSE file for details.

Copyright (c) 2016-2017 Adobe Systems Incorporated.

## Documentation

The [User Sync Documentation](https://adobe-apiplatform.github.io/user-sync.py/) covers all aspects of the tool, from both a general and a technical point of view.  The following links are good places to start:

- [User Sync Overview](https://www.adobe.io/apis/cloudplatform/usermanagement/docs/UserSyncTool.html)
- [Non-Technical Primer](https://spark.adobe.com/page/E3hSsLq3G1iVz/)
- [User Manual](https://adobe-apiplatform.github.io/user-sync.py/en/user-manual/)
- [Step-by-Step Setup](https://adobe-apiplatform.github.io/user-sync.py/en/success-guide/)


## System Requirements

To run User Sync, you must have an up-to-date 64-bit Python installed on your system, either Python 2.7 or Python 3.4+.  In addition you must have User Management API Credentials for your organization (see [the official documentation](https://www.adobe.io/products/usermanagement/docs/gettingstarted))

## Installation and Use

The connector is packaged as a self-contained Python Executable (_aka_ [PEX](https://github.com/pantsbuild/pex)) file.  See the [releases page](https://github.com/adobe-apiplatform/user-sync.py/releases) to get the latest build for your platform.  Download the release tarball onto your system, extract it, and you will have a file named `user-sync` (or `user-sync.pex`) that can be executed from the command line:

* On Mac OS X and Linux systems, use the command `user-sync`.
* On Windows systems, use the command `python user-sync.pex`.

Until you have personalized your User Sync configuration (see next section), you won't be able to do much.  But you can test that your installation is working with:

* `user-sync --version` (or `python user-sync.pex --version`) and
* `user-sync --help` (or `python user-sync.pex --help`)

There are a wide variety of command-line arguments; see [the docs](https://adobe-apiplatform.github.io/user-sync.py/en/user-manual/command_parameters.html) for details.

## Configuration

You will need a personalized User Sync configuration to use the tool effectively.  The documentation includes a [Setup and Success Guide](https://adobe-apiplatform.github.io/user-sync.py/success-guide/) that will take you step-by-step through the configuration process.  In addition, the `examples` directory (also available as a tarball on the [releases page](https://github.com/adobe-apiplatform/user-sync.py/releases)) contains sample configuration files which include all of the possible options with their descriptions and default values.

## Build Instructions

To build User Sync from source, you will need to have a complete Python/Pip/Virtualenv stack present on your system, as well as `make` and the ability to install Python modules that use native extensions.

The platform-independent overview is:

1. Get the sources into a `user-sync.py` directory (either via `git` or by downloading the source tarball from a release).
1. Make a clean virtual environment for the 64-bit Python version you want to use (2.7 or 3.4+) and activate it.
1. Switch into the `user-sync.py` directory, and give the command `make pex`.

While that looks simple, the `make pex` command will try to download and install all of the User Sync dependencies, and that process can be tricky.  Below is platform-specific advice to make sure it goes as well as possible.

### Ubuntu and other Debian variants

We pre-package releases on Ubuntu, so the advice here is definitely accurate for that platform, but something similar should work for most other Debian variants.

* Make sure you have Python 2.7.5 or above; earlier versions will give you InsecurePlatformWarning messages due to older SSL components.
* Take all security updates before you start.
* You don't need a GUI on the platform, as the entire build can be done from the command line.  Server variants are fine.
* You will need a standard C development environment to build a variety of the modules that use native extensions.  Use this command to get one:
    ```bash
    sudo apt-get install build-essential
    ```
* Make sure you use the system package manager to install the following packages (and their dependencies):
    * python-dev
    * python-pip (server variants often don't have it pre-installed)
	* python-virtualenv (ditto)
	* pkg-config
    * python-devel
    * python-pip (see above)
	* python-virtualenv
	* pkgconfig
	* python-dbus
	* libssl-dev
	* libldap2-dev
	* libsasl2-dev
	* libdbus-glib-1-dev
	* libffi-dev
* For convenience, you can copy and paste this command:
    ```bash
    sudo apt-get install python-dev python-pip python-virtualenv pkg-config python-dbus libssl-dev libldap2-dev libsasl2-dev libdbus-glib-1-dev libffi-dev
    ```

### CentOS and other RedHat variants

We pre-package releases on CentOS, so the advice here is definitely accurate for that platform, but something similar should work for most other RedHat variants.

* Make sure you have Python 2.7.5 or above; earlier versions will give you InsecurePlatformWarning messages due to older SSL components.
* Take all security updates before you start.
* You don't need a GUI on the platform, as the entire build can be done from the command line.  Server variants are fine.
* You will need a standard C development environment to build a variety of the modules that use native extensions.  Use this command to get one:
    ```bash
    sudo yum group install "Development Tools"
    ```
* Your OS may not know about `pip` by default, although it will know about `virtualenv`.  Rather than installing `pip` manually, we recommend telling your OS about the Red Hat "Extra Package for Enterprise Linux (EPEL)" package library, which on CentOS you can do with:
    ```bash
    sudo yum install epel-release
    ```
* Make sure you use the system package manager to install the following packages (and their dependencies):
    * python-devel
    * python-pip (see above)
	* python-virtualenv
	* pkgconfig
	* python-dbus
	* openssl-devel
	* openldap-devel (includes sasl)
	* dbus-glib-devel
	* libffi-devel
* For convenience, you can copy and paste this command:
    ```bash
    sudo yum install python-devel python-pip python-virtualenv pkgconfig python-dbus openssl-devel openldap-devel dbus-glib-devel libffi-devel
    ```

### Mac OS X

On Mac OS X there are a wide variety of ways to get current Python installations that don't interfere with the system-required Python, and a wide way of creating virtual environments for that Python. We strongly recommend using `pyenv` for installation and maintenance and `pyenv-virtualenv` for virtual environment support.  And we recommend getting those via `homebrew`.  The sequence for this is:

1. Install the latest version of the Apple Developer Command Line Tools via `xcode-select --install`.
1. Install Homebrew per the directions [here](http://docs.brew.sh).
1. Install `openssl` via Homebrew: `brew install openssl`
1. Install `pyenv` via Homebrew: `brew install pyenv`
1. Install `pyenv-virtualenv` via Homebrew: `brew install pyenv-virtualenv`
1. Find your desired Python version with: `pyenv install --list`
1. Install that python with `pyenv install ##`, where `##` is the desired version number.
1. Create a virtual environment for your builds, with `pyenv virtualenv ## myname`.
1. In your `user-sync.py` source directory, activate that virtual environment with `pyenv local myname`.

This sequence not only ensures that you are set up to do Python extension compiles, but also that they will know how to find the openssl libraries, and that you can easily install any other needed development libraries with Homebrew.

In general, regardless of how you get your Python, you will need:

* The latest security updates.
* The latest `openssl` (see above).
* The latest version of XCode and/or the Developer Command Line Tools (see above).
* Installs of modern openssl, ffi, and pkg-config libraries.  With Homebrew, you can do:
    ```bash
    homebrew install pkg-config openssl libffi
    ```
* To know how to include the XCode `include` directory in a compile spawned from `pip` install.  If you didn't do a command line developer tools install (which puts the headers into /usr/include), you can do:
    * `CFLAGS="-I$(xcrun --show-sdk-path) pip install ...`.
* To know how to include a modern `openssl` header path in a `pip` install.  If you installed your Python via Homebrew or `pyenv`, this is taken care of for you, see above.  Otherwise, if you installed ssl with Homebrew, you can do:`CFLAGS="-I$(brew --prefix openssl)/include"  LDFLAGS="-L$(brew --prefix openssl)/lib" pip install ...`.

### Windows

Windows is the trickiest platform because you need a command line development environment and package manager, and many of the dependencies don't have Windows builds available on PyPI.  So rather than building from Windows on scratch, we recommend the following procedure:
  
* Install [Cygwin](https://www.cygwin.com/) to get a bash command-line, together with basic tools such as `git` and `make`.
* Use the [python.org](https://python.org) installers for the desired version of Python.
* For the User Sync dependencies that don't have Windows 64-bit wheels on [PyPI](https://pypi.python.org/), get builds from [Christoph Guelke's excellent site](http://www.lfd.uci.edu/~gohlke/pythonlibs/).  We have stashed the ones needed for the current release in the `external` directory, and that's where the `Makefile` looks for them, so if you go fetch your own be sure to put them in that same directory.

Having performed the above steps, you can follow the "generic" instructions and the build should "just work".
