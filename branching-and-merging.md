<h1> Branching and Merging </h1>

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
