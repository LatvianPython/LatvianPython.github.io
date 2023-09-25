---
layout: post
quote: A Nostalgic Journey Back to Lingo
syntax_highlighting: true
title: Latvian Wordle
---

I have fond memories of playing Lingo during my younger years, the glow of a CRT monitor illuminating my
eager face. Recently, encountering Wordle on the internet sparked a sense of déjà vu, reminding me of
the existence of its predecessor, Lingo.
<br>

Driven by a wave of nostalgia, I felt compelled to revisit Lingo. However, after indulging in the game 
for a while, my curiosity piqued, and I found myself pondering over the possible words used in the game. 
To my knowledge, no list or text file accompanied the game, revealing the words. Nonetheless, I was 
determined to uncover the mystery of the 999 words.
<br>

Initially, I assumed that a simple examination of the binary file would reveal the words in plaintext,
but to my surprise, it was a jumbled array of bytes. This complexity led me to consult my copy of Ghidra 
to decipher the intricate details surrounding the words.
<br>

Through meticulous debugging, I located the subroutine responsible for preparing new words for guessing. 
This discovery provided insights into both the algorithm and the data's location in the binary. Reversing
the algorithm proved to be relatively straightforward, consisting of a few manageable steps. Subsequently,
processing the data through my newly developed function revealed all the words. Remarkably, I managed to 
create a version capable of encoding words, allowing me the option to customize the word list.
<br>

Below is the decoding function I developed during this nostalgic and enlightening journey:


{% highlight python %}
def decode(word):
    divisor = 0x21
    translated = b""
    for _ in range(5):
        word, char = word // divisor, word % divisor
        ascii_value = translation_table[char] - 0x20
        translated += struct.pack("B", ascii_value)

    return translated.decode("cp1257")
{% endhighlight %}

Now, let's take a closer look at the reversed encoding function:

{% highlight python %}
def encode(word):
    encoded = 0
    divisor = 0x21
    for char in reversed(word.encode("cp1257")):
        value = inverted_translation_map[char + 0x20]
        encoded = (encoded * divisor + value) or value

    return encoded
{% endhighlight %}


