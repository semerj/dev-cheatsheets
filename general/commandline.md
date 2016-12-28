## `awk` / `gawk`

search multiple files with same regex
```sh
$ awk -F, '/regex/' csvfile.csv
```

subset multiple values in first column
```sh
$ awk -F, '$1 ~ /(regex1|regex2|regex3)/' csvfile.csv
```

subset multiple values in ALL columns
```sh
$ awk -F, '/(regex1|regex2|regex3)/' csvfile.csv
```

filter by 1st column value and print all columns
```sh
$ awk -F, '$1 == "regex" { print $0 }' csvfile.csv
```

filter by 1st column value and print 2nd and 3rd columns only
```sh
$ awk -F, '$1 == "regex" { print $2 "," $3 }' csvfile.csv
```

filter by multiple values in first three columns and print all columns
```sh
$ awk -F, '$1 == "regex" && $2 > 80 || $3 <= 20 { print $0 }' csvfile.csv
```

prints total number of rows
```sh
$ awk 'END { print FNR }' csvfile.csv
# or
$ cat csvfile.csv | wc -l
```

print first row
```sh
$ awk -F, 'NR==1 { print $0 }' csvfile.csv
# or
$ head -1 csvfile.csv
```

print range of lines (e.g. 10-15)
```sh
$ awk 'NR==10, NR==15' file
```

print line immediately before a "/regex/" match (but not the line that matches itself)
```sh
$ awk '/regex/ { print x }; { x=$0 }' csvfile.csv
```

sample 1% of rows and include first row (header)
```sh
$ awk 'BEGIN {srand()} !/^$/ { if (rand() <= .01 || FNR==1) print $0}' csvfile.csv
```

remove beginning 4 characters from each line
```sh
$ awk '{print substr($1,5); }' file
```

create separate file for every value based on column
```sh
# create file
$ cat << EOF > myfile.csv
heredoc> one,1
heredoc> one,2
heredoc> two,1
heredoc> two,2
heredoc> three,1
heredoc> four,1
heredoc> EOF

# create new file based on first column unique values
$ awk -F, '{num=$1; print > num"_file.csv"}' myfile.csv
```

calculate average for second column
```sh
$ awk -F, '{colSum+=$2} END {print "Average =", colSum/NR}' myfile.csv
```

concatenate all csv files into large file and only keep the first header
```sh
$ awk 'FNR==1 && NR!=1{ next; }{ print }' *.csv > bigfile.csv
```

## `basename`

```sh
$ basename filename.ext .ext
filename
```

## brace expansion

```sh
$ echo mkdir -p foo/bar/{baz,qrs}
mkdir -p foo/bar/baz foo/bar/qrs

$ echo robot-{one,two,three,four}-x
robot-one-x robot-two-x robot-three-x robot-four-x

$ echo robot/{c3po,r2d2}/{sound.mp3,info.txt}
robot/c3po/sound.mp3 robot/c3po/info.txt robot/r2d2/sound.mp3 robot/r2d2/info.txt

$ echo x-{wing,b{ee,oo}p}
x-wing x-beep x-boop
```

brace expansion sequences
```sh
$ echo wow{1..5}
wow1 wow2 wow3 wow4 wow5

$ echo img{0..50..10}
img0 img10 img20 img30 img40 img50
```

## `cat`

write lines to file
```sh
$ cat << EOF > myfile
> line one
> line two
> line three
> EOF
```

combine multiple files
```sh
$ cat file1 file2 > newfile
```

append a file to the end of an existing file
```sh
$ cat file >> existingfile
```

any subsequent lines typed will go into the file until `CTRL-D` is typed
```sh
$ cat > file
```

any subsequent lines are appended to the file until `CTRL-D` is typed
```sh
$ cat >> file
```

alterative to piping data to command
```sh
$ cat file | somecommand
# or
$ < file somecommand
```

print column number of first row (transform columns into rows first)
```sh
$ head -n 1 file | tr "," "\n" | cat -n
```

## `convert`

