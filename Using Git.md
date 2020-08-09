<h1> Using Git </h1>



We said that a `VCS` tracks
who made which changes,
for this to work, we need to tell `Git` who we are.
We can do this by using
the `git config` command and then setting the values of
`user.email` and `user.name` to
our email and our name like this. 

```sh
~$ git config --global user.email "myemail@example.com"
~$ git config --global user.name "my_name"
```
We use the `--global` flag to state that we want to
set this value for all git repositories that we'd use.

<h2> Working with a git repository </h2>

There are
two ways to start working with a git repository.

We can create one from scratch using
the `git init` command

```sh
~$ git init
```

or we can
use the `git clone` command to make a copy of
a repository that already exists somewhere else.

```sh
~$ git clone repository_name
```

So when we run `git init` we
initialize an empty git repository
in the current directory.

```sh
~/checks$ git init
Initialized empty Git repository in /home/nishit/checks/.git/
```

The message that we get mentions a directory called.
git.

We can check that this directory exist using
the `ls-la` command which
lists files that start with a `.`

```sh
~/checks$ ls -la
total 12
drwxrwxr-x  3 nishit nishit 4096 Aug  9 14:58 .
drwxr-xr-x 33 nishit nishit 4096 Aug  9 14:50 ..
drwxrwxr-x  7 nishit nishit 4096 Aug  9 14:51 .git
```

We can also use the ls-l.git command to
look inside of it and
see the many different things it contains.
This is called a Git directory.


```sh
~/checks$ ls -l .git
total 32
drwxrwxr-x 2 nishit nishit 4096 Aug  9 14:51 branches
-rw-rw-r-- 1 nishit nishit   92 Aug  9 14:51 config
-rw-rw-r-- 1 nishit nishit   73 Aug  9 14:51 description
-rw-rw-r-- 1 nishit nishit   23 Aug  9 14:51 HEAD
drwxrwxr-x 2 nishit nishit 4096 Aug  9 14:51 hooks
drwxrwxr-x 2 nishit nishit 4096 Aug  9 14:51 info
drwxrwxr-x 4 nishit nishit 4096 Aug  9 14:51 objects
drwxrwxr-x 4 nishit nishit 4096 Aug  9 14:51 refs
```

You can think of it as a `database` for your `Git project`
that stores the changes and the change history.
We can see it contains
a bunch of different files and directories.
We won't touch any of these files directly,
we'll always interact with them through Git commands.

The area outside the git directory is the working tree.
The `working tree` is the current version of your project.
This working tree will contain
all the files that are currently tracked by Git
and any new files that we
haven't yet added to the list of track files.

To make Git track our file,
we'll add it to the project using the `git add`
command passing the file that we want as a parameter. 

```sh
~/checks$ git add cpu_usage.py 
```
The `staging area` which is also known
as the `index` is a file maintained by
Git that contains all of the information about
what files and changes
are going to go into your next command. 

We can use the `git status` command
to get some information about
the current working tree and
pending changes.

```sh
~/checks$ git status 
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   cpu_usage.py

```
We see that our new file is marked to be committed,
this means that our change is
currently in the staging area.
To get it committed into the `.git` directory,
we run the `git commit` command. 

```sh
~/checks$ git commit 
[master (root-commit) b9c9fbe] Added cpu_usage.py
 1 file changed, 14 insertions(+)
 create mode 100644 cpu_usage.py
```

When we run this command,
we tell `Git` that we want to save our changes.
It opens a text editor where
we can enter a `commit message`.
If you want, you can change
the editor used to your preferred editor. 
