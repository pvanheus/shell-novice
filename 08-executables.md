---
layout: page
title: The Unix Shell
subtitle: Executable scripts and the PATH
minutes: 15
---
> ## Learning Objectives {.objectives}
> * Understand what an executable script is
> * Know the elements that make a script executable
> * Be able to create an executable script
> * Know what an environment variable is
> * Know the function of the PATH environment variable
> * Be able to set the PATH environment variable
> * Be able to make the PATH setting permanent by adding it to .bash_profile

In the previous session we created a shell script, a small program that when
interpreted by the `bash` shell ran some commands. We ran the one of the shell scripts
with the command:

> ~~~ {.bash}
> $ bash do-stats.sh *[AB].txt
> ~~~

The limitation of running the script like this is that the script must be
in the same directory as the data. Ideally, we'd like to keep our programs,
including scripts, in a software collection in a separate directory from where
we store our data.

In addition, we would like to run our script like we run a command. So:

> ~~~ {.bash}
> $ do-stats.sh *[AB].txt
> ~~~

To achieve this we need the script to be *executable* and *in our PATH*.

## Executable scripts and interpreters

For a script to be able to run as a command we need it to be executable. To understand that,
let's go to the `north-pacific-gyre/2012-07-30` directory and do a *long listing*:

> ~~~ {.input}
> $ ls -l 
> ~~~
> ~~~ {.output}
> total 272
> -rw-r--r-- 1 pvh messagebus   86 Mar  7 14:01 do-stats.sh
> -rw-r--r-- 1 pvh messagebus  184 Nov 17 09:02 goodiff
> -rw-r--r-- 1 pvh messagebus  198 Nov 17 09:02 goostats
> -rw-r--r-- 1 pvh messagebus 4406 Nov 17 09:02 NENE01729A.txt
> ...
> ~~~