<h1> Version Control </h1>

In software engineering, version control is system responsible for managing changes to computer programs, documents, large web sites, or other collections of information. Version control is a component of software configuration management.

<h2> Diffing Files </h2>

Imagine you had two copies of some code,
and you wanted to see what the difference was between
them.

`diff` command is used to get lines which are different between both files.

```sh
$ cat fibo1.py 
# Function for nth Fibonacci number 

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

# Driver Program 

print(Fibonacci(9)) 

$ cat fibo2.py 
# Function for nth fibonacci number - Space Optimisataion 
# Taking 1st two fibonacci numbers as 0 and 1 

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

#This code is contributed by Saket Modi 
$ diff fibo1.py fibo2.py
1c1,2
< # Function for nth Fibonacci number 
---
> # Function for nth fibonacci number - Space Optimisataion 
> # Taking 1st two fibonacci numbers as 0 and 1 
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
