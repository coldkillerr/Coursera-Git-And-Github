<h1> Reverting Changes </h1>

Being able to revert our changes is one of
the most powerful features
offered by version control systems.
There's a bunch of different techniques available
depending on which changes we need to undo. 

<h2> Undoing chaanges before staging</h2>

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
