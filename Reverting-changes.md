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

~/checks$ ls -l
total 8
-rw-rw-r-- 1 nishit nishit   0 Aug 10 20:20 auto-update.py
-rw-rw-r-- 1 nishit nishit 258 Aug 10 17:12 cpu_usage.py
-rw-rw-r-- 1 nishit nishit   0 Aug 10 20:20 gather-info.sh
-rw-rw-r-- 1 nishit nishit  23 Aug 10 17:35 output.txt

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

~/checks$ git log
commit 14c6722ea6dec8a962f87a7dbea9679c92858c7e (HEAD -> master)
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 20:21:49 2020 +0530

    Added auto-update.py

commit ce3f89575ce49b538759b64c733503d7def330b3
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 17:00:38 2020 +0530

    added unwanted_file to gitignore

commit d0c84718b14446e262f0b981106cfa7521a4c8de
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 15:51:28 2020 +0530

    Added exit status 1 and 0 for if and else

commit 734791da9c926530c0413da3f8a62970a478f8c0
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Sun Aug 9 17:01:41 2020 +0530

    added . at the end of sentences
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