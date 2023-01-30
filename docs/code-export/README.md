# D2IT2 Code Export

We assume that the analyst will be working in a jupyter notebook, or
probably more specifically, a [jupyter-lite][planetshine] notebook.

The challenge here is how do we allow the user to explore data and write
analysis in such a way that it can also be exported to an "app" without
too much manual modification.

Some tidying is not a bad thing, but also we don't want to force analysts to
have to learn a lot of arbitrary structure just to be able to share an
analysis method.

We know that things like modules and classes are relatively advanced
concepts for this audience, and if required we only want them to be used in
a "follow these three steps" approach and not requiring a full understanding
of `__init__` etc. In fact, any understanding of [dunder-methods][dunder]
is probably too much, as is the difference between
[function and method][func-v-method].

## Turning notebooks into code

This is probably the simplest part of the task as it is very well supported.

The simplest approach is to use nbconvert to parse and extract the code blocks
from a notebook.

```
import nbformat
from pathlib import Path

notebook = Path("Untitled.ipynb").read_text()

notebook_code = nbformat.reads(notebook, as_version=4)
notebook_code.cells[0]  # Reads the first cell
```

**QUESTION:** I'm currently not aware of a way of getting the filename for the current notebook, or a way to access the cells directly.

It's possible to get hold of the ipython context with:

```
ip = get_ipython()
```

as well as getting access to inputs `_ih` and outputs `_oh` as well as the
'runtime' outputs, e.g. `_5`, but don't know if you can access the actual notebook.

See [read-notebook.ipynb](./read-notebook.ipynb).


## Accessing code by reference

Of course, Python does allow several different ways of accessing code directly.


### Inspect
One of the simplest is to use `inspect`:

```
import inspect

class foo:
      def bar():
          print 'Hello'

print(inspect.getsource(foo))
```

See [inspect.ipynb](./inspect.ipynb).

The issue with this however is that any imports or other globals referenced from
within the function is not written correctly.


### Pickle

Python functions are first class objects, and so can be pickled just
like any other native python code. However, pickling functions is fraught with
both version compatibility and security issues, so we won't consider this approach
for this project.

### Dill

The [dill][dill] library is an enhanced version of pickle which can serialize
more 'exotic' (the term itself uses) types but does suffer from some of the same
issues as native pickle when pickling functions. However, it does also includes
some very useful tools that can be combined to to create a more standard export:

``` python
def write_code(func):
    for name, mod in dill.detect.globalvars(func).items():
        print(dill.source.importable(mod, alias=name))
    print(dill.source.getsource(func))
```

This is relatively robust, but there are several cases that won't work as expected,
most notably anything that references signficant global scoped variables. Try
for example to serialize a function that uses a globally imported random.





[planetshine]: https://jupyterlite.readthedocs.io/en/latest/
[dunder]: https://www.geeksforgeeks.org/dunder-magic-methods-python/
[func-v-method]: https://www.geeksforgeeks.org/difference-method-function-python/
[dill]: https://github.com/uqfoundation/dill
