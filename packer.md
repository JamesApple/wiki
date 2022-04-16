# Packer

|Term|Definition|
|--|--|
|||

# Docker builder

The builder starts a Docker container, runs provisioners within this container,
then exports the container for reuse or commits the image

Images can be built using config tools like ansible.

https://www.packer.io/plugins/builders/docker

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

# Will commit ImageSha256
source "docker" "ubuntu" {
  # Start from this image
  image  = "ubuntu:xenial"
  # Saves the result as a new docker image. Otherwise saved as .tar
  commit = true
  # Export path for .tar
  export_path = './my-tar.tar'
  # Delete the container after running
  discard = true
  message = "Hello"
  # Modifies the docker metadata for the saved image
  changes = [
    "USER www-data",
    "WORKDIR /var/www",
    "ENV HOSTNAME www.example.com",
    "VOLUME /test1 /test2",
    "EXPOSE 80 443",
    "LABEL version=1.0",
    "ONBUILD RUN date",
    "CMD [\"nginx\", \"-g\", \"daemon off;\"]",
    "ENTRYPOINT /var/www/start.sh"
  ]

  # Do some imperative changes to the image
}

build {
  name    = "learn-packer"
  sources = [
    "source.docker.ubuntu"
  ]
  provisioner "shell" {
    environment_vars = [
      "FOO=hello world",
    ]
    inline = [
      "echo Adding file to Docker Container",
      "echo \"FOO is $FOO\" > example.txt",
    ]
  }

  post-processors {
    # Name/Import the image we generate
    # "docker-tag" for committed and "docker-import for exported"
    post-processor "docker-import" {
        repository =  "myrepo/myimage"
        tag = "0.7"
    }
    # Push that repo/tag
    post-processor "docker-push" {}
  }

  post-processors {
    post-processor "docker-tag" {
        repository =  "myrepo/myimage"
        tag = "0.7"
    }
    post-processor "docker-push" {}
  }
}


```

# CLI
```sh
# Same as tf init
packer init .
packer fmt . && packer validate .

packer build ./*.hcl

```
