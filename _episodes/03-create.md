---
title: Creating a Repository
teaching: 10
exercises: 0
questions:
- "Where does Git store information?"
objectives:
- "Create a local Git repository."
- "Describe the purpose of the `.git` directory."
keypoints:
- "`git init` initializes a repository."
- "Git stores all of its repository data in the `.git` directory."
---

Once Git is configured,
we can start using it.

We will continue with our analysis of the Gapminder dataset, where we have produced some interesting results.

First, let's move into our `gapminder-analysis` directory:

~~~
$ cd ~/Desktop/gapminder-analysis
~~~
{: .language-bash}

Then we tell Git to make `gapminder-analysis` a [repository]({{ page.root }}{% link reference.md %}#repository)—a place where Git can store versions of our files:

~~~
$ git init
~~~
{: .language-bash}

It is important to note that `git init` will create a repository that
includes subdirectories, like `functions`, and their files---there is no need to create
separate repositories nested within the `gapminder-analysis` repository, whether
subdirectories are present from the beginning or added later. Also, note
that the creation of the `gapminder-analysis` directory and its initialization as a
repository are completely separate processes.

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~
{: .language-bash}

~~~
data  functions  gapminder-analysis.rmd  results
~~~
{: .output}

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `gapminder-analysis` called `.git`:

~~~
$ ls -a
~~~
{: .language-bash}

~~~
.  ..  data  functions  gapminder-analysis.rmd  .git  results
~~~
{: .output}

Git uses this special subdirectory to store all the information about the project, 
including all files and sub-directories located within the project's directory.
If we ever delete the `.git` subdirectory,
we will lose the project's history.

We can check that everything is set up correctly
by asking Git to tell us the status of our project:

~~~
$ git status
~~~
{: .language-bash}
~~~
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	data/
	functions/
	gapminder-analysis.rmd
	results/

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

If you are using a different version of `git`, the exact wording of the output might be slightly different. 

Git recognizes that there are files and folders within the `gapminder-analysis` which it could track, but it will not watch these files until we explicitely tell it too.

> ## Places to Create Git Repositories
>
> Along with tracking information about the gapminder dataset (the project we have already created), 
> Your labmate would also like to track census data from around the world.
> Despite your concerns, your labmate creates a `census` project inside his `gapminder-analysis` 
> project with the following sequence of commands:
>
> ~~~
> $ cd ~/Desktop              # return to Desktop directory
> $ cd gapminder-analysis     # go into planets directory, which is already a Git repository
> $ ls -a                     # ensure the .git subdirectory is still present in the planets directory
> $ mkdir census              # make a subdirectory planets/moons
> $ cd census                 # go into moons subdirectory
> $ git init                  # make the moons subdirectory a Git repository
> $ ls -a                     # ensure the .git subdirectory is present indicating we have created a new Git repository
> ~~~
> {: .language-bash}
>
> Is the `git init` command, run inside the `census` subdirectory, required for 
> tracking files stored in the `census` subdirectory?
> 
> > ## Solution
> >
> > No. Your labmate does not need to make the `census` subdirectory a Git repository 
> > because the `gapminder-analysis` repository will track all files, sub-directories, and 
> > subdirectory files under the `gapminder-analysis` directory.  Thus, in order to track 
> > all information about moons, your labmate only needed to add the `census` subdirectory
> > to the `gapminder-analysis` directory.
> > 
> > Additionally, Git repositories can interfere with each other if they are "nested":
> > the outer repository will try to version-control
> > the inner repository. Therefore, it's best to create each new Git
> > repository in a separate directory. To be sure that there is no conflicting
> > repository in the directory, check the output of `git status`. If it looks
> > like the following, you are good to go to create a new repository as shown
> > above:
> >
> > ~~~
> > $ git status
> > ~~~
> > {: .language-bash}
> > ~~~
> > fatal: Not a git repository (or any of the parent directories): .git
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}
> ## Correcting `git init` Mistakes
> You explain to your labmate how a nested repository is redundant and may cause confusion
> down the road. Your labmate would like to remove the nested repository. How can they undo 
> their last `git init` in the `census` subdirectory?
>
> > ## Solution -- USE WITH CAUTION!
> >
> > ### Background
> > Removing files from a git repository needs to be done with caution. To remove files from the working tree and not from your working directory, use
> > ~~~
> > $ rm filename
> > ~~~
> > {: .language-bash}
> > 
> > The file being removed has to be in sync with the branch head with no updates. If there are updates, the file can be removed by force by using the `-f` option. Similarly a directory can be removed from git using `rm -r dirname` or `rm -rf dirname`.
> >
> > ### Solution
> > Git keeps all of its files in the `.git` directory.
> > To recover from this little mistake, your labmate can just remove the `.git`
> > folder in the moons subdirectory by running the following command from inside the `planets` directory:
> >
> > ~~~
> > $ rm -rf census/.git
> > ~~~
> > {: .language-bash}
> >
> > But be careful! Running this command in the wrong directory, will remove
> > the entire Git history of a project you might want to keep. Therefore, always check your current directory using the
> > command `pwd`.
> {: .solution}
{: .challenge}
