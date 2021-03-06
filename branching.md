<h1> Branching </h1>

In Git, a branch at
the most basic level is
just a `pointer` to a particular commit.
But more importantly, it represents
an independent line of development in a project.
Of which the commit it points to is
the latest link in a chain of developing history.
The default branch that Git creates for you
when a new repository initialized is called `master`.
The master branch is commonly used to
represent the known `good state` of a project.

<h2> Creating New Branches </h2>

We could get a list of all branches using `git branch` command

```
~/checks$ git branch 
* master
```
We can create a new branch by writing branch name at the end of the `git branch` command. 

```sh
~/checks$ git branch new-branch
nishit@SHITBOT:~/checks$ git branch 
* master
  new-branch
```
The branch with the `*` is the one we are currently working on 


We can change our branch by calling `git checkout`

```sh
~/checks$ git checkout new-branch 
Switched to branch 'new-branch'
~/checks$ git branch 
  master
* new-branch
```


We can use the `git checkout -b
new branch` to create and change to that branch in one command.

```sh
~/checks$ git checkout -b new-new-branch 
Switched to a new branch 'new-new-branch'
```
<h2> Operating on New Branches </h2>

```sh
~/checks$ nano empty_file.py

~/checks$ git add empty_file.py

~/checks$ git commit -a -m "added empty_file.py"
[new-new-branch c253c81] added empty_file.py
 1 file changed, 6 insertions(+)
 create mode 100755 empty_file.py

~/checks$ git log -3
commit c253c81e5912bd0e9fda2c3687dd1165142599dd (HEAD -> new-new-branch)
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 23:22:52 2020 +0530

    added empty_file.py

commit 8fd51b708874582b09b541d714505f6ee5e4978f (new-branch, master)
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 20:41:40 2020 +0530

    Revert "Added auto-update.py, gather-info.sh , output.txt"
    removed auto-update.py , gather-info.sh , output.txt
    This reverts commit ee13293d88f673a7148289e8b0b53020c4666994.

commit ee13293d88f673a7148289e8b0b53020c4666994
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 20:21:49 2020 +0530

    Added auto-update.py, gather-info.sh , output.txt
```



We've added a commit in
this `branch` and it's looking better.
Let's check the last two entries in the log.
We see the last two commits in this branch.
Notice how next to the latest `commit ID`,
git shows that this is where `head` is pointing
to and that the branch is called `new-new-branch`.
Next to the previous commit,
git shows that both the master and
the new feature branches are
pointing to that snapshot of the project.


```sh
~/checks$ git status
On branch new-new-branch
nothing to commit, working tree clean

~/checks$ git log -2
commit c253c81e5912bd0e9fda2c3687dd1165142599dd (HEAD -> new-new-branch)
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 23:22:52 2020 +0530

    added empty_file.py

commit 8fd51b708874582b09b541d714505f6ee5e4978f (new-branch, master)
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 20:41:40 2020 +0530

    Revert "Added auto-update.py, gather-info.sh , output.txt"
    removed auto-update.py , gather-info.sh , output.txt
    This reverts commit ee13293d88f673a7148289e8b0b53020c4666994.
```
We can see that the HEAD points to the `new-new-branch`

```sh
~/checks$ ls -l
total 8
-rw-rw-r-- 1 nishit nishit 258 Aug 10 17:12 cpu_usage.py
-rwxrwxr-x 1 nishit nishit  50 Aug 10 23:21 empty_file.py
nishit@SHITBOT:~/checks$ git checkout master 
Switched to branch 'master'
nishit@SHITBOT:~/checks$ ls -l
total 4
-rw-rw-r-- 1 nishit nishit 258 Aug 10 17:12 cpu_usage.py
```
As soon as we change the branch we see that the file contents change too.

```sh
~/checks$ git status
On branch master
nothing to commit, working tree clean
~/checks$ git log -2
commit 8fd51b708874582b09b541d714505f6ee5e4978f (HEAD -> master, new-branch)
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 20:41:40 2020 +0530

    Revert "Added auto-update.py, gather-info.sh , output.txt"
    removed auto-update.py , gather-info.sh , output.txt
    This reverts commit ee13293d88f673a7148289e8b0b53020c4666994.

commit ee13293d88f673a7148289e8b0b53020c4666994
Author: coldkillerr <yonishitjain123456@gmail.com>
Date:   Mon Aug 10 20:21:49 2020 +0530

    Added auto-update.py, gather-info.sh , output.txt

```
Now the `HEAD` points to both the `master` and `new-new-branch` .

<h2> Deleting Branches </h2>

We can do that by using `git branch -d`.

```sh
~/checks$ git branch -d new-new-new-branch 
Deleted branch new-new-new-branch (was 8fd51b7).
```
Git warns us about `Unmerged branches` 

```sh
~/checks$ git branch -d new-new-branch 
error: The branch 'new-new-branch' is not fully merged.
If you are sure you want to delete it, run 'git branch -D new-new-branch'.
```


