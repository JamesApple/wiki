# Packer

```hcl
# Defines meta level settings such as the version of packer required and
# plugins.
packer {
  required_plugins {
    docker = {
      version = ">= 0.0.7"
      # Only needed for non-hashi plugins ()
      source = "github.com/hashicorp/docker"
    }
  }
}

source "docker" "ubuntu" {
  image  = "ubuntu:xenial"
  commit = true
}

build {
  name    = "learn-packer"
  sources = [
    "source.docker.ubuntu"
  ]
}


```

# CLI
```sh
# Same as tf init
packer init .
packer fmt . && packer validate .

packer build ./*.hcl

```
