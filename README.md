## This project has a new home!

Due to GitHub's unfortunate new practice of using repositories to train Microsoft LLMs, I have made the difficult decision to migrate all of my projects to [Codeberg](https://codeberg.org/).

[This project's new home can be found here.](https://codeberg.org/computablee/Minecraft-Java-on-ARM-Windows)

# Minecraft Java Edition on ARM Windows

So if you're anything like me, you often prefer playing on Java Minecraft because that's what all your friends have and you want to do a server together.
Not to fear, this is possible! Just a little tedious.
I was able to get this running on an Acer Spin 7 running Minecraft Java 1.19.2 at about 20 FPS.
Here I document how I was able to do this, as well as difficulties and gameplay issues that occured.

**Note: If you want the best performance/highest framerate, you will want to play the Bedrock edition from the Windows Store.
It runs significantly smoother at higher FPS.**
If you still want to run Java edition, let's go!

## Getting Java Installed
The first thing you need is a version of Java for ARM Windows.
I tried installing a 32-bit Java and letting the Windows x86 emulator handle translation, but I was not able to get it running.
Oracle has a version of the JRE for ARM Windows for Java 16, but the latest versions of Minecraft require Java 17.
Therefore, we need to get it from a 3rd party.

I chose [Zulu Java, found here](https://www.azul.com/downloads/?package=jdk#download-openjdk).
Select Java 17 on Windows for ARM 64-bit, and download the JDK.
It doesn't come with an installer, so I chose to unzip it and move it to `C:\Users\[user]\AppData\Local`, which you can access by typing `%localappdata%` in Windows search.

After this is installed, we're ready for the next part.

## Getting Python Installed
The launcher we'll be using is a special launcher called `portablemc` found through PIP.
I chose this because it supports ARM-based LWJGL, which we will need.
The default launcher (as well as launchers like MultiMC) will pull x64-based LWJGL, which won't work for our purposes.

Go to [Python's Windows downloads](https://www.python.org/downloads/windows/) and select the ARM64 Windows installer for Python 3.11.
When you install, make sure to add to PATH.
This is disabled by default--we want it enabled.

After Python is finished installing, we can install the launcher.

## Installing the Launcher
`portablemc`'s GitHub repo [can be found here](https://github.com/mindstorm38/portablemc).
However, we won't be downloading it from GitHub.
I post this so you can see the nice README for the command line interface.
I'll show you the command you (probably) want, and we'll be able to throw it into a `.bat` file for ease of use.

But before we get to that--open CMD and type in `pip install --user portablemc`.
This is super easy.
If you get an error that `pip` isn't the name of a command, you probably didn't ensure that Python was added to your PATH.
Not a problem, you'll just need to give the full path of `pip`.
You'll have to find that on your system.

After `portablemc` is installed, we're not quite done.

## OpenCL/OpenGL Compatibility Packs
The last thing we need to install is the OpenCL and OpenGL Compatibility Pack.
You can find this on the Windows store.
Go ahead and install it.
This is probably the easiest step!

## Running Minecraft
Okay, we're finally here.
I'm going to give you the command I ran, but **make sure to replace everything in [brackets] with your info.**
Here's the command I ran:
```
C:\Users\[your user]\AppData\Roaming\Python\Python311-arm64\Scripts\portablemc.exe start -m -l=[your Microsoft email] --jvm=C:\[your Zulu Java path]\bin\java.exe "--jvm-args=-Xms512m -Xmx1G" --lwjgl=3.3.1 [your Minecraft Version]
```
Make sure to give the full path to `portablemc.exe`, your Microsoft email, your Zulu Java path, and your Minecraft version.
Also feel free to allocate more RAM if you like, but **be aware I noticed that even with `--Xmx1G`, it would often use about 2 GB of RAM!**
I keep it so low because my Acer Spin 7 only has about 8 GB of RAM... not too much to play with.

Feel free to make sure the command works for you before throwing it in a `.bat` file.
I put the command in a `.bat` file in my Documents folder, created a shortcut on my desktop, then set the shortcut icon to the Minecraft Java icon.
Besides the fact that you don't get a GUI launcher, it does run!

## Performance Issues
So I keep my render distance at about 6-8 chunks with minimum smooth lighting and other details turned down.
Without the F3 menu up, I estimated about 20 FPS on a server.
With the F3 menu up, it tends to drop a significant bit, down to about 10 FPS(!!!).
Other menus seem to lag the game, too (like chests, crafting, inventory, etc.) so just be aware of this issue.
