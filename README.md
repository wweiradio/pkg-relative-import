# pkg-relative-import
explain why "Attempted relative import beyond toplevel package" is fired

## relative import

Relative import is introduced in [[https://docs.python.org/2/tutorial/modules.html#packages| python packages]].

<code>
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
</code>

The explanation in the document failed to mention that it depends on one condition, the interpreter is invoked from outside of the toplevel packages.

If the interperter is invoked from, for example, echo.py, which tries to use formats/wavread.py, the original relative import
<code>
        #in echo.py
        from ..formats import wavread
</code>
will not work. And it will fire 'ValueError: Attempted relative import in non-package'.

This indicates that :
 * relative import applys only in package
 * whether it is a package depends on where to launch the command. The directory containing the launching python file is not treated as package anymore

from above explanation, you can see why [[http://stackoverflow.com/questions/1918539/can-anyone-explain-pythons-relative-imports# | relative import does not work]]


