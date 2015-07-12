---
layout: post
title: Python: Is "is" equal to equality?
subtitle: What's the difference between "is" and "=="?
---
Python is a language seemingly obsessed with readability, with it's English-like syntax often making it possible to read lines of code as grammatical sentences and with very thorough style guide on writing Pythonic code as documented in [**PEP8**](https://www.python.org/dev/peps/pep-0008/). 

It's, therefore, a common idea to use the `is` operator for conditionals instead of `==`. It's more readable and looks like it should work along side `and`, `or` and `not`, commonly done in other languages as `&&`, `||` and `!`.

So what's the problem with using `is` to test for equality? 

Let's take a look at an example. 
~~~
>>> a = 1
>>> b = 1
>>> a is b
True
>>> a == b
True
>>> b = 2
>>> a is b
False
>>> a == b
False
~~~
So far everything is as we expect, but what happens if we use slightly bigger numbers?
~~~
>>> c = 300
>>> d = 300
>>> c is d
False
>>> c == d
True
~~~

That's odd, isn't it? If we check the [**Is Operator in the Python Docs**](https://docs.python.org/2/library/operator.html?highlight=#operator.is_) it says that it _"Tests object identity"_. This means we're not testing if two objects have the same _value_, we're testing if the two objects are the _same_.

As everything in Python is an object we're able to check this ourselves in the Python intepreter using the `id` function, which will return the ID reference of the object.

We can test the `id` function does as we expect by creating two variables which point to the same object and checking their IDs.
~~~
>>> x = "testing id"
>>> y = x
>>> id(x)
30538192L
>>> id(y)
30538192L
~~~
The IDs are the same, so it's the same object.

Using the same variables as before with our integers we can check if they are the same.
~~~
>>> id(a)
30649176L
>>> id(b)
30649176L
>>> id(c)
31719000L
>>> id(d)
31718976L
~~~
As we can see, the objects for our `a` and `b` variables are exactly the same, however our variables for `c` and `d` are different. 
This means the oddity with the `is` operator is making sense - it's returning `True` for `a is b` because they are the same objects and `False` for `c is d` because the objects are not the same.

But why?

Well this comes down to how the CPython intepreter, upon starting, creates a bunch of objects to cache. The [**Plain Integer Objects page within the Python C Docs**](https://docs.python.org/2/c-api/int.html) explains how _"The current implementation keeps an array of integer objects for all integers between -5 and 256, when you create an int in that range you actually just get back a reference to the existing object. So it should be possible to change the value of 1. I suspect the behaviour of Python in this case is undefined"_. 

This means when we defined `a` and `b` to `1` we were assigning the same pre-created object to these variables, so our `is` operator determines them to be identical. Our `c` and `d` variables were defined to `300` which does not have a pre-existing cached object and so creates two new objects which are different. 

What does this mean?

Well we should not use the `is` operator in conditionals unless you are explicitly wanting to determine if the two objects are one and the same. 
In practise, this means the time you will use `is` the most is when checking if a variable `is None` because the `None` object is another one of the pre-created objects - it will always be the same and is faster than `==` because it is implemented in C and is checking the object ID reference, rather than testing equality.