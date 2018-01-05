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

