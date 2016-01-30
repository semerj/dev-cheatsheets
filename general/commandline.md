## (g)awk
search multiple files with same regex
```bash
$ awk -F, '/regex/' csvfile.csv
```
subset multiple values in first column
```bash
$ awk -F, '$1 ~ /(regex1|regex2|regex3)/' csvfile.csv
```
subset multiple values in ALL columns
```bash
$ awk -F, '/(regex1|regex2|regex3)/' csvfile.csv
```
filter by 1st column value and print all columns
```bash
$ awk -F, '$1 == "regex" { print $0 }' csvfile.csv
```
filter by 1st column value and print 2nd and 3rd columns only
```bash
$ awk -F, '$1 == "regex" { print $2 "," $3 }' csvfile.csv
```
filter by multiple values in first three columns and print all columns
```bash
$ awk -F, '$1 == "regex" && $2 > 80 || $3 <= 20 { print $0 }' csvfile.csv
```
prints total number of rows
```bash
$ awk 'END { print FNR }' csvfile.csv
# or
$ cat csvfile.csv | wc -l
```
print first row
```bash
$ awk -F, 'NR==1 { print $0 }' csvfile.csv
# or
$ head -1 csvfile.csv
```
print range of lines (e.g. 10-15)
```bash
$ awk 'NR==10, NR==15' file
```
print line immediately before a "/regex/" match (but not the line that matches itself)
```bash
$ awk '/regex/ { print x }; { x=$0 }' csvfile.csv
```
sample 1% of rows and include first row (header)
```bash
$ awk 'BEGIN {srand()} !/^$/ { if (rand() <= .01 || FNR==1) print $0}' csvfile.csv
```
remove beginning 4 characters from each line
```bash
$ awk '{print substr($1,5); }' file
```
create separate file for every value based on column
```bash
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
```bash
$ awk -F, '{colSum+=$2} END {print "Average =", colSum/NR}' myfile.csv
```
concatenate all csv files into large file and only keep the first header
```bash
$ awk 'FNR==1 && NR!=1{ next; }{ print }' *.csv > bigfile.csv
```

## basename
```bash
$ basename filename.ext .ext
filename
```

## brace expansion
```bash
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
```bash
$ echo wow{1..5}
wow1 wow2 wow3 wow4 wow5

$ echo img{0..50..10}
img0 img10 img20 img30 img40 img50
```

## cat
write lines to file
```bash
$ cat << EOF > myfile
> line one
> line two
> line three
> EOF
```
combine multiple files
```bash
$ cat file1 file2 > newfile
```
append a file to the end of an existing file
```bash
$ cat file >> existingfile
```
any subsequent lines typed will go into the file until `CTRL-D` is typed
```bash
$ cat > file
```
any subsequent lines are appended to the file until `CTRL-D` is typed
```bash
$ cat >> file
```
alterative to piping data to command
```bash
$ cat file | somecommand
# or
$ < file somecommand
```
print column number of first row (transform columns into rows first)
```bash
$ head -n 1 file | tr "," "\n" | cat -n
```

## convert
convert `.jpg`'s into a `.gif`
```bash
# resize images and provide a new path for output
$ mogrify -path resized_images -resize 400x400 *.jpg

$ convert ./resized_images/*.jpg new.gif

# if you need to rotate the gif
$ convert -rotate 270 new.gif rotated.gif
```
download sequential image files and convert images to a single pdf
```bash
$ curl "https://www.webpage.com/[1-50].jpg" -o "page_#1.jpg"
$ convert *.jpg document.pdf
```

## cp
copy directory and all its subdirectories recursively
```bash
$ cp -r dir newdir
```
copy all files with same extension to a path (only works with zsh?)
```bash
$ cp **/*.jpeg ~/new/path
```

## cron
```bash
* * * * * /path/to/command
| | | | |
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)
```
commands
```bash
# input cron job
$ crontab -e

# list all cron jobs
$ crontab -l

# remove all cron jobs
$ crontab -r
```
examples
```bash
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
```bash
# run once a day, "0 0 * * *"
@daily /path/to/command

# run once an hour, "0 * * * *"
@hourly	/path/to/command
```
redirect standard output and error of cron to file
```bash
$ 59 23 * * * /home/john/bin/backup.sh > /home/john/logs/backup.log 2>&1

