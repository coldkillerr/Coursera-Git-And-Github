<h1> Using Git </h1>



We said that a `VCS` tracks
who made which changes,
for this to work, we need to tell `Git` who we are.
We can do this by using
the `git config` command and then setting the values of
`user.email` and `user.name` to
our email and our name like this. 

```sh
~$ git config --global user.email "myemail@example.com"
~$ git config --global user.name "my_name"
```
We use the `--global` flag to state that we want to
set this value for all git repositories that we'd use.

<h2> Working with a git repository </h2>

There are
two ways to start working with a git repository.

We can create one from scratch using
the `git init` command

```sh
~$ git init
```

or we can
use the `git clone` command to make a copy of
a repository that already exists somewhere else.

```sh
~$ git clone repository_name
```

So when we run `git init` we
initialize an empty git repository
in the current directory.

```sh
~/checks$ git init
Initialized empty Git repository in /home/nishit/checks/.git/
```

The message that we get mentions a directory called.
git.

We can check that this directory exist using
the `ls-la` command which
lists files that start with a `.`

```sh
~/checks$ ls -la
total 12
drwxrwxr-x  3 nishit nishit 4096 Aug  9 14:58 .
drwxr-xr-x 33 nishit nishit 4096 Aug  9 14:50 ..
drwxrwxr-x  7 nishit nishit 4096 Aug  9 14:51 .git
```

We can also use the ls-l.git command to
look inside of it and
see the many different things it contains.
This is called a Git directory.


```sh
~/checks$ ls -l .git
total 32
drwxrwxr-x 2 nishit nishit 4096 Aug  9 14:51 branches
-rw-rw-r-- 1 nishit nishit   92 Aug  9 14:51 config
-rw-rw-r-- 1 nishit nishit   73 Aug  9 14:51 description
-rw-rw-r-- 1 nishit nishit   23 Aug  9 14:51 HEAD
drwxrwxr-x 2 nishit nishit 4096 Aug  9 14:51 hooks
drwxrwxr-x 2 nishit nishit 4096 Aug  9 14:51 info
drwxrwxr-x 4 nishit nishit 4096 Aug  9 14:51 objects
drwxrwxr-x 4 nishit nishit 4096 Aug  9 14:51 refs
```

You can think of it as a `database` for your `Git project`
that stores the changes and the change history.
We can see it contains
a bunch of different files and directories.
We won't touch any of these files directly,
we'll always interact with them through Git commands.

The area outside the git directory is the working tree.
The `working tree` is the current version of your project.
This working tree will contain
all the files that are currently tracked by Git
and any new files that we
haven't yet added to the list of track files.

To make Git track our file,
we'll add it to the project using the `git add`
command passing the file that we want as a parameter. 

```sh
~/checks$ git add cpu_usage.py 
```
The `staging area` which is also known
as the `index` is a file maintained by
Git that contains all of the information about
what files and changes
are going to go into your next command. 

We can use the `git status` command
to get some information about
the current working tree and
pending changes.

```sh
~/checks$ git status 
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   cpu_usage.py

```
We see that our new file is marked to be committed,
this means that our change is
currently in the staging area.
To get it committed into the `.git` directory,
we run the `git commit` command. 

```sh
~/checks$ git commit 
[master (root-commit) b9c9fbe] Added cpu_usage.py
 1 file changed, 14 insertions(+)
 create mode 100644 cpu_usage.py
```

When we run this command,
we tell `Git` that we want to save our changes.
It opens a text editor where
we can enter a `commit message`.
If you want, you can change
the editor used to your preferred editor. 

Each time you make a commit,
Git records a new snapshot of the state of your project at that moment. 

When we operate with Git, our files can be either tracked or untracked.
Tracked files are part of the snapshots,
while untracked files aren't a part of snapshots yet.
This is the usual case for new files.
Each track file can be in one of three main states,
`modified`,
`staged` or `committed`.

If a file is in the `modified` state,
it means that we've made changes to it that we haven't `committed` yet.
The changes could be adding, modifying or deleting the contents of the file.
Git notices anytime we modify our files.
But won't store any changes until we add them to the staging area.

So, the next step is to `stage` those changes.
When we do this, our modified files become `stage files`.
In other words, the changes to those files are ready to be `committed` to the project.
All files that are staged will be part of the next snapshot we take.
And finally, when a file gets committed,
the changes made to it are safely stored in a snapshot in the Git directory.
This means that typically a file tracked by Git,
will first be `modified` when we change it in any way.
Then it becomes `staged` when we mark those changes for tracking.
And finally it will get `committed` when we store those changes in the VCS


