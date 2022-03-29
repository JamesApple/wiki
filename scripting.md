[Finally understand JQ](https://earthly.dev/blog/jq-select/)

## If statements
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
