
<h1> Working with Remote Repository </h1>

Whenever we're operating with remotes, Git uses `remote branches`
to keep copies of the data that's stored in the `remote repository.`


When we call a `git clone` to get a local copy of a remote repository,
Git sets up that remote repository with the default `origin` name.
We can look at the configuration for
that remote by running `git remote -v` in the directory of the repo.

```sh
~/checks/checks$ git remote -v
origin	https://github.com/coldkillerr/checks.git (fetch)
origin	https://github.com/coldkillerr/checks.git (push)
```

They'll usually point at the same place.
But in some cases, you can have the `fetch URL` use `HTTP` for `read only` access,
and the `push URL` use `HTTPS` or `SSH` for access control.

If we want to get even more information about our remote,
we can call git remote show origin. 

```sh
~/checks/checks$ git remote show origin 
* remote origin
  Fetch URL: https://github.com/coldkillerr/checks.git
  Push  URL: https://github.com/coldkillerr/checks.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```



We could have a look at the remote branches that
our Git repo is currently tracking by running `git branch -r.` 

```sh
~/checks/checks$ git branch -r
  origin/HEAD -> origin/master
  origin/master
```

These branches are `read only`
We can look at the commit history, like we would with local branches, but
we can't make any changes to them directly.
To modify their contents, we'll have to go through the workflow we called out before.

We can also use git status to check the status of our changes in remote
branches as well. 

```sh
~/checks/checks$ git status
On branch master
Your branch is up to date with 'origin/master'.
```

<h2> What if the remote repo is out of date ? </h2>

Lets assume someone addes files to our repository on the repository which have not been added to our remote repository.

```sh
~/checks/checks$ git remote show origin 
* remote origin
  Fetch URL: https://github.com/coldkillerr/checks.git
  Push  URL: https://github.com/coldkillerr/checks.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (local out of date)
```

From the above command we see that the `master` branch is locally `out of date`

This happens when there were commits
done to the repo that aren't yet reflected locally.

Git doesn't keep remote and local branches
in sync automatically,
it waits until we execute
commands to move data around when we're ready.

To sync the data, we use the `git fetch` command. 

Fetched content is downloaded to
the remote branches on our repository.
So it's not automatically mirrored to our local branches. 

```sh
~/checks/checks$ git fetch
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 683 bytes | 683.00 KiB/s, done.
From https://github.com/coldkillerr/checks
   c5ee76c..939480c  master     -> origin/master
```


We can run `git checkout` on
these branches to see the working tree,
and we can run git log to see the commit history.

```sh
~/checks/checks$ git checkout 
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```
Let's look at the current commits in the remote repo by
running `git log origin/master.`


```sh
~/checks/checks$ git log origin/master
commit 939480ccbdc8a423e7874701b3683255bd7e1c57 (origin/master, origin/HEAD)
Author: Nishit Jain <yonishitjain123456@gmail.com>
Date:   Sat Aug 15 13:56:16 2020 +0530

    Create plain_python.py

commit c5ee76c24cfaac37d8fb058ed335e6adc57bfb79 (HEAD -> master)
Merge: bc5feac 24a8e4e
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Sat Aug 15 13:43:06 2020 +0530

    Merge branch 'master' of https://github.com/coldkillerr/checks

commit bc5feaca5ecd16b7f070fddc2212296cd622de2f
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Sat Aug 15 13:42:07 2020 +0530

    Added two new files

commit 24a8e4e278e786604e196e4309ce8c8df85f47c9
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Thu Aug 13 12:50:38 2020 +0530

nishit@SHITBOT:~/checks/checks$ git checkout 
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```

Looking at this output,
we can see that the remote `origin/branch`
is pointing to the latest commit.
While the `local master branch` is pointing
to the previous commit we made earlier on. 

Lets see what happens when we run `git status` now

```sh
~/checks/checks$ git status
On branch master
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

Git status helpfully tells us that
there's a commit that we don't have in our branch by letting us know our branches
behind their remote origin/master branch.
If we want to integrate
the branches into our master branch,
we can perform a merge operation,
which merges the origin/master branch
into our local master branch.


To do that, we'll call `git merge origin/master`

```sh
~/checks/checks$ git merge origin/master 
Updating c5ee76c..939480c
Fast-forward
 plain_python.py | 4 ++++
 1 file changed, 4 insertions(+)
 create mode 100644 plain_python.py

