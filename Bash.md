```sh
var=val
echo $var
# default
echo "${undec:-'Some val'}"
# substring
echo "${var:0:2}"
arr=(a1 a2)
echo arr[@] # > element count
echo arr[@]:2:1 # from, to
for i in "${arr[@]}"; do
  echo "$i"
done

# Expansion
echo {1..10} # => 1, 2, 3, ... 10
echo {a..z}

# Last programs return value
$?

# Script PID
$$

# Arg count passed to script 
$#

# All args passed to script 
$@

# Sequential args
$1, $2


# Get input
read aval
echo $aval


if [ "$1" != b ]; then
else
fi

[ "$age" -eq 15 ] || [ "$name" == "bill" ]

# Expressions
echo $(( 10 + 5 ))

# Subshell. Allows working cross directory
(cd ~/folder && dosomethinginthiisfolder) #

cat > hi << EOF
    $1
EOF

cat > hi << "EOF"
    $1
EOF

# Redirect errror 
ls 2> fails.log

# Redirect all
ls > alls.log 2>&1

cat > hiya.file <(echo "Hiya")

case "$1" in
  0) echo "Zero";;
  1) echo "Two";;
  *) echo "Not null";;
esac

for var in {1..3}
do
  echo $var
done

for out in $(ls); do; break; done

while [ true ]; do; done

function func ()
{
  echo $1
  return 0
}

cut -d ',' -f 1 input.txt

sed -i 's/yes/no/g' file.txt

grep -x '.*' file.txt # => count
grep -r '.*' dir/

# String search only
fgrep "a string"

trap "rm $TMP_FILE; exit" SIGHUP SIGINT SIGTERM
```