convert `.jpg`'s into a `.gif`
```sh
# resize images and provide a new path for output
$ mogrify -path resized_images -resize 400x400 *.jpg

$ convert ./resized_images/*.jpg new.gif

# if you need to rotate the gif
$ convert -rotate 270 new.gif rotated.gif
```

download sequential image files and convert images to a single pdf
```sh
$ curl "https://www.webpage.com/[1-50].jpg" -o "page_#1.jpg"
$ convert *.jpg document.pdf
```

## `cp`

copy directory and all its subdirectories recursively
```sh
$ cp -r dir newdir
```

copy all files with same extension to a path (only works with zsh?)
```sh
$ cp **/*.jpeg ~/new/path
```

## `cron`

```sh
* * * * * /path/to/command
| | | | |
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)
```

add cron job
```sh
$ crontab -e
```

list all cron jobs
```sh
$ crontab -l
```

remove all cron jobs
```sh
$ crontab -r
```

examples
```sh
# run every 5 minutes
*/5 * * * * /path/to/command

# run every 5 hours
0 */5 * * * /

# run every day at 3am:
0 3 * * * /path/to/command

# run 5 minutes after midnight, every day:
5 0 * * * /path/to/command

# run at 2:15pm on the first of every month:
15 14 1 * * /path/to/command

# run at 10pm on weekdays:
0 22 * * 1-5 /path/to/command

# run at 23 minutes after midnight, 2am, 4am ..., everyday:
23 0-23/2 * * * /path/to/command
```

special commands
```sh
# run once a day, "0 0 * * *"
@daily /path/to/command

# run once an hour, "0 * * * *"
@hourly /path/to/command
```

redirect standard output and error of cron to file
```sh
$ 59 23 * * * /home/john/bin/backup.sh > /home/john/logs/backup.log 2>&1

# 2>&1 indicates that the standard error (2>) is redirected to the same file descriptor that is pointed by standard output (&1)
```

## `csvkit`

manipulate csv files
```sh
# basics
$ csvlook file.csv | less -S
$ csvcut -c column_c,column_a data.csv > new.csv
$ csvgrep -c phone_number -r "555-555-\d{4}" data.csv > matching.csv
```

convert csv
```sh
$ in2csv data.xls > data.csv
$ in2csv data.json > data.csv
$ csvjson data.csv > data.json
```

csvsql
```sh
$ sql2csv --db postgresql:///database --query "select * from data" > extract.csv
$ csvsql --query "select name from data where age > 30" data.csv > old_folks.csv
$ csvsql --db postgresql:///database --insert data.csv
```

## `curl`

download sequential files from api
```sh
$ curl "http://api.example.com/v1/page/[01-99]" -o "page_#1.html"
```

## `cut`

remove first 4 characters from each row
```sh
$ cut -c1-4 file
```

## `diff`

download and diff 2 files using process substitution
```sh
$ diff <(curl http://somesite/file1) <(curl http://somesite/file2)
```

## `du`

view file size
```sh
$ du -h file.csv
```

## `echo`

```sh
$ echo -n "Hello" > hello-world
$ echo " World" >> hello-world
$ cat hello-world
Hello World
```

arithmetic
```sh
$ echo $((4*5+1))
21
```

## `find`

find directories matching name
```sh
$ find /some/dir -type d -name pattern
```

find all files with same pattern
```sh
$ find /some/dir -type f -print | grep 'pattern'
```

find files matching name in current dir and subdir
```sh
$ find . -name "pattern"
```

find and copy all mp3's from all subdirectories to a new directory
```sh
$ find . -name '*.mp3' -exec cp {} ~/Documents/mp3 \;
```

find and remove all files ending with `.txt`
```sh
$ find . -name "*.txt" -exec rm {} ";"
```

find and remove all files less than 1 kilobyte
```sh
$ find -type f -size -1k -exec rm {} +
```

find all zip files and unzips in the directory they were found
```sh
$ find . -name '*.zip' -execdir unzip '{}' ';'
```

