# What's Ultisnips?

## Helpers:

### Insert current clipboard using vim substitution

```snippets
snippet plug "Insert current clipboard using vim substitution" w
`!v substitute(@+, ' \|\n', '', 'g' )`
endsnippet
```

### Get current date in ISO format

```snippets
snippet date "Date in ISO format" i
`!p if not snip.c: snip.rv = getISO()`
endsnippet
```

### Get current date in Y M D

`!v strftime("%Y-%m-%d")`
