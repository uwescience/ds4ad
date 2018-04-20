As we saw in the previous lesson, we can refer to commits by their
identifiers.  You can refer to the _most recent commit_ of the working
directory by using the identifier `HEAD`.

We've been adding one line at a time to `ingredients.txt`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `ingredients.txt`.

~~~
$ nano ingredients.txt
$ cat ingredients.txt
~~~
{: .bash}

~~~
avocados
tomatoes
cilantro
bananas
~~~
{: .output}

Now, let's see what we get.

~~~
$ git diff HEAD ingredients.txt
~~~
{: .bash}

~~~
diff --git a/ingredients.txt b/ingredients.txt
index b36abfd..0848c8d 100644
--- a/ingredients.txt
+++ b/ingredients.txt
@@ -1,3 +1,4 @@
 avocados
 tomatoes
 cilantro
+bananas.
~~~
{: .output}

which is the same as what you would get if you leave out `HEAD` (try it).  The
real goodness in all this is when you can refer to previous commits.  We do
that by adding `~1` to refer to the commit one before `HEAD`.

~~~
$ git diff HEAD~1 ingredients.txt
~~~
{: .bash}

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:


~~~
$ git diff HEAD~2 ingredients.txt
~~~
{: .bash}

~~~
diff --git a/ingredients.txt b/ingredients.txt
index df0654a..b36abfd 100644
--- a/ingredients.txt
+++ b/ingredients.txt
@@ -1 +1,4 @@
 avocados
+tomatoes
+cilantro
+bananas
~~~
{: .output}

We could also use `git show` which shows us what changes we made at an older commit as well as the commit message, rather than the _differences_ between a commit and our working directory that we see by using `git diff`.

~~~
$ git show HEAD~2 ingredients.txt
~~~
{: .bash}

~~~
commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Start notes on guacamole ingredients

diff --git a/ingredients.txt b/ingredients.txt
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/ingredients.txt
@@ -0,0 +1 @@
+avocados
~~~
{: .output}

In this way,
we can build up a chain of commits.
The most recent end of the chain is referred to as `HEAD`;
we can refer to previous commits using the `~` notation,
so `HEAD~1` (pronounced "head minus one")
means "the previous commit",
while `HEAD~123` goes back 123 commits from where we are now.

We can also refer to commits using
those long strings of digits and letters
that `git log` displays.
These are unique IDs for the changes,
and "unique" really does mean unique:
every change to any set of files on any computer
has a unique 40-character identifier.
Our first commit was given the ID
`f22b25e3233b4645dabd0d81e651fe074bd8e73b`,
so let's try this:

~~~
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b ingredients.txt
~~~
{: .bash}

~~~
diff --git a/ingredients.txt b/ingredients.txt
index df0654a..93a3e13 100644
--- a/ingredients.txt
+++ b/ingredients.txt
@@ -1 +1,4 @@
 avocados
+tomatoes
+cilantro
+bananas
~~~
{: .output}

That's the right answer,
but typing out random 40-character strings is annoying,
so Git lets us use just the first few characters:

~~~
$ git diff f22b25e ingredients.txt
~~~
{: .bash}

~~~
diff --git a/ingredients.txt b/ingredients.txt
index df0654a..93a3e13 100644
--- a/ingredients.txt
+++ b/ingredients.txt
@@ -1 +1,4 @@
 avocados
+tomatoes
+cilantro
+bananas
~~~
{: .output}

All right! So
we can save changes to files and see what we've changed—now how
can we restore older versions of things?
Let's suppose we accidentally overwrite our file:

~~~
$ nano ingredients.txt
$ cat ingredients.txt
~~~
{: .bash}

~~~
lime
~~~
{: .output}

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   ingredients.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

We can put things back the way they were
by using `git checkout`:

~~~
$ git checkout HEAD ingredients.txt
$ cat ingredients.txt
~~~
{: .bash}

~~~
avocados
tomatoes
cilantro
~~~
{: .output}

As you might guess from its name,
`git checkout` checks out (i.e., restores) an old version of a file.
In this case,
we're telling Git that we want to recover the version of the file recorded in `HEAD`,
which is the last saved commit.
If we want to go back even further,
we can use a commit identifier instead:

~~~
$ git checkout f22b25e ingredients.txt
~~~
{: .bash}