find files modified today (for date added use `-atime 0`)
```sh
$ find . -type f -mtime 0
```

find files size 0 bytes
```sh
$ find . -type f -size 0
```

find all python files modified between two dates and times
```sh
$ gfind . -type f -name "*.py" -newermt "2014-10-01 00:00:00" ! -newermt "2014-10-10 23:59:59"
```

find and move only files to directory
```sh
$ find . -maxdepth 1 -type f -exec mv {} destination_path \;
```

## `fold`

break long lines into shorter lines with 30 characters
```sh
$ cat file.txt | fold -sw 30
```

## `grep`

ignore case
```sh
$ grep -i pattern file
```

match all files in directory
```sh
$ grep -r pattern *
```

count matching lines in multiple files
```sh
$ grep -r -c pattern /directory
```

lines that do not match pattern
```sh
$ grep -v pattern file
```

match either regex
```sh
$ grep 'pattern1\|pattern2' file
# or
$ grep -E 'pattern1|pattern2' file
```

match all regexs
```sh
$ grep pattern1 file | grep pattern2
# or
$ grep -e pattern1 -e pattern2 file
```

search multiple files and omit filename
```sh
$ grep -h pattern file1.txt file2.txt
# or
$ grep -h pattern *.txt
```

print 3 lines above and below each matched pattern
```sh
$ grep -C 3 pattern file
```

return word following pattern
```sh
$ ggrep -Po '(?<=pattern\s)\w+' file
```

piping search term to grep
```sh
# non-GNU grep
$ cat patterns.txt | grep -f /dev/stdin file
# GNU grep
$ cat patterns.txt | grep -f - file
```

## `head` + `tail`

display first and last 5 lines of file
```sh
$ (head -5; tail -5) < file
```

skip header row
```sh
$ tail +2 file
```

display first 3 lines in many files
```sh
$ head -3 *.txt | cat
```

## job control command list

list all jobs
```sh
$ jobs -l
```

run command in background
```sh
$ my_command &
```

run program in background after logging out
```sh
$ nohup command-name &
```

list background processes
```sh
$ bg
```

bring a background process to the foreground
```sh
$ fg %1
```

kill a process
```sh
$ kill %1
```

stop foreground process
```sh
$ Ctrl-z
```

## `join`

join 2 files by 2nd field on both, and return 1st field from file1.csv and 3rd field from file2.csv (must sort join field on both files first)
```sh
$ join -t, -1 2 -2 2 -o 1.1,2.3 file1.csv file2.csv
```

## `jq`

filter JSON by value
```sh
$ jq '.[].key | select(.subkey == "hi")' file.json
```

count items in JSON object
```sh
$ jq '. | length' file.json
```

return JSON objects with id as index (from original JSON) and other filtered fields
```sh
$ jq -c '. | keys[] as $k | .[$k] | {id: $k, pmid: .MedlineCitation.PMID, aff: .MedlineCitation.Article.AuthorList[].AffiliationInfo[].Affiliation}' file.json
```

concatenate JSON objects
```sh
$ jq -s "[.[0], .[1]]" file1.json file2.json
```

## `launchctl`

run script at startup in OSX
```sh
$ cd $HOME/Library/LaunchAgents
$ vim com.john.myscript.plist
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
    <string>com.john.myscript</string>
  <key>ProgramArguments</key>
  <array>
    <string>/Users/john/code/myscript.sh</string>
  </array>
  <key>RunAtLoad</key>
    <true/>
  <key>StandardErrorPath</key>
    <string>/tmp/myscript.err</string>
  <key>StandardOutPath</key>
    <string>/tmp/myscript.out</string>
</dict>
</plist>
```

```sh
$ launchctl load com.john.myscript.plist
```

## `less`

view streaming data
```sh
$ less +F streaming_file
```

## `md5`

hash function
```sh
$ echo -n "password" | md5
```

## `mdfind`

spotlight equivalent: search inside files and metadata
```sh
# restrict the search to a single directory
$ mdfind -onlyin ~/Documents essay
```

## `mv`

