## Useful commands

### How can I look at the start of a file?
  ```shell
  head - n # <filename>
  ```
## Use tail to display all but the first 15 lines of file?  
  ```shell
  tail  -n +15 <filename>
  ```  
### How can I view a file's contents piece by piece?
  ```shell
  less <filename1> <filename2>
  ```
  then use **:n** to move to next file (filename2) and **:p** to go back to previous one (filename1) 

### How can I list everything below a directory?
  ```shell
  ls -R -F <path>
  ```
  This shows every file and directory in the current level, then everything in each sub-directory, and so on.

### How can I select columns from a file?
  head and tail let you select rows from a text file. If you want to select columns, you can use the command cut.
  ```shell
  cut -f 2-5,8 -d , <filename>
  ```
  which means "select columns 2 through 5 and columns 8, using comma as the separator". cut uses -f (meaning "fields") to specify columns and -d (meaning "delimiter") to specify the separator. *You need to specify the latter because some files may use spaces, tabs, or colons to separate columns.*

### How can I select lines containing particular values?
  grep selects lines according to what they contain. grep's more common flags:

* -c: print a count of matching lines rather than the lines themselves
* -h: do not print the names of files when searching multiple files
* -i: ignore case (e.g., treat "Regression" and "regression" as matches)
* -l: print the names of files that contain matches, not the matches
* -n: print line numbers for matching lines
* -v: invert the match, i.e., only show lines that don't match
  ```shell
  grep -c <wordToSearch> <filename1> <filename2>
  ```
Count how many lines contain the wordToSearch in the filename1 and filename2 files. 

### How can I merge two files line by line?
  ```shell
  paste -d , seasonal/autumn.csv seasonal/winter.csv
  ```
combine the autumn and winter data files in a single table using a comma as a separator

### How can I combine many commands?
  This is a pipeline that uses cut, grep, and head in that order to select the first value in column 2 of seasonal/autumn.csv after the header "Tooth".
  ```shell
  cut -f 2 -d , seasonal/autumn.csv | grep -v "Tooth" | head -n 1
  ```
## How can I count the records in a file?
  The command wc (short for "word count") prints the number of characters, words, and lines in a file. 
  You can make it print only one of these using -c, -w, or -l respectively.
  * wc -c  prints the number of characters
  * wc -w  prints the number of words
  * wc -l  prints the number of lines
  E.G. Use grep and wc in a pipe to count how many records there are in seasonal/spring.csv from July 2017
  ```shell
  grep "2017-07" seasonal/spring.csv | wc -l
  ```
## What other wildcards can I use?
The shell has other wildcards as well (not only *), though they are less commonly used:
* ? matches a single character, so 201?.txt will match 2017.txt or 2018.txt, but not 2017-01.txt.
* [...] matches any one of the characters (not entire words!) inside the square brackets, so 201[78].txt matches 2017.txt or 2018.txt, but not 2016.txt.
* {...} matches any of the command-separated patterns inside the curly brackets, so {*.txt, *.csv} matches any file whose name ends with .txt or .csv, but not files whose names end with .pdf.
Which expression would match singh.pdf and johel.txt but not sandhu.pdf or sandhu.txt?
  ```shell
  {singh.pdf, j*.txt}
  ```

## How can I sort lines of text?
By default *sort* use to sort in ascending alphabetical order, but the flags * -n* and *-r* can be used to sort *numerically* and *reverse* the order of its output, while *-b* tells it to *ignore leading blanks* and *-f* tells it to *fold case* (i.e., be case-insensitive).
This example shows a pipeline to sort the names of the teeth in seasonal/winter.csv in descending alphabetical order without including the header "Tooth".
  ```shell
   cut -f 2 -d , seasonal/winter.csv | grep -v "Tooth"  | sort -r
  ```

## How can I remove duplicate lines?
**uniq** is used to remove *adjacent* duplicated lines because uniq is built to work with very large files. In order to remove non-adjacent lines from a file, it would have to keep the whole file in memory (or at least, all the unique lines seen so far). By only removing adjacent duplicates, it only has to keep the most recent unique line in memory.
* uniq -c display unique lines with a count of how often each occurs.
  ```shell
  cut -f 2 -d , seasonal/*.csv | grep -v "Tooth" | sort |  uniq -c
  ```
this pipeline: get the second column from all of the data files in seasonal, remove the word "Tooth" from the output so that only tooth names are displayed, sort the output so that all occurrences of a particular tooth name are adjacent; and
display each tooth name once along with a count of how often it occurs.


## How does the shell store information?
Environment variables' names are conventionally written in upper case and are available all the time.
To get a complete list (which is quite long), you can type set in the shell.
To get the value of a variable called X, you must write $X.
  ```shell
  echo $OSTYPE
  ```

## How else does the shell store information?
The other kind of variable is called a shell variable, which is like a local variable in a programming language.
To create a shell variable, you simply assign a value to a name:
  ```shell
  training=seasonal/summer.csv
  head -n 1 $training
  This is the first row of the seasonal/summer.csv
  ```
without any spaces before or after the = sign.

## How can I repeat a command many times?
The loop's parts are:
1. The skeleton `for ...variable... in ...list...; ...body...; done
2. The list of things the loop is to process (separated by space).
3. The variable that keeps track of which thing the loop is currently processing (declared after for).
4. The body of the loop that does the processing.

  ```shell
  for suffix in docx odt pdf; do echo $suffix; done
  docx
  odt
  pdf
  ```
Also notice where the semi-colons go: the first one comes between the list and the keyword do, and the second comes between the body and the keyword done.
  ```shell
  for filename in people/*; do echo $filename; done
  people/agarwal.txt
  ```
## How can I record the names of a set of files?
  ```shell
  for f in $files; do echo $f; done
  seasonal/autumn.csv
  seasonal/spring.csv
  seasonal/summer.csv
  seasonal/winter.csv
  ```
## How can I run many commands in a single loop?
You can write a body as a pipeline of two commands instead of a single command.
We can obtain the same output of
  ```shell
  grep -h 2017-07 seasonal/*.csv
  ```
with a for cycle and a pipeline:
  ```shell
  for file in seasonal/*.csv; do grep 2017-07 $file; done
  2017-07-10,incisor
  2017-07-10,wisdom
  2017-07-20,incisor
  2017-07-21,bicuspid
  2017-07-10,incisor
  2017-07-16,bicuspid
  2017-07-23,bicuspid
  2017-07-25,canine
  2017-07-01,incisor
  2017-07-17,canine
  ```
