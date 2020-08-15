
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
