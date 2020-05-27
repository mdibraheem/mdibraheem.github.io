---
layout:     post
title:      "Surviving Unix: The essential commands"
date:       2015-12-11
summary:    This post is for a younger me looking at the blinking cursor on a black screen in fear.
categories: programming
---

This post is for a younger me looking at the blinking cursor on a black screen in fear.

If you are don’t have much experience in Unix, chances are you are alien to the command line way of doing things. So, why in modern OSes, when we have beautiful graphical interfaces, do we need to use the command line.  The fact of the matter is that, when you get used to the command line, it makes you much more efficient and productive.

So, before we go over any other command, the first command you should know is the man (short for manual) command. It shows the manual pages of any command you want.

~~~ shell	
man ls
# This will show manual page for ls command
~~~

Now that you know the man command, let’s go into the essential commands you need to know. This is my list and I am in no way an expert here; quite the opposite actually.

# Step 1: Learn File Navigation
So the first step is to learn how to navigate through the file system. There are 3 commands and some operators here that you should know.

## pwd (Present Working Directory or Current Working Directory)
This command will display the directory you currently in; imagine the folder you have open in finder or file explorer.

~~~ shell  
pwd
# This command works without any arguments
~~~

## cd (Change Directory)
If you want to move to any other folder in your system, you can use the cd command.

~~~ shell
cd projects
# This will cd into projects folder inside current working directory
~~~

## ls (List Directory Content)

~~~ shell
ls
# This will list contents of current working directory 

ls projects
# This will list contents of projects folder
~~~

## Operators
* tilde ( ~ ) means the home directory
* the single dot ( . ) is used for current directory
* the double dot ( .. ) is used for parent directory, one level higher than the current directory.

~~~ shell
# Imagine our current working directory is ~/downloads/books
 
cd ~
# will change current directory to be home directory
 
cd ./fiction
# is same as cd fiction
# move to fiction folder inside books folder
 
cd ../videos
# will cd to be ~/downloads/videos
# .. will go one up to downloads and then to videos folder
~~~

# Basic File / Folder Operations

## mv (move)
The move command renames or moves a file.

~~~ shell
mv old.txt new.txt
# This will rename old.txt to new.txt
 
mv old.txt new_folder/old.txt
# This will move old.txt to new_folder
 
# If you supply -i flag it will ask you to confirm
# if another file with same name already exists at destination
mv old.txt new.txt -i
~~~

## cp (copy)
The move command copies a file and works in similar fashion to move command.

~~~ shell
cp old.txt new.txt
# This will copy old.txt and paste as new.txt
 
# If you supply -i flag it will ask you to confirm
# if another file with same name already exists at destination
cp old.txt new.txt -i
~~~

## rm (remove)
The rm command deletes files. Be very careful while using the rm command as it has a lot of destructive power. I am not going to share here some of the flags that make rm truly dangerous as this command is very unforgiving when using these tools.

~~~ shell
rm file.txt
# will remove file.txt
rmdir (remove directory)
~~~

The rmdir command deletes directories provided they are empty.

~~~ shell 
rmdir folder
# will remove folder
~~~

## mkdir (make directory)
The mkdir command creates a new directory.

~~~ shell
mkdir new_folder
~~~

# Viewing Text Files

## cat (concatenate)
the cat command prints the contents of a file in stdout. If you give it more than one file, it concatenates and prints all files to stdout.

~~~ shell
cat file1.txt
cat file1.txt file2.txt file3.txt
~~~

## more / less
They both do more or less the same thing with less doing more than more and more doing quite less than less. less was written by a guy who could not take it anymore, with more. Other than starting a pun fight, their main job is to pagify long text files. Less is more advanced so just stick with less.

## head / tail
They both help to see the ends of a long text document. I use tail very commonly to see the end of a log file. head is used to see the top lines of a file.

# Basic Utilities

## wc (word count)
The wc command can count the number of lines, words or bytes. By default, it will give information about all three but you can specify flags (-l for lines, -w for words and -c for bytes) to get only the information you need.

