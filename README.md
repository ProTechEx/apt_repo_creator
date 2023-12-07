# apt repo creator
Ubuntu/Debian Offline Repository Creation Script

When I googled how to create my own offline repository of packages for use in an offline Ubuntu/Debian machine, the results were disheartening and the steps to be taken scattered all over the place.

The files within this gist will allow you to:

- Download specific apt-get packages... with dependencies included!
- Create a `Packages.gz` file so that you can add the repository folder you create to the target machine's `/etc/apt/sources.list` file.

## Before using

This gist is comprised of 3 scripts:

- `dependencies.sh`
- `getpkg.sh`
- `mkrepo.sh`

Be sure to make them executable by `chmod`'ing them.

## Dependencies

There are two scripts within this gist which will aid you in creating your offline repo: 

- `getpkg.sh`
- `mkrepo.sh`

The script `getpkg.sh` depends on the package `apt-rdepends`.

The script `mkrepo.sh` depends on the program `dpkg-scanpackages` which is part of the `dpkg-dev` package.

In order to install this dependencies simply run:

`$ sudo ./dependencies.sh`

## Downloading packages

To download an apt-get package (including its dependencies) simply run:

`$ ./getpkg.sh <package-name> <packages-directory>`

where `<package-name>` is the name of the package you are trying to store locally and `<packages-directory>` is the path to the directory where you'll be storing the downloaded packages.

Example:

`$ ./getpkg.sh build-essential /home/user/my-repo`

**WARNING:** Since this script downloads not only the deb package, but its dependencies as well, be sure you have a decent amount of free hard-disk space since things can quickly escalate. For example, the package `ubuntu-desktop` when downloaded with dependencies and all, weighs well over **300MB**. You have been warned.

## Turning your packages directory into a repo

The following should only be done **once**, after having downloaded **all** the packages you deem necessary to be in your offline repository (as in, after having followed the steps in the **Downloading packages** section of this guide).

In order to transform the directory where you placed your repository's packages into an actual apt-get valid repo, just run:

`$ ./mkrepo.sh <packages-directory>`

where `<packages-directory>` is the path to the directory where you stored the downloaded packages.

Example:

`$ ./mkrepo.sh /home/user/my-repo`

## Using your offline repo

Let's say you have a machine with no internet connection which runs Ubuntu, and you wish to install some apps not included in the provided apt-get repository.

You followed the steps in this guide and stored within a usb pen-drive a directory called `my-repo`, which contains the apps you want to install and a file named `Packages.gz`.

You plug this repo-wielding usb pen-drive into the offline Ubuntu machine, and you make sure to mount it to the route `/media/repo-usb`.

Now you can add the following line to the `/etc/apt/sources.list` file, in the offline Ubuntu machine:

`deb [trusted=yes] file:/media/repo-usb/my-repo ./`

Run the command:

`$ sudo apt-get update`

Et voilà! You can now `sudo apt-get install` your packages to your heart's content.

