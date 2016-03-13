---
layout: post
title: Linux Command Cookbook
subtitle: Linux commands I find useful
---

I'm a big fan of the [O'Reilly Cookbook series](http://shop.oreilly.com/category/series/cookbooks.do) as useful reference material covering hundreds of ready-to-use copy-and-paste examples to solve problems. So I'm following in their footsteps in writing tried and tested "recipes" that I've found useful and/or often use. 

### Change to your home directory
    cd
### Change to the previous directory you were in
    cd -
### Make a directory tree
	mkdir -p /tmp/this/is/a/deep/folder/structure
### Rerun the previous command
    !!
### Rerun the previous command as sudo
    sudo !!
### Echo my IP
	ifconfig | grep inet
### Time how long a command took to execute
	time command
### Show the full path of the command
	which command
### Count lines of code
	find . -type f -name "*.py" -print0 | xargs -0 wc -l
### Redirect all output to /dev/null
	command > /dev/null 2>&1
### Redirect stdout and stderr to different files
	command > messages.log 2> errors.log
### Redirect stdout and stderr to the same file
	command &> messages.log
### Don't record command in history
    <space>command
### Run a command repeatedly
    watch <command>
### Find the 10 largest files in the current directory
    du -a . | sort -n -r | head -n 10
### Find files bigger than 10MB in the current directory
	find . -size +10M
### Stream a log file
    tail -f /var/log/file.log
### Stream multiple log files
    tail -f /var/log/{file1,file2}.log
### Find log files less that 1 week old
	find . -name '*.log' -mtime -7 -print
### Start Python's SimpleHTTPServer
    python -m SimpleHTTPServer
### Download a file
	wget http://adammckay.co.uk/
### Generate a 10GB file of random data
    dd if=/dev/urandom of=data bs=10M count=1024
### Create 50 empty files
    touch {1..50}
### Create a tar.gz archive
    tar cvzf backup.tar.gz /home/adam
### Uncompress tar.gz archive
    tar -xvf thumbnails-14-09-12.tar.gz
### Open an editor in the terminal
    ctrl+x+e
### Delete all files of a specific type
	rm $(find . -name '*.pyc')
### Check if command succeeded
	echo $?
### Run a number of commands
	command1; command2; command3
### Run a number of commands only if the previous one succeeded
	command1 && command2 && command3
### Run a number of commands at the same time
	command1 & command2 & command3
### Run a command in the background
	command &
### Run a command in the background and ignore hangup signals
	nohup command &
