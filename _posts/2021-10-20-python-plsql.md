---
layout: post
quote: Wrapper for cx_Oracle. 🐍
syntax_highlighting: true
title: Python PL/SQL
---

Rivals [Ruby PL/SQL](https://github.com/rsim/ruby-plsql)

Built on top of [cx_Oracle](https://pypi.org/project/cx-Oracle/)

Implementation at [GitHub](https://github.com/LatvianPython/python_plsql)

<br>
I do not use Ruby, and I have too much time on my hands, so I created a library in Python
so I would not have to write 5 lines of cx_Oracle code just to use a PL/SQL function. 

<br>
Usually, if you want to call a function using cx_Oracle, you have to open a new cursor, prepare the parameters 
which can be tedious especially if the parameters are record or table types then call the function and translate
the data into Python types.

<br>
Instead of doing all of that, you can just call the PL/SQL function as if it were a simple Python function, see:

{% highlight python %}
>>>plsql.sum_ints(pt_ints=[1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
55
{% endhighlight %}

I do not even want to type out how the same example would work from cx_Oracle, it is that tedious when compared
to the alternative where the call is handled by the library.

<br>
For procedures where you do not have any parameters, it is not worth it to use this library.

<br>
There are also a few issues with it, as there are many cases with complex types where you have memory access violation
errors, these occur in the Oracle Call Interface, or just somewhere in cx_Oracle. 

<br>
I have used this library to mediocre effect, by writing a test suite for a feature I implemented.

<br>
Having said all of that, I can not recommend using this anywhere, due to all of the memory access violations
I am not sure how far you can push this library, but since the memory access violations happen when calling cx_Oracle
code I have to assume that you will not be able to use cx_Oracle if you have complex types used in function parameters.

<br>
I would want to someday rewrite the library again, having already done that multiple times, perhaps the next time
will solve all of the problems.