# 2>&1 indicates that the standard error (2>) is redirected to the same file descriptor that is pointed by standard output (&1)
```

## csvlook
view csv file
```bash
$ csvlook "file.csv" | less -S
```

## curl
download sequential files from api
```bash
$ curl "http://api.example.com/v1/page/[01-99]" -o "page_#1.html"
```

## cut
remove first 4 characters from each row
```bash
$ cut -c1-4 file
```

## du
view file size
```bash
$ du -h file.csv
```

## echo
```bash
$ echo -n "Hello" > hello-world
$ echo " World" >> hello-world
$ cat hello-world
Hello World
```
arithmetic
```bash
$ echo $((4*5+1))
21
```

## find
find directories matching name
```bash
$ find /some/dir -type d -name pattern
```
find all files with same pattern
```bash
$ find /some/dir -type f -print | grep 'pattern'
```
find files matching name in current dir and subdir
```bash
$ find . -name "pattern"
```
find and copy all mp3's from all subdirectories to a new directory
```bash
$ find . -name '*.mp3' -exec cp {} ~/Documents/mp3 \;
```
find and remove all files ending with `.txt`
```bash
$ find . -name "*.txt" -exec rm {} ";"
```
find and remove all files less than 1 kilobyte
```bash
$ find -type f -size -1k -exec rm {} +
```
find all zip files and unzips in the directory they were found
```bash
$ find . -name '*.zip' -execdir unzip '{}' ';'
```
find files modified today (for date added use `-atime 0`)
```bash
$ find . -type f -mtime 0
```
find files size 0 bytes
```bash
$ find . -type f -size 0
```
find all python files modified between two dates and times
```bash
$ gfind . -type f -name "*.py" -newermt "2014-10-01 00:00:00" ! -newermt "2014-10-10 23:59:59"
```
find and move only files to directory
```sh
$ find . -maxdepth 1 -type f -exec mv {} destination_path \;
```

## fold
break long lines into shorter lines with 30 characters
```bash
$ cat file.txt | fold -sw 30
```

## grep
ignore case
```bash
$ grep -i pattern file
```
match all files in directory
```bash
$ grep -r pattern *
```
count matching lines in multiple files
```bash
$ grep -r -c pattern /directory
```
lines that do not match pattern
```bash
$ grep -v pattern file
```
match either regex
```bash
$ grep 'pattern1\|pattern2' file
# or
$ grep -E 'pattern1|pattern2' file
```
match all regexs
```bash
$ grep pattern1 file | grep pattern2
# or
$ grep -e pattern1 -e pattern2 file
```
search multiple files and omit filename
```bash
$ grep -h pattern file1.txt file2.txt
# or
$ grep -h pattern *.txt
```
print 3 lines above and below each matched pattern
```bash
$ grep -C 3 pattern file
```
return word following pattern
```bash
$ ggrep -Po '(?<=pattern\s)\w+' file
```
piping search term to grep
```bash
# non-GNU grep
$ cat patterns.txt | grep -f /dev/stdin file
# GNU grep
$ cat patterns.txt | grep -f - file
```

## head + tail
display first and last 5 lines of file
```bash
$ (head -5; tail -5) < file
```
skip header row
```bash
$ tail +2 file
```
display first 3 lines in many files
```bash
$ head -3 *.txt | cat
```

## join
join 2 files by 2nd field on both, and return 1st field from file1.csv and 3rd field from file2.csv (must sort join field on both files first)
```bash
$ join -t, -1 2 -2 2 -o 1.1,2.3 file1.csv file2.csv
```

## launchctl
run script at startup in OSX
```
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
```
$ launchctl load com.john.myscript.plist
```

## less
view streaming data
```bash
$ less +F streaming_file
```

## md5
hash function
```bash
$ echo -n "password" | md5
```

## mdfind
spotlight equivalent: search inside files and metadata
```bash
# restrict the search to a single directory
$ mdfind -onlyin ~/Documents essay
```

## mv
create nested directories
```bash
$ mkdir -p foo/bar/{baz,qrs}
```
remove/rename file basename
```bash
$ for f in file*; do mv "$f" "newfile${f#file}"; done
```

## pandoc
convert markdown to pdf
```bash
$ pandoc --latex-engine=/usr/texbin/pdflatex input.md -o output.pdf
```
convert markdown to latex
```bash
$ pandoc -s readme.md -o readme.tex
```

## paste
combine multiple files column-wise
```bash
$ paste *.txt > agg-data.txt
```

## pbcopy and pbpaste
copy list of files to clipboard
```bash
$ ls ~ | pbcopy
```
capture contents of a file
```bash
$ pbcopy < file.txt
```

## python
pretty-print json
```bash
$ cat file.json | python -m json.tool
```
start http server
```bash
# python 2
$ python -m SimpleHTTPServer &
# python 3
$ python -m http.server
```
transpose all rows
```bash
$ head csvfile.csv | python -c "import sys,csv; csv.writer(sys.stdout).writerows(zip(*csv.reader(sys.stdin)))"
```

## Rscript
print min, median, mean, max, 1st/3rd quartiles
```bash
$ csvcut -c 5 file.csv | { echo 'd<-scan()'; cat; echo; echo 'summary(d)'; } | R --slave
```

