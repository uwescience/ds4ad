### Background
At a high level, computers do four things:

-   run programs
-   store data
-   communicate with each other, and
-   interact with us

They can do the last of these in many different ways,
including through a keyboard and mouse, or touch screen interfaces, or speech recognition using systems.
While such hardware interfaces are becoming more commonplace, most interaction is still
done using screens, mice, touchpads and keyboards.

We are all familiar with **graphical user interfaces** (GUI - windows, icons and pointers).
They are easy to learn and fantastic for simple tasks where a vocabulary consisting of
"click" translates easily into "do the thing I want". But this magic relies on
wanting a simple set of things, and having programs that can do exactly those things.

If you wish to do complex, purpose-specific things it helps to have a richer means
of expressing your instructions to the computer. It doesn't need to be complicated or
difficult, just a vocabulary of commands and a simple grammar for using them.

This is what the shell provides - a simple language and a **command-line interface**
to use it through.

The heart of a command-line interface is a **read-evaluate-print loop**, or REPL, called
so because when you type a command and press the Enter (or Return) key, the shell:
1. Reads it
2. Executes (or "evaluates" it)
3. Prints the output

and then prints the prompt and waits for you to enter another command.

### The Shell

A shell is a program like any other.
What's special about it is that its job is to run other programs
rather than to do calculations itself.
The most popular Unix shell is Bash,
the Bourne Again SHell
(so-called because it's derived from a shell written by Stephen Bourne).
Bash is the default shell on most modern implementations of Unix
and in most packages that provide Unix-like tools for Windows.

The terminal is an application that we use to run the shell, it provides the
window to run the shell command in. The terminal can also be used to run
other command-line interface programs.

### What does it look like?

A typical shell command and output looks something like this:

~~~
nbuser@nbserver:~$
nbuser@nbserver:~$ ls -F /
bin/   dev/  home/  lib64/  mnt/  proc/  run/   srv/  tmp/  var/
boot/  etc/  lib/   media/  opt/  root/  sbin/  sys/  usr/
nbuser@nbserver:~$
~~~

The first line shows only a **prompt**, indicating that the shell is waiting
for input. Your shell may use different text for the prompt. Most importantly:
when typing commands, either from these lessons or from other sources,
*do not type the prompt*, only the commands that follow it.

The part that you type (in this example `ls -F /`)
typically has the following structure: a **command**,
some **flags** (also called **options** or **switches**) and an **argument**.
Flags start with a dash (`-`), and change the behaviour of a command.
Arguments tell the command what to operate on (e.g. files and directories).
Sometimes flags and arguments are referred to as parameters.
A command can be called with more than one flag and more than one argument: but a
command doesn't always require an argument or a flag.

In the example above, our **command** is `ls`, with a **flag** `-F` and an
**argument** `/`. Each part is separated by spaces: if you omit the space
between `ls` and `-F` the shell will look for a command called `ls-F`, which
doesn't exist. Also, capitalization matters: `LS` is different to `ls`.

Next we see the output that our command produced. In this case it is a listing
of files and folders in a location called `/` - we'll cover what all these mean
later today.

Finally, the shell again prints the prompt and waits for you to type the next
command.

In the examples for this lesson, we'll show the prompt as `$ `. You can make your
prompt look the same by entering the command `PS1='$ '`. But you can also leave
your prompt as it is - often the prompt includes useful information about who and where
you are.

Open a shell window and try entering `ls -F /` for yourself (don't forget that spaces
and capitalization are important!). You can change the prompt too, if you like.

### How does the shell know what `ls` and its flags mean?

Every command is a program stored somewhere on the computer, and the shell keeps a
list of places to search for commands (the list is in a **variable** called `$PATH`,
but those are concepts we'll meet later and not too important at the moment). Recall
that commands, flags and arguments are separated by spaces.

So let's look at the REPL (read-evaluate-print loop) in more detail. Notice that the
"evaluate" step is made of two parts:

1. Read what was typed (`ls -F /` in our example)  
    The shell uses the spaces to split the line into the command, flags, and arguments
2. Evaluate:  
    a. Find a program called `ls`  
    b. Execute it, passing it the flags and arguments (`-F` and `/`) to
       interpret as the program sees fit
3. Print the output produced by the program

and then print the prompt and wait for you to enter another command.

> ## Command not found
> If the shell can't find a program whose name is the command you typed, it
> will print an erorr message like:
>
> ~~~
> $ ls-F
> -bash: ls-F: command not found
> ~~~
>
> Usually this means that you have mis-typed the command - in this case we omitted
> the space between `ls` and `-F`.

### Is it difficult?

It isn't difficult, but it is a different model of interacting than a GUI, and that
will take some effort - and some time - to learn. A GUI
presents you with choices and you select one. With a CLI the choices are combinations
of commands and parameters, more like words in a language than buttons on a screen. They
are not presented to you so
you must learn a few, like learning some vocabulary in a new language. But a small
number of commands gets you a long way, and we'll cover those essential few today.

### Flexibility and automation

The grammar of a shell allows you to combine existing tools into powerful
pipelines and handle large volumes of data automatically. Sequences of
commands can be written into a *script*, improving the reproducibility of
workflows and allowing you to repeat them easily.

In addition, the command line is often the easiest way to interact with remote machines and supercomputers.
Familiarity with the shell is near essential to run a variety of specialized tools and resources
including high-performance computing systems.
As clusters and cloud computing systems become more popular for scientific data crunching,
being able to interact with the shell is becoming a necessary skill.
We can build on the command-line skills covered here
to tackle a wide range of scientific questions and computational challenges.
