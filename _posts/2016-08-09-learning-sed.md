---
layout:     post
title:      Learning sed utility
date:       2016-08-09 12:32:18
summary:    I use it to replace old ruby hash (rocket) syntax to the new syntax. 
categories: programming
---

Sed, the Streaming Editor, is a Unix utility that helps you edit text in the command line. It is called streaming editor because it applies editing filters to streams of text. sed has its own language and you can write scripts to apply a batch of edits to the text.

# s for Substitution
The most common command you are going to use is the sed substitute command (s).
~~~ shell
sed 's/oldpattern/newpattern/' input.txt
 
# This will print output to stdout, leaving the original file unchanged. 
 
# or in command pipeline.
 
echo pemmsylvamia | sed 's/m/n/'
 
# In this example, I am using s command to replace m with n while supplying input text from previous command
~~~

By default, this will only substitute the first occurrence of the pattern in each line. If you want to apply the substitution for all occurrences, add g (global) flag at the end.

~~~ shell	
sed 's/oldpattern/newpattern/g' input.txt
~~~
 
# Regex Power

The real power of sed comes from using regex patterns. By default, sed only supports a subset of modern regex implementations in various languages. I suggest passing -E flag to sed for extended regex support.
Example: Converting old ruby hash syntax to new syntax

~~~ ruby	
{:apple => 10, :pine_apple => 20, :orange => 'unavailable'} # old style
{apple: 10, pine_apple: 20, orange: 'unavailable'} # new style
~~~

Old ruby only had rocket syntax for hashes. To convert it to the new style, we will use the help of capture groups in Regex.

~~~ shell
sed -E 's/:([a-zA-z0-9_]+) *=>/\1:/g' input_file.rb
~~~

* [a-zA-Z0-9_] is character set that matches underscore, all letters and digits. sed does not support \w (word character) that is available in modern Regex implementations.
* The + sign matches 0 or more occurrences of the preceding character. [a-zA-Z0-9_]+ means at least one-word character is present after the : sign.
* The parenthesis () marks start and end of a capture group. We are capturing the key from the key-value pair.
* The * sign is matching 0 or more occurrences of preceding space character.
* \1 is showing is back-referencing the capture group, the key from the key-value pair.

Given our input, this command successfully converts the hash to the new syntax.

~~~ shell
echo "{:apple => 10, :pine_apple => 20, :orange => 'unavailable'}" | sed -E 's/:([a-zA-z0-9_]+) *=>/\1:/g'
#> {apple: 10, pine_apple: 20, orange: 'unavailable'}
~~~
