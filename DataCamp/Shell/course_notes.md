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