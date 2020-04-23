# Input and Output

So far we've run a few commands that write out information to the terminal for you to read. For your convenience, `bash` has some built-in methods that we'll find very useful soon. First, let's see how to read the contents of a file to the terminal.

## The Internet Is For Cats

The `cat` command is the way we turn the contents of a file directly into output on a screen. Pick a text file in your home directory, or try one of the system files like `/etc/hosts`. Then give it to the `cat`:

```
$ cat /etc/hosts
```

This will print the contents of the file to the screen. Cool -- not terribly useful in this case, but cool. However, remember that part of the job of the operating system, and the shell, is to help programs work together with each other. One of the ways it can do that is with _pipes_.

## A Series Of Tubes

The Internet might not exactly be one, but your shell commands can be. When a program writes output that you see on the terminal, it does it by writing to a system construct called `stdout` ("standard output.") There is a corresponding construct called `stdin`, allowing an interactive program to take in input supplied from outside -- any time you type information into a running, interactive program, you're using `stdin`. Our `bash` shell allows us to chain programs together in a row, connecting the output of one to the input of the next, kind of like that gross movie. We call this connection a `pipe`, and it's denoted by `|`, which you'll hear called the _pipe character_ out loud.

We know about the `cat` command, which just sent a bunch of output to `stdout` so we could see it. What if instead of seeing the complete contents, we want to know how many lines are in the file?

We can use the `wc` (word count) command to count how many lines, words, and characters are in its input. We'll connect the output of our `cat` command to the input of the `wc` command with a pipe -- in more typical speech, we'll `cat` the file and pipe it to `wc`.

```
$ cat /etc/hosts | wc
```

It's as simple as that -- we enter both commands, one after the other, with our pipe character in between. By looking at the raw output we sent earlier, and the output of the `wc` command, we can see that it received the right input and gave accurate counts.

## Getting Chatty

Where `cat` needs to read the contents of a file to get its input, we can use the more flexible `echo` command to make the terminal say just about anything we want. Try

```
$ echo "Mr. Daniel looks like a spaniel"
```

Like a dutiful echo, it takes the input you gave it (mind the quotation marks!) and hands it right back on `stdout`. Try piping this new command to `wc`.

## Strings Attached

If you have worked with other programming languages, you may know that this item of text about poor Mr. Daniel is what we call a _string_. A string is a single chunk of textual information that we use or store on the computer. We call it this because it's a long, well, string of characters, in order. Now, you'll see that we used quotes to group the whole Mr. Daniel text together into one large block. When it came back out, the quotes were missing. They aren't part of the string -- they're just a signal to `bash` that we want all the things inside to be part of the _same_ string. Otherwise, `bash` will treat each space, tab, or other _whitespace character_ as a separator between pieces of input. The shell is versatile, but it's not that smart -- a thing you'll be reminded of over and over. It can't guess the difference between "a space between a command and its arguments" and "a space in the middle of my input." You have to tell it.

That said, in the case of `echo` specifically, our command will actually work with or without quotes. That's because `echo` will take all the strings that come after it and stick them together in its output. `cat` actually does something similar -- if you give it the paths to several different files, it will _concatenate_ all the files' contents together, in order, in the output. That's why it's called `cat`: it's short for __concatenate__.

### Paths Are Strings

Now that you know that, you can see that we are in a pickle if a directory name has a space in it. This can still happen -- if you're using MacOS, you'll see it used liberally, even by the system itself. However, when supplying a path with a space in it as an argument to your commands, you will want to put quotes around the path. There are other ways of dealing with weird characters (including more quotes!) inside a path or string, but we'll hear about how to deal with those later on.