## rm
remove entire directory recursively
```bash
$ rm -rf file
```
remove file interactively (prompt before removal)
```bash
$ rm -i file
```

## sed
return first line and matching pattern
```bash
$ sed '1p;/pattern/!d' file
```

substitute all string occurrences in a line
```bash
$ sed s/pattern/replace_string/g file
```
substitute all string occurrences in first 3 rows
```bash
$ sed 1,3s/pattern/replace_string/g file
```
substitue commas for spaces and save file in place
```bash
$ sed -i '' 's/ /,/g' file
```
remove all quotes from lines
```bash
$ sed 's/"//g' file
# or
$ tr -d '"' file
```
multiple substitutions from lines
```bash
$ sed -e s/pattern1/replace_string1/ -e s/pattern2/replace_string2/ file
```
remove beginning 4 characters from each row
```bash
$ sed 's/^....//' file
# or
$ sed 's .\{4\}  '
```
delete whitespace
```bash
# leading whitespace (spaces, tabs)
$ sed 's/^[ \t]*//' file

# trailing whitespace (spaces, tabs)
$ sed 's/[ \t]*$//' file

# leading and trailing (; alternative to -e option)
$ sed 's/^[ \t]*//; s/[ \t]*$//' file
```
remove the line containing the pattern
```bash
$ sed '/pattern/d' file
```
remove all empty lines
```bash
$ sed '/^$/d' file

$ sed '/./!d' file
```
replace pattern in all files in folder
```bash
$ for f in *.xml; do
    gsed -i 's/&#x1E;/?/g' $f
  done
```
print specific lines of file (e.g. 10-15)
```bash
$ sed -n 10,15p file
```
pretty print first 5 lines of json file
```bash
$ for i in {1..5}; do
    sed -n "$i"p file.json | python -m json.tool
  done
```
remove first and last characters
```bash
$ sed 's/^.\(.*\).$/\1/' file
# or
$ rev file | cut -c2- | rev | cut -c2-
```
remove lines with fewer than 5 character
```bash
$ gsed -r '/.{5,}/!d' file
```

## (g)shuf
shuffle lines
```bash
$ seq 1 5 | gshuf
```

## sort
sort by first two columns
```bash
$ sort -t, -k 1,2 csvfile.csv
```
sort and return unique files
```bash
$ sort -u file
```

## tar
```bash
# extract all the files in mydir.tar into the mydir directory
$ tar xvf mydir.tar

# create the archive and compress with gzip
$ tar zcvf mydir.tar.gz mydir

# create the archive and compress with bz2
$ tar jcvf mydir.tar.bz2 mydir

# create the archive and compress with xz
$ tar jcvf mydir.tar.xz mydir

# extract all the files in mydir.tar.xz into the mydir directory. note you do not have to tell tar it is in gzip format.
$ tar xvf mydir.tar.gz
```

## tr
translate braces into parenthesis
```bash
$ tr '{}' '()' < file
```
translate uppercase lowercase
```bash
$ echo 'THIS IS ALL CAPS' | tr "[:upper:]" "[:lower:]"
$ echo 'This is partial caps' | tr "[:upper:]" "[:lower:]"
```
delete specific character
```bash
$ echo "the geek stuff" | tr -d 't'
```
strip out non-printable characters
```bash
$ tr -cd "[:print:]" < file
```
join all the lines in a file into a single line
```bash
$ tr -s '\n' ' ' < file
```
transpose 1st line
```bash
$ head -1 csvfile.csv | tr ',' '\n'
# or
$ head -1 csvfile.csv | awk -vRS=',' 'NF'
# or
$ head -1 csvfile.csv | sed 's/\(,\)/\1\n/g'
```
remove empty lines
```bash
$ tr -s '\n' < csvfile.csv
# or
$ awk '/./' csvfile.csv
```

## uniq
count number of unique values (sort first)
```bash
$ uniq -c
```

## wget
download all files listed between <a href= from
```bash
$ wget -q -O- ftp://66.97.146.93/ | \
  grep -o '<a href=['"'"'"][^"'"'"']*['"'"'"]' | \
  sed -e 's/^<a href=["'"'"']//' -e 's/["'"'"']$//' | \
  xargs -l wget
```

## zip, gzip, bzip2, xz
* `gzip`: most common
* `bzip2`: better compression, slower
* `xz`: most space efficient, slower
* `zip`: legacy, windows
* `tar`: archived files, tarballs

compress all files in current directory individually
```bash
$ gzip *
```
zip contents of entire directory into one zip file
```bash
$ zip -r somedir.zip somedir
```
zip all files matching pattern into one zip file
```bash
$ find ./directory -name 'pattern' -exec zip zipfile.zip {} \;
```
decompress file
```bash
$ gunzip somedir
# or
$ gzip -d somedir
```
