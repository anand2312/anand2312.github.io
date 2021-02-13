Often while using dictionaries in Python, you may come across an error which looks something like this:
```py
001 | Traceback (most recent call last):
002 |   File "<string>", line 2, in <module>
003 | KeyError: 3
```
This error is raised when you try to access a key that _isn't present_ in your dictionary.

## How to handle the KeyError.
### Option 1: [dict.get](https://docs.python.org/3/library/stdtypes.html#dict.get)
The `dict.get` method will return the value for the key, or `None` (or a default value that you specify) if the key doesn't exist, and hence will _never raise_ a KeyError.
```py
>>> my_dict = {"foo": 1, "bar": 2}
>>> print(my_dict.get("foo"))
1
>>> print(my_dict.get("foobar"))
None
>>> print(my_dict.get("foobar", 3))    # here 3 is the default value to be returned, in case the key doesn't exist
3
>>> print(my_dict)
{'foo': 1, 'bar': 2}
```
### Option 2: [dict.setdefault](https://docs.python.org/3/library/stdtypes.html#dict.setdefault)
The `dict.setdefault` method is similar to the `dict.get`, but it will also _insert_ the new key with the default value into the dictionary.
```py
>>> print(my_dict.setdefault("foobar", 3))
None
>>> print(my_dict)
{'foo': 1, 'bar': 2, 'foobar': 3}
```
### Option 3: [collections.defaultdict](https://docs.python.org/3/library/collections.html#collections.defaultdict)
The Python `defaultdict` type behaves almost exactly like a regular Python dictionary, but if you try to access or modify a missing key, then `defaultdict` will automatically insert the key and generate a default value for it.
While instantiating a `defaultdict`, we pass in a function that tells it how to create a default value for missing keys.
```py
>>> from collections import defaultdict
>>> my_dict = defaultdict(int, {"foo": 1, "bar": 2})
>>> print(my_dict)
defaultdict(<class 'int'>, {'foo': 1, 'bar': 2})
```
In this example, we've used `int`, which when used like a function, returns 0 - this means that if we try to access a non-existent key, it provides the default value of 0.
```py
>>> print(my_dict["foobar"])
0
>>> print(my_dict)
defaultdict(<class 'int'>, {'foo': 1, 'bar': 2, 'foobar': 0})
```
### Option 4: Using a [try](https://docs.python.org/3/reference/compound_stmts.html#try) and `except` block
Of course, you can also just use a simple try and except block to handle this error too!
```py
>>> my_dict = {"foo": 1, "bar": 2}
>>> try:
...   print(my_dict["foobar"])
... except KeyError:
...   # handle it the way you want
```





