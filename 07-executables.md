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
> $ ls -l -F | head -n 5
> ~~~
> ~~~ {.output}
> total 272
> -rw-r--r-- 1 pvh messagebus   86 Mar  7 14:01 do-stats.sh
> -rw-r--r-- 1 pvh messagebus  184 Nov 17 09:02 goodiff
> -rw-r--r-- 1 pvh messagebus  198 Nov 17 09:02 goostats
> -rw-r--r-- 1 pvh messagebus 4406 Nov 17 09:02 NENE01729A.txt
> ...
> ~~~

Notice the `-rw-r--r--` lines. These specify the *permissions* associated
with those files. The first block is for the owner (`rw-`, read and write permission)
and then for members of the same group as the owner and then for everyone else (in both
cases `r--`, read permissions). Any script that we want to run as a command must
have *executable* permission set and we can do that with the `chmod` command:

> ~~~ {.bash}
> $ chmod +x do-stats.sh
> ~~~

Now the long listing shows that the `do-stats.sh` script file is executable:

> ~~~ {.input}
> $ ls -l -F | head -n 5
> ~~~
> ~~~ {.output}
> total 272
> -rwxr-xr-x 1 pvh messagebus   86 Mar  7 14:01 do-stats.sh
> -rw-r--r-- 1 pvh messagebus  184 Nov 17 09:02 goodiff
> -rw-r--r-- 1 pvh messagebus  198 Nov 17 09:02 goostats
> -rw-r--r-- 1 pvh messagebus 4406 Nov 17 09:02 NENE01729A.txt
> ~~~

This permission setting will allows Linux to run the script as a command, but there
is a piece of information missing: how do we want to run the script? When we ran the
script previously we told Linux what program we wanted to use to interpret the script:

> ~~~ {.bash}
> $ bash do-stats.sh *[AB].txt
> ~~~

We told Linux that we wanted the `bash` program to interpret the `do-stats.sh` script.
If we are going to run it as a command, we need to tell Linux how to interpret the script.
We can do this by adding a *hash bang* line to the top of the script. This is named
after the two characters `#!`. It has to be the very first line of the script:

> ~~~ {.bash}
> #!/bin/bash
>
> for datafile in $@
> do
>   echo $datafile
>   bash goostats $datafile stats-$datafile
> done
> ~~~

The path `/bin/bash` tells Linux which command to use to interpret the script. This has
to be an *absolute path* (i.e. it starts with /). Since we know that `bash` is the command
we want to use to interpret the script, we can find the path to `bash` with `which`:

> ~~~ {.input}
> $ which bash
> ~~~
> ~~~ {.output}
> /bin/bash
> ~~~

> ### Which shell? bash and sh {.callout}
> We have been using `bash` for these lessons because it is the default shell on Linux
> for most users. There is an older shell called `sh` (`/bin/sh`) that `bash` was
> based on. Often scripts use `#!/bin/sh` instead of `#!/bin/bash` because
> many of the features of `bash` are also present in `sh`.
>
> This distinction is only really important when you are not running a script
> in your own environment. For example, if you are running a script on a
> cluster you might need to use `#!/bin/sh` depending on the cluster configuration.
> If you do need to use `#!/bin/sh` you need to make sure your scripts work
> with `sh`. There are some tips on [stackoverflow](http://stackoverflow.com/questions/5725296/difference-between-sh-and-bash)
> but a full discussion of differences is beyond the scope of this lesson.

With the hash bang line in place and the script executable, we can now run it. We need
to specify the path to the script though:

> ~~~ {.input}
> $ ./do-stats.sh *[AB].txt
> ~~~

If the script is not executable we'll get an error:

> ~~~ {.output}
> -bash: ./do-stats.sh: Permission denied
> ~~~

If the script is executable but the *hash bang* line is incorrect we might get a different error:

> ~~~ {.output}
> -bash: ./do-stats.sh: bash: bad interpreter: No such file or directory
> ~~~

## Environment variables and the PATH

Now that we have an executable script we would like to be able to keep it in different place
to where we keep our data but we still want to be able to run it as a command. To make this
happen we have to modify our `PATH`. The PATH is an *environment variable*.

Variables are bindings between names and values. We have seen them before in the context of loops
(loop variables) and scripts (variables for script arguments such as `$1`). While loop and
script variables only last for the duration of the loop or script we are running, environment
variables keep their values for the whole duration that the shell session we're in is running.

The `PATH` environment variable is used to store the locations that `bash` searches for commands.
Remember that to get to the value of a variable we need to use `$`. Let's examine it:

> ~~~ {.input}
> $ echo $PATH
> ~~~
> ~~~ {.output}
> /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/people/pvh/.rvm/bin
> ~~~

The value of the `PATH` environment variable is a colon (':') separated list of directory names. When we
type a command name (e.g. `ls`) in the `bash` shell, `bash` will search each of these directories in order
until it finds a executable file matching the command name. We can find out where these files are found
using `which`:

> ~~~ {.input}
> $ which ls
> ~~~
> ~~~ {.output}
> /bin/ls
> ~~~

## Choosing a location for your scripts

Where you choose to store your scripts has to do with how you intend to use them. Project specific scripts
might be stored in a directory associated with the project in question. Scripts which are more generally
useful could be stored in a path such as `~/bin`. Remember, however, that `~` is a special character
interpreted by the `bash` shell and we might not want to use it in our `PATH` specific. As a substitute you can
use `$HOME`, the value of the `HOME` environment variable, which always contains the name of your home
directory. So let's create a directory `$HOME/bin` to store scripts in.

> ~~~ {.bash}
> $ mkdir $HOME/bin
> ~~~

Now let's copy the `do-stats.sh` script to that `$HOME/bin` directory:

> ~~~ {.bash}
> $ cp -p ~/data-shell/north-pacific-gyre/2012-07-03/do-stats.sh ~/bin
> ~~~

The `-p` option for `cp` tells it to preserve the permissions (including the execute permission)
when copying the file.

And now let's add `$HOME/bin` to the `PATH`:

> ~~~ {.bash}
> $ PATH=$PATH:$HOME/bin
> ~~~

When adding a directory path to `PATH` we need to include the original value of `PATH` (`$PATH`)
on the right hand side because we want to add a value, not replace the value of `PATH`. The new
value we are adding (`$HOME/bin`) and the old value we still want to keep (`$PATH`) are
separated by a colon (`:`). In this example we are adding `$HOME/bin` at the end of `PATH`, which
means it will be searched last. If we want it to be searched first we could add it at the start
of `PATH`.

Now that `do-stats.sh` is executable, has a correct hash bang line, and is in a directory
that is in our `PATH` we can use it like a command:

> ~~~ {.bash}
> $ cd ~/data-shell/north-pacific-gyre/2012-07-03 
> $ do-stats.sh *[AB].txt
> ~~~

The `PATH` setting is only valid for the `bash` session we are in when we set it. If you want
the setting to be inherited by other programs that `bash` starts you need to `export` it
with:

> ~~~ {.bash}
> $ export PATH
> ~~~

## The .bash_profile and making settings permanent

The way we have set `PATH` will allow us to put scripts in `$HOME/bin`, make them executable
and then run them like commands; this will only work for the shell we are in when we
set `PATH`. If we want to make this setting permanent we need to add it to the `bash` settings
file, which we can do by adding the change we want to `~/.bash_profile`. Edit this with:

> ~~~ {.bash}
> $ nano ~/.bash_profile
> ~~~

And add the settings you need:

> ~~~ {.bash}
> PATH=$PATH:$HOME/bin
> export PATH
> ~~~

These settings will now be set every time you log in.