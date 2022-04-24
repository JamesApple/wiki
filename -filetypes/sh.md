# Associative Array

```sh
declare -A opts
opts=(
	["Open Jira Ticket"]=jira_ticket
)

opts["Another"]=Option

# Index Access
echo ${opts["Open Jira Ticket"]}

# Echo Keys
echo "${!opts[@]}" | rofi -dmenu -i

# Echo Values
echo "${opts[@]}"

# Get one option
option=${options[$( printf "%s\n" "${!options[@]}" | rofi -dmenu -i )]}

# Iterate array
for key in "${!opts[@]}"; do
    echo "Key:   ${key}"
    echo "Value: ${array[$key]}"
done

# Length
echo "${#opts[@]}"
```

# If statements
```zsh
  if statement; then echo 'Success!'; fi

  if ! statement; then echo 'Negated Success!'; fi

  if grep 'hi' filename
  then echo 'Found'
  else echo 'Nope' 
  fi

  if [[ 1 > 2 ]]; then echo '1 is gt 2'; fi

  if [[ -f filename.md ]] 
  then echo 'file exists'
  fi

  if [[ -n 'String' ]]
  then echo 'String > zero length'
  fi

  if [[ 'String' =~ '^St.*' ]]
  then echo 'Matched regex'
  fi
```
# List of file test flags

-e
file exists

-a
file exists

This is identical in effect to -e. It has been "deprecated," and its use is discouraged.

-f
file is a regular file (not a directory or device file)

-s
file is not zero size

-d
file is a directory

-b
file is a block device (floppy, cdrom, etc.)

-c
file is a character device (keyboard, modem, sound card, etc.)

-p
file is a pipe

-h
file is a symbolic link

-L
file is a symbolic link

-S
file is a socket

-t
file (descriptor) is associated with a terminal device

This test option may be used to check whether the stdin ([ -t 0 ]) or stdout ([ -t 1 ]) in a given script is a terminal.

-r
file has read permission (for the user running the test)

-w
file has write permission (for the user running the test)

-x
file has execute permission (for the user running the test)


-O
you are owner of file

-G
group-id of file same as yours

-N
file modified since it was last read

f1 -nt f2
file f1 is newer than f2

f1 -ot f2
file f1 is older than f2

f1 -ef f2
files f1 and f2 are hard links to the same file
