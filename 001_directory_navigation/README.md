# Directories and Navigation

## Where am I?

As we discovered in the last section, when you first start `bash` -- we call this opening a new _session_ -- we start out in your home directory. This is denoted by the `~`, which always means "the home directory of __the currently logged in user__." That is, whoever's username you see in the prompt string, that's whose home directory you'll end up in if you go to `~`.

`~` is one of a couple of special _path elements_ you'll get to know.

## So Where's Home, Exactly?

Earlier we did 

```
$ ls
```

and based on the things in the directory, you might have been able to tell that you were looking at things from your personal user directory. However, there's a better way to tell what directory you're in, which is to say, what `ls` will show you. Try running this command:

```
$ pwd
```

This will give you some output, which might have looked like `/home/janeybob` or `/Users/Angry Badger`. This is the path to your _working directory_ -- the directory you're in right now, and the directory where any command you run will be executed.

But wait a minute! We were in `~`! That's not `~`!

What you're looking at now is the _absolute path_ of your home directory. This is kind of like the GPS coordinates of your directory: no matter where you are right now, or who the logged in user is, that's a set of absolute directions to another directory. In order to navigate the directories on your computer, the shell always starts by turning your instructions into an absolute path. For you, right now, `~` _expands to_ the absolute path that you saw when you ran `pwd`.

## Changing The Scenery

As we're all finding out here in 2020, it's not always so great to have to stay home all the time. But using computers, we can often see what's going on somewhere else, without moving at all.

First, let's make somewhere to move to. In your terminal, we're going to create a new directory. Since we're already in your home directory, the new directory will be created right there.

```
$ mkdir wild_blue_yonder
```

Now, run `ls` again. In the list of contents in your home directory, you should see your new directory sitting there. Let's see what's inside of it! We do this by giving `ls` an _argument_: an extra item of input that gives the command more information about what we want to do.

```
$ ls wild_blue_yonder
```

Well, that wasn't very exciting. Nothing came back. There's nothing actually inside your brand new directory -- it's like an empty file folder, waiting for some contents to be placed inside of it. But what you did see is that by passing an argument to `ls`, we were able to change WHAT DIRECTORY it lists the contents of. (That's what `ls` stands for: "list"! You'll see a lot of that, where commands are constructed from just one or two letters of an English word. There's a reason for this: programmers are very lazy, and don't like to type a lot.)

> Caution: `bash` and Unix-like operating systems are CASE SENSITIVE. If you try to ls WILD_BLUE_YONDER, it won't work.

But we can do one thing with this new directory that's actually interesting. We can go into it. For that we'll use a new two-letter command to change directories:

```
$ cd wild_blue_yonder
```

Go ahead and run `ls` again. Yup... you got a big fat nothing, exactly what you probably expected. You are in a directory without any contents, so `ls` has nothing to show you. But now, let's run `pwd` again, to see the absolute path of the working directory, where we are now. What do you notice?

### Path Elements

Let's examine what an absolute path looks like. The absolute path to your directory starts with `/`, and there are three of those in the path now. `/` is the Unix _path separator_: it marks the boundary between the names of successive directories that lead, like a trail of breadcrumbs, to the directory you want to name. You know that the absolute path to your home directory ended in your username, and when you entered the `wild_blue_yonder` directory, it added that to the end of your path, separated by a `/`. This means that for each `/` you find in the path, you're going one layer deeper into your file system. `wild_blue_yonder` is inside your home directory, which is named the same thing as your username -- kind of like having your name written on your door. Your home directory is inside of a directory called `home` or `Users`, depending on your operating system -- this is the directory that holds all the users' home directories on your computer.

But what is THAT directory inside of? The only thing before that is a `/`, and that's just a path separator. What's it separating the users' directory from?

### The Root Directory

That's a bit of a trick question, is what that is. At the very top level of the file system is one big directory that holds absolutely everything on the whole system. Every absolute path starts here. We call it the root directory, and we'll often hear the file system described as a `tree`, as it branches out level by level. The root directory doesn't actually have a real name -- you won't see it listed in `ls`, because you'd have to be listing the contents of the directory that the root directory is inside of. But there's no such thing -- the root directory is all alone. It's lonely at the top.

Every absolute path is evaluated STARTING WITH THE ROOT DIRECTORY. It's the invisible empty member before that first `/`. That makes it easy to tell if the path is absolute -- absolute paths start with `/`. _Relative paths_ don't.

### Relative Paths

You've already used a relative path. You used it to enter the new directory you'd made, using `cd`. You didn't type an absolute path starting with `/`; you just typed the name of a directory that was inside your current _working directory_. Your _relative path_ was evaluated _relative to_ the path of your working directory. You didn't supply the whole path as your argument to `cd`, you just supplied the relative path, and `bash` used that plus your working directory to decide what absolute path you meant.

There are other ways to form a relative path -- that is, something that is not absolute, but that has enough information to decide what absolute path it should be evaluated _relative to_. One of these ways is to supply the `~` as the first element of your path. That means, roughly

1. Expand the ~ into the absolute path to my home directory
2. Evaluate the rest of the path relative to that -- in other words, just stick it on the end.

For example, `cd ~/Documents` would place you in a directory called Documents that's inside of your home directory, if such a place exists.

Try this:

```
$ cd ~
$ pwd
```

Are you where you expected to be? Where else can you go?

If you ever get totally confused, and don't know what directory you're in anymore, both of those commands will be a help. The second one tells you where you are; the first one just yeets you right back out to your home directory where it's nice and safe.

### More Relative Paths

You've seen how `~` expands to "my home directory". There are a few other magic expansions to be aware of.

`..` is something that you'll use all the time. It translates to "up one level from where I am now." Run `pwd` again to take note of where you are. Then try

```
$ cd ..
$ pwd
```

Where are you now? What happens if you do both of those things again? And again?

(Before too long, you will top out in the root directory. There's nothing above the root directory, so `..` from there is just the same place again, it doesn't magically deliver you into warp space or something.)

`.` is another magic `bash` expansion, and it's one that will seem useless to you at the moment. It just means "the current working directory." That's right: if my working directory contains a directory called `elephant`, then `cd ./elephant` and `cd elephant` will land me the exact same results: I'll be inside `elephant`. Keep it in your mind, though, because later when we run certain kinds of command, knowing this will help you understand why some things look the way they do.

Finally, it's worth mentioning `/` here. Technically, it's an absolute path, but when you give it by itself, it takes you right to that good old root directory. You can always think of all absolute paths as also being relative paths, being evaluated relative to the root directory. Because that's not confusing at all.

## What's So Special About A Working Directory

As you've seen, the behavior of something like `ls` depends on what directory you're in when you run it. That's because the directory you're in when you run the command also becomes _the directory the command runs in_ -- that is, any paths it evaluates during the course of running, such as paths you passed it yourself, will be evaluated relative to that working directory. Right now, this seems obvious and trivial, but it's a small distinction now that will matter a lot more later.