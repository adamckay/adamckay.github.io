---
layout: post
title: Python - List Information and a little Cookbook
subtitle: All about lists 
---
Python has a "[batteries included](https://docs.python.org/2/tutorial/stdlib.html#batteries-included)" philosophy with an extensive standard library and included modules. One part of the standard library that I use very often are iterators and sequences, or more specifically, the `list` datatype. Here I'm going to explain a bit about lists in Python and why they're awesome with a few examples. 

## Firstly, what is an iterator?
An iterator is a datatype that represents a stream of data which will, when iterated over, return its elements one at a time. There are multiple iterable types in Python such as `list`, `str` and `tuple`, for this article I am going to focus on `list`.

## Onto the List
A `list` is exactly how it sounds - just a list of things. In other programming languages the equivalent datatype would be an `array`, however an `array` would be of a fixed length and only capable of storing elements of the same type. Python does not have this restriction, a `list` can grow in size as more elements are added to it and it is able to store objects of different types. 

List indices start at 0 and are constructed with square brackets, separating items with a comma like `[42, 'magrathea']` or if you already have a sequence you are able to call `list()` on it.

~~~python
>>>list('vogon')
['v', 'o', 'g', 'o', 'n']
~~~

### List Slicing
Slicing a list allows you to define a start and an end to return subsets and is rather powerful.

~~~python
L[start:end] # start index to end-1 index 
L[start:] # start index to end of list
L[:end] # beginning of list to end-1 index 
L[:] # the full list (creates a copy)

>>> numbers = list(range(10))
>>> numbers
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> numbers[4:8]
[4, 5, 6, 7]
>>> numbers[2:]
[2, 3, 4, 5, 6, 7, 8, 9]
>>> numbers[:3]
[0, 1, 2]
>>> numbers[:]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
~~~

There is also a third argument you can use when slicing - `step`. This specifies how often to step through the list. By default, this is 1 which means it will iterate over every single element in the list, however you can change this to be any integer, positive or negative. If `step` is a negative integer it will iterate through the list backwards (which is often using when determining if a string is a palindrome).

~~~python
>>> numbers = list(range(10))
>>> numbers[::2]
[0, 2, 4, 6, 8]
>>> answer = list('forty two')
>>> answer[::-1]
['o', 'w', 't', ' ', 'y', 't', 'r', 'o', 'f']
~~~

### Joining Elements in a List
If you have a list of strings it's a task to join them together to create a single string. Instead of using a `for` loop to iterate over each string in the list to concatenate it with another string, it's possible to use the `join()` function with a delimiter. 

~~~python
>>> words = ['There\'s', 'a', 'frood', 'who', 'really', 'knows', 'where', 'his', 'towel', 'is']
>>> ' '.join(words)
"There's a frood who really knows where his towel is"
~~~

### Adding Elements in a List
Using the Python built-in function `sum()` we're able to pass an iterable and it will add each element together.

~~~python
>>> numbers = [1, 2, 4, 8, 12, 15]
>>> sum(numbers)
42
~~~

### Sorting a List
Another Python built-in function makes this a doddle - `sorted()`.

~~~python
>>> numbers = [15, 2, 12, 1, 8, 4]
>>> sorted(numbers)
[1, 2, 4, 8, 12, 15]
~~~

### Finding the max and min values in a List
Again, the batteries-included Python standard library has provided us with more built-in functions to make our lives easier - `max()` and `min()`.

~~~python
>>> numbers = [15, 2, 12, 1, 8, 4]
>>> max(numbers)
15
>>> min(numbers)
1
~~~

### Concatenating Lists
Suppose we have multiple separate lists and we want to concatenate them into a single one, we can simply use the `+` operator.

~~~python
>>> humans = ['arthur', 'trillian']
>>> betelgeuseans = ['ford', 'zaphod']
>>> robots = ['marvin']
>>> humans + betalgeuseans + robots
['arthur', 'trillian', 'ford', 'zaphod', 'marvin']
~~~

