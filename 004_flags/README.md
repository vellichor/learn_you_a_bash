# Flags and Arguments

Oddly, this is not a section about international politics. Instead, we're talking about the arguments (sometimes called _operands_) we pass to our bash commands. An argument, generally speaking, is a piece of information we want the command to operate on when we run it. Think of the paths we give to `mv` to tell it what we want to move, and where, or the path we give to `cd` to tell it where to move _us_ to.

## Flags

Flags, sometimes called switches, are a special kind of argument we can pass to a command. While an argument is usually a piece of information we want the command to work on, a flag tells the command _how_ to do its work.

Let's look at our friend `ls` again.

```
$ ls
```

The output of this command is a list of all the files in the current working directory, given as relative paths. But ls has a lot of different ways to display the data. For example, what if we want ls to show us a marker for which of our directory entries are files, and which are directories? We can use the `-F` switch. Note the `-` in front of that -- by programming convention, we mark flag commands with the `-`, which is sometimes spoken out loud as "hyphen", "dash", "minus", or "tack." If you work with me in person, you will hear me call it "minus" -- I would tell you to run `ls -F` by saying "run ell-ess minus capital eff."

```
$ ls -F
```

You'll see that the regular files didn't change, but the directories now have a `/` after them, to help you see that they are not regular files. Next, try the `-a` switch for ls (`a` stands for _all_.)

You saw a couple new entries: they are the `.` and `..` special entries. That's right, those path elements are actually treated as special directory entries that automatically exist in every directory. When you use them, the path expansion doesn't replace them, but uses them as an actual directory entry on the path. (Try this: `ls ./././././`!)

You'll be able to see that these special directory entries are, in fact, directories by using both the `-a` and `-F` flags together. `bash` can process several different flags or switches after just one `-`, so there are two ways to issue this command:

```
$ ls -aF
$ ls -a -F
```

These two commands are exactly equivalent, as you can see if you run them both and compare the output that you see.

You can use flags and regular arguments together:

```
$ ls -aF ~
```

WHOA! Look at all those entries! Try comparing that to what you saw in your home directory before:

```
$ ls ~
```

What's new? Looks like you saw a bunch of files or directories that start with `.` when you used the switches, and they disappeared again when you took them away.

This is because `bash` allows you to create what's called a _hidden_ file. The way to do this is very simple: you just start your filename with `.`. Files that are named this way don't show up in a regular directory listing, but do show up if you use the `-a` switch. Try it now:

```
echo "shhhhhh!!!" > .my_hidden_file
```

You'll be able to cat your file, move your file, or eventually delete your file (we'll get to that soon) just like any regular file. But it won't show up when you list the directory, unless you use `-a`.

What's the point? Well, many programs save their configuration data, like the settings you choose, into files in your home directory so that the settings will still be there after the program exits and then starts again. The programmers don't want you messing with them, and they know that when you go looking for your own files, you don't want to sift through the clutter of every configuration file ever made by any program you've ever run in the history of your computer. They also don't want you going "aaaah what's this, I didn't make this file, it's probably some garbage!" and deleting all your configuration files by mistake (and then filing a bug report because their software lost all your settings!!)

So programs and programmers use these hidden files for data that they know you won't want to work with directly most of the time, and it helps reduce the amount of time you spend messing around with files that neither of you actually wants you messing with by accident.

## Manpages

Most systems come with a help system called `man` (for _manual_, not _mansplaining_!) already installed. It runs as a command inside your shell. When a new program is installed on your system, it will often install its own manpage -- a new help entry in the help system.

Before we read this, I'll have to introduce you to some new ways to talk about commands. We'll use `mv` as an example.

When we talked about `mv`, I told you that it can take two paths: the first is the file you want to move, and the second is the destination you want to put it at. In a manpage, that will be written in the form of an example, like this:

> mv source target

`source` and `target` are what's called "metavariables": stand-ins for values that you, the user, are expected to supply. In the manpage, they may be underlined or otherwise emphasized.

Now, `ls` doesn't require an argument, but it can use and understand them. That means that the argument is optional. You'll see that written in example form like this:

> ls [file ...]

The metavariable `file` (like other metavariables, it usually displays with an underline or other emphasis) is in square brackets `[]`, indicating that it's optional. As you know, when you type this command, you didn't include any square brackets. They're there in the example form to show you that you can leave this argument out if you want. The `...` metavariable is a special one that stands for "any number of additional values for the metavariable that came before me." In plain English, it means here that you can give `ls` as many files (paths) as you want. (Go ahead, try it!)

So now that you've gotten that down, let's get into a manpage. Use the `man` command to launch the help view; next, list the name of the command you want to look up, because commands typically install their help pages with the same name as themselves.

```
$ man ls
```

You're looking at the first screen of the help page. Press the spacebar to go forward one screen, and the `b` key to go back one. When we're done looking at it you'll press `q` to quit and get your command prompt back.

You'll see the example line up top, as I described, but I left something out: there's another optional block full of alphabet soup! It looks something like this: `[-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1]`

As you already guessed, the square brackets mean that all of these are optional (of course they are! We've called `ls` by itself plenty of times, so clearly every argument is optional.) You can also notice that the first character in the square brackets is a `-` -- that means that each individual letter of the jumble that comes after it are letters that can be used as flags to `ls`.

There are so many! How do I know what they do? Well, that's what the rest of this very long manpage contains. Hit spacebar a few times to roll forward in the document (you can always go back with `b`.)

> Fun fact: when you press keys while a program is running, you're using `stdin` again! Your typing is being added to the `stdin` _buffer_, and the program is capturing data off that buffer and using the keycodes you pressed to decide what you want it to do.

As you walk through this document, you'll see an explanation for each and every metavariable and switch that's listed in the example. Some of the explanations may not be familiar to you yet, or might refer to operations you don't yet know how to do. That's fine -- use your Google-fu to check out ones you're interested in, and leave the others for later when you have more context. Your manpage will always be here for you.



