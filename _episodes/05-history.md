---
title: Exploring History
teaching: 25
exercises: 0
questions:
- "How can I identify old versions of files?"
- "How do I review my changes?"
- "How can I recover old versions of files?"
objectives:
- "Explain what the HEAD of a repository is and how to use it."
- "Identify and use Git commit numbers."
- "Compare various versions of tracked files."
- "Restore old versions of files."
keypoints:
- "`git diff` displays differences between commits."
- "`git checkout` recovers old versions of files."
---

As we saw in the previous lesson, we can refer to commits by their
identifiers.  You can refer to the _most recent commit_ of the working
directory by using the identifier `HEAD`.

We've been adding one line at a time to `README.md`, so it's easy to track our
progress by looking, so let's do that using our `HEAD`s.  Before we start,
let's make a change to `README.md`, and make our description slightly more specific.

~~~
$ nano README.md
$ cat README.md
~~~
{: .language-bash}

~~~
# Gapminder Analysis

**Author:** Anthony Valente
**Depends:** ggplot2, dplyr
**Start Date:** 2020-01-28

This repository contains analyses of life expectancy vs GDP. 
I also explore the change in life expectancy with year.
~~~
{: .output}

Now, let's see what we get.

~~~
$ git diff HEAD mars.txt
~~~
{: .language-bash}

~~~
diff --git a/README.md b/README.md
index 78fb607..7fa6160 100644
--- a/README.md
+++ b/README.md
@@ -5,3 +5,4 @@
 **Start Date:** 2020-01-28
 
 This repository contains analyses of life expectancy vs GDP.
+I also explore the change in life expectancy with year.
~~~
{: .output}

which is the same as what you would get if you leave out `HEAD` (try it).  The
real goodness in all this is when you can refer to previous commits.  We do
that by adding `~1` 
(where "~" is "tilde", pronounced [**til**-d*uh*]) 
to refer to the commit one before `HEAD`.

~~~
$ git diff HEAD~1 README.md
~~~
{: .language-bash}

If we want to see the differences between older commits we can use `git diff`
again, but with the notation `HEAD~1`, `HEAD~2`, and so on, to refer to them:


~~~
$ git diff HEAD~2 README.md
~~~
{: .language-bash}

~~~
diff --git a/README.md b/README.md
index 58b6dcf..7fa6160 100644
--- a/README.md
+++ b/README.md
@@ -2,5 +2,7 @@
 
 **Author:** Anthony Valente
 **Depends:** ggplot2, dplyr
+**Start Date:** 2020-01-28
 
 This repository contains analyses of life expectancy vs GDP.
+I also explore the change in life expectancy with year.
~~~
{: .output}

We could also use `git show` which shows us what changes we made at an older commit as 
well as the commit message, rather than the _differences_ between a commit and our 
working directory that we see by using `git diff`.

~~~
$ git show HEAD~2 README.md
~~~
{: .language-bash}

~~~
commit 6375275d8b1b48f2b4bf540c747aa237fa4c80f1
Author: Anthony Valente <valenta4@uw.edu>
Date:   Mon Jan 13 20:35:50 2020 -0800

    Add dependency statement and authorship

diff --git a/README.md b/README.md
index fc323d1..58b6dcf 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,6 @@
 # Gapminder Analysis
 
+**Author:** Anthony Valente
+**Depends:** ggplot2, dplyr
+
 This repository contains analyses of life expectancy vs GDP.
~~~
{: .output}

In this way,
we can build up a chain of commits.
The most recent end of the chain is referred to as `HEAD`;
we can refer to previous commits using the `~` notation,
so `HEAD~1`
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
`2e8b51d3bf9679bc23341f1d8d8fd003b1a8d176`,
so let's try this:

~~~
$ git diff 2e8b51d3bf9679bc23341f1d8d8fd003b1a8d176 README.md
~~~
{: .language-bash}

~~~
diff --git a/README.md b/README.md
index fc323d1..7fa6160 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,8 @@
 # Gapminder Analysis
 
+**Author:** Anthony Valente
+**Depends:** ggplot2, dplyr
+**Start Date:** 2020-01-28
+
 This repository contains analyses of life expectancy vs GDP.
+I also explore the change in life expectancy with year.
~~~
{: .output}

That's the right answer,
but typing out random 40-character strings is annoying,
so Git lets us use just the first few characters (typically seven for normal size projects):

~~~
$ git diff 2e8b51d README.md
~~~
{: .language-bash}

~~~
diff --git a/README.md b/README.md
index fc323d1..7fa6160 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,8 @@
 # Gapminder Analysis
 
+**Author:** Anthony Valente
+**Depends:** ggplot2, dplyr
+**Start Date:** 2020-01-28
+
 This repository contains analyses of life expectancy vs GDP.
+I also explore the change in life expectancy with year.
~~~
{: .output}

All right! So
we can save changes to files and see what we've changed—now how
can we restore older versions of things?
Let's suppose we change our mind about the last update to
`README.md` (our update to the description).

`git status` now tells us that the file has been changed,
but those changes haven't been staged:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

We can put things back the way they were
by using `git checkout`:

~~~
$ git checkout HEAD README.md
$ cat README.md
~~~
{: .language-bash}

~~~
# Gapminder Analysis

**Author:** Anthony Valente
**Depends:** ggplot2, dplyr
**Start Date:** 2020-01-28

This repository contains analyses of life expectancy vs GDP.
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
$ git checkout 2e8b51d README.md
~~~
{: .language-bash}