create nested directories
```sh
$ mkdir -p foo/bar/{baz,qrs}
```

remove/rename file basename
```sh
$ for f in file*; do mv "$f" "newfile${f#file}"; done
```

## `pandoc`

convert markdown to pdf
```sh
$ pandoc --latex-engine=/usr/texbin/pdflatex input.md -o output.pdf
```

convert markdown to latex
```sh
$ pandoc -s readme.md -o readme.tex
```

## `paste`

combine multiple files column-wise
```sh
$ paste *.txt > agg-data.txt
```

## `parallel`

run python command line script with different arguments from a text file in parallel
```sh
$ parallel --jobs 0 "python foo.py --arg={}" ::: $(cat args.txt)
```

## `pbcopy` / `pbpaste`

copy list of files to clipboard
```sh
$ ls ~ | pbcopy
```

capture contents of a file
```sh
$ pbcopy < file.txt
```

## `python`

pretty-print json
```sh
$ cat file.json | python -m json.tool
```

start http server
```sh
# python 2
$ python -m SimpleHTTPServer &
# python 3
$ python -m http.server
```

transpose all rows
```sh
$ head csvfile.csv | python -c "import sys,csv; csv.writer(sys.stdout).writerows(zip(*csv.reader(sys.stdin)))"
```

## `Rscript`

print min, median, mean, max, 1st/3rd quartiles
```sh
$ csvcut -c 5 file.csv | { echo 'd<-scan()'; cat; echo; echo 'summary(d)'; } | R --slave
```

## `rm`

remove entire directory recursively
```sh
$ rm -rf file
```

remove file interactively (prompt before removal)
```sh
$ rm -i file
```

## `rsync`

```sh
# get remote server ip
$ hostname -I

# copy files from remote to local
$ rsync -avzhe ssh remote-user@[INSERT IP]:~/remote_dir ~/local_dir

# copy files from local to remote
$ rsync -avzhe ssh ~/local_dir remote-user@[INSERT IP]:~/remote_dir
```

## `sed`

return first line and matching pattern
```sh
$ sed '1p;/pattern/!d' file
```

substitute all string occurrences in a line
```sh
$ sed s/pattern/replace_string/g file
```

substitute all string occurrences in first 3 rows
```sh
$ sed 1,3s/pattern/replace_string/g file
```

substitue commas for spaces and save file in place
```sh
$ sed -i '' 's/ /,/g' file
```

remove all quotes from lines
```sh
$ sed 's/"//g' file
# or
$ tr -d '"' file
```

multiple substitutions from lines
```sh
$ sed -e s/pattern1/replace_string1/ -e s/pattern2/replace_string2/ file
```

remove beginning 4 characters from each row
```sh
$ sed 's/^....//' file
# or
$ sed 's .\{4\}  '
```

delete whitespace
```sh
# leading whitespace (spaces, tabs)
$ sed 's/^[ \t]*//' file

# trailing whitespace (spaces, tabs)
$ sed 's/[ \t]*$//' file

# leading and trailing (; alternative to -e option)
$ sed 's/^[ \t]*//; s/[ \t]*$//' file
```

remove the line containing the pattern
```sh
$ sed '/pattern/d' file
```

remove all empty lines
```sh
$ sed '/^$/d' file
$ sed '/./!d' file
```

replace pattern in all files in folder
```sh
$ for f in *.xml; do
    gsed -i 's/&#x1E;/?/g' $f
  done
```

print specific lines of file (e.g. 10-15)
```sh
$ sed -n 10,15p file
```

pretty print first 5 lines of json file
```sh
$ for i in {1..5}; do
    sed -n "$i"p file.json | python -m json.tool
  done
```

remove first and last characters
```sh
$ sed 's/^.\(.*\).$/\1/' file
# or
$ rev file | cut -c2- | rev | cut -c2-
```

remove lines with fewer than 5 character
```sh
$ gsed -r '/.{5,}/!d' file
```

match any character group inside brackets
```sh
$ sed -n "s/^\[\(.*\)\]$/\1/p"
```

