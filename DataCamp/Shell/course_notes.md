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
  The command wc (short for "word count") prints the number of characters, words, and lines in a file. You can make it print only one of these using -c, -w, or -l respectively.
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
   ```shell