~~~
$ cat README.md
~~~
{: .language-bash}

~~~
# Gapminder Analysis

This repository contains analyses of life expectancy vs GDP.
~~~
{: .output}

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md

~~~
{: .output}

Notice that the changes are currently in the staging area.
Again, we can put things back the way they were
by using `git checkout`:

~~~
$ git checkout HEAD README.md
~~~
{: .language-bash}

> ## Don't Lose Your HEAD
>
> Above we used
>
> ~~~
> $ git checkout 2e8b51d README.md
> ~~~
> {: .language-bash}
>
> to revert `mars.txt` to its state after the commit `2e8b51d`. But be careful! 
> The command `checkout` has other important functionalities and Git will misunderstand
> your intentions if you are not accurate with the typing. For example, 
> if you forget `mars.txt` in the previous command.
>
> ~~~
> $ git checkout 2e8b51d
> ~~~
> {: .language-bash}
> ~~~
> Note: checking out '2e8b51d'.
>
> You are in 'detached HEAD' state. You can look around, make experimental
> changes and commit them, and you can discard any commits you make in this
> state without impacting any branches by performing another checkout.
>
> If you want to create a new branch to retain commits you create, you may
> do so (now or later) by using -b with the checkout command again. Example:
>
>  git checkout -b <new-branch-name>
>
> HEAD is now at 2e8b51d Start notebook on life expectancy vs gdp
> ~~~
> {: .error}
>
> The "detached HEAD" is like "look, but don't touch" here,
> so you shouldn't make any changes in this state.
> After investigating your repo's past state, reattach your `HEAD` with `git checkout master`.
{: .callout}

It's important to remember that
we must use the commit number that identifies the state of the repository
*before* the change we're trying to undo.
A common mistake is to use the number of
the commit in which we made the change we're trying to discard.
In the example below, we want to retrieve the state from before the most
recent commit (`HEAD~1`), which is commit `2e8b51d`:

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
> {: .language-bash}
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
>
>
> > ## Solution
> >
> > The answer is (5)-Both 2 and 4. 
> > 
> > The `checkout` command restores files from the repository, overwriting the files in your working 
> > directory. Answers 2 and 4 both restore the *latest* version *in the repository* of the file 
> > `data_cruncher.py`. Answer 2 uses `HEAD` to indicate the *latest*, whereas answer 4 uses the 
> > unique ID of the last commit, which is what `HEAD` means. 
> > 
> > Answer 3 gets the version of `data_cruncher.py` from the commit *before* `HEAD`, which is NOT 
> > what we wanted.
> > 
> > Answer 1 can be dangerous! Without a filename, `git checkout` will restore **all files** 
> > in the current directory (and all directories below it) to their state at the commit specified. 
> > This command will restore `data_cruncher.py` to the latest commit version, but it will also 
> > restore *any other files that are changed* to that version, erasing any changes you may 
> > have made to those files!
> > As discussed above, you are left in a *detached* `HEAD` state, and you don't want to be there.
> {: .solution}
{: .challenge}

> ## Reverting a Commit
>
> Jennifer is collaborating on her Python script with her colleagues and
> realizes her last commit to the project's repository contained an error and
> she wants to undo it.  `git revert [erroneous commit ID]` will create a new 
> commit that reverses Jennifer's erroneous commit. Therefore `git revert` is
> different to `git checkout [commit ID]` because `git checkout` returns the
> files within the local repository to a previous state, whereas `git revert`
> reverses changes committed to the local and project repositories.  
> Below are the right steps and explanations for Jennifer to use `git revert`,
> what is the missing command?
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
> What is the output of the last command in
>
> ~~~
> $ cd planets
> $ echo "Venus is beautiful and full of love" > venus.txt
> $ git add venus.txt
> $ echo "Venus is too hot to be suitable as a base" >> venus.txt
> $ git commit -m "Comment on Venus as an unsuitable base"
> $ git checkout HEAD venus.txt
> $ cat venus.txt #this will print the contents of venus.txt to the screen
> ~~~
> {: .language-bash}
>
> 1. ~~~
>    Venus is too hot to be suitable as a base
>    ~~~
>    {: .output}
> 2. ~~~
>    Venus is beautiful and full of love
>    ~~~
>    {: .output}
> 3. ~~~
>    Venus is beautiful and full of love
>    Venus is too hot to be suitable as a base
>    ~~~
>    {: .output}
> 4. ~~~
>    Error because you have changed venus.txt without committing the changes
>    ~~~
>    {: .output}
>
> > ## Solution
> >
> > The answer is 2. 
> > 
> > The command `git add venus.txt` places the current version of `venus.txt` into the staging area. 
> > The changes to the file from the second `echo` command are only applied to the working copy, 
> > not the version in the staging area.
> > 
> > So, when `git commit -m "Comment on Venus as an unsuitable base"` is executed, 
> > the version of `venus.txt` committed to the repository is the one from the staging area and
> > has only one line.
> >  
> >  At this time, the working copy still has the second line (and 
> >  `git status` will show that the file is modified). However, `git checkout HEAD venus.txt` 
> >  replaces the working copy with the most recently committed version of `venus.txt`.
> >  
> >  So, `cat venus.txt` will output 
> >  ~~~
> >  Venus is beautiful and full of love.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Getting Rid of Staged Changes
>
> `git checkout` can be used to restore a previous commit when unstaged changes have
> been made, but will it also work for changes that have been staged but not committed?
> Make a change to `README.md`, add that change, and use `git checkout` to see if
> you can remove your change.
{: .challenge}
