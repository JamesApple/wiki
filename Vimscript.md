# Vimscript

## Functions
Functions implicitly evaluate to 0 without a return
```vim
" a:argument
function DisplayName(name)
echo a:name
endfunction

call DisplayName("Your Name")

" Variable length arguments
function Varg(...)
echo a:0
echo a:1
echo a:000
endfunction

call Varg("a", "b")

" Functions as a variable
let MyFunc = function("Varg")
MyFunc(1, 2)
let ListFuncs = [function("Varg"), function("DisplayName")]
```

## Variables
```vim
&shiftwidth " Use option as var
if exists("g:my_global")
```

## Echo
`echom` saves echos to `:messages` to be viewed later.

## Sets
```vim
set setting! " Toggle boolean
set setting? " Check value
```

## Augroup
```vim
" Set local setting
augroup filetype_vim
autocmd!
autocmd FileType vim setlocal foldmethod=marker
augroup END

" Set filetype
au BufNewFile,BufRead *.pn set filetype=potion

```

## Ifs
`==?` is insensitive. `==#` is always sensitive
```vim
if 0 ==? 1
echom "if"
elseif "nope!"
echom "elseif"
else
echom "finally!"
endif
```

## Concatenation
`'Hello'.' world'`

## String functions
`:h functions`
```vim
strlen("foo") " => 3
len("foo") " => 3
split("one two three") " => ['one', 'two', 'three']
split("one,two,three", ",") " => ['one', 'two', 'three']
join(["foo", "bar"], "...") " => foo...bar
tolower("Foo")
toupper("Foo")
'string'[0:1] " -> 'st'
'string'[-3:-1] " -> 'ing
```

## Execution
`execute 'nmap '.'<leader>'.variable`
`:normal I# `: Insert `# ` at start of selected lines

## Current word
`<cword>` or `cWORD>` can be used in ex commands EG:
`:!rg <cWORD>`

## Shell escapes
```vim
echo shellescape("<cWORD>")         | " -> <cWord>
echo expand("<cWORD>")              | " -> The current word
echo shellescape(expand("<cWord>")) | " -> The current word shell escaped
```

## List operations

Slicing:                               `['a', 'b', 'c', 'd'][0:2] -> ['a', 'b', 'c']`
Overslicing:                           `['a', 'b'][0:999999] -> ['a', 'b']`
Backwards slicing:                     `['a', 'b', 'c', 'd'][-2:-1] -> ['c', 'd']`
Concatenation:                         `['a'] + ['b']`
Length:                                `len(['a', 'b']) -> 2`
Method Retrieval:                      `get(['a', 'b'], 0, 'default') -> 'a'`
Default Retrieval:                     `get(['a', 'b'], 999, 'default') -> 'default'`
Index:                                 `index(['a', 'b'], 'b') -> 1`
Missing Index:                         `index(['a', 'b'], 'c') -> -1`
Joining:                               `join(['a', 'b'], ',') ->  'a, b'`
Reversing:                             `reverse(['a', 'b']) -> ['b', 'a']`
Clone:                                 `deepcopy(['a'])`
Mapping (With existing `Double` func): `map([1, 2], 'Double'.'(v:val)')`

## Loops
```vim
for i in [1, 2, 3, 4]
echo i
endfor

let c = 0
while c <= 4
let c +=1
echo c
endwhile
```

## Dictionaries
```vim
let dict = { 'a': 'b', 3: 'c' }
dict.a " -> 'b'
dict['a'] " -> 'b'
dict[3] " -> 'c'
dict.d = 'e'
unlet dict.d
remove(dict, 'a')
get(dict, 'a', 'default')
has_key(dict, 'c')
items(dict) " -> [['a', 'b'], [3, 'c']]
keys(dict)
values(dict)
```

## Syntax Highlighting

```vim
" Define keyword rubyKeyword and add typeof and break
syntax keyword rubyKeyword typeof break
" Define syntax on regex \v is very magic regex
syntax match rubyComment "\v#.*$"
" Define syntax region. This is for " strings
syntax region rubyString start=/\v"/ skip=/\v\\./ end=/\v"/
Links rubyKeyword syntax group to Keyword highlighting group
highlight link rubyKeyword Keyword
```

## Buffers
```vim
setlocal filetype=dirvish
setlocal buftype=nofile
```
Append text to buffer at line 3: `append(3, ['foo', 'bar'])`

## Files

Filename in current path:     `fnamemodify('foo.txt', ':p')`
Current File:                 `expand('%')`
List files in current dir:    `globpath('.', '*')`
Files in nested dirs as list: `split(globpath('.', '**'), '\n')`

## Plugin Folders

/colors:   Files in this dir can be loaded by running `color <TAB>` and should contain all code needed to set up the scheme
/plugin:   Runs once every time vim starts. Always gets loaded
/ftdetect: The files in this directory should set up autocommands that detect and set the filetype of files, and nothing else
/ftplugin: Contains directories containing subsections of filetype code or single files such as `markdown.vim`
/indent:   A lot like ftplugin files. They get loaded based on their names.
/compiler: They should set compiler-related options in the current buffer based on their names.
/autoload: Delays the loading of your plugin's code until it's actually needed
/after:    Files in this directory will be loaded every time Vim starts, but after the files in /plugin
/doc:      Vimhelp style documentation

## Autoloading functions
Given `autoload/helpers.vim` which contains the function `helpers#Flatmap`. Running `helpers#Flatmap()` loads and runs the function on demand without being loaded at start
Multiple sub directories can be used by chaining the # `helpers#fns#Flatmap` for `helpers/fns/flatmap.vim`