## `scp`

copy files/folders

| from   | to     | type   | scp                                                  | sftp            |
|:-------|:-------|:-------|------------------------------------------------------|-----------------|
| local  | server | file   | `scp -P [PORT] file.txt pi@[INSERT IP]:~`            | `put file.txt`  |
| local  | server | folder | `scp -r -P [PORT] ~/folder/ pi@[INSERT IP]:/home/pi` | `put -r folder` |
| server | local  | file   | `scp pi@[INSERT IP]:file.txt .`                      | `get file.txt`  |
| server | local  | folder | `scp -r pi@[INSERT IP]:folder/ .`                    | `get -r folder` |

## `sftp`

```sh
$ sftp pi@[INSERT IP]
$ lpwd # local working directory
$ pwd  # remote working directory
```

## `shuf` / `gshuf`

shuffle lines
```sh
$ seq 1 5 | gshuf
```

## `sort`

sort by first two columns
```sh
$ sort -t, -k 1,2 csvfile.csv
```

sort and return unique files
```sh
$ sort -u file
```

## `ssh`

remote network access
```sh
$ ssh user@[EXTERNAL IP] -p [PORT]
```

from local network
```sh
$ ssh user@[LOCAL IP] -l
```

copy public ssh key to server
```sh
$ ssh-copy-id -i ~/.ssh/id_rsa.pub username@[INSERT IP]
```

## `tar`

create archive and compress with gzip
```sh
# gzip
$ tar zcvf mydir.tgz mydir
```

extract all files/directories (don't need to specify gzip format)
```sh
$ tar xvf mydir.tgz
```

## `tee`

redirect stderr (2) into stdout (1), then pipe stdout into `tee`, which copies it to the terminal and to log file
```sh
./myscript.sh 2>&1 | tee log.txt
```

## `tr`

translate braces into parenthesis
```sh
$ tr '{}' '()' < file
```

translate uppercase lowercase
```sh
$ echo 'THIS IS ALL CAPS' | tr "[:upper:]" "[:lower:]"
$ echo 'This is partial caps' | tr "[:upper:]" "[:lower:]"
```

delete specific character
```sh
$ echo "the geek stuff" | tr -d 't'
```

strip out non-printable characters
```sh
$ tr -cd "[:print:]" < file
```

join all the lines in a file into a single line
```sh
$ tr -s '\n' ' ' < file
```

transpose 1st line
```sh
$ head -1 csvfile.csv | tr ',' '\n'
# or
$ head -1 csvfile.csv | awk -vRS=',' 'NF'
# or
$ head -1 csvfile.csv | sed 's/\(,\)/\1\n/g'
```

remove empty lines
```sh
$ tr -s '\n' < csvfile.csv
# or
$ awk '/./' csvfile.csv
```

## `uniq`

count number of unique values (sort first)
```sh
$ uniq -c
```

## `wget`

list file names in FTP directory
```sh
$ wget --no-remove-listing ftp://ftpserver/ftpdirectory/
```

download all files listed between `<a href=`
```sh
$ wget -q -O- ftp://66.97.146.93/ | \
  grep -o '<a href=['"'"'"][^"'"'"']*['"'"'"]' | \
  sed -e 's/^<a href=["'"'"']//' -e 's/["'"'"']$//' | \
  xargs -l wget
```

## `zip`, `gzip`, `bzip2`, `xz`

| Program | Notes                        |
|:--------|:-----------------------------|
| `gzip`  | most common                  |
| `bzip2` | better compression, slower   |
| `xz`    | most space efficient, slower |
| `zip`   | legacy, windows              |
| `tar`   | archived files, tarballs     |

compress all files in current directory individually
```sh
$ gzip *
```

zip contents of entire directory into one zip file
```sh
$ zip -r somedir.zip somedir
```

zip all files matching pattern into one zip file
```sh
$ find ./directory -name 'pattern' -exec zip zipfile.zip {} \;
```

decompress file
```sh
$ gunzip somedir
# or
$ gzip -d somedir
```
