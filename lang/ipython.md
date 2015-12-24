# IPython

## Install
```sh
$ pip install "ipython[all]"
```

## Shortcuts
* `CMD + /`: comment out highlighted rows

## Convert notebook to PDF
`.tplx` file used to suppress numbered titles
```bash
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

## r2py IPython magic integration - [API](http://rpy.sourceforge.net/rpy2/doc-2.4/html/interactive.html#module-rpy2.ipython.rmagic)
Enable
```python
In [1]: %load_ext rpy2.ipython
```

Share variables from python to rpy2
```python
In [1]: %Rpush pandas_df

In [2]: %%R
         library(ggplot2)
         ggplot(pandas_df, aes(x=x, y=y)) + geom_point()
```

Share variables from rpy2 to python
```python
In [1]: _ = %R R_df <- data.frame(x=rnorm(10), y=rnorm(10))

In [2]: %Rpull R_df

In [3]: R_df.head()
```
