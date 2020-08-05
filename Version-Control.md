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

