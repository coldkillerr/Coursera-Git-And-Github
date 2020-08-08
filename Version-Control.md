<h1> Version Control </h1>

In software engineering, version control is system responsible for managing changes to computer programs, documents, large web sites, or other collections of information. Version control is a component of software configuration management.

<h2> Diffing Files </h2>

Imagine you had two copies of some code,
and you wanted to see what the difference was between
them.

`diff` command is used to get lines which are different between both files.

```sh
$ cat fibo1.py 


def Fibonacci(n): 
	if n<0: 
		print("Incorrect input") 
	# First Fibonacci number is 0 
	elif n==1: 
		return 0
	# Second Fibonacci number is 1 
	elif n==2: 
		return 1
	else: 
		return Fibonacci(n-1)+Fibonacci(n-2) 



print(Fibonacci(9)) 

$ cat fibo2.py 

def fibonacci(n): 
	a = 0
	b = 1
	if n < 0: 
		print("Incorrect input") 
	elif n == 0: 
		return a 
	elif n == 1: 
		return b 
	else: 
		for i in range(2,n): 
			c = a + b 
			a = b 
			b = c 
		return b 

# Driver Program 

print(fibonacci(9)) 

$ diff fibo1.py fibo2.py

3,4c4,7
< def Fibonacci(n): 
< 	if n<0: 
---
> def fibonacci(n): 
> 	a = 0
> 	b = 1
> 	if n < 0: 
6,11c9,12
< 	# First Fibonacci number is 0 
< 	elif n==1: 
< 		return 0
< 	# Second Fibonacci number is 1 
< 	elif n==2: 
< 		return 1
---
> 	elif n == 0: 
> 		return a 
> 	elif n == 1: 
> 		return b 
13c14,18
< 		return Fibonacci(n-1)+Fibonacci(n-2) 
---
> 		for i in range(2,n): 
> 			c = a + b 
> 			a = b 
> 			b = c 
> 		return b 
17c22
< print(Fibonacci(9)) 
---
> print(fibonacci(9)) 
```
The less than `<` symbol tells us that
the first line was removed from the first file,
and the greater than `>` symbol tells us that
the second line was added to the second file.
In other words, the old line got replaced by the new one.

We can see that diff splits
the changes in two separate sections.
The section that starts with `3,4c4,7` shows
a line in the first file that was
replaced by two different lines in the second file.

The `number` at the beginning of this section indicates
the line number in the first and second files.
The `c` in between
the numbers means that a line was changed.

We can also
use the `-u` flag to tell
diff to show the differences in
another format. 

<h2> Applying Changes </h2>

Imagine a colleague sends you a script with
a bug and asked you to help fix the issue.
Once you understood what was wrong with the script,
you could describe to them what they need to change. 

To make the change clear,
you could send them a `diff` with the change so that
they can see what the modified code looks like.

To do this, we typically use a command line :

```sh
$ diff-u old_file new_ file > change.diff
```
The `>` redirects o/p to change.diff

The generated file is usually referred to as
a `diff file` or sometimes a `patch file`.

There's a command called `patch` to go through the file
that needs to be changed,
and apply the modifications. 

Example :

```sh

$ cat cpu_usage.py 
import psutil
import shutil

def cpu_usage(percent):
	usage=psutil.cpu_percent()
	return usage <percent

if cpu_usage(75):
	print('ERROR ! CPU OVERLOADED')
else:
	print('ALL GOOD')

$ python3 cpu_usage.py 
ERROR ! CPU OVERLOADED
```

Now this is erroneous.
So we made a `diff file` with the correct code.

```sh
$ python3 cpu_usage1.py 
DEBUG: 12.3
ALL GOOD

$ diff cpu_usage.py cpu_usage1.py > cpu_usage.diff

$ cat cpu_usage.diff
5,6c5,7
< 	usage=psutil.cpu_percent()
< 	return usage <percent
---
> 	usage=psutil.cpu_percent(1)
> 	print('DEBUG: {}'.format(usage))
> 	return usage> percent

```

So we have the `diff file`
and we want to apply it to our script.
We'll use the `patch` command.
We'll pass the name of the file that we want to
patch in this case, `cpu_usage.py`,
as the first parameter to the command
and then we'll provide the diff
file through standard input.

```sh
$ patch cpu_usage.py cpu_usage.diff 
patching file cpu_usage.py

$ cat cpu_usage.py
import psutil
import shutil

def cpu_usage(percent):
	usage=psutil.cpu_percent(1)
	print('DEBUG: {}'.format(usage))
	return usage> percent

if cpu_usage(75):
	print('ERROR ! CPU OVERLOADED')
else:
	print('ALL GOOD')
	
$ python3 cpu_usage.py 
DEBUG: 10.4
ALL GOOD

```

The `patch` command can
detect that there were changes made to
the file and will do its best to apply the diff anyways.
But sometimes, you might be modifying
a bunch of `large files` inside of a huge project.
We can also diff `whole directory` structures
and in that case the diff file can
specify where each change file should
be without us having to do any manual juggling. 


