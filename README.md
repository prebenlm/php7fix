# About
This script will take a single file as input and change the file so any occurrences of $this->$var() is replaced with $this->{var}() for PHP 7 compatibility.
The purpose of running this script will be to replace all such occurrences if your php-files have a lot of them and you are required to update the code to be compatible with PHP 7.

If you don't know what you're doing or are unsure about any of the expressions used here, then don't run it.

You are responsible for adding and committing any changes, this script will not do so. 

I take no responsibility of anything breaking as a result of you running this.

This script assumes you're working with your code in a git-repo.


## Requirements
Requires an up to date version of sed. I've used: sed (GNU sed) 4.2.2

To find the version you have, you can run: sed --version


## Basic usage
Available options are: -p as in "print" and -t as in "test". Options must come before the filename.

### Simple examples (one file)
All of the follwing examples will run on a single file named test.txt

The file will be changed and no output given

```sh fixmethod test.txt```


A git diff report will be printed for the file, but the file will be reset to it's original state (With git checkout)

```sh fixmethod -pt test.txt```


The file will be changed and a git diff report will be printed for the file

```sh fixmethod -p test.txt```


### Advanced examples (multiple files)

Both these examples are run with the -tp option, meaning output will be printed and the files will be reset to their original state after running.

Removing `> git.diff` at the end will have the output returned to your terminal instead of being written to a file named git.diff in your folder.

Will run on all php-files in the folder and any subdirectories of it. 

```find . -name '*.php' -exec sh fixmethod -tp "{}" \; > git.diff```


Will run on all .php-files in the folder and any subdirectories of it, EXCEPT folders ./cache and ./hooks

```find . -type d \( -path ./cache -o -path ./hooks \) -prune -o -name '*.php' -exec sh fixmethod -tp "{}" \; > git.diff```


#### Please note
The git diff will be printed for each file the command is run on regardless of whether there were any changes made by the script or not.

Also note that you may experience that running the command may change line endings or even cause git to pick up on line ending changes for files, even after git checkout on the file, if you have a .gitattributes file with options that causes this (or global options) like crlf=input

Running this may also inadvertedly change file permissions for files.