~/checks/checks$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```
We see that now our `master` branch is up to
date with the remote `origin/master` branch.

Since `fetching` and `merging` are so
common, Git gives us the `git pull` command that does both for us.
Running git pull will fetch the `remote copy` of the current branch and
automatically try to merge it into the current local branch.

If there is a new branch we can checkout that branch using `git checkout`

<h2> Git Remotes Cheat-Sheet </h2>

|          Command          |                                              Explanation & Links                                              |
|:-------------------------:|:-------------------------------------------------------------------------------------------------------------:|
| git remote                | Lists remote repos                                                                                            |
| git remote -v             | List remote repos verbosely                                                                                   |
| git remote show <name>    | Describes a single remote repo                                                                                |
| git remote update         | Fetches the most up-to-date objects                                                                           |
| git fetch                 | Downloads specific objects                                                                                    |
| git branch -r             | Lists remote branches; can be combined with other branch arguments to manage remote branches                  |
| git merge <branch>        | Merge joins branches together.                                                                                |
| git merge --abort         | If there are merge conflicts (meaning files are incompatible), --abort can be used to abort the merge action. |
| git log --graph --oneline | This shows a summarized view of the commit history for a repo.                                                |
  
  <h2> Solving Conflicts </h2>
  
  Imagine you and your co-worker made a change in the same part of the code . Now this leads to the arousal of a conflict.
  
  ~/checks/checks$ git add -p
diff --git a/plain_python.py b/plain_python.py
index 68a2498..de37084 100644
--- a/plain_python.py
+++ b/plain_python.py
@@ -1,4 +1,4 @@
 def main():
-  pass
+  print('We are in the main')
   
 main()
(1/1) Stage this hunk [y,n,q,a,d,e,?]? y

```sh

~/checks/checks$ git commit -m "Added a print function in main function"
[master 8190a5d] Added a print function in main function
 1 file changed, 1 insertion(+), 1 deletion(-)
~/checks/checks$ git push
Username for 'https://github.com': coldkillerr
Password for 'https://coldkillerr@github.com': 
To https://github.com/coldkillerr/checks.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/coldkillerr/checks.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
Git rejected our change,
that's because the remote repository contains changes that we don't have in
our local branch that Git can't `fast-forward`. 

This means we need to sync our local remote branch
with the remote repository before we can push.

We can run `git pull` to do this .

```sh
~/checks/checks$ git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 679 bytes | 679.00 KiB/s, done.
From https://github.com/coldkillerr/checks
   939480c..dde8f0f  master     -> origin/master
Auto-merging plain_python.py
CONFLICT (content): Merge conflict in plain_python.py
Automatic merge failed; fix conflicts and then commit the result.
```

 Git tried to automatically merge the local and
remote changes to `plain_python.py`, but found a conflict.
Let's first look at the `tree` of commits on all branches as
represented by `git log --graph --oneline --all`

```sh
~/checks/checks$ git log --oneline --graph --all
* 8190a5d (HEAD -> master) Added a print function in main function
| * dde8f0f (origin/master, origin/HEAD) added print function in main function
|/  
* 939480c Create plain_python.py
*   c5ee76c Merge branch 'master' of https://github.com/coldkillerr/checks
|\  
| * 24a8e4e Added a line to the README.md file
* | bc5feac Added two new files
|/  
* f271073 Initial commit
```
The graph indicates that our current commit and the commit in the `origin/master`
branch share a common ancestor, but they don't follow one another. 

This means that we'll need to do a `three-way merge`
To do this, let's look at the actual changes in that
commit by running `git log -p origin/master` 