And then the current status of our files using the Git status command.
When we run Git status,
Git tells us a bunch of things, including that we're on the master branch. 

```sh
~/checks$ git status
On branch master
nothing to commit, working tree clean
```

For now, notice how it says that there's nothing to commit and
that the working tree is clean.
Let's modify a file to change that. 

```sh
~/checks$ nano cpu_usage.py 
```
So, now that we've made the change, let's call Git status again and
see the new output Again, Git tells us a lot of things,
including giving us some tips for commands that we might want to use. 

```sh
~/checks$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   cpu_usage.py

no changes added to commit (use "git add" and/or "git commit -a")

```

change that by running the `git add` command.

```sh
~/checks$ git add cpu_usage.py 

~/checks$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   cpu_usage.py
```
Our file is currently part of the `staging area`, and
it will be committed once we run `git commit`.
In this case, instead of opening up an editor, let's pass the commit message
using the `-m` flag, stating that we added `.` at the end of the sentences.

```sh
~/checks$ git commit -m "added . at the end of sentences"
[master 734791d] added . at the end of sentences
 1 file changed, 3 insertions(+), 3 deletions(-)
~/checks$ git status
On branch master
nothing to commit, working tree clean
```

We see that once again, we have no changes to commit.
Because the change we made has gone through the full cycle of `modified`,
`staged` and `committed`.


So to sum up, we work on modified files in our `working tree`.
When they're ready, we staged these files by adding them to the `staging area`.
Finally, we `commit` the changes sitting in our staging area, which takes a snapshot
of those files and stores them in the database that lives in the `git directory`. 

<h3> Commit Message </h3>

A `commit message` is
generally broken up into a few sections.
The first line is a `short summary` of
the commit followed by a `blank line`.
This is followed by a `full description` of
the changes which details why they're
necessary and anything that might be
especially interesting about them
or difficult to understand.
When you run the git commit command,
Git will open up a text editor of
your choice so you can write your commit message.

<h3> Git Log file </h3>

Take a look at what git tracks as part of the `log`.

```sh
~/checks$ git log
commit 734791da9c926530c0413da3f8a62970a478f8c0 (HEAD -> master)
Author: username <email>
Date:   Sun Aug 9 17:01:41 2020 +0530

    added . at the end of sentences

commit b9c9fbeb5d057039dc72ded7cdf9e9e9f28de52a
Author: username <email>
Date:   Sun Aug 9 15:43:50 2020 +0530

    Added cpu_usage.py
```

We can get more information like changes , additions , etc using the `-p` tag which stands for patch.


```sh
~/checks$ git log -p
commit d0c84718b14446e262f0b981106cfa7521a4c8de (HEAD -> master)
Author: name <email>
Date:   Mon Aug 10 15:51:28 2020 +0530

    Added exit status 1 and 0 for if and else

diff --git a/cpu_usage.py b/cpu_usage.py
index 3b891d0..79f9b8f 100644
--- a/cpu_usage.py
+++ b/cpu_usage.py
@@ -1,5 +1,6 @@
 import psutil
 import shutil
+import sys
 
 def cpu_usage(percent):
        usage=psutil.cpu_percent(1)
@@ -8,7 +9,9 @@ def cpu_usage(percent):
 
 if cpu_usage(75):
        print('ERROR ! CPU OVERLOADED.')
+       sys.exit(0)
 else:
        print('ALL GOOD.')
+       sys.exit(1)
 
 

commit 734791da9c926530c0413da3f8a62970a478f8c0
Author: name <email>
Date:   Sun Aug 9 17:01:41 2020 +0530

    added . at the end of sentences

diff --git a/cpu_usage.py b/cpu_usage.py
index 1f618b3..3b891d0 100644
--- a/cpu_usage.py
+++ b/cpu_usage.py
@@ -3,12 +3,12 @@ import shutil
 
 def cpu_usage(percent):
        usage=psutil.cpu_percent(1)
-       print('DEBUG: {}'.format(usage))
+       print('DEBUG: {}.'.format(usage))
        return usage> percent
 
 if cpu_usage(75):
-       print('ERROR ! CPU OVERLOADED')
+       print('ERROR ! CPU OVERLOADED.')
 else:
-       print('ALL GOOD')
+       print('ALL GOOD.')
 
 

commit b9c9fbeb5d057039dc72ded7cdf9e9e9f28de52a
Author: name <email>
Date:   Sun Aug 9 15:43:50 2020 +0530

    Added cpu_usage.py

diff --git a/cpu_usage.py b/cpu_usage.py
new file mode 100644
index 0000000..1f618b3
--- /dev/null
+++ b/cpu_usage.py
@@ -0,0 +1,14 @@
+import psutil
+import shutil
+
+def cpu_usage(percent):
+       usage=psutil.cpu_percent(1)
+       print('DEBUG: {}'.format(usage))
+       return usage> percent
+
+if cpu_usage(75):
+       print('ERROR ! CPU OVERLOADED')
+else:
+       print('ALL GOOD')
+
+
```


