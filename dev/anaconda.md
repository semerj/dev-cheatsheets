# conda

**Note:** `activate` and `deactivate` in `~/anaconda/bin/` should be renamed to `conda-activate` and `conda-deactivate` to avoid issues with `virtualenvwrapper.sh`.

### example
```bash
# creates anaconda environment py33 with python 3.3
$ conda create -n py33 python=3.3 anaconda

# assumes ~/anaconda/bin/activate is the first 'activate' in PATH
$ source activate py33

$ which python
~/anaconda/envs/py33/bin/python
(py33)

$ python -V
Python 3.3.4 :: Anaconda 1.9.2 (x86_64)
(py33)

$ source deactivate

$ conda remove -n py33 --all
```

### create

use the `--yes` and `--quiet` options to automatically answer yes to the confirmation question and hide the progress bar, respectively.

create _myenv_ environment with python 2.7:
```bash
$ conda create --yes -n myenv python=2.7
```

create _myenv_ environment with python 2.7 and scipy:
```bash
$ conda create --yes -n myenv python=2.7 scipy
```

create _myenv_ environment with python 2.7 and scipy version 0.12.0:
```bash
$ conda create --yes -n myenv python=2.7 scipy=0.12.0
```

### install

places a package in an existing environment:
```bash
$ conda install -n myenv pandas

# same as
$ conda install -p ~/anaconda/envs/myenv pandas
```

### activate/deactivate
```bash
$ source activate myenv

$ source deactivate
```

### remove

remove existing environment _myenv_:
```bash
$ conda remove -n myenv --all
```

### info

display information about anaconda environments:
```bash
$ conda info -e
# conda environments:
#
_build                   ~/anaconda/envs/_build
py33                     ~/anaconda/envs/py33
root                  *  ~/anaconda

$ conda info --all
```

### list

show linked packages and their versions in a specific environment:
```bash
$ conda list -n py33

# same as
$ conda list -p ~/anaconda/envs/py33
```
w
