---
title: Synchronizing Windows Fonts with WSL for LaTeX Typesetting
mathjax: false
date: 2024-05-15 10:30:00
tags:
- WSL
- Fonts
- LaTeX
category:
- 经验
---

Recently, while working on my writing projects in WSL (Windows Subsystem for Linux), I encountered issues with the limited font selection when using XeLaTeX to compile my documents. XeLaTeX, a popular LaTeX compiler, relies on the system font library, which means maintaining the font library in WSL is crucial for a smooth typesetting experience. To address this problem, I decided to explore ways to synchronize the fonts installed on my Windows system with WSL.

<!--more-->

# Problem

WSL, being a separate Linux subsystem, does not automatically synchronize the fonts installed on the Windows system. This limitation results in a restricted font selection when using certain applications in WSL, such as XeLaTeX for document compilation. The lack of a diverse font library can impact the overall typesetting quality and hinder the creative process.

# Solution

## Installing Individual Fonts in WSL

If you only need to install a specific font in WSL for your LaTeX projects, you can follow these steps:

1. Check Font Location: Ensure that the font file (e.g., VictorMono.ttf) is located in a directory accessible from your WSL instance. For simplicity, you can copy the font file to your WSL home directory.

2. Install Font: Use the `fc-cache` command to update the font cache in WSL and make the font available. Open a terminal in your WSL instance and run the following commands:

   ```shell
   sudo cp /mnt/c/path/to/VictorMono.ttf /usr/local/share/fonts/
   sudo fc-cache -fv
   ```

   Note that `/mnt/c` is the path to access the Windows C: drive from within WSL.

3. Verify Installation: Confirm that the font is installed by running the following command in your WSL terminal:

   ```shell
   fc-list | grep "Victor Mono"
   ```

## Synchronizing the Windows Fonts Directory with WSL

If you want to synchronize all the fonts installed on your Windows system with WSL, you can directly copy the entire fonts directory. Here's how:

1. In your WSL terminal, create a directory to store the synchronized fonts, for example:

   ```shell
   mkdir -p ~/.fonts
   ```

2. Copy the Windows fonts directory to the WSL fonts directory:

   ```shell
   cp -r /mnt/c/Windows/Fonts/* ~/.fonts/
   ```

   This command copies all the font files from the Windows fonts directory `C:\Windows\Fonts` to the `~/.fonts` directory in WSL.

3. Update the font cache in WSL:

   ```shell
   fc-cache -fv
   ```

   This command updates the font cache in WSL, making the newly installed fonts available.

Now, you can use the fonts installed on your Windows system in various applications within WSL, including XeLaTeX for your LaTeX projects. Having a rich font library at your disposal greatly enhances the typesetting experience and allows for more creative control over your documents.

# Conclusion

By following the steps outlined above, you can easily synchronize the fonts installed on your Windows system with WSL, either individually or by copying the entire fonts directory. This ensures a consistent font experience between Windows and the Linux environment in WSL, particularly when using XeLaTeX for document compilation.

Maintaining a diverse font library in WSL is essential for LaTeX users who rely on XeLaTeX for typesetting. It enables them to access a wide range of fonts, enhancing the visual appeal and professional quality of their documents.

I hope this article helps you in using custom fonts with XeLaTeX in WSL. If you have any questions or suggestions, feel free to leave a comment below.