### Counting the frequency of elements in a List
We can easily count the frequency of elements in a list with the built-in function [`Counter()`](https://docs.python.org/2.7/library/collections.html?highlight=counter#counter-objects) from `collections`.

~~~python
# Setting up our list
>>> h2g2 = """For instance, on the planet Earth, man had always assumeed that he was more intelligent than dolphins because he had achieved so much — the wheel, New York, wars and so on — whilst all the dolphins had ever done was muck about in the water having a good time. But conversely, the dolphins had always believed that they were far more intelligent than man - for precisely the same reasons.

Time is an illusion. Lunchtime doubley so.
"""
>>> words = h2g2.split(' ')

>>> from collections import Counter
>>> Counter(words).most_common(5)
[('had', 4), ('the', 4), ('he', 2), ('was', 2), ('than', 2)]
~~~

### Using a List as a Stack
In Computer Science a stack is a first-in-last-out abstract datatype which allows you to push elements into a list and pop them out (like putting paper into a pile, the first sheet goes to the bottom and will be the last you get to when picking from the top). The `pop()` method on the `list` datatype removes the element at the specified index and returns it, if no index is specified then it gets the last element in the list. 

~~~python
>>> stack = []  # Creating our stack
>>> stack.append('arthur')
>>> stack.append('ford')
>>> stack.append('trillian')
>>> stack.pop()
'trillian'
>>> stack.pop()
'ford'
>>> stack
['arthur']
>>> stack.pop()
'arthur'
>>> stack
[]
~~~

### Using a List as a Queue
In Computer Science a queue is a first-in-first-out abstract datatype which allows you to push elements into a list and retrieve them in the order they were inserted (like a normal queue at a shop). There are two methods to implement a queue, we can use the `pop()` method similar as used when implementing a Stack by specifying the index to be 0 (and therefore the first element in the list) or we can use [`deque`](https://docs.python.org/2.7/library/collections.html?highlight=counter#deque-objects) from the `collections` module. A `list` incurs O(n) costs for `pop(0)` so is not suitable to use for large datasets. `deque` gives O(1) performance and behaves in a similar fashion as a `list`. 

~~~python
>>> from collections import deque
>>> queue = deque()
>>> queue.append('arthur')
>>> queue.append('ford')
>>> queue.append('trillian')
>>> queue.popleft()
'arthur'
>>> queue.popleft()
'ford'
>>> queue
deque(['trillian'])
>>> queue.popleft()
'trillian'
>>> queue
deque([])
~~~

### List Comprehension
Finally, we can't talk about the `list` datatype without discussing list comprehensions. List comprehensions are an *awesome* Pythonic way of creating lists. 

Previously you may have created a list using something like this:

~~~python
new_list = []
for something in old_list:
    if condition(something):
        new_list.append(something)
~~~

However, with a list comprehension are able to rewrite the above into a simple one-liner to enhance readability:

~~~python
new_list = [something for something in old_list if condition(something)]
~~~

Some examples for why List Comprehensions are pretty awesome:

#### List of numbers divisible by 3
For loop:

~~~python
numbers = []
for x in range(50):
    if x % 3 == 0:
        numbers.append(x)
~~~

List Comprehension:

~~~python
numbers = [x for x in range(50) if x % 3 == 0]
~~~


#### List of squares
For loop:

~~~python
squares = []
for x in range(50):
	squares.append(x**2)
~~~

List Comprehension:

~~~python
squares = [x**2 for x in range(50)]
~~~


#### Remove vowels from a sentence
For loop:

~~~python
>>> vowels = 'aeiou'
>>> h2g2 = "Time is an illusion. Lunchtime doubley so."
>>> no_vowels = []
>>> for letter in h2g2:
... 	if not letter in vowels:
... 		no_vowels.append(letter)
>>> ''.join(no_vowels)
'Tm s n llsn. Lnchtm dbly s.'
~~~

List Comprehension:

~~~python
>>> vowels = 'aeiou'
>>> h2g2 = "Time is an illusion. Lunchtime doubley so."
>>> ''.join([letter for letter in h2g2 if not letter in vowels])
'Tm s n llsn. Lnchtm dbly s.'
~~~

----------
Please see the Python Docs [on lists](https://docs.python.org/2/tutorial/datastructures.html#more-on-lists) and [sequences in general](https://docs.python.org/2/library/stdtypes.html#iterator-types)) for further info as the basics are thoroughly explained there.

**PS:-** For another great list, check out the [**Awesome Python**](http://awesome-python.com/) list full awesome of frameworks, libraries and software.
