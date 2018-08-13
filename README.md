Recursive Decorator
==========================
[![Build Status](https://travis-ci.org/yakobu/recursive_decorator.svg?branch=master)](https://travis-ci.org/yakobu/recursive_decorator)
[![Coverage Status](https://coveralls.io/repos/github/yakobu/recursive_decorator/badge.svg?branch=master)](https://coveralls.io/github/yakobu/recursive_decorator?branch=master)

Decorator to transform function calls alone the stack.

What is ``recursive_decorator``?
----------------------------

``recursive_decorator`` is a decorator that allows us to transform function 
and **sub function's call** at runtime, motivated by the need to modify all calls along the stack.

* Functions will not be replaced, new instances will be returned.
* Function cannot be wrapped more then once with same transformer.
* Methods can be wrapped as well. 

Examples:
---------

Print Stack Calls
------------------

```python
   >>> from recursive_decorator import recursive_decorator 
   
   >>> def print_function_name_transformer(f):
   ...:    def transformed_func(*args, **kwargs):
   ...:        print(f.__name__)
   ...:        return f(*args, **kwargs)
   ...:    return transformed_func
   
   
   >>> def third():
   ...:    pass

   >>> def second():
   ...:    third()

   >>>  @recursive_decorator(print_function_name_transformer)
   ...: def first():
   ...:     second()
   
   >>> first()
    first
    second
    third
```

Wrap with Try Except
----------------------

```python
   >>> import sys
   >>> import ipdb

   >>> from recursive_decorator import recursive_decorator

   >>> def wrap_function_with_try_except(f):
   ...:    def transformed_func(*args, **kwargs):
   ...:        try:
   ...:            return f(*args, **kwargs)
   ...:        except:
   ...:            ipdb.set_trace(sys._getframe().f_back)
   ...:    return transformed_func


   >>> def throws_exception():
   ...:    raise Exception


   >>> @recursive_decorator(wrap_function_with_try_except)
   >>> def function():
   ...:    throws_exception()
   ...:    print("steal will be called after continue!!!")
 
    >>> function()
     21     throws_exception()
---> 22     print("steal will be called after continue!!!")
     23 


   
   ```
   
   
