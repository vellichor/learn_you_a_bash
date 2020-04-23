# Intro

## What's the Shell?

Glad you asked! In Unix systems, we are accustomed to thinking of the Operating System as a distinct part, and Applications as something that run on top of it. This is basically true, although the Operating System (__OS__) is itself a program. However, the native language of the operating system is based around `processes` (running programs) which communicate between one another by reading and writing to shared physical devices like the hard drive or the system memory. If you run the same `program` 3 times in a row, you will have created 3 different `processes`, each having a unique `process id`.

The basic functions of the operating system are performed by the `kernel`, a master program that manages other processes and makes sure they all get a share of the system resources (memory, processor, disk space.) Drivers, additional code modules that are used by the kernel to extend its abilities, translate between the kernel and more complicated pieces of hardware, like sound and video cards, or network adapters.

Sounds complicated. In fact, it is, and if you wanted to deal with all of this directly, you would have to write your own program that allowed you to issue system calls directly to all of this hardware and coordinate the results to show them on the screen or play a sound (more hardware). If you were really clever, you'd make this program interactive, so that you could keep sending it commands that it could understand, or names of other programs, and it would keep running them and managing the input and output for you.

In fact, somebody has written this program. IN FACT, `bash` is just one of the many shell programs that have been written over the years. A shell program behaves exactly as I've described: once it starts, it interactively accepts commands from the user, runs them, accepts any additional input that these commands need, and sends the output back to the user. When the program either finishes running or detaches from the shell, the shell is ready to accept another command from you.

> A shell is a program for running programs!

Some commands are run inside the shell program itself, especially those for moving around between directories or directly reading or writing files. Other commands are separate programs that are started by the shell as a new process.

`bash` is the default shell for most Unix-based operating systems these days. Some other popular shells are `zsh`, `csh`, and even `eshell`, a shell that lives entirely inside the text editor `emacs`.

### Wait, what's Unix?

Unix is the name for a group of related operating systems originally developed by AT&T. Although the official Unix operating systems are not freely available, many operating systems have been developed to behave like generic Unix systems, including Linux and BSD. Linux has a number of different _distributions_, like Ubuntu or Arch Linux, which group the Linux kernel together with a set of tools for installing and managing the operating system and other programs, logging in, and providing the graphical user interface so that you can view your programs inside movable windows and use the mouse to navigate around. Although the original BSD is no longer maintained and is not typically installed on any modern system, the BSD-derived FreeBSD and the Darwin operating system that provides the core of modern Mac operating systems are both Unix-like OSes in the BSD family. That means that MacOS is a Unix-derived system!

## Getting To the Shell

This process is different depending on whether you're using Linux or a Macbook. (If you are using some other Unix-like OS, I'm going to let you Google how to get set up.)

### On a Macbook

Open `Finder` at the left end of the Dock, and navigate to your Applications directory. Inside, you'll find a `Utilities` directory. Open it and doubleclick the application called Terminal. You'll see a window pop up, whose title bar says `bash` on it, and a line that looks like this, for me:

```
pomelo:~ cobalt$ 
```

It'll look different for you. Why? Let's talk about the anatomy of the prompt.

The first thing you see on my example here is `pomelo`, which in my case is the name of my computer. (I name all of my computers after fruits, because I am very gay.) Yours probably says something like `Angry Badger's Macbook`, or `Wile E Coyote's iMac`, because MacOS makes up a computer name for you based on the name you tell it is yours when you set it up, plus the type of Mac you have got. Basically, everything before the `:` is the name of the computer you're running things on right now. (Later on, if you want a cool one like mine, I'll show you how to change it by running a program from the shell!) You may think it's obvious what computer you're on. But later, I'll teach you to open a shell FROM your computer that RUNS ITS COMMANDS, and ITSELF, on ANOTHER COMPUTER. Once you're doing that, it really helps for the prompt to tell you where you are!

After the `:`, there are two more things. First, you see a `~`. That `~` is shorthand for your _home directory_, the directory your computer set up for you to put all your personal files in. If other people use your computer, using different accounts, then they have a different directory for their files. If you were logged in as them, __`~` would mean their home directory instead!!__

Then there's a space, and then your username. Mine is `cobalt`. Yours might be `Angry Badger` or something. At the very end, there's a `$`, which is the actual command prompt -- the place you type. If you click into this window and type something, now, it'll show up after the `$`.

So, meet me in the section called __Your First Command__.

### On Linux

Look around for a program in your launcher that looks like a black computer screen, with a `>` in it or something. Otherwise, if your interface allows you to type a text command somewhere, just type in `/bin/bash` there and hit Enter.

You'll see a window pop up, with a line that would look like this, for me:

```
cobalt@pomelo:~$ 
```

It'll look different for you. Why? Let's talk about the anatomy of the prompt.

The first thing you see on my example here is `cobalt`, which is my name, and thus, my username. Then there's an `@` and then `pomelo`, which is the name of my computer. (I name all of my computers after fruits, because I am very gay.) If you called your user account `janeybob` and you set your computer's name (the _hostname_) as `dingus`, you would see `janeybob@dingus`. It kind of looks like an email address, and that's for a reason -- the real answer is more like _an email address looks like this_. But we'll get into that in another section. Basically, everything before the `:` is about who you are and what computer you're running things on. You may think it's obvious what computer you're on. But later, I'll teach you to open a shell FROM your computer that RUNS ITS COMMANDS, and ITSELF, on ANOTHER COMPUTER. Once you're doing that, it really helps for the prompt to tell you where you are!

After the `:`, there are two more things. First, you see a `~`. That `~` is shorthand for your _home directory_, the directory your computer set up for you to put all your personal files in. If other people use your computer, using different accounts, then they have a different directory for their files. If you were logged in as them, __`~` would mean their home directory instead!!__

At the very end, there's a `$`, which is the actual command prompt -- the place you type. If you click into this window and type something, now, it'll show up after the `$`.

So let's run your first command!

## Your First Command

From now on, when I have a command for you to type at the command prompt, I'll show it to you this way:

```
$ ls
```

That is, I'll include the `$`, but not the stuff before it. You'll still see that on your computer, and you should get used to looking at it to help you understand what directory you're in.

What do I mean about _what directory we're in_? Just like when you open Finder or a file browser to look at the files on your computer, you have to be _in_ some directory for the computer to know what to show you. In fact, every program runs _in_ some directory -- we'll explain what that's all about in the next section.

For now, just run the `ls` command -- type it after the prompt and hit Enter. Whenever I talk about _running a command_, this is typically what I want you to do: type it in, and press enter.

Unless you're really new at using this computer, you probably saw the names of some files pop up. If you look in your Finder or file browser, you will be able to see that these names match up with what's in your personal directory on your computer.

Let's go on to the next section, where I'll explain how to move into other directories, and what it means to be in one.