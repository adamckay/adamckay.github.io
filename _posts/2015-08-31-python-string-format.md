---
layout: post
title: Python - String Formatting
subtitle: Commonly used .format() usages
---

Python has a number of ways to format strings including the plain old string concatenation with `+`, the `%` operator and the `.format()` method. String concatenation with `+` can make long strings harder to modify in the future and is very limited for localisation. String formatting with `%` can be difficult to read as there is no indication of the variable that is going to be inserted into the string. String formatting with `.format()` is much better as it is very readable, is easy to localise and can provide a wide range of options for formatting varaibles.

##.format() 
[**String formatting**](https://docs.python.org/2/library/stdtypes.html#str.format) with `.format()` is very powerful allowing you to format strings in a wide range of ways, however, remembering all of the options can be difficult so here are the most common methods that you could need. 

###Basic Formatting
Positional formatting is the most basic use-case that you may have, however this does not take advantage of the internationalisation that is possible.  
`'{} {}'.format('arthur', 'dent')` results in `arthur dent`.

You can also explicitly set the positional index without having to rearrange the variable arguments.  
`'{1} {0}'.format('ford', 'prefect')` results in `prefect ford`.

###Named Placeholders
We can set keywords to make the strings easier to read and this also makes it possible localise. This is my preferred method.  
`'{first} {second}'.format(first='tricia', second='mcmillan')` results in `tricia mcmillan`.

###Truncating Long Strings
Shortening long strings to a specific number of characters.  
`'{:.6}'.format('slartibartfast')` results in `slarti`.

Shortening long strings to a specific number of characters set as an argument.  
`'{:.{}}'.format('slartibartfast', 6)` results in `slarti`.

###Padding and Aligning Strings
Strings are, by default, formatted to take up as many characters as necessary, however it is possible to explicitly define the length of a string to pad it if required.  
`'{:10}'.format('zaphod')` results in `zaphod⋅⋅⋅⋅`.  
`'{:>10}'.format('zaphod')` results in `⋅⋅⋅⋅zaphod`.

It is also possible to pad a string by a certain amount as set as an argument.  
`'{:<{}s}'.format('zaphod', 10)` results in `zaphod⋅⋅⋅⋅`.

It is even possible to center align strings.  
`'{:^10}'.format('zaphod')` results in `⋅⋅zaphod⋅⋅`.

###Truncate and Pad Strings
It is possible to both truncate and pad strings at the same time.  
`'{:10.3}'.format('zaphod')` results in `zap⋅⋅⋅⋅⋅⋅⋅`.

###Numbers as Strings
We can format numbers very easily.  
`'{:d}'.format(42)` results in `42`.

###Padding Numbers
In the same way as strings, we can also pad numbers to a specific length.  
`'{:4d}'.format(42)` results in `⋅⋅42`.  
`'{:03d}'.format(42)` results in `042`.  
`'{:06.2f}'.format(3.141592654)` results in `003.14`.

###Signed Numbers
By default, negative numbers are signed, but we can also set positive numbers to be prefixed too.  
``{:+d}'.format(42)` results in `+42`.

###Datetime
We can also format datetime objects.  
`'{:%Y-%m-%d %H:%M}'.format(datetime(1978, 3, 8, 4, 2))` results in `1978-03-04 04:02`.
