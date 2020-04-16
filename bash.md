***
title: Bash Tips and Tricks
date: 2020-03-27 19:37:12+00:00
author: Gaxpert
***

# Bash Tips and Tricks

##Scripts



Cut string
`var=${var:5}`

Concatenate all arguments into a string space separated
`str="'$*'"`

Remove substring from string
`echo 'string' | sed 's#\SUBSTRING##g`

Replace char(x) by char(y)
`echo "$string" | tr x y`

Contains substring
``case "$string" in  
  *foo*)  
    Do stuff  
    ;;  
esac```

Replace substring `${var/before/after}`   

For all substring occurrences `${var//before/after}`  ex: `echo ${filesMatch//"/home/z/.notes/"/"  "} 

Loop through all files in dir 
``for file in *.mp3  
do  
 echo $file  
done``

Remove lines from file under 3 length (replace 3)
`sed -r '/^.{,3}$/d' filename`

String to lower case 
` echo "$a" | tr '[:upper:]' '[:lower:]'`

###Special variables

Script name
`$0`

Number of arguments
`$#`

All arguments
`$@`

Process id
`$$`

Exit code
`$?`

###If

####File test

Test if file exists
```bash
if [ -f $filename ]; then
	echo "its a regular file"
else
	echo "not a regular file"
fi
```
Check if its a regular file
`-f $filename`
True if file exists
`-e $filename`
True if file is not empty
`-s $filename`
True if dir
`-d $dir`

###Strings test
True if its argument is empty
`-z `
True if not empty
`-n`

###Numeric comparisons / Arithmetic
|  Operator |    Returns true when first arg...       |     	
| ------------- |-------------  | 
|    -eq    |    equal to          | 
|    -ne    |    Not equal to          |
|    -lt   |   Less than |
|    -gt   |   Greater than |
|    -le   |   Less than or equal to |
|    -ge   |   Greater than or equal to |

###Pattern matching 
Use case
```bash
case $1 in)
	hi | hello)
		echo Matches hi or hello ;;
	what* )
		echo Matches what*  (wildcard expansion) ;;
esac
```





##Terminal

Similar to cat File | grep X --> 
`grep -E "string" file` ex: 
`grep -E "lalt" /usr/share/X11/xkb/rules/base.lst`

Get X line of output 
`Command | sed -n 2p`

