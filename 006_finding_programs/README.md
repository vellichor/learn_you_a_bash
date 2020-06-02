# Finding and Installing Programs and Commands

So far, all the commands I've introduced you to have been bash built-in commands, like `ls` and `mv`, or very commonly installed programs like `man`. Now it's time to see what happens when we want to use a program that may or may not be installed on our machine.

## Which program?

Which indeed! It turns out that if you want to see whether a command is available to be run from your command line, you can use the command `which`. Much like the way we executed `man`, we use `which` as the command and then pass it the name of another command we want to use. Let's try it now:

```
$ which ls
```

You already know that `ls` is available, because we've been using it this whole time. That means you are probably not surprised to see that this command generated some output. You might be surprised to see that this output is... __*drumroll, please*___ A PATH!

Yes indeed, these `bash` built-ins are supplied as files, which live on the filesystem, and that means they have a path. In fact, every runnable command is secretly a file. In fact, try this one!

```
$ which bash
```

Your shell is a program, and a program is a file. Check that out, there it is!

## Installing New Commands

Now let's look for something you most likely don't already have. Unless your system is fairly old, it probably no longer comes with the Discordian calendar preinstalled:

```
$ which ddate
```

If you saw a path appear for this one, then lucky you! You're running a really old system and should probably think about upgrading. But if you didn't, let's talk about how to install command line utilities that are missing from your system.

## Installing New Commands

This is one of those things that's going to be different based on what distribution you're running. If you're on a Mac, use the Mac instructions, below. If you're using another Unix-like OS, follow me to the Unix section where we'll run a few commands to see what _package manager_, or, software installer, lives on your computer.

### Mac

You're going to want to go grab a really useful tool called Homebrew. Homebrew is a package manager built for MacOS, to allow you to install new commands at the command line much the same way you would do for various Linux systems.

First, go here: [https://brew.sh/]

Next, just copy and paste the line starting with `bash` that's shown in the box on this webpage. Most of the time, I will advise you strongly against copying and pasting random `bash` code into your terminal! As we've seen, it's very easy to delete files and more with just a simple command, so most of the time you should not run scripts and snippets you find online unless you can read them and understand what they do. For this one, I'll quickly explain what's here. Today, as I'm writing this, the install command is

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
You can easily recognize the web URL at the end of this: it's a web URL, in this case one that points to a shell script hosted on Github. It, and the command before it `curl -fsSL`, are enclosed in `$()`, which is a bash shorthand for "please run the command inside here first, then use the results in the rest of the command outside the parentheses." Outside the parentheses, we see `/bin/bash -c`, or, the `bash` command followed by the `-c` flag. What's this? From the outside in:

* `bash -c` is an invocation of the bash shell, and the -c flag tells it to run whatever comes next inside the new `bash` shell, then exit. Yes, you can run `bash` inside of `bash`! `bash` is a program, and you can run any program from inside the shell. When this `bash -c` command finishes, it will return you to the outer shell you ran it from. It's specified this way because the authors wanted it to work right even for someone who has chosen to run one of the other shell programs we talked about earlier.

* curl is a very simple client for the Web. Rather than do what Chrome or Safari or Firefox does, and render a webpage according to the code that exists at the path you gave it, what `curl` does is very straightforward: it fetches that code directly, and it prints it right out to the screen. The flags `-fsSL` make sure that `curl` tries to fix the problem if the file has been moved, and that it prints only the code, not any other status messages. You can use `man curl` to find out more.

So, we start a new `bash` shell and then tell it to run the code found at that URL and then exit. The code in that script will install the Homebrew tool for you.

Once this is done, we can install the ddate package this way:

```
$ brew install ddate
```

Now when you `which ddate`, you should see that it's been installed for you. You can install other packages this way using `brew install`.

### Linux Systems

As we mentioned earlier, the Linux kernel comes in a lot of different _distributions_ -- __distro__ for short. For each distro there is a preferred package manager that was installed along with the rest of your operating system. We'll use `which` again to see which one is installed.

At times, you may get a message saying "Permission denied." Linux programs are typically installed for the whole system at once, so in order to use some of these commands, you need to assume _superuser_ privileges. To do this, you'll use `sudo`. This is a program that enters superuser mode, executes the command you supply next, then returns you to your normal privileges. If you needed to `ls` a directory that your own user account is not allowed to read, for example, you'd type

```
$ sudo ls /top/secret/directory
```

The system will ask you for your password, to make sure you're really the owner of this account, then run your command for you.

USE THIS WITH CAUTION. As you've seen, command-line tools are powerful, and can make big changes very fast. If your regular user account doesn't have permission to do something, then think carefully about whether there's a reason for that. Check what directory you're in, what paths you're about to operate on, and whether that's really what you want to do. If it is, then `sudo` away!

#### Advanced Package Tool (apt)

If you're using Ubuntu, Debian, or other related distros, you'll use the apt system. The command you'll use most of the time is `apt-get`:

```
$ which apt-get
```

If you got a path back here, then this is your package manager. You'll install packages with this command. Make sure your computer's list of available software is up to date now and then -- you should do this now by running 

```
$ apt-get update
```

Then, to install the `ddate` package, use

```
$ apt-get install ddate
```

Now when you `which ddate`, you should see that it's been installed for you. You can install other packages this way using `apt-get install`.

#### Yum

RedHat and CentOS Linux use the `yum` package installer. Let's see if you have it on your system now:

```
$ which yum
```

If you got a path back here, then this is your package manager. You'll install packages with this command. Make sure your computer's list of available software is up to date now and then -- you should do this now by running 

```
$ yum update
```

Then, to install the `ddate` package, use

```
$ yum install ddate
```

Now when you `which ddate`, you should see that it's been installed for you. You can install other packages this way using `apt-get install`.


#### Pacman

If you're running Arch Linux, I'm a bit impressed that you've found yourself in my guide. This distro is largely managed from the command line, and would be pretty difficult to install if you're unfamiliar with working at the shell. So, good going! Let's look to see if Pacman is your package manager:


```
$ which pacman
```

If you got a path back here, then this is your package manager. You'll install packages with this command. To install the `ddate` package, use

```
$ pacman -S ddate
```

Now when you `which ddate`, you should see that it's been installed for you. You can install other packages this way using `pacman -S`.

#### Other Package Managers

Linux is all about customization, and there are many other package managers out there that you may run into. A few notable ones are Portage, which uses the `emerge` command to install packages, or RPM, which is present on all systems running `yum` (`yum` is a friendlier wrapper around the `rpm` system.) If these are your package managers, using them is out of scope for this guide -- use your Googling skills to find out how to install a package using each of these packaging systems.