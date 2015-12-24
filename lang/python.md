# Python
### Read text
```python
# all at once
text = open('textfile', 'r').read()
text = open('textfile', 'r').readlines()

# first line only
f = open('textfile', 'r')
print file.readline()

# line by line
f = open('textfile', 'r')
for line in f:
    print line

with open('textfile', 'r') as f:
    for line in f:
        print line
```
### Write text
```python
f = open('textfile', 'w')
for line in lines:
    f.write(line)
```
### Read CSV
```python
import csv

# read everything
csv_file = []
with open('file.csv', 'rb') as f:
    reader = csv.reader(f)
    for line in reader:
        csv_file.append(line)

# read specific columns
csv_file = []
with open('file.csv', 'rb') as f:
    reader = csv.DictReader(f)
    for line in reader:
        csv_file.append([line['col1'], line['col2']])

# read first line
first_row = csv.reader(open('file.csv', 'r')).next()
```
### Write CSV
```python
import csv

with open('file.csv', 'wb') as f:
    writer = csv.writer(f)
    writer.writerows(iterable)

# using field names
list_of_dicts = [{'first': 'a', 'second': 1},
                 {'first': 'b', 'second': 2}]

with open('file.csv', 'wb') as f:
    fieldnames = ['second']
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    for line in list_of_dicts:
        line.writerow(line)
```
### Directory
```python
import os

os.getcwd()           # get working directory
os.chdir(path)        # change working directory
os.listdir('.')       # list files in current directory
```
### Idioms
dict comprehensions for counting frequencies in a list
```python
mylist = [0, 1, 0, 1, 1, 0, 0, 1, 0]
d = {x: mylist.count(x) for x in mylist}
>>> {0: 5, 1: 4}
```
fatten list of lists
```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [x for row in matrix for x in row]
>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]

# python3
from itertools import chain
flat = list(chain.from_iterable(map(lambda x: x, matrix)))
>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]

flat = list(chain(*[x for x in mat]))
>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
