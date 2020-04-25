# Working With Files

In the last section, we used `mkdir` to create a directory so that we could try out some basic navigation. Here, we'll look at how to create, read, and remove our own files.

Please note that we are working with power tools here. Unlike more user-friendly operating systems, where it's sometimes a bit hard to permanently delete things even when you want to, changes made directly to our filesystem are immediate and permanent.

As you follow this guide (at your own risk!) make sure you always know exactly what files you're operating on, and be sure you mean to do what you are about to do. As we've explored already, a relative path can evaluate to a totally different absolute path, depending on what working directory you're in. Using `ls` and `pwd` frequently to make sure you're where you think you are is a very good idea.

Now that I've put the fear in you, let's go into the directory we made in the last step. We'll start by modifying files here, where you have less chance of accidentally writing over something else important.

```
$ cd ~/wild_blue_yonder
```

There's nothing in here, so we're going to start by creating a file. We'll use _file redirection_

## File Redirection

This is another way of connecting things to `stdin` and `stdout`. Instead of connecting a `stdout` to a `stdin`, we can use the characters `>` or `<` to write to, or read from, a file.

### Writing to Files

You can write a file on the filesystem by redirecting `stdout` to the path where you want the file to be created. That means you'll run a command that produces some output on `stdout`, then supply the `>` file redirection character, and then the destination path, which can be relative or absolute. Let's try it now:

```
$ echo "you say potahto" > potato
```

Try using `ls` and `cat` to check out the new file you made. It's a real file! You can use Finder or your file browser to navigate to it and open it up. It's just a single text file, containing exactly what we told it to. BOY HOWDY.

Now, what happens if we cat to this file again?

```
$ echo "you say potato" > potato
```

Go `cat` that file, and see if it says what you expected.

You may have been surprised to see that your original "you say potahto" was gone! This is because the `>` file redirection is the _write_ operation: it will write your string to your path, WRITING RIGHT OVER anything that was there before. This is why I have you working in your own `wild_blue_yonder` directory for now: I don't want you overwriting things that you wanted to keep!

Now, obviously, sometimes you want to be able to write new text into a file WITHOUT destroying what was there. That's a job for the _append_ operator: `>>`. Yeah, it's just two of them. Go try it out:

```
$ echo "you say potahto" >> potato
```

Check out the contents of `potato` with `cat` again. This time you should see your new input on a new line, with the old file contents safely intact.

### Reading From Files

OK, we have our file, and we have, like a goldfish, immediately forgot how many words were in it. We can cat it to `wc` like before:

```
$ cat potato | wc
```

And rather than flinging a mammal and a tuber into a water closet, it will tell us that the `potato` file has 6 words on 2 lines.

But we don't actually need to use `cat` for this -- we can use `bash` to send `wc` the input DIRECTLY FROM THE FILE. We use the `<` redirection for this. Because the arrow is pointing the other way, we still put the command on the left, and the file on the right. The left-facing arrow means "take the contents on the right, and write them to the thing on the left."

```
$ wc < potato
```

We do it this way because, again, `bash` is not too smart. It wants the first thing in each command to be an actual command -- something it can execute. A regular old non-executable text file can't be in that position because `bash` simply can't parse what we want it to be doing in that case. Instead, we supply the command first, followed by the left-facing redirection operator and the file to take the input from.

## Moving A File

OK, we're convinced. It should have been potahto all along. But our file is still called `potato`! We'll have to do something about this.

When we refer to a file in `bash` we do it by its path. We could say in this case that the path is the full _name_ of the file. This means that, as far as our operating system is concerned, changing the name of the file is the same thing as moving it. There's no difference. After the operation, the same piece of information is at a different absolute path. To perform this operation in either of its forms, we use `mv`.

### The Arguments of Move

The first argument of `mv` is simple: it's just the path, relative or absolute, to a file or directory that exists on the filesystem. (It has to exist; if you try to change the name of something that doesn't exist, `bash` will inform you that that's nonsense.) The second argument of `mv` can be:

* The relative or absolute path of the new name you want the file or directory to have OR
* The relative or absolute path of an EXISTING DIRECTORY you want the file or directory to be moved into.

So let's sort that out. There are several possible situations:

1. The destination path does not exist. `mv` assumes you want to create a new item with this path. In this case, _the enclosing directory must already exist_.
    1. If it doesn't, we can't create an entry inside a non-existent directory, and the operation will fail.
    1. If it does, `mv` changes the item's path to the one you asked for.
1. The destination path exists, and refers to a file.
    1. If the thing you are moving is also a file, THE EXISTING FILE AT THE DESTINATION PATH WILL BE REPLACED. 
    1. If the thing you are moving is a directory, this operation will fail.
1. The destination path exists, and refers to a directory. `mv` assumes you want to move the item into this directory, and creates a new path for the item by sticking the LAST ELEMENT of the item's current path (i.e. the name that `ls` shows for this item) onto the directory path you have given.

Let's put some of this into action and see how it behaves. `pwd` to make sure you're still in your `wild_blue_yonder` directory, and then create a couple of new directories:

```
$ mkdir foo
$ mkdir bar
```

Now try the following:

- Rename your `potato` file to `potahto` (use `ls` to see if you've gotten it right)
- Try to move the `potahto` file into the nonexistent directory `kalamazoo`
- Move the `potahto` file into the existing directory `foo`
- Move the directory `bar` inside the directory `foo`
- From inside `foo`, try to rename the directory `bar` to `potahto`

Try using `cd` to move into different directories and do the same kind of moving and renaming by changing the relative paths you use. You can use things like `ls`, `ls .`, and `ls ..` to help you see what you're doing. DON'T NAVIGATE OUTSIDE YOUR `wild_blue_yonder` directory for now -- you're playing around moving files and you want to be very sure you know where you're doing that right now.

Any time you are manipulating paths, `ls` is your friend. By using `ls` to describe what exists (if anything) at the path you give, you can be sure that the file you are going to create or move will not overwrite something you cared about.