---
layout: page
title: The Truth of Two-Tuple Tuples
permalink: /the-truth-of-two-tuple-tuples/
---

```python
if ((),()):
  # if a tuple of tuples is True

  # are two tuples of tuples true too?
  if ((),()) and ((),()):
    # if the truth of two tuples of tuples are True
    
    if all(tuple(((),()) for _ in range(22))):
      # then twenty-two also are True
      truth_of_twenty_two_tuples_of_tuples = """
      If a tuple of tuples is True,
      are two tuples of tuples True too?
      Twenty-two two-tuple tuples are True,
      if two tuples of tuples are True
      """
      print(truth_of_twenty_two_tuples_of_tuples)
```