~~~ shell
wc file.txt
# &amp;gt;      7       12       78
# so file.txt has 7 lines, 12 words and 78 bytes.
 
wc -l file.txt
# &amp;gt; 7
 
wc -w file.txt
# &amp;gt; 12
 
wc -c file.txt
# &amp;gt; 78
~~~

## curl
curl takes any URL and prints the content of the output. By default, it outputs to stdout but if you want to save the file you can use redirection (see next section).

~~~ shell    
curl example.com
# This will print the html file to screen.
~~~

## grep
The grep utility searches input text and select only those lines that match a given pattern.

~~~ shell  
grep "cat" input.txt
# This will select lines that have word cat inside
 
# grep is case senstive by default.
# If you want case insensitive search, pass -i flag
grep -i "cat" input.txt
# this will match cat, CAT, Cat etc,
 
# you can match regex as well
grep -i "[cC]at" input.txt
# this will only match cat and Cat but not CAT.

Grep command is so powerful and you can learn much more about it using man grep.
~~~

## find
find command helps you find files that match given criteria.

~~~ shell    
find . -name "example.txt"
# This will find file with the name example.txt
 
# To make the search case insensitive, use -iname
find . -iname "example.txt"
 
# You can provide any folder to search files in
find ~/downloads -iname "example.txt"
# You can use * inside the file name to match anything
find . -iname "*.txt"
# This will find all .txt files
 
find . -iname "c*n.txt"
# This will match can.txt ccn.txt captain.txt and every text file with name starting with c and ending with n.
~~~

## xargs
xargs is a utility to help build and execute commands from stdin. It is a little advanced for a beginning user but really helpful when you start writing your own commands and scripts.If we have a folder having many files and we want to delete all text files, we can use xargs in combination with the find command.

~~~ shell 
find . -name '*.txt' | xargs rm
# find outputs full names of all text files
# xargs executes rm taking each file as input
~~~

## sed
sed is a streaming editor and can apply edits to text documents with commands. It is a really useful utility when dealing with any kind of text documents. The most common thing you will do with sed is the substitution.

~~~ shell
sed 's/old/new/' example.txt
# This command will replace every instance of old with new in example.txt
~~~

## awk
awk is an excellent utility for processing text files based on numbers and writing reports. It searches the file for a pattern, and it performs an action on lines where the pattern is matched. I am not going to give you an example here because if you have reached so far, you have already survived the Unix command line. But it is a very powerful tool and you will not regret the time you took to learn it.

# Pipe and redirection

## Standard Input and Standard Output
In simple words, imagine StdIn as the input stream to a program and StdOut as the output. When program outputs something it outputs to StdOut which is usually connected to the terminal and displayed there. Similarly, programs that get input from user use StdIn and is usually connected to keyboard input.

# ( > ) The Output Redirect
( > ) The Output redirect is used when you want to save the output to a file instead of seeing it on the terminal. It creates a new file if it doesn’t exist and overrides the file if it exists.

~~~ shell
ls
# ls will output the list of contents of the directory
 
ls > list.txt
# using output redirect, instead of printing output,
# it saves into list.txt file, overriding content if file already exists.
 
# If you don't want to override content, but instead add to it
# You can use >> operator
echo 'The End' >> list.txt
# This will append the message 'The End' at the end of list.txt
~~~

## ( < ) The Input Redirect
( < ) Using this operator, you can read a file in Standard Input of a program. It works as if you type the content of the file into the program. For example, wc command takes input, if you use it without a file name; you can use input redirect to pass it the file as if you typed the contents yourself.

~~~ shell    
wc < file.txt
~~~

## ( | ) The Pipe Operator
The most useful of all, the pipe operator takes the output of one program and sends it to the next program as input. It is so powerful as it lets you make complex programs on the fly using simple programs.

~~~ shell    
ls | wc -l
# This program outputs the number of files/folders in current directory
# ls command outputs list of content
# its output is fed to wc command as input
# wc command counts the total number of lines in the output
~~~
