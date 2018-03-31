---
title: "Intro to Python Multiple Inheritance Closures and Decorators"
date: 2016-11-14T19:04:25-05:00
draft: false
---

## Why use python?
- It’s a great all purpose scripting language.
- Run on any Linux/Unix by default.
- Easy to set up and run pretty much anywhere.
- Interactive shell.
- Thanks to dynamic typing and concise language,  it’s extremely fast to iterate on something new.
- Tons of utility libraries.


## Multiple Inheritance as means of Mixin and Dependency Injection
It allows a subclass to specify its own inheritance tree. It doesn’t have to rely on its parent class’s inheritance, but rather injecting in its own dependencies via multiple inheritance.

An example I really like is how a Counter object is a subclass of a dictionary and uses the dictionary to store its counts.
Instead of using a regular dictionary, we can make our counter use an OrderedDict by use of multiple inheritance. An OrderedDict is a dictionary that keeps the order of the objects that you put in it.
We can then make a class OrderedCounter that inherits from Counter and OrderedDict.

```
class OrderedCounter(Counter, OrderedDict)
    pass
```
OrderedCounter inherits from both Counter and OrderedDict, and based on Python’s MRO (method resolution order), OrderedDict gets used in place of Counter’s parent dict. 

## Closures
Closure is the idea that methods bring the variables with them from the scope that they are called. This allows us to be able to create multiple functions with the exact same code but acts upon a different set of variables.

Here we can make a series of exponential functions, each with a different base, and we can dynamically specify the exponent as we call the function.
```
def mypow(base):
    def somepow(exp):
        return pow(base, exp)
    return somepow
```
For example, 
```
twopow = mypow(2)
threepow = mypow(3)

twopow(5) # 32
threepow(5) # 243
```

This works because when `mypow` is called, a local variable `base` is created within the `mypow` function. Even though the `somepow` function is returned independetly, it knows that it will need the `base` variable to execute, so all the local variables within the `mypow` function is passed onto `somepow`.


## Decorators
This is a very powerful design pattern that transparently adds features to a function without touching the original function. Though the decorator pattern can be implemented in most languages, it is highly encouraged as standard practice in Python via simple language annotations, and even more powerful when leveraged with closures.
Example: Adding logging or caching to a function.

Say we have a function that returns the square of a number:
```
def square(x):
    return x * x
```

We can easily add a a logging functionality that will tell us the input and ouput when the square function is called.
```
def logging(origfunc):
    def func(x):
        print('Input is', x)
        answer = origfunc(x)
        print('Output is', answer)
    return func
```
And so to add the decorator, we can either explicitly write:
```
square = logging(square)
```
Or idiomatically annotate the `square` function via:
```
@logging
def square(x)
    return x * x
```
