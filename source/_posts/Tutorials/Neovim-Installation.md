---
title: Neovim Installation
mathjax: false
date: 2024-03-24 11:46:59
tags:
- Neovim
- Linux
category:
- 经验
---

从这篇开始，以后还是尽可能用英文记录。

This post is to teach you how to install Neovim on your Linux machine.

Especially without package manager like `apt` or `yum`.

<!--more-->

# Background

I recently set up a new Docker container for development, and I want to install Neovim on it.

The problem is the base image I used is `node:latest`, which is actually not for development but for running Node.js apps. Thus it literally has nothing but Node.js installed. And many pacakges cannot be installed via `apt`.

Thus I have to install Neovim without package manager.

# Pre-knowledge

## What is `FUSE` (Filesystem in Userspace)

FUSE is a userspace filesystem framework. It consists of a kernel module (fuse.ko), a userspace library (libfuse.*) and a mount utility (fusermount).

One of the most important features of FUSE is allowing secure, non-privileged mounts. This opens up new possibilities for the use of filesystems. A good example is sshfs: a secure network filesystem using the sftp protocol.

More about `FUSE` can be found on the [Linux Kernel documentation](https://www.kernel.org/doc/html/next/filesystems/fuse.html)

## What is AppImage

The AppImage format is a format for packaging applications in a way that allows them to run on a variety of different target systems (base operating systems, distributions) without further modification.

With AppImage, **applications are packaged with all their dependencies into a single file**. This file can be executed on any Linux distribution without installation. Users just need to make the AppImage file executable and then run it.

More about `AppImage` can be found on the [official website](https://appimage.org/).

## Their Relationship

While FUSE and AppImage serve different purposes, they can be related in the context of how applications access and manage files:

- **Integration and Portability**: AppImage can use FUSE to mount or access certain types of file systems in a way that is transparent to the user. For example, an application distributed as an AppImage could interact with a custom or special-purpose file system provided through FUSE. This could be useful for applications that need to deal with non-standard data sources.

- **Sandboxing and Security**: Some applications packaged as AppImages might use FUSE to sandbox themselves or to provide a virtualized view of the filesystem to the application, enhancing security or isolating the application from the rest of the system.


# Steps

## 1. Download Neovim package

Since 8.3, Neovim don't provide `deb` package any more. And it seems that `AppImage` is recommended.

So we'll install Neovim via `AppImage`.

First we need to download the `AppImage` package from [Neovim's GitHub release page](https://github.com/neovim/neovim/releases)

```shell
wget https://github.com/neovim/neovim/releases/download/v0.9.5/nvim.appimage
```

## 2. Make it executable

```shell
chmod u+x nvim.appimage
```

## 3. Unpack it

### No FUSE

If your system doesn't support `FUSE`, you can unpack it to get the executable.

In my case, my base image cannot support `FUSE`, so I have to unpack it.

```shell
./nvim.appimage --appimage-extract
```

### With FUSE

If your system supports `FUSE`, you can directly run the `AppImage` file.

```shell
./nvim.appimage
```


## 4. Install it

### No FUSE

```shell
sudo mv squashfs-root/usr/bin/nvim /usr/local/bin/nvim
sudo mv squashfs-root/usr/share/nvim /usr/local/share/nvim
```

Add the following line to your `.bashrc` or `.zshrc`:

```shell
export VIMRUNTIME=/usr/local/share/nvim/runtime
```

### With FUSE
    
You just need to move the `AppImage` file to `/usr/local/bin`:

```shell
sudo mv nvim.appimage /usr/local/bin/nvim
```

## 5. Test it

Just run `nvim` anywhere in your terminal to check if it works.

```shell
nvim
```

## * Uninstall

### With FUSE

If you want to uninstall Neovim, just remove the files you moved to `/usr/local/bin` and `/usr/local/share/nvim`.

Like this:

```shell
sudo rm -R /usr/local/bin/nvim
sudo rm -R /usr/local/share/nvim
```

And remove the line you added to `.bashrc` or `.zshrc`.

### No FUSE

Same logic, just remove the files you moved.

No need to remove the line in `.bashrc` or `.zshrc` if you don't add it manually.

```shell
sudo rm -R /usr/local/bin/nvim
```

# Insights

How does the above steps work? With the pre-knowledge we have learned, we can understand it better.

The process of installing Neovim on a Linux system without a package manager, as described above, leverages the unique capabilities of AppImage and FUSE to provide a seamless and flexible installation method. Here's a deeper insight into how these technologies work together to facilitate this process.

## The Role of AppImage

AppImage is a universal software package format for Linux systems. The key feature of AppImage is **its ability to bundle applications along with all their dependencies into a single executable file.** This approach eliminates the need for traditional installation processes, where dependencies must be individually installed and managed. Instead, users can simply download, make executable, and run an AppImage, thereby significantly simplifying application deployment and usage on Linux.

When you download the Neovim AppImage and make it executable, **you're essentially preparing a self-contained version of Neovim** that can run independently of the system's package management and without the need to resolve dependencies manually. This is particularly advantageous in environments like Docker containers, where minimizing the footprint and complexity of installations is crucial.

## The Effect of FUSE

FUSE (Filesystem in Userspace) further complements the portability and ease of use offered by AppImage. By allowing userland applications to create their own file systems without altering kernel code, FUSE provides a mechanism for AppImages to mount themselves as virtual file systems. **This capability is critical when the AppImage needs to unpack or access its contents transparently, as if it were installed in the traditional sense.**

For systems with FUSE enabled, running an AppImage is as straightforward as executing any binary file. **The AppImage mounts itself using FUSE, providing the application with access to its bundled dependencies as if they were part of the host system's file system.** This seamless integration simplifies application execution and removes the need for manual unpacking in most cases.

However, in environments where FUSE is not available or practical, AppImages offer an alternative method to extract their contents manually. This flexibility ensures that applications packaged as AppImages can still be utilized in restricted or minimal environments, such as certain Docker containers or systems with strict security policies that disallow kernel modules like FUSE.

That's why if we don't have `FUSE` not only we need to move files to `/usr/local/bin`, but also move files to `/usr/local/share` which contians the dependencies.
