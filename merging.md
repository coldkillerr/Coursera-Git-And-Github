<h1> Merging </h1>

Merging is the term that Git uses for combining branch data and
history together.
We'll use the git merge command, which lets us take the independent snapshots and
history of one `git branch`, and tangle them into another.

```sh
~/checks$ git merge new-new-branch 
Updating 8fd51b7..c253c81
Fast-forward
 empty_file.py | 6 ++++++
 1 file changed, 6 insertions(+)
```

Git uses two different algorithms to perform a merge,
`fast-forward` and `three-way merge`.
The merge we just performed is an example of a fast-forward merge.


This kind of `merge` occurs when all the commits
in the checked out branch are also in the branch that's being merged.
If this is the case,
we can say that the `commit history` of both branches doesn't diverge.
In these cases, all Git has to do is update the `pointers` of the branches to
the same commit, and no actual merging needs to take place.


On the other hand, a `three-way merge` is performed when the history of the merging
branches has diverged in some way, and
there isn't a nice linear path to combine them via fast-forwarding.
This happens when a commit is made on one branch
after the point when both branches split. 

<h2> Merge Conflicts </h2>

From time to time,
we might find that both the
branches we're trying to merge
have edits to the same part of the same file.
This will result in something called a merge conflict.
Normally, Git can automatically merge files for us.
But when we have a merge conflict,
it will need a little help to figure out what to do

```sh
~/checks$ nano empty_file.py 
~/checks$ git branch 
* master
  new-branch
  new-new-branch

~/checks$ git commit -a -m 'Added a '''line''' to the main function'
[master 8cd9f96] Added a line to the main function
 1 file changed, 1 insertion(+), 1 deletion(-)
~/checks$ git checkout new-branch 
Switched to branch 'new-branch'
nishit@SHITBOT:~/checks$ ls
cpu_usage.py
   
~/checks$ git checkout new-new-branch 
Switched to branch 'new-new-branch'
```

```sh
~/checks$ nano empty_file.py 
~/checks$ git commit -a -m 'Added a print function to the main function'
[new-new-branch d04a331] Added a print function to the main function
 1 file changed, 1 insertion(+), 1 deletion(-)
~/checks$ git checkout master 
Switched to branch 'master'
```
```sh
~/checks$ git merge new-new-branch 
Auto-merging empty_file.py
CONFLICT (content): Merge conflict in empty_file.py
Automatic merge failed; fix conflicts and then commit the result.
nishit@SHITBOT:~/checks$ nano empty_file.py 
```

```sh
nishit@SHITBOT:~/checks$ git commit -a -m 'Added both lines (comment and print)'[master 498d83e] Added both lines (comment and print)
nishit@SHITBOT:~/checks$ git merge new-new-branch 
Already up to date.
nishit@SHITBOT:~/checks$ 
```



```python3
#!/usr/bin/env python3

def main():
<<<<<<< HEAD
        '''This is just an empty function'''
=======
        print('This is the main function')
>>>>>>> new-new-branch

main()
```




