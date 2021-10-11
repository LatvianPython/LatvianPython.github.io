---
layout: post
quote: JS Safe 1.0 implemented in Python. 🐍
syntax_highlighting: true
---

JS Safe 1.0 is a CTF challenge created for 
[Google CTF 2018 Beginners Quest](https://gctf-2018.appspot.com/#beginners/)

If you wish to crack the JS version: 
[JS Safe 1.0](https://storage.googleapis.com/gctf-2018-attachments/7a50da3856dc766fc167a3a9395e86bdcecabefc1f67c53f0b5d4a660f17cd50)

Python version: [Python Safe 1.0](https://github.com/LatvianPython/Python-Safe-1.0/blob/master/encrypted_data.py)

<br>
## If you do not wish to be spoiled on how to solve it, do not continue reading.


<br>
I will not be going into details on how to crack the safe as the existing write-ups 
for [JS Safe 1.0](https://ericgrandt.com/blog/google-ctf-jssafe/) 
should give enough hints that can be used to break the Python implementation
of the safe

<br>
Reimplementing the safe into Python was my idea of a birthday present for a friend of mine,
with my own version of the safe, I was able to add my own secret information with a new password
into the safe (the actual gift).

<br>
It took a solid evening of hacking to understand the inner workings of the JS Safe enough 
to be able to create the same challenge for Python. The hardest part was writing 
the script that would create the obfuscated code that will check the
provided password against the stored hash.

<br>
The safe works by executing a series of obfuscated steps. In the end, what happens is that
the user-provided password is compared against a password hash and in the case of a match,
the secret information is decrypted.

<br>
The main function of the Python Safe is almost the same as the JS version, the only real difference
is the use of ``eval`` instead of ``Function.constructor.apply.apply(x, y)`` 

{% highlight python %}
def check_pw(password):
    code = "--snip--"

    env = {
        "a": lambda x, y: x[y],
        "b": lambda x, _: eval(x),
        "c": lambda x, y: x + y,
        "d": lambda x, _: chr(x),
        "e": 0,
        "f": 1,
        "g": password,
        "h": 0,
    }

    for lhs, fn, arg1, arg2 in list(map("".join, zip(*[iter(code)] * 4))):
        env[lhs] = env[fn](env[arg1], env[arg2])

    return not env["h"]
{% endhighlight %}


What follows are all the high-level steps run by the safe:

1. add integers to `env` (`2` to `32`, `64`, `128`, `192`, and `224`)
2. add password hash bytes uses integers from 1. to construct intermediate integers and final hash bytes
3. construct string versions of lambda functions, and add lambdas to env via `eval` lambda already 
   in the `env`
4. construct and execute import statements
5. hash the user-provided password using the created lambda functions
6. perform a constant-time comparison on the provided password hash with the stored hash
7. null out `env` values, while leaving one in to serve as a hint


<br>
These are all the string constants that will be used to create the lambda functions
and to import all the necessary libraries:

{% highlight python %}
IMPORT_HASHLIB = "import hashlib"
IMPORT_STRUCT = "import struct"
SINGLE_PARAM_LAMBDA = "lambda x,*_:"
MULTI_PARAM_LAMBDA = "lambda x,y:"
LAMBDA_EXEC = "exec(x, globals())"
LAMBDA_XOR = "x[0]^x[1]"
LAMBDA_OR = "x[0]|x[1]"
LAMBDA_HASH = "hashlib.sha256(x.encode('utf-8')).digest()"
LAMBDA_UNPACK = "struct.unpack('32B',x)"
LAMBDA_TUPLE = "(x,y)"
{% endhighlight %}

During the safe runtime, these lambdas will be created and used:

{% highlight python %}
lambda x,*_:exec(x, globals())
lambda x,*_:x[0]^x[1]
lambda x,*_:x[0]|x[1]
lambda x,*_:hashlib.sha256(x.encode('utf-8')).digest()
lambda x,*_:struct.unpack('32B',x)
lambda x,y:(x,y)
{% endhighlight %}

The constant-time comparison uses the tuple generation, XOR, and OR lambdas to check 
if the hashes match. If they do match, you have found the correct password, and the safe will open.

<br>
The script that creates the obfuscated code can be used to create other scripts 
where you may want to obfuscate what exactly is being run. 
Likely just for the creation of other CTF challenges.