The first thing listed for each commit is its `identifier`,
which is a long string of letters and numbers
that uniquely identify each commit. 
Then we get the `date` and `time` the commit was made.
Finally the `commit message` is displayed.


The separate step between `staging` and
`committing` allows us to
stage several changes in one commit.
But if we already know that
the current changes are the ones that we want to `commit`,
we can skip the `staging` step
and go directly to the commit. 

We do this by using the 
`-a` flag to the git commit command.
This flag automatically stages
every file that's tracked and
modified before doing the commit
letting it skip the `git add` step. 

`git commit -a` is a shortcut to stage
any changes to tracked files and commit them in one step. 

```sh
$ git commit -a -m "Added exit status 1 and 0 for if and else "
[master d0c8471] Added exit status 1 and 0 for if and else
 1 file changed, 3 insertions(+)
```

These shortcuts are useful
when making small changes that we know we'll want
to commit directly without keeping them in
the staging area and having to write
long and complex descriptions. 

<h2> Head </h2>

Git uses the head alias to represent
the currently checked out snapshot of your project.
This lets you know what the contents
of your working directory should be.
In this case, the current snapshot
is the latest commit in the project. When you run git commands like diff, branch, or status,
git will use the head bookmark as
a basis for whatever operation it's performing. 

```sh
~/checks$ git log
commit d0c84718b14446e262f0b981106cfa7521a4c8de (HEAD -> master)
Author: name <email>
Date:   Mon Aug 10 15:51:28 2020 +0530

    Added exit status 1 and 0 for if and else

commit 734791da9c926530c0413da3f8a62970a478f8c0
Author: name <email>
Date:   Sun Aug 9 17:01:41 2020 +0530

    added . at the end of sentences

commit b9c9fbeb5d057039dc72ded7cdf9e9e9f28de52a
Author: name <email>
Date:   Sun Aug 9 15:43:50 2020 +0530

    Added cpu_usage.py

```

We can view info about a specific commit using `git show commit_id`

```sh
~/checks$ git show 734791da9c926530c0413da3f8a62970a478f8c0
commit 734791da9c926530c0413da3f8a62970a478f8c0
Author: name <email>
Date:   Sun Aug 9 17:01:41 2020 +0530

    added . at the end of sentences

diff --git a/cpu_usage.py b/cpu_usage.py
index 1f618b3..3b891d0 100644
--- a/cpu_usage.py
+++ b/cpu_usage.py
@@ -3,12 +3,12 @@ import shutil
 
 def cpu_usage(percent):
        usage=psutil.cpu_percent(1)
-       print('DEBUG: {}'.format(usage))
+       print('DEBUG: {}.'.format(usage))
        return usage> percent
 
 if cpu_usage(75):
-       print('ERROR ! CPU OVERLOADED')
+       print('ERROR ! CPU OVERLOADED.')
 else:
-       print('ALL GOOD')
```

<h2> Some git commands </h2>

|      Command      |                                    Explanation & Link                                   |
|:-----------------:|:---------------------------------------------------------------------------------------:|
| git commit -a     | Stages files automatically                                                              |
| git log -p        | Produces patch text                                                                     |
| git show          | Shows various objects                                                                   |
| git diff          | Is similar to the Linux `diff` command, and can show the differences in various commits |
| git diff --staged | An alias to --cached, this will show all staged files compared to the named commit      |
| git add -p        | Allows a user to interactively review patches to add to the current commit              |
| git mv            | Similar to the Linux `mv` command, this moves a file                                    |
| git rm            | Similar to the Linux `rm` command, this deletes, or removes a file                      |


<h2> gitignore files </h2>

`.gitignore` files are used to tell the git tool to intentionally `ignore` some files in a given Git repository. For example, this can be useful for configuration files or metadata files that a user may not want to check into the master branch. 

Example :

```sh
~/checks$ echo unwanted_file > .gitignore

~/checks$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore

nothing added to commit but untracked files present (use "git add" to track)
~/checks$ git add .gitignore 
~/checks$ git commit -a -m 'added unwanted_file to gitignore' 
[master ce3f895] added unwanted_file to gitignore
 1 file changed, 1 insertion(+)
 create mode 100644 .gitignore
```

