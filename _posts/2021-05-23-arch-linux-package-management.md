---
layout: post
title: Arch Linux Package Management
subtitle: A Beginner's Cheat Sheet to pacman
categories: archlinux
tags: pacman tutorial
---

## Introduction

Arch Linux comes with a powerful package manager called pacman. Often new users take time to figure out its commands because it does not have the usual 'obvious' commands like apt. So here is a short post about pacman commands.

_Note : All pacman commands need to be run with superuser privileges. You need to prepend the commands with `sudo` for them to work. For the sake of brevity, I have omitted `sudo`._

## Installing Packages

### Download and Install

`-S` flag asks pacman to synchronize packages.

```sh
pacman -S package1 package2 package3
```

will download install specified packages along with their necessary dependencies.

### Selecting Repo in Case of Multiple Providers

By default, package is searched in repos in the order they are listed in `/etc/pacman.conf`.

In case a package is available from more than one source, then you can specify the repo name before package name to select the repo you want to download the package from. For example, I am using Endeavour OS, with Chaotic AUR added to my repo list. So, I can get the `polybar` package from two sources - EndeavourOS reop and Chaotic AUR repo. Since Endeavour OS repo has a preference in my `pacman.conf`, pacman will download polybar from that repo. However, if I want to download it from Chaotic AUR, I can run the following command.

```sh
pacman -S chaotic-aur/polybar
```

### Updating Packages

```sh
pacman -Syu
```

will download new package database and then install the packages that have new updates available. This will include your kernel and other core packages too so make sure you have backups ready in case something goes wrong. Timeshift is a popular backup tool.

### Searching For a Package

```sh
pacman -Ss search terms
```

will search for the supplied terms and provide you with a list of matching entries.

### Download But Don't Install

You may want to download packages, but not install them right away. This can be done by using the `-w` flag with one of the sync commands.

```sh
pacman -Sw reflector   # downloads reflector but doesn't install it
pacman -Syuw   # downloads available updates but doesn't install them
```

### Details for a package

```sh
pacman -Si package-name
```

will show details for the supplied package name.

## Removing packages

### Remove a Package

```sh
pacman -R package1 package2 package3
```

will remove the specified packages.

### Remove along with unneeded dependencies

```sh
pacman -Rs package1 package2 package3
```

`-s` flag instructs pacman to remove any dependencies of the specified packages, that are no longer required by any other package on the system.

### Removing groups of packages

```sh
pacman -Rsu gnome
```

will remove the gnome group, but preserve any packages that are required by one or more packages installed on the system.

## Query commands

### List of installed packages

```sh
pacman -Q
```

will output the list of installed packages along with their versions.

### List packages no longer needed

If you remove packages without using the `-s` command, then unneeded dependencies can pile up on your system. You can list them with the following command

```sh
pacman -Qdtq
```

To uninstall them, simply pipe them to `pacman -Rs` in with the following command

```sh
`pacman -Qdtq | pacman -Rs -`
```

### Packages no longer part of any repository

Sometimes, when a package is no longer maintained, it is removed from repositories. You may want to remove them since they are most certainly no longer required

```sh
pacman -Qm   # list foreign packages
pacman -Qmq | pacman -Rs -    # remove foreign packages
```

### Details for a package

```sh
pacman -Qi package-name
```

will display information for a the supplied package name. Note that this works only for installed packages.

### Know owner of a file

In case a system file is damaged, reinstalling the relevant package may help you fix it. In such case, you need to know which package owns the file in question.

```sh
pacman -Qo /usr/bin/zsh
```

This command tells me package that owns the file I specified.

### List explicitly installed packages

To get the list of package installed explicitly, meaning they are not installed as dependencies, run the following command

```sh
pacman -Qe
```

The "explicitly installed" tag can be modified as described [here](#h-mark-a-package-as-dependency).

### List packages installed as dependency

To get the packages that were installed as a dependency, run the following command

```sh
pacman -Qd
```

Again, this tag can be modified as described [here](#h-mark-a-package-as-explicitly-installed).

### Verify packages

```sh
pacman -Qk
```

This will check if all the files for the installed packages are available on the system.

## Database commands

### Mark a package as explicitly installed

```sh
pacman -D --asexplicit package-name
```

This will mark the package as explicitly installed and this package won't be considered when you run `pacman -Qdtq` or remove commands unless you explicitly specify the package name as a candidate for removal.

### Mark a package as dependency

```sh
pacman -S --asdeps package-name
```

Works as totally reverse of the previous command. Specified package will be considered for removal and also as unneeded dependency if no package depends on it.

## File commands

### List files installed by a package

```sh
pacman -Fl package-name
```

will show the list of files that a package will install onto your system.

### Search for file or filepaths

```sh
pacman -F filename
```

will search for filename and list the packages that provide a file by that name. This is specially useful when I need to know which package provides a specific header file I need to use.

`-F` only accepts complete filenames, without path. To do a regex search, with filepaths and partial names, use the `-x` flag.

```sh
pacman -Fx path/to/file
```

## Conclusion

Thats all for this post.

You can receive latest updates from this blog as an RSS feed. See [Follow this blog via RSS](/blog/website/2021/05/23/follow-this-blog.html).

Follow me on GitHub : [flyingcakes85](https://github.com/flyingcakes85)
