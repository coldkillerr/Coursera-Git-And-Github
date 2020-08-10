<h1> Reverting Changes </h1>

Being able to revert our changes is one of
the most powerful features
offered by version control systems.
There's a bunch of different techniques available
depending on which changes we need to undo. 

<h2> Undoing changes before staging</h2>

You can change a file back to
its earlier committed state by using
the `git checkout` command
followed by the name of the file you want to revert. If you need to check out individual changes
instead of the whole file,
you can do that using the `-p` flag.


```shell
~/checks$ nano cpu_usage.py 
~/checks$ python3 cpu_usage.py 
Traceback (most recent call last):
  File "cpu_usage.py", line 7, in <module>
    if cpu_usage(75):
NameError: name 'cpu_usage' is not defined
~/checks$ git status 
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   cpu_usage.py

no changes added to commit (use "git add" and/or "git commit -a")
nishit@SHITBOT:~/checks$ git checkout cpu_usage.py
Updated 1 path from the index
nishit@SHITBOT:~/checks$ python3 cpu_usage.py 
DEBUG: 9.1.
ALL GOOD.

nishit@SHITBOT:~/checks$ git status 
On branch master
nothing to commit, working tree clean
```

<h2> Undoing changes after adding them </h2>

If we realize we've added something to
the staging area that we didn't actually want to commit,
we can unstage our changes
by using the `git reset` command. 

```sh

~/checks$ python3 cpu_usage.py  > output.txt
~/checks$ cat output.txt 
DEBUG: 11.5.
ALL GOOD.
~/checks$ git status 
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	output.txt

nothing added to commit but untracked files present (use "git add" to track)
~/checks$ git add *
~/checks$ git status 
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   output.txt

:~/checks$ git reset 
~/checks$ git status 
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	output.txt

nothing added to commit but untracked files present (use "git add" to track)
```
<h2> Ammending Commits </h2>

When we run `git commit --amend`,
git will take whatever is
currently in our staging area and
run the git commit
workflow to overwrite the previous commit.

Let's see this in an example.


```sh

~/checks$ touch auto-update.py

~/checks$ touch gather-info.sh

~/checks$ git add auto-update.py 

~/checks$ git status 
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   auto-update.py
	new file:   output.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	gather-info.sh

~/checks$ git commit -m "Added auto-update.py"
[master 14c6722] Added auto-update.py
 2 files changed, 2 insertions(+)
 create mode 100644 auto-update.py
 create mode 100644 output.txt

~/checks$ git status 
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	gather-info.sh

nothing added to commit but untracked files present (use "git add" to track)

~/checks$ git status 
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	gather-info.sh

nothing added to commit but untracked files present (use "git add" to track)

~/checks$ git add gather-info.sh 

~/checks$ git commit --amend 
[master ee13293] Added auto-update.py, gather-info.sh , output.txt
 Date: Mon Aug 10 20:21:49 2020 +0530
 3 files changed, 2 insertions(+)
 create mode 100644 auto-update.py
 create mode 100644 gather-info.sh
 create mode 100644 output.txt
~/checks$ 
```
Fixing up a local commit with `amend` is
great and you can push it to
a shared repository after you fixed it.
But you should `avoid amending`
commits that have already been `made public`.
If this sounds confusing now, don't worry.
We'll mention it again when we talk about
collaborating with others through shared repositories.

<h2> Rollbacks </h2>

There are a few ways to rollback commits in Git.
For now, we'll focus on using the `git revert` command.
Git revert doesn't just mean undo.
Instead, it creates a commit that contains the inverse of
all the changes made in
the bad commit in order to cancel them out.
For example,
if a particular line was added in the bad commit,
then in the reverted commit,
the same line will be deleted.
This way you get the effect of having undone the changes,
but the history of the commits in the project remains
consistent leaving a record of exactly what happened. 

```sh
~/checks$ git revert HEAD 
Removing output.txt
Removing gather-info.sh
Removing auto-update.py
[master 8fd51b7] Revert "Added auto-update.py, gather-info.sh , output.txt" removed auto-update.py , gather-info.sh , output.txt This reverts commit ee13293d88f673a7148289e8b0b53020c4666994.
 3 files changed, 2 deletions(-)
 delete mode 100644 auto-update.py
 delete mode 100644 gather-info.sh
 delete mode 100644 output.txt
 
 ~/checks$ git log -p -2
commit 8fd51b708874582b09b541d714505f6ee5e4978f (HEAD -> master)
Author: username <email>
Date:   Mon Aug 10 20:41:40 2020 +0530

    Revert "Added auto-update.py, gather-info.sh , output.txt"
    removed auto-update.py , gather-info.sh , output.txt
    This reverts commit ee13293d88f673a7148289e8b0b53020c4666994.

diff --git a/auto-update.py b/auto-update.py
deleted file mode 100644
index e69de29..0000000
diff --git a/gather-info.sh b/gather-info.sh
deleted file mode 100644
index e69de29..0000000
diff --git a/output.txt b/output.txt
deleted file mode 100644
index 6ca0f15..0000000
--- a/output.txt
+++ /dev/null
@@ -1,2 +0,0 @@
-DEBUG: 11.5.
-ALL GOOD.


```
