# Text Tools

Earlier, we learned about using `echo` and `cat` to write lines of text into a file, or review things that are there. We also used `wc` to find out some things about our files. But there's a lot more you might want to do with a text file, like interactively editing it, or finding out which files contain certain words.

## grep

Little tool with a funny name. `grep` is a command line utility that allows you to search through a file or `stdin` for particular lines, then print the ones that match a pattern.

## cut

Often, files on the filesystem or values in some output act like tables, with whitespace characters or commas separating sequential values in a line.

## sed

The Stream EDitor, `sed`, lets you apply a regular expression to a stream (like `stdin`) and print the results. Similar to `grep`, but rather than just printing or counting matching lines, sed actually gives you the results of the original stream with your changes applied. That means if you need to replace all instances of a word in a text, `sed` is a good choice.

## awk

Dark magic. `awk` is an extremely full featured streaming text processor. `man awk`, of course, will get you lots more info.

