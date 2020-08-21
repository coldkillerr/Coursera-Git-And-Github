
<h1> Pull Requests </h1>


As we called out, we can use `GitHub` to look at other people's code and
collaborate with them. 

We're trying to edit a file in a project that we don't have a right access to.
GitHub tells us it created a fork of this project for us,
which we can commit our changes to.
And if we submit changes to this file, it will create a new branch so
that we can send a pull request.


But what exactly is a `fork`?

Forking is a way of creating a copy of the given repository so
that it belongs to our user.
In other words, our user will be able to push changes to the forked copy,
even when we can't push changes to the other repo.
When collaborating on projects hosted on GitHub, the typical workflows,
first, create a fork of the repo, and then work on that local fork.


A `forked repo` is just like a normal repo,
except Github knows which repo it forked from.
So we can eventually merge our changes back into the main repo by creating
a pull request.


A `pull request` is a commit or series of commits that you send to the owner
of the repository so that they incorporate it into their tree.
This is a typical way of working on GitHub, because in most projects,
only a few people have commit rights to the repo.
But anybody can suggest patches, bug fixes or even new features by sending pull
requests that people with commit access can then apply.
Typically, the owners of the repo will review the changes before merging them.
Checking that they match the development guidelines for the project and
that the license is valid and so on. 


One way of doing this is through the `web application` .

Lets look at the other way 

<h2> The typical Pull Request Workflow </h2>

Lets contribute to a colleague's code.
First we will make a `fork` of the original code .
Then we can start working on that code by cloning into it on our local machine.


```sh
~$ git clone https://github.com/nishitjain12/test.git
fatal: destination path 'test' already exists and is not an empty directory.

~$ cd test

~/test$ git checkout -b mybranch
Switched to a new branch 'mybranch'

~/test$ ls
example.py  new_file.py  README.md
nishit@SHITBOT:~/test$ cat example.py 
def git_opeation():
 print("I am adding example.py file to the remote repository.")
git_opeation()

~/test$ nano example.py 

~/test$ git add example.py 

~/test$ git commit -a -m 'added main if conditional'
[mybranch e75cc4d] added main if conditional
 1 file changed, 3 insertions(+), 1 deletion(-)

~/test$ git push -u origin mybranch 

Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 426 bytes | 426.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a pull request for 'mybranch' on GitHub by visiting:
remote:      https://github.com/nishitjain12/test/pull/new/mybranch
remote: 
To https://github.com/nishitjain12/test.git
 * [new branch]      mybranch -> mybranch
Branch 'mybranch' set up to track remote branch 'mybranch' from 'origin'.

```

Now as the code has been pushed we are ready to make a `pull request` to the original repository from where it has been `forked`.We can do this using the web app.

Before creating a pull request,
it's always important to check
that the code will `merge` successfully.



`GitHub` tells us that our change can be
automatically merged, which is great news.
If this wasn't the case,
we'd need to `rebase` our change against
the current branch of the original repo
so that it could be merged.


The window is showing us the `TextBox`
where we can enter `comments` about our change.
We should use this to explain
why we're creating this pull request.
This lets the person that will approve the change
understand why they should
merge the change into the main tree.

After all this we can create the pull request by clicking the `Create Pull Request` button. 


The number next to the name of
our pull request is the identifier
that's used in GitHub to track issues and pull requests.
We can use this `identifying number` to
access this pull request anytime we need it. 

<h2> Updating an Existing Pull Request </h2>

When we send a `pull request`,
it's pretty common to receive some `comments` from
the `project maintainers` asking for some improvements.
The improvement could be to add `documentation` or tests,
or maybe we need to make sure that change works for
all cases or that it
follows the project style `guidelines`.

So back to our change,
lets say we got a comment from one of our colleagues. 

Now do the same process to get the repo on your local machine .
Then change the required code and commit it .Then push your code

```sh
~/test$ nano example.py 
~/test$ git commit -a -m 'added a comment in main function'
[mybranch 751a875] added a comment in main function
 1 file changed, 2 insertions(+)

~/test$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 471 bytes | 471.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/nishitjain12/test.git
   e75cc4d..751a875  mybranch -> mybranch
```
