# The Symbol '@'
The @ python symbol is used mainly in two places, one as a decorator and one as simple Matrix Multiplication operator.
<br> Example of Matrix Multiplication, we are using numpy matrices. You cannot use it for python list matrix multiplication.
```python
import numpy as np

A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

C = A @ B
print(C) 
```
## Output
```
[[19 22] [43 50]]
```

# The decorator '@'
This decorator is a design pattern in Python that allows a user to add new functionality to an existing object without modifying its structure. 
## A code without the decorator
```python
def lowercase(function):
    def wrapper():
        func = function()
        lowercase_sent = func.lower()
        return lowercase_sent

    return wrapper

def input_AI():
    return 'Hello There'

decorate = lowercase(input_AI)
decorate()
```
but Using @ decorator we can do-

```python
def lowercase(function):
    def wrapper():
        func = function()
        lowercase_sent = func.lower()
        return lowercase_sent

    return wrapper

@lowercase
def input_AI():
    return 'Hello There'
input_AI()
```
## Output for both cases
```
hello there
```
# Explanation
The @ decorator takes the functoin into the lowercase function as argument. The variable func gets the output of input_AI() and returns 
final output we want. Finally we return the wrapper function which will output the lower case when input_AI() is ran.
## Example:
```python
def square(function):
    def wrapper(a,b):
        x=function(a,b)
        return x**2
    return wrapper

@square
def num(a,b):
    return a+b

num(1,2)
```
## Output
```
9
```
# Multi Decoration
You can use many decorations at the same time, The important thing is that order matters, which is BOTTOM to UP. For example:
```python
def upper_case(function):
    def wrapper():
        func=function()
        return func.upper()
    return wrapper

def splitter(function):
    def wrapper():
        func=function()
        return func.split()
    return wrapper

def length(function):
    def wrapper():
        func=function()
        return len(func)
    return wrapper

@length
@splitter
@upper_case
def hello():
    return "hello world how are you all doing"

hello()
```
## OUTPUT SEQUENTIALLY
```
HELLO WORLD HOW ARE YOU ALL DOING ---> ['HELLO,'WORLD', 'HOW', 'ARE', 'YOU', 'ALL', 'DOING'] ---> 7
```
# Using *args and **kwargs and dealing with arguments in target function
```python
def print_content(function):
    def wrapper(*args,**kwargs):
        '''This is a docstring'''
        t, dict=function(*args, **kwargs)
        print(t,dict)
        return t,dict
    return wrapper

@print_content
def id_info(*args,**kwargs):
    return args, kwargs
id_info(1,2,name="Anirudh")
```
## OUTPUT
```
((1, 2), {'name': 'Anirudh'})
```
# Debug your decorator
These functions will specifiy the name and docstring of the wrapper function
```python
# ...continued from above example
print(print_content.__name__) # OUTPUT: 'wrapper'
print(print_content.__doc__) # OUTPUT: 'This is a docstring'
```

# Decorator with arguments
Now lastly we will see how these decorators take arguments and how they work.<br>
Example:
```python
def repeat(num_times):
    def decorator_repeat(func):
        def wrapper(*args, **kwargs):
            for _ in range(num_times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator_repeat

@repeat(num_times=3)
def greet(name):
    print(f"Hello {name}")

greet("World")
```
# Explanation
decorator_repeat is a decorator in the usual sense. It takes a function func and returns a new function wrapper that calls func multiple times.<br>
wrapper is the function that replaces the decorated function. It takes any number of arguments and keyword arguments, calls func num_times times, and returns the result of the last call.<br>
When you use @repeat(num_times=3) before the greet function definition, it's equivalent to greet = repeat(num_times=3)(greet). This replaces greet with the wrapper function returned by decorator_repeat.
