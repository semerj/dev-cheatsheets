# IPython / Jupyter

## Console

```python
<object>?                         # Information about the object
<object>.<TAB>                    # tab completion

# run scripts / profile / debug
%run myscript.py

%timeit range(1000)               # measure runtime of statement
%run -t  myscript.py              # measure script execution time

%prun <statement>                 # run statement with profiler
%prun -s <key> <statement>        # sort by key, e.g. "cumulative" or "calls"
%run -p  myfile.py                # profile script

%run -d myscript.py               # run script in debug mode
%debug                            # jumps to the debugger after an exception
%pdb                              # run debugger automatically on exception
```

Examine history
```python
%history
%history ~1/1-5  # lines 1-5 of last session
```

Run shell commands
```python
!make  # prefix command with "!"
```

Clean namespace
```python
%reset
```

Run code from clipboard
```python
%paste
```

## Debugger

```python
n                   # execute next line
b 42                # set breakpoint in the main file at line 42
b myfile.py:42      # set breakpoint in 'myfile.py' at line 42
c                   # continue execution
l                   # show current position in the code
p data              # print the 'data' variable
pp data             # pretty print the 'data' variable
s                   # step into subroutine
a                   # print arguments that a function received
pp locals()         # show all variables in local scope
pp globals()        # show all variables in global scope
```

## Command line

```sh
ipython --pdb -- myscript.py argument1 --option1  # debug after exception
ipython -i -- myscript.py argument1 --option1     # console after finish
```

## Shortcuts

* `CMD + /`: comment out highlighted rows

## Extensions

Autoreload
```python
In [1]: %load_ext autoreload

In [2]: %autoreload 2
```

## Convert notebook to PDF

`.tplx` file used to suppress numbered titles
```sh
ipython nbconvert --to=latex --post=PDF --template=secnum.tplx input.ipynb
```

`secnum.tplx`
```
((* extends 'article.tplx' *))

((* block commands *))
\setcounter{secnumdepth}{0} % Turns off numbering for sections
((( super() )))
((* endblock commands *))
```
