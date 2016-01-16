# pkg-relative-import
==============================
explain why "Attempted relative import beyond toplevel package" is fired

## relative import
==============================
Relative import is introduced in [python packages].(https://docs.python.org/2/tutorial/modules.html#packages).

```
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

The explanation in the document failed to mention that it depends on one condition, the interpreter is invoked from outside of the toplevel packages.

If the interperter is invoked from, for example, echo.py, which tries to use formats/wavread.py, the original relative import
```
        #in echo.py
        from ..formats import wavread
```
will not work. And it will fire 'ValueError: Attempted relative import in non-package'.

This indicates that :
 * relative import applys only in package
 * whether it is a package depends on where to launch the command. The directory containing the launching python file is not treated as package anymore

from above explanation, you can see why the [relative import does not work](http://stackoverflow.com/questions/1918539/can-anyone-explain-pythons-relative-imports#)


## start.py
==============================

If you run start.py in the root directory, no errors found.
```
D:\Python27\python.exe /src/python/pkg-relative-import/start.py
in parent
in top level parent
in sub2 relative2
run relative after parent

Process finished with exit code 0

```

But if you run start.py in pkg/start.py, the ValueError fires.

```
D:\Python27\python.exe E:/kka/src/python/pkg-relative-import/pkg/start.py
Traceback (most recent call last):
  File "E:/kka/src/python/pkg-relative-import/pkg/start.py", line 5, in <module>
    import sub.relative
  File "E:\kka\src\python\pkg-relative-import\pkg\sub\relative.py", line 6, in <module>
    from .. import parent
ValueError: Attempted relative import beyond toplevel package

```

This is because when executing relative.py, from .. import parent, the directory pkg is no longer treated as a package. Therefore, sub is the topmost package. .. is beyond toplevel package.

Hope this readme explain this confusing issue.


