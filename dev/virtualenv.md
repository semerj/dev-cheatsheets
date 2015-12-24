# [Virtualenv](http://virtualenvwrapper.readthedocs.org/en/latest/command_ref.html#managing-environments)

_note: anaconda python does not work with virtualenv/virutalenvwrapper. use `conda` instead._

```bash
$ type -a python
python is ~/anaconda/bin/python
python is /usr/bin/python

$ type -a python3
python3 is /usr/local/bin/python3
```

### config file
add to `~/.zshrc` file:
```bash
export WORKON_HOME=$HOME/.virtualenvs         # virtualenvs location
export PROJECT_HOME=$HOME/Envs                # project working directories location
export VIRTUALENVWRAPPER_PYTHON=              # full path of interpreter to use
export VIRTUALENV_PYTHON=                     # overrides interpreter above
export VIRTUALENVWRAPPER_VIRTUALENV=          # full path of virtualenv binary to use
source /usr/local/bin/virtualenvwrapper.sh            
```

or specify `VIRTUALENV_PYTHON` with python directory before `mkproject`/`mkvirtualenv`

### example
```bash
$ export VIRTUALENV_PYTHON=/usr/local/bin/python3

$ mkproject py3

$ python -V
Python 3.3.4
(py2)
```

### mkvirtualenv/mkproject
creates an environment inside _~/.virtualenvs_
```bash
$ mkvirtualenv venv

# or
$ mkvirtualenv --python=/usr/local/bin/python3 venv
```

creates an environment inside _~/.virtualenvs_ and a project directory inside _~/Envs_
```bash
$ mkproject py33
```

### install ipython
```
$ pip install "ipython[all]"
```

### workon 
shows current environments
```bash
$ workon
py33
venv
``` 

### deactivate
switches out of current environment
```bash
$ deactivate
```

### rmvirtualenv
removes project/environment
```bash
$ rmvirtualenv py33
```

### mktmpenv
creates temporary environment in `WORKON_HOME` directory. deleted when deactivated.
```bash
mktmpenv
```

### requirements
after installing modules, create _requirements.txt_
```bash
$ pip install Flask requests
$ pip freeze > requirements.txt
$ cat requirements.txt
Flask==0.8
requests==0.11.1

# use requirements.txt modules for new virtualenv
$ pip install -r requirements.txt
```
_postmkvirtualenv_ is run when a new environment is created, letting you automatically install commonly-used tools.
