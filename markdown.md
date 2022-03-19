# Get first word with cut
```sh
 echo "word1  word2" | cut -f 1 -d " "
 ```

# Check if file exists
```sh
[ -e file ] || echo 'Does not exist'
```

# Fail from nested function in script
```sh
fail () { : "${script_failure_or_something:?$1}"; }
```

