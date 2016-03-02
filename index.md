---
layout: page
title: The Unix Shell
---
The Unix shell has been around longer than most of its users have been alive.
It has survived so long because it's a power tool
that allows people to do complex things with just a few keystrokes.
More importantly,
it helps them combine existing programs in new ways
and automate repetitive tasks
so that they don't have to type the same things over and over again.
Use of the shell is fundamental to using a wide range of other powerful tools 
and computing resources (including "high-performance computing" supercomputers).
These lessons will start you on a path towards using these resources effectively.

> ## Prerequisites {.prereq}
>
> This lesson guides you through the basics of file systems and the
> shell.  If you have stored files on a computer at all and recognize
> the word “file” and either “directory” or “folder” (two common words
> for the same thing), you're ready for this lesson.
>
> If you're already comfortable manipulating files and directories,
> searching for files with `grep` and `find`, and writing simple loops
> and scripts, you probably won't learn much from this lesson.

> ## Getting ready {.getready}
>
> This lesson will be run on the SANBI computer cluster and you will need to be able to log in there. If you do not know how to do
> that, take a look at [these instructions](http://docs.wp.sanbi.ac.za/2016/03/01/logging-in-to-the-sanbi-cluster-head-node/). If you see
> the command line prompt that looks like:
>
> ~~~ {.input}
> username@queue00:~$
> ~~~
>
> You are ready to proceed. In the examples this prompt is shown as **$** for brevity. Once you are logged into the SANBI computer cluster:
>
> 1. Download [shell-novice-data.zip](./shell-novice-data.zip). You can use the **wget** command for this:
>
> ~~~ {.input}
> $ wget https://github.com/pvanheus/shell-novice/raw/gh-pages/shell-novice-data.zip
> ~~~
>
> 2. Unzip/extract the file using the **unzip** command:
>
> ~~~ {.input}
> $ unzip shell-novice-data.zip
> ~~~
>
> This will unpack the data into a directory *data-shell* in your home directory.
> In the lesson, you will find out how to access the data in this folder.


## Topics

1.  [Introducing the Shell](00-intro.html)
2.  [Files and Directories](01-filedir.html)
3.  [Creating Things](02-create.html)
4.  [Pipes and Filters](03-pipefilter.html)
5.  [Loops](04-loop.html)
6.  [Shell Scripts](05-script.html)
7.  [Finding Things](06-find.html)

## Other Resources

*   [Reference](reference.html)
*   [Discussion](discussion.html)
*   [Instructor's Guide](instructors.html)