```sh
~/checks/checks$ git log -p origin/master 
commit dde8f0ff50f8868b56a80a304c0efd1fe6820980 (origin/master, origin/HEAD)
Author: Nishit Jain <email>
Date:   Sat Aug 15 14:56:27 2020 +0530

    added print function in main function

diff --git a/plain_python.py b/plain_python.py
index 68a2498..76102b5 100644
--- a/plain_python.py
+++ b/plain_python.py
@@ -1,4 +1,4 @@
 def main():
-  pass
+  print('This is main')
   
 main()

commit 939480ccbdc8a423e7874701b3683255bd7e1c57
Author: Nishit Jain <email>
Date:   Sat Aug 15 13:56:16 2020 +0530

    Create plain_python.py
```

Let's fix it by editing the file to remove the `conflict`.

```sh
~/checks/checks$ git commit -a -m "solved a conflict"
[master b62d4d7] solved a conflict

~/checks/checks$ git push
Username for 'https://github.com': coldkillerr
Password for 'https://coldkillerr@github.com': 
remote: Invalid username or password.
fatal: Authentication failed for 'https://github.com/coldkillerr/checks.git/'

~/checks/checks$ git push
Username for 'https://github.com': coldkillerr
Password for 'https://coldkillerr@github.com': 
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 624 bytes | 624.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To https://github.com/coldkillerr/checks.git
   dde8f0f..b62d4d7  master -> master
```

Yes, after fixing the conflict, we were able to push our work to the remote repo.
Let's look at the commit history of the master branch now,
by calling `git log --graph --oneline`


```sh
~/checks/checks$ git log --oneline --graph --all
*   b62d4d7 (HEAD -> master, origin/master, origin/HEAD) solved a conflict
|\  
| * dde8f0f added print function in main function
* | 8190a5d Added a print function in main function
|/  
* 939480c Create plain_python.py
*   c5ee76c Merge branch 'master' of https://github.com/coldkillerr/checks
|\  
| * 24a8e4e Added a line to the README.md file
* | bc5feac Added two new files
|/  
* f271073 Initial commit
```

We see that the latest commit is the merge, followed by the two commits that
caused the merge conflict, which are on split paths in our graph.
As we called out before, when Git needs to do a `three-way merge`, we
end up with a `separate commit` for merging the branches back into the main tree

<h2> Pushing Remote Branches </h2>

As we called up before,
when using Git to work on
a new feature or a big refactor of some kind,
it's recommended best practice
to create separate branches.
There are many advantages to doing this.

Lets create and swirch to a new branch `refactor`

```sh
~/checks/checks$ git checkout refactor
Already on 'refactor'
```

Lets update the code to add a new `hello_world` feature 

```sh
~/checks/checks$ git commit -a -m 'Added a new hello_world() function'
[refactor db79f36] Added a new hello_world() function
 1 file changed, 5 insertions(+)
```

Now when we push the code to the `origin` repository we have to add the `-u` for `upstream` and the branch name to be pushed .

```sh
~/checks/checks$ git push -u origin refactor 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 403 bytes | 403.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'refactor' on GitHub by visiting:
remote:      https://github.com/coldkillerr/checks/pull/new/refactor
remote: 
To https://github.com/coldkillerr/checks.git
 * [new branch]      refactor -> refactor
Branch 'refactor' set up to track remote branch 'refactor' from 'origin'.
```
As we see git gives us a lot of info and from the last line we see that the branch has been pushed.
Lets make some more changes to the branch refactor.