~~~
$ cat ingredients.txt
~~~
{: .bash}

~~~
avocados
~~~
{: .output}

~~~
$ git status
~~~
{: .bash}

~~~
# On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   ingredients.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Notice that the changes are on the staged area.
Again, we can put things back the way they were
by using `git checkout`:

~~~
$ git checkout HEAD ingredients.txt
~~~
{: .bash}

> ## Don't Lose Your HEAD
>
> Above we used
>
> ~~~
> $ git checkout f22b25e ingredients.txt
> ~~~
> {: .bash}
>
> to revert `ingredients.txt` to its state after the commit `f22b25e`. But be careful!
> The command `checkout` has other important functionalities and Git will misunderstand
> your intentions if you are not accurate with the typing. For example,
> if you forget `ingredients.txt` in that command, Git will tell you that "You are in
> 'detached HEAD' state." It's "look, but don't touch" here, so you shouldn't
> make any changes in this state.
> After investigating your repo's past state, reattach your HEAD with ``git checkout master``
{: .callout}

It's important to remember that
we must use the commit number that identifies the state of the repository
*before* the change we're trying to undo.
A common mistake is to use the number of
the commit in which we made the change we're trying to get rid of.
In the example below, we want to retrieve the state from before the most
recent commit (`HEAD~1`), which is commit `f22b25e`:

![Git Checkout](../fig/git-checkout.svg)

So, to put it all together,
here's how Git works in cartoon form:

![https://figshare.com/articles/How_Git_works_a_cartoon/1328266](../fig/git_staging.svg)

> ## Simplifying the Common Case
>
> If you read the output of `git status` carefully,
> you'll see that it includes this hint:
>
> ~~~
> (use "git checkout -- <file>..." to discard changes in working directory)
> ~~~
> {: .bash}
>
> As it says,
> `git checkout` without a version identifier restores files to the state saved in `HEAD`.
> The double dash `--` is needed to separate the names of the files being recovered
> from the command itself:
> without it,
> Git would try to use the name of the file as the commit identifier.
{: .callout}

The fact that files can be reverted one by one
tends to change the way people organize their work.
If everything is in one large document,
it's hard (but not impossible) to undo changes to the introduction
without also undoing changes made later to the conclusion.
If the introduction and conclusion are stored in separate files,
on the other hand,
moving backward and forward in time becomes much easier.

> ## Recovering Older Versions of a File
>
> Jennifer has made changes to the Python script that she has been working on for weeks, and the
> modifications she made this morning "broke" the script and it no longer runs. She has spent
> ~ 1hr trying to fix it, with no luck...
>
> Luckily, she has been keeping track of her project's versions using Git! Which commands below will
> let her recover the last committed version of her Python script called
> `data_cruncher.py`?
>
> 1. `$ git checkout HEAD`
>
> 2. `$ git checkout HEAD data_cruncher.py`
>
> 3. `$ git checkout HEAD~1 data_cruncher.py`
>
> 4. `$ git checkout <unique ID of last commit> data_cruncher.py`
>
> 5. Both 2 and 4
{: .challenge}

> ## Reverting a Commit
>
> Jennifer is collaborating on her Python script with her colleagues and
> realizes her last commit to the group repository is wrong and wants to
> undo it.  Jennifer needs to undo correctly so everyone in the group
> repository gets the correct change.  `git revert [wrong commit ID]`
> will make a new commit that undoes Jennifer's previous wrong
> commit. Therefore `git revert` is different than `git checkout [commit
> ID]` because `checkout` is for local changes not committed to the
> group repository.  Below are the right steps and explanations for
> Jennifer to use `git revert`, what is the missing command?
>
> 1. `________ # Look at the git history of the project to find the commit ID`
>
> 2. Copy the ID (the first few characters of the ID, e.g. 0b1d055).
>
> 3. `git revert [commit ID]`
>
> 4. Type in the new commit message.
>
> 5. Save and close
{: .challenge}

> ## Understanding Workflow and History
>
> What is the output of cat recipe.txt at the end of this set of commands?
>
> ~~~
> $ cd guacamole
> $ nano recipe.txt #input the following text: Cut open the avocados and remove the pits.
> $ git add recipe.txt
> $ nano recipe.txt #add the following text: Remove the avocado flesh from the skin with a spoon.
> $ git commit -m "Describe avocado preparation"
> $ git checkout HEAD recipe.txt
> $ cat recipe.txt #this will print the contents of recipe.txt to the screen
> ~~~
> {: .bash}
>
> 1.
>
> ~~~
> Remove the avocado flesh from the skin with a spoon.
> ~~~
> {: .output}
>
> 2.
>
> ~~~
> Cut open the avocados and remove the pits.
> ~~~
> {: .output}
>
> 3.
>
> ~~~
> Cut open the avocados and remove the pits.
> Remove the avocado flesh from the skin with a spoon.
> ~~~
> {: .output}
>
> 4.
>
> ~~~
> Error because you have changed recipe.txt without committing the changes
> ~~~
> {: .output}
>
> > ## Solution
> >
> > Line by line:
> > ~~~
> > $ cd guacamole
> > ~~~
> > {: .bash}
> > Enters into the 'guacamole' directory
> >
> > ~~~
> > $ nano recipe.txt #input the following text: Cut open the avocados and remove the pits.
> > ~~~
> > {: .bash}
> > We created a new file and wrote a sentence in it, but the file is not tracked by git.  
> >
> > ~~~
> > $ git add recipe.txt
> > ~~~
> > {: .bash}
> > Now the file is staged. The changes that have been made to the file until now will be committed in the next commit.
> >
> > ~~~
> > $ nano recipe.txt #add the following text: Remove the avocado flesh from the skin with a spoon.
> > ~~~
> > {: .bash}
> > The file has been modified. The new changes are not staged because we have not added the file.
> >
> > ~~~
> > $ git commit -m "Describe avocado preparation"
> > ~~~
> > {: .bash}
> > The changes that were staged (Cut open the avocados and remove the pits.) have been committed. The changes that were not staged (Remove the avocado flesh from the skin with a spoon.) have not. Our local working copy is different than the copy in our local repository.
> >
> > ~~~
> > $ git checkout HEAD recipe.txt
> > ~~~
> > {: .bash}
> > With checkout we discard the changes in the working directory so that our local copy is exactly the same as our HEAD, the most recent commit.
> >
> > ~~~
> > $ cat recipe.txt #this will print the contents of recipe.txt to the screen
> > ~~~
> > {: .bash}
> > If we print recipe.txt we will get answer 2.
> >
> {: .solution}
{: .challenge}

> ## Checking Understanding of `git diff`
>
> Consider this command: `git diff HEAD~3 ingredients.txt`. What do you predict this command
> will do if you execute it? What happens when you do execute it? Why?
>
> Try another command, `git diff [ID] ingredients.txt`, where [ID] is replaced with
> the unique identifier for your most recent commit. What do you think will happen,
> and what does happen?
{: .challenge}

> ## Getting Rid of Staged Changes
>
> `git checkout` can be used to restore a previous commit when unstaged changes have
> been made, but will it also work for changes that have been staged but not committed?
> Make a change to `ingredients.txt`, add that change, and use `git checkout` to see if
> you can remove your change.
{: .challenge}

> ## Explore and Summarize Histories
>
> Exploring history is an important part of git, often it is a challenge to find
> the right commit ID, especially if the commit is from several months ago.
>
> Imagine the `guacamole` project has more than 50 files.
> You would like to find a commit with specific text in `ingredients.txt` is modified.
> When you type `git log`, a very long list appeared,
> How can you narrow down the search?
>
> Recall that the `git diff` command allow us to explore one specific file,
> e.g. `git diff ingredients.txt`. We can apply a similar idea here.
>
> ~~~
> $ git log ingredients.txt
> ~~~
> {: .bash}
>
> Unfortunately some of these commit messages are very ambiguous e.g. `update files`.
> How can you search through these files?
>
> Both `git diff` and `git log` are very useful and they summarize a different part of the history for you.
> Is it possible to combine both? Let's try the following:
>
> ~~~
> $ git log --patch ingredients.txt
> ~~~
> {: .bash}
>
> You should get a long list of output, and you should be able to see both commit messages and the difference between each commit.
>
> Question: What does the following command do?
>
> ~~~
> $ git log --patch HEAD~3 *.txt
> ~~~
> {: .bash}
{: .challenge}
