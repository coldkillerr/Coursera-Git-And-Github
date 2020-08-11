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
