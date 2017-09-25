
# NetLogo Python extension

This NetLogo extension allows you to run Python code from NetLogo. It works with both Python 2 and 3, and should work with almost all Python libraries.

## Building

Run `sbt package`.

If compilation succeeds, `py.jar` will be created. This file and `pyext.py` should then be placed in a folder named `py` in your NetLogo `extensions` directory.

## Primitives


### `py:setup`

```NetLogo
py:setup python-executable
```


Create the Python session that this extension will use to execute code. The session will be started with the given Python executable. This command *must* be run before running any other Python extension primitive. Running this command again will shutdown the current Python environment and start a new one.

 The executable may be specified as a relative path, absolute path, or just the executable name if it is on your PATH. For Mac OS and Linux, you will usually just do:

```NetLogo
py:setup "python3"
```

to use Python 3 and

```NetLogo
py:setup "python"
```

to use Python 2.

If you use virtualenv or Conda, simply specify the path of the Python executable in the environment you wish to use:

```NetLogo
py:setup "/path/to/myenv/bin/python
```

The path may be relative or absolute. So, if you have a virtual environment in the same folder as your model, you can do:

```NetLogo
py:setup "myenv/bin/python
```
    


### `py:run`

```NetLogo
py:run python-statement
```


Runs the given Python statements in the current Python session. To make multi-line Python code easier to run, this command will take multiple strings, each of which will be interpreted as a separate line of Python code. For instance:

```NetLogo
(py:run
  "import matplotlib"
  "matplotlib.use('TkAgg')"
  "import numpy as np"
  "import matplotlib.pyplot as plt"
  "for i in range(10):"
  "    plt.plot([ x ** i for x in arange(-1, 1, 0.1) ])"
  "plt.show()"
)
```

`py:run` will wait for the statements to finish running before continuing. Thus, if you have long running Python code, NetLogo will pause while it runs.



### `py:runresult`

```NetLogo
py:runresult python-expression
```


Evaluates the given Python expression and reports the result. `py:runresult` attempts to convert from Python data types to NetLogo data types. Numbers, strings, and booleans convert as you would expect. Any list-like object in Python (that is, anything with a length that you can iterate through) will be converted to a NetLogo list. For instance, Python lists and NumPy arrays will convert to NetLogo lists. Python dicts (and dict-like objects) will convert to a NetLogo list of key-value pairs (where each pair is represented as a list). `None` will be converted to `nobody`. Other objects will simply be converted to a string representation.



### `py:set`

```NetLogo
py:set variable-name value
```


Sets a variable in the Python session with the given name to the given NetLogo value. NetLogo objects will be converted to Python objects as expected. `value` should only be a number, string, boolean, list, or nobody (agents and the like are currently converted to strings).

```NetLogo
py:set "x" [1 2 3]
show py:runresult "x" ;; Shows [1 2 3]
```


