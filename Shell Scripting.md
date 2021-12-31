# Shell Scripting

Another common pattern is wantiing to get the output of a command as a variable. This can be down with command substitution. Whenever you place `$(CMD)` it will excute `CMD`, get output of the commond and substitute in place.

example:

```shell

#!/bin/bash

echo "start program at $(data)" # data will be substitute

echo "running program $0 with $# arguments with pid $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # when pattern is not found, grep has exit status 1
    # when redirect STDOUT and STDERR to null register since we do not care about them
    if [[ $? -ne 0]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done 


```

- Curly braces `{}` - Whenever you have a common substring in a series of commands, you can use curly braces for bash to expand this automatically. This comes in very handy when moving or converting files.

```shell
convert image.{png, jpg}
# will expand to 
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# will expand to 
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# Globbing techniques can also be combined
mv *{.py, .sh} folder
# will move all *.py and *.sh files

mkdir foo bar
# This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
touch {foo,bar}/{a..h}
touch foo/x bar/y
# Show differences between files in foo and bar
diff <(ls foo) <(ls bar)
# Outputs
# < x
# ---
# > y
```

Some differences between shell functions and scripts that we should keep in mind are:

- Functions have to be in the same languages as the shell, while scripts can be written in any language. This is why including a shebang for scripts is important.
- Functions are loaded once when their definition is read. Scripts are loaded every time they executed. This makes functions slightly faster to load, but whenever you change them you will have to reload their definition.
- Functions are executed in the current shell environment whereas scripts execute in their own process. Thus, functions can modify environment variables, e.g. change your current directory, whereas scripts can’t. Scripts will be passed by value environment variables that have been exported using export.
- As with any programming language, functions are a powerful construct to achieve modularity, code reuse, and clarity of shell code. Often shell scripts will include their own function definitions.

# Shell Tools

## Finding files

One of the most common repetitive tasks that every programmer faces is finding files or directories. All UNIX-like systems come packaged with find, a great shell tool to find files. find will recursively search for files matching some criteria. Some examples:

```shell
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'
```