#language #golang
# Maps
```go
stringToIntMap := make(map[string]int)
stringToIntMap["String"] = 2
stringToIntMap["String"]++
stringToIntMap["String"] // 3
delete(stringToIntMap, "String")
```

# Defer
Runs a function wherever the current function exits. A function with multiple returns may need multiple cleanups but defer always cleans up after itself
```go
defer socket.Close()
defer func() { r.leave <- client }()
```

# Classes
Go refers to classes as types and instances of those classes as objects.

# Channel (`chan`)
A channel is a bit like a publish subscribe mechanism. A channel has a type `chan string`
```go
myChannel := chan string
myChannel <- "A string"
close(myChannel)
```

# Fix silly issue with LSP
```sh
export CGO_ENABLED=0
```