```sh

~/checks/checks$ nano cpu_usage.py 
~/checks/checks$ nano empty_file.py 

~/checks/checks$ git commit -a -m 'changed percentage in cpu_usage.py , added add function in empty_file.py'
[refactor 8b608b3] changed percentage in cpu_usage.py , added add function in empty_file.py
 2 files changed, 4 insertions(+), 1 deletion(-)

~/checks/checks$ git push -u origin refactor 
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 506 bytes | 506.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/coldkillerr/checks.git
   db79f36..8b608b3  refactor -> refactor
Branch 'refactor' set up to track remote branch 'refactor' from 'origin'.

~/checks/checks$ git log --graph --oneline 
* 8b608b3 (HEAD -> refactor, origin/refactor) changed percentage in cpu_usage.py , added add function in empty_file.py
* db79f36 Added a new hello_world() function
*   b62d4d7 solved a conflict
|\  
| * dde8f0f added print function in main function
* | 8190a5d Added a print function in main function
|/  
* 939480c Create plain_python.py
*   c5ee76c Merge branch 'master' of https://github.com/coldkillerr/checks
|\  
| * 24a8e4e Added a line to the README.md file
* | bc5feac Added two new files
|/  
* f271073 Initial commit

~/checks/checks$ git checkout master 
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
nishit@SHITBOT:~/checks/checks$ git pull
Already up to date.

~/checks/checks$ git log --graph --oneline 
*   3b2cd39 (HEAD -> master, origin/master, origin/HEAD) Merge pull request #1 from coldkillerr/refactor
|\  
| * db79f36 Added a new hello_world() function
|/  
*   b62d4d7 solved a conflict
|\  
| * dde8f0f added print function in main function
* | 8190a5d Added a print function in main function
|/  
* 939480c Create plain_python.py
*   c5ee76c Merge branch 'master' of https://github.com/coldkillerr/checks
|\  
| * 24a8e4e Added a line to the README.md file
* | bc5feac Added two new files
|/  
* f271073 Initial commit

~/checks/checks$ git checkout refactor 
Switched to branch 'refactor'
Your branch is up to date with 'origin/refactor'.
nishit@SHITBOT:~/checks/checks$ git log --graph --oneline 
* 8b608b3 (HEAD -> refactor, origin/refactor) changed percentage in cpu_usage.py , added add function in empty_file.py
* db79f36 Added a new hello_world() function
*   b62d4d7 solved a conflict
|\  
| * dde8f0f added print function in main function
* | 8190a5d Added a print function in main function
|/  
* 939480c Create plain_python.py
*   c5ee76c Merge branch 'master' of https://github.com/coldkillerr/checks
|\  
| * 24a8e4e Added a line to the README.md file
* | bc5feac Added two new files
|/  
* f271073 Initial commit

```
As we can see the changes made in the reafctor has not been merged .

```sh
~/checks/checks$ git checkout refactor 
Switched to branch 'refactor'
Your branch is up to date with 'origin/refactor'.

~/checks/checks$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: changed percentage in cpu_usage.py , added add function in empty_file.py
```

```sh
~/checks/checks$ git log --oneline --graph 
* ecb3a4d (HEAD -> refactor) changed percentage in cpu_usage.py , added add function in empty_file.py
*   3b2cd39 (origin/master, origin/HEAD, master) Merge pull request #1 from coldkillerr/refactor
|\  
| * db79f36 Added a new hello_world() function
|/  
*   b62d4d7 solved a conflict
|\  
| * dde8f0f added print function in main function
* | 8190a5d Added a print function in main function
|/  
* 939480c Create plain_python.py
*   c5ee76c Merge branch 'master' of https://github.com/coldkillerr/checks
|\  
| * 24a8e4e Added a line to the README.md file
* | bc5feac Added two new files
|/  
* f271073 Initial commit
```
Now we can see the `master` branch and linear history with our list of commits.
We're ready to `merge` our commits back onto the main trunk of our repo and
have this `fast forwarded`.
To do that, we'll check out the master branch and merge the refactor branch. 

```sh
~/checks/checks$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
~/checks/checks$ git merge
Already up to date.
~/checks/checks$ git merge refactor 
Updating 3b2cd39..ecb3a4d
Fast-forward
 cpu_usage.py  | 2 +-
 empty_file.py | 3 +++
 2 files changed, 4 insertions(+), 1 deletion(-)
```

Awesome, we were able to merge our branch through a `fast forward merge` and
keep our history `linear`.
We're now done with our `refactor` and
can get rid of that branch, both `remotely` and `locally`

```sh
~/checks/checks$ git push --delete origin refactor 
To https://github.com/coldkillerr/checks.git
 - [deleted]         refactor

~/checks/checks$ git branch -d refactor 
Deleted branch refactor (was ecb3a4d).
```

Now we can push the changes to the github repository.

```sh
~/checks/checks$ git push
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 509 bytes | 509.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/coldkillerr/checks.git
   3b2cd39..ecb3a4d  master -> master
```
<h2> Another use of rebase </h2>

