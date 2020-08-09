<h1> Using Git </h1>

<h2> Adding username </h2>

we said that a VCS tracks
who made which changes,
for this to work, we need to tell Git who we are.
We can do this by using
the Git config command and then setting the values of
user.email and user.name to
our email and our name like this. 

```sh
~$ git config --global user.email "myemail@example.com"
~$ git config --global user.name "my_name"
```
