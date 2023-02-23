---
title:  Python functions (notes)
tags:
  - python
---


# Functions in Python
This is the page for Functions in [Python](Python.md), it also contains information about functions in other programming languages.

## Motivation
When working on programs often similar problems will make themselves apparant.
Instead of repeating however many lines of code needed to solve the problem, a function can be used instead, different values will be used, however the same 

## Example:
Find the maximum of x & y and store it in a [variable](pyVariable.md)
```python
x = 3
y = 9
maximum = x
if y > maximum:
	maximum = y
```

## Solution with Function
Parameters: take the two values as input
Output: return the maximum
```python
def get_max(x, y):
maximum = x
if y > maximum
	maximum = y
return maximum
```

This function can now be used with different [variables](pyVariable.md)
	max1 = get_max(4, 5)
	max2 = get_max(9, 0)

Using functions is useful because if any errors are found there exists only one code part to check.

## Functions in Python
- Functions allow for the use of inputs using parameters, they can also return values. Neither is necessary
- Naming conventions is similar to [variables](pyVariable.md) (A-z, 0-9, and _ )
```python
def fun1():
	# do something (side effects)
def fun2():
	# create some result and return value
	return result
def fun3(x):
	# do something with x (side effects)
def fun4(x, y, z):
	# do something with x, y, z and return value
	return (x + y) * z
```

## Formal and Actual Parameters
- Formal parameters are those specified in the function definition.
- Actual parameters (or arguments) are those specified when calling the function.
##### Example code:
```python
def get_max(x, y):
	return x if x > y else y
max1 = get_max(4, 5)
max2 = get_max(-12, my_var)
```
- x and y are formal parameters, 4 and 5, and -12 and my_var are actual parameters (arguments)

## Argument Passing
- The actual parameters are passed to the function using call by value with value = object reference.
	- Analogous to [variable](pyVariable.md) assignment.
	- E.g., a = 2 and b = a -> both refer to the same object.
- Important when dealing with mutable objects (e.g., lists) since changes done within the function will be reflected outside! Example:
```python
def add_to_list(some_list, item):
	# append directly changes the list in-place
	some_list.append(item)

my_list = [1, 2, 3]
print(my_list) # [1, 2, 3]
add_to_list(my_list, 4) # some_list = my_list
print(my_list) # [1, 2, 3, 4]
```

## Positional and Keyword Arguments
	Arguments can be passed as positional arguments or keyword arguments.
	Keyword arguments can be at any position and in an arbitrary order.
	Positional arguments must not appear after a keyword argument.
##### Example code:
```python
def get_max(x, y):
	return x if x > y else y
```
- get_max(4, 5) -> x, y positional (x=4, y=5)
- get_max(4, y=5) -> x positional (x = 4), y keyword
- get_max(x=4, y=5) -> x, y keyword
- get_max(y=4, x=5) -> x, y keyword (different order)
- get_max(y=4, 5) -> not allowed (pos arg after kw arg)

## Variable Arguments 
- You can specify your function to allow arbitrary many arguments (also zero):^1
	- For positional arguments: ***args***. Every argument will be collected in a typle.
	- For keywords: ***kwargs***. Every argument will be collected in a dictionary
##### Example code:
```python
def fun(*args, **kwargs):
	# Do something with tuple args
	# Do something with dict kwargs

# Example call with fun(1, 2, x=3, y=4)
args = (1, 2)
kwargs = {"x": 3, "y": 4}
```

Variable number of arguments can be mixed with normal parameter specification under the the following rules:
- Zero or more normal parameters can occur before args
- Zero or more normal parameters can occur following args however they may only be accessed with "keyword-only arguments"
- No parameters can occur after kwargs
- Regular parameters can not be contained within args or kwargs
##### Example code:
```python
def fun(x, *args, y, **kwargs):
	# Do something

fun(1)           # Error: missing kw-only arg y
fun(1, 2)        # Error: missing kw-only arg y
fun(1, y=2)      # x=1, args=(), y=2, kwargs={}
fun(1, 3, y=2)   # x=1, args=(3,), y=2, kwargs={}
fun(1, z=4, y=2) # x=1, args=(), y=2, kwargs={"z":4}
```

## Parameter Unpacking
- Parameters can have optional default argument values:
```python
def create_filled_list(size=3, val=0)
	return [val] * size
```
- Normal parameters must not appear afterwards, e.g.,
	`(size=3, val=0, normal)`
- Different ways of calling such a function:
```python
create_filled_list()              # size=3, val=0
create_filled_list(2)             # size=2, val=0
create_filled_list(2, 1)          # size=2, val=1
create_filled_list(size=2)        # size=2, val=0
create_filled_list(val=1)         # size=3, val=1
create_filled_list(val=1, size=2) # size=2, val=1
create_filled_list(2, val=1)      # size=2, val=1
```
- Default parameters are evaluated once in the beginning, so mutable parameters (e.g., lists) can be changed!

## Type Hinting
- Parameters and return values can have type hints.
- Helpful to show what your function expects as inputs and returns as output.
##### Example code:
```python
def get_max(x: int, y: int) -> int:
	return x if x > y else y
```
- Only hints, you can still pass and return anything!

## Return Value
- Functions can return arbitrary values:
```python
def get_max(x, y):
	return x if x > y else y
```
- Or they can return nothing:
```python
def print_hello():
	print("hello")
```
- In the background, Python actually still returns the special value ***None***, so we could also write either of these two:
```python
def print_hello(): 
	print("hello")
	return None

def print_hello():
	print("hello")
	return
```
- Once the return keyword is encountered, the function will terminate its exectution and returh the specified value.

## Namespaces and Scopes

- A namespace is a dictionary that maps names (variables, functions, etc.) to their objects.
- There are several namespaces: the built-in namespace, the global/module namespace, enclosing/nested.
- The scope of a name defines the namespaces look-up order (it essentially determines the visibility of a name): First, the local namespace is searched, then any enclosing namespaces, followed by the global namespace and lastly the built-in namespace.

- The built-in namespace contains all python built-ins, like len, dict, print, etc.
- The global namespace contains names defined in the main script/module (e.g., global variables)
- The local namespace contains names defined at the innermost level (e.g., local variables within a function)
- In case of nested structures (e.g., a function within a function), the enclosing namespaces contain the names defined in the respective nesting level.

##### Example code:
```python
x = str(12)

def func(a)
	c = 10
	return a + c
```

- Relevent namespaces:
	- str is part of the built-in namespace
	- x and func are part of the global namespace
	- a and c are part of the local namespace