We looked at
an example use case for git rebase.
Where we used it to rebase
a feature branch so that it could be cleanly integrated.


There are many other possible uses of rebase.
One common example is to
rebase the changes in the master branch
when someone else also made
changes and we want to keep history linear.


This is a pretty common occurrence
when you're working on a change that's small
enough not to need a separate branch and
your collaborators just happened
to commit something at the same time.

So lets make a change to our code 

```sh
~/checks/checks$ nano empty_file.py 

~/checks/checks$ git commit -a -m 'added substract function'
[master 4fd1edb] added substract function
 1 file changed, 5 insertions(+), 1 deletion(-)
```

We want to check if one of
our teammates also made a change
in the `master` branch while we were working on our change.
We saw how to do that by running `git pull` which
will automatically create a `three-way merge` if necessary.


In this example, we want to look at
a different approach to keep our project history linear.
So we'll start by calling `git
fetch` which you might remember we'll put
the latest changes into the origin slash
master branch but we
won't apply them to our local master branch. 

```sh
~/checks/checks$ git fetch
```
This means that if we tried to merge our changes,
we end up with a `three-way merge`.
Instead, we'll now run `git rebase` against
our `origin/master` to rebase our changes
against those made by
our colleague and keep history linear. 


```sh
~/checks/checks$ git rebase origin/master
First, rewinding head to replay your work on top of it...
Applying: added substract function
Using index info to reconstruct a base tree...
M	empty_file.py
Falling back to patching base and 3-way merge...
Auto-merging empty_file.py
CONFLICT (content): Merge conflict in empty_file.py
error: Failed to merge in the changes.
Patch failed at 0001 added substract function
hint: Use 'git am --show-current-patch' to see the failed patch
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
```
We've got a conflict and we'll need to fix it.
Git is giving us a lot of info on what
it tried to do including what worked,
what didn't work and what we can do about it.


Since we asked it to rebase,
it tried to `rewind` our changes and apply them
on top of what was in the `origin/master` branch. But when trying to `merge` our changes with
the changes made by our colleague in the file,
there was a `merge conflict`


The output gives us a bunch
of instructions on how to solve this.
We could fix the conflict,
skip the conflicting commit
or even abort the rebased completely.
In this example, we want to fix the conflict.
So let's do that. We'll start by looking at
the current state of the `empty_file.py` file. 

To solve the conflict lets edit the file as per the requirement.

```sh
~/checks/checks$ nano empty_file.py 
```
```sh
~/checks/checks$ python3 empty_file.py 
This is the main function
Hello world
13
1
```
So now the conflict is resolved and the code is working .

```sh
~/checks/checks$ git add empty_file.py 

~/checks/checks$ git rebase --continue 
Applying: added substract function
```

Now we've added the changes and continued with the rebase using `git rebase --continue`

let's check out the output of
`git log --graph --oneline`
to see what the history looks like at this point. 

```sh
~/checks/checks$ git log --graph --oneline 
* a94d394 (HEAD -> master) added substract function
* c24f2af (origin/master, origin/HEAD) Update empty_file.py
* ecb3a4d changed percentage in cpu_usage.py , added add function in empty_file.py
*   3b2cd39 Merge pull request #1 from coldkillerr/refactor
|\  
| * db79f36 Added a new hello_world() function
|/  
*   b62d4d7 solved a conflict
|\  
| * dde8f0f added print function in main function
* | 8190a5d Added a print function in main function
|/  
* 939480c Create plain_python.py
*   c5ee76c Merge branch 'master' of https://github.com/coldkillerr/checks
|\  
| * 24a8e4e Added a line to the README.md file
* | bc5feac Added two new files
|/  
* f271073 Initial commit
```


What we did just now to resolve the conflict is
very similar to what we did earlier to merge our changes.
The difference is, that the commit history
ended up being `linear` instead of `branching` out.


We're now ready to `push` our new check to the remote repo.

```sh
~/checks/checks$ git push 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 382 bytes | 382.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/coldkillerr/checks.git
   c24f2af..a94d394  master -> master
```

