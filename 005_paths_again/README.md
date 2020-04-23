# More About Paths

By now you are clearly getting the impression that paths are at the bottom of a lot of stuff on the command line. You'd be right!

When we use the computer to run a program, that program is a file (an executable one.) The program might remember some settings you had set last time you ran it -- if so, it saved those settings in a file, and loaded them from that file when you started it again. You might have used the program to play back a video file, open and edit a text file, or operate on dozens of other types of file.

In short: nearly everything you work with is a file, and every file has a path. The path is how the computer tells the filesystem what file to operate on.

## Path Expansion

Because we've already touched on the special characters `~`, `.`, and `..`, you've already seen path expansion in action. Path expansion is the way that `bash` turns a shorter, relative path into an absolute path by _expanding_ the special characters you typed into a longer path segment that you intended to use.

What else can it do?

### Working with Multiple Files

Get into your `wild_blue_yonder` directory again and make some more files. You can call them whatever you want, but make sure a few of them start with the same few letters, and at least one is totally different. For example, here are mine:

```
$ echo hello > alice.txt
$ echo goodbye > alexander.txt
$ echo "granny smith" > apple.txt
$ echo navel > orange.txt
```

Now, I can `ls` and I'll see them all again.

```
pomelo:wild_blue_yonder chi$ ls
alexander.txt apple.txt
alice.txt     orange.txt
```

Now, suppose I only want to see the ones that start with `a`. That is, I want to see all the paths relative to my directory whose names start with `a`, and then have absolutely any combination of letters afterwards. It turns out there is a special character for "absolutely any combination of letters": `*`

```
$ ls a*
```

We call this the _wildcard_ character. It can occur anywhere in the path, and it will do what's called "greedy" matching: it will match as many characters as it can. Try some other ones:

```
$ ls *a
```

```
$ ls *.txt
```

```
ls *a*
```

Now, you can see that just like we used `ls` with a path before to list the contents of another directory, we are able to supply `ls` with a wildcard path and have it list several different paths. We can also use path expansion to do other operations on multiple paths at once.

From the last exercise, you should have a directory inside your `wild_blue_yonder` directory called `foo`. (If you don't, first make sure you're really in the right directory! Then try creating the directory again: `mkdir foo`)

We can use path expansion to move many files at once into that directory! From the `wild_blue_yonder` directory, let's move all text files beginning with `a` to the `foo` directory. Just like before, we'll supply our two paths, the source and the destination:

```
$ mv a*.txt foo
```

What does `ls foo` show you now? What's still in your current working directory?

### What Really Happened

I told you earlier that `mv` takes two arguments: the source and the destination. It's more correct to say that `mv` takes AT LEAST TWO arguments. (Check out `man mv` -- that manpage shows both of the two different ways to call `mv`.)

The last argument in line is the destination to move something to. All the other arguments in between are items you want to move to the destination. However, since moving many files to another _file_ doesn't make any sense, calling `mv` in this way requires that the last thing in line is a directory. In the manpage, you'll see that the form with more than two arguments uses `directory` as the last metavariable for this reason.

What do you think will happen if you do this:

```
$ ls foo/*
```

Try it. Was it what you expected? How about this:

```
$ mv foo/* .
```

List your working directory and your foo directory and see what happened.

### Risky Business!

Now you can see why it's important to use `ls` to be sure of where you are, and what your path refers to. If you guess wrong about what files will be matched by your expanded path, you could affect files you didn't mean to work on at all, or move them to completely the wrong place. That's bad enough when you're moving files around -- imagine if you'd _deleted_ them?

### Deletion

Yes indeed, with great power comes great responsibility, and it's time to learn a deceptively simple command that is guaranteed to make you ruin at least one computer system at some point. I'm talking about `rm`.

The `rm` command (for _remove_) takes any number of paths, including path expansions, and permanently deletes ALL of the files listed. It doesn't move them to the trash, or put them in another folder. It literally tells the filesystem to _forget_ that these paths exist, and mark any area of the hard disk that contained any part of those files as "empty, ready to write." The data that was in the files will not necessarily be overwritten right away, but you cannot, by any ordinary methods outside of a computer forensics laboratory, get those files back. They are gone. Donezo.

> Never `rm` before you `ls`

Because `rm` performs the exact same `bash` expansion that `ls` does, it is possible to preview what exactly you are about to delete. My advice to you is this: when you're writing your `rm` command, start by writing it as an `ls` command instead. Take a careful look at the output and make sure you ONLY see files you intend to destroy. Do a `pwd` for good measure, since `ls` will show you relative paths and you want to see what they will be resolved against. ONLY THEN should you re-issue your command as an `rm` command: when you are absolutely certain you understand what will be `rm`d.