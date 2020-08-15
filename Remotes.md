
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
