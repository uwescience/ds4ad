Once Git is configured,
we can start using it.

We will continue with the story of Wolfman and Dracula who are developing a
delicious guacamole recipe.

First, let's create a directory for our work and then move into that directory:

~~~
$ mkdir guacamole
$ cd guacamole
~~~


Then we tell Git to make `guacamole` a [repository]({{ page.root }}/reference/#repository)—a place where
Git can store versions of our files:

~~~
$ git init
~~~


If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~


But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `guacamole` called `.git`:

~~~
$ ls -a
~~~


~~~
.	..	.git
~~~


Git uses this special sub-directory to store all the information about the project,
including all files and sub-directories located within the project's directory.
If we ever delete the `.git` sub-directory,
we will lose the project's history.

We can check that everything is set up correctly
by asking Git to tell us the status of our project:

~~~
$ git status
~~~


If you are using a different version of git than I am, then then the exact
wording of the output might be slightly different.

~~~
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
~~~


> ## Places to Create Git Repositories
>
> Along with tracking information about guacamole (the project we have already created),
> Dracula would also like to track information about salsa.
> Despite Wolfman's concerns, Dracula creates a `salsa` project inside his `guacamole`
> project with the following sequence of commands:
>
> ~~~
> $ cd             # return to home directory
> $ cd guacamole     # go into guacamole directory, which is already a Git repository
> $ ls -a          # ensure the .git sub-directory is still present in the guacamole directory
> $ mkdir salsa    # make a sub-directory guacamole/salsa
> $ cd salsa       # go into salsa sub-directory
> $ git init       # make the salsa sub-directory a Git repository
> $ ls -a          # ensure the .git sub-directory is present indicating we have created a new Git repository
> ~~~
>
>
> Is the `git init` command, run inside the `salsa` sub-directory, required for
> tracking files stored in the `salsa` sub-directory?
>
> > ## Solution
> >
> > No. Dracula does not need to make the `salsa` sub-directory a Git repository
> > because the `guacamole` repository will track all files, sub-directories, and
> > sub-directory files under the `guacamole` directory.  Thus, in order to track
> > all information about salsa, Dracula only needed to add the `salsa` sub-directory
> > to the `guacamole` directory.
> >
> > Additionally, Git repositories can interfere with each other if they are "nested" in the
> > directory of another: the outer repository will try to version-control
> > the inner repository. Therefore, it's best to create each new Git
> > repository in a separate directory. To be sure that there is no conflicting
> > repository in the directory, check the output of `git status`. If it looks
> > like the following, you are good to go to create a new repository as shown
> > above:
> >
> > ~~~
> > $ git status
> > ~~~
> >
> > ~~~
> > fatal: Not a git repository (or any of the parent directories): .git
> > ~~~
> >
>

> ## Correcting `git init` Mistakes
> Wolfman explains to Dracula how a nested repository is redundant and may cause confusion
> down the road. Dracula would like to remove the nested repository. How can Dracula undo
> his last `git init` in the `salsa` sub-directory?
>
> > ## Solution -- USE WITH CAUTION!
> >
> > To recover from this little mistake, Dracula can just remove the `.git`
> > folder in the salsa subdirectory by running the following command from inside the 'guacamole' directory:
> >
> > ~~~
> > $ rm -rf salsa/.git
> > ~~~
> >
> >
> > But be careful! Running this command in the wrong directory, will remove
> > the entire git-history of a project you might want to keep. Therefore, always check your current directory using the
> > command `pwd`.
>
