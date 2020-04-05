---
layout: page
title: The Song of The True Twenty-Two-"Two-Tuples" Tuple
permalink: /the-song-of-the-twenty-two-two-tuple-tuple/
---

For your singing pleasure, I present the following syntactically correct, runnable, Pythonic, tongue-twister and limerick:

```python
# in Python, a non-empty tuple evaluates to true.
# a tuple with two empty tuples counts as non-empty.
if ((),()):
  # so a two-tuple tuple is true.   

  # now, what about two tuples, 
  # each of which is a two-tuple tuple?
  if ((),()) and ((),()):
    # Since one of them is true, 
    # then one "and" another one is also true.
    # so two two-tuple tuples are also true.
    
    # So if we...
    # make a tuple, 
    # and put twenty-two items in it, 
    # and each of those items is a tuple
    # with two tuples in it...
    twenty_two_two_tuple_tuple = tuple(((),()) for _ in range(22))

    # ... and we use the "all" keyword, 
    # to check if every item in the tuple is true....
    if all(twenty_two_two_tuple_tuple):  
      # then we can make a limerick!
      song_of_the_twenty_two_two_tuple_tuple = """
      If a "two-tuples" tuple is True,
      then two "two-tuples" tuples are too.
      So a twenty-two-"two-tuples" tuple is True,
      since a "two-tuples" tuple is True.
      """
      print(song_of_the_twenty_two_two_tuple_tuple)
```

Try [running the program yourself!](https://repl.it/languages/python3) 
