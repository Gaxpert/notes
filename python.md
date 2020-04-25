---
title: Python
date: 2020-04-03 20:38:13+01:00
author: Gaxpert
---

# Python

## VirtualEnvWarpper
start env
`mkvirtualenv test`
Leave env
`deactivate`
Activate env
`workon test1`
Remove env
`rmvirtualenv test`

### Jupyter
List kernels
`jupyter kernelspec list`
Remove kernel
`jupyter kernelspec remove`

## Tips and tricks
### List vs set
Use **list** to preserve order, **set** otherwise
### Lists
Filtering sequence elements
```python
mylist = [1, 4, -5, 10, -7, 2, 3, -1]
[n for n in mylist if n > 0]
```

One downside is that it might produce a large result if the input is large. We can use a generator
```python
pos = (n for n in mylist if n > 0)

for x in pos:
	print(x)
```

If we can't use any method, for example if we need to use exceptions, we can use **filter**
```python
values = ['1', '2', '-3', '-', '4', 'N/A', '5']
def is_int(val):
	try:
		x = int(val)
		return True
	except ValueError:
		return False

ivals = list(filter(is_int, values))
print(ivals)
# Outputs ['1', '2', '-3', '4', '5']
```

### List vs generators
Generators don't load all the data into memory

### Comprehension
List comprehension
```python
[x*x for x in [1,2,3,4,5]]
```
Generator comprehension (Parenthesis instead of brackets)
```python
(x*x for x in [1,2,3,4,5])
```

Basic list comprehension
```python
list = [n for n in VAR]
list = [n for n in [1,2,3,4,5]]
```

If comprehension
```python
list = [n for n in VAR if n > 2]
```

Nested for loop. Instead of
```python
my_list = []
for letter in 'abcd':
	for num in range(4):
		my_list.append((letter, num))
```
With list comprehension. Note: letter and num must match the ones on **for**
```python
my_list = [(letter, num) for letter in 'abcd' for num in range(4)]
```

#### Zip
Zip creates list of tuples based on the same index
```python
words = ['a', 'b', 'c']
numbers = ['1', '2', '3']
my_dict = {word: number for word, number in zip (words, numbers)}
```


### Dictionaries
Find common elements in dictionaries
```python
a = {
	'x' : 1,
	'y' : 2,
	'z' : 3
}
b = {
	'w' : 10,
	'x' : 11,
	'y' : 2
}
# Find keys in common
a.keys() & b.keys()
 # { 'x', 'y' }
# Find keys in a that are not in b
a.keys() - b.keys()
 # { 'z' }
# Find (key,value) pairs in common
a.items() & b.items() # { ('y', 2) }
```

Sort by key
```python
from operator import itemgetter
rows_by_fname = sorted(rows, key=itemgetter('fname'))
rows_by_uid = sorted(rows, key=itemgetter('uid'))
```

Extract subset of another dictionary
```python
prices = {
	'ACME': 45.23,
	'AAPL': 612.78,
	'IBM': 205.55,
	'HPQ': 37.20,
	'FB': 10.75
}

# Make a dictionary of all prices over 200
p1 = { key:value for key, value in prices.items() if value > 200 }

# Make a dictionary of tech stocks
tech_names = { 'AAPL', 'IBM', 'HPQ', 'MSFT' }
p2 = { key:value for key,value in prices.items() if key in tech_names }
```



### Unpacking
```python
p = (4,5)
x, y = p
```

If we want to throw away some variable
```python
p = (3,4,5)
x, _, y = p
```

Unpack N elements
```python
record = ('Dave', 'dave@example.com', '91923213', '92323193')
name, email, *phone_numbers = record
```

Combine with splitting 
```python
line = 'nobody:*:-2:-2:Unprivileged User:/var/empty:/usr/bin/false'
uname, *fields, homedir, sh = line.split(':')
```

Unpack multiple throw away variables (Star unpacking)
```python
record = ('ACME', 50, 123.45, (12, 18, 2012))
name, *_, (*_, year) = record
```

Split list into head and tail
```python
items = [1, 10, 7, 4, 5, 9]
head, *tail = items
```
`print(head)`
> 1
`print(tail)`
> [10, 7, 4, 5, 9]

### Keeping last N items
Keeping a limited history is the perfect use for a collections.deque
```python
from collections import deque

def search(lines, pattern, history=5):
	previous_lines = deque(maxlen=history)
	for line in lines:
		if pattern in line:
			yield line, previous_lines
		previous_lines.append(line)
# Example use on a file
if __name__ == '__main__':
	with open('somefile.txt') as f:
				print(pline, end='')
				print(line, end='')
				print('-'*20)
```

### Finding largest or smallest N items
heapq has two functions - nlargest() and nsmallest().
These functions are only appropiate when were trying to find a small number of items. In case we only want the smallest or largest it is fast to use min() and max()
```python
import heapq

nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
print(heapq.nlargest(3, nums)) # Prints [42, 37, 23]
print(heapq.nsmallest(3, nums)) # Prints [-4, 1, 2]
```

It also accepts a key parameter for complicated data structures
```python
portfolio =[
	{'name':'IBM', 'shares': 100, 'price': 91.1},
	{'name': 'AAPL', 'shares': 50, 'price': 543.22},
	{'name': 'FB', 'shares': 200, 'price': 21.09},
	{'name': 'HPQ', 'shares': 35, 'price': 31.75},
	{'name': 'YHOO', 'shares': 45, 'price': 16.35},
	{'name': 'ACME', 'shares': 75, 'price': 115.65}
]

cheap = heapq.nsmallest(3, portfolio, key=lambda s: s['price'])
expensive = heapq.nlargest(3, portfolio, key=lambda s: s['price'])
```

#### Naming slices

Instead of 
```python
record = '....................100 .......513.25 ..........'
cost = int(record[20:32]) * float(record[40:48])
```
Do
```python
SHARES = slice(20,32)
PRICE = slice(40,48)
cost = int(record[SHARES]) * float(record[PRICE])
```

## String manipulation

Need to split string into fields, but the delimiters are not consistent. With **re** we can specify multiple separators
```python
line = 'asdf fjdk; afed, fjek,asdf, foo'
import re
#Split by  ;  ,    whitespace (the \s)
re.split(r'[;,\s]\s*', line)
```

Matching string at start or end
```python
filename = 'spam.txt'
filename.endswith('.txt') #true
filename.startsswith('file:') #false
```
To match against multiple choices (requires tupple as input)
```python
filename = os.listdir('.')
[name for name in filenames if name.endswith(('.c', '.h'))]
```

### Match strings the Unix way
fnmatch can be used to match with wildcard patterns. Provides two functions **fnmatch()** and **fnmatchcase()**
```python
from fnmatch import fnmatch, fnmatchcase
fnmatch('foo.txt', '*.txt') #true
fnmatch('foo.txt', '?oo.txt') #true

names = ['Dat1.csv', 'Dat2.csv', 'config.ini', 'foo.py']
[name for name in names if fnmatch(name, 'Dat*.csv')]
```
fnmatch matches the patterns using the same case-sensitivity rules as the underlying OS. To match exactly as supplied use fnmatchcase






















##Libs 

itqdm
 Progress bar / tqdm:
from tqdm import tqdm
for files in tqdm(files):
	#do something with files
