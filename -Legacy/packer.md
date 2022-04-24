# [Packer](https://www.packer.io/docs/terminology)
 

Packer files end in: `.pkr.hcl`

|Term|Definition|Links|
|--|--|--|
|Artifacts|Results of a build. Every builder produces a single artifact||
|Template|The HCL or JSON file that packer executes from. The entrypoint to one or multiple parallel builds.|[HCL Syntax](https://www.packer.io/docs/templates/hcl_templates)|
|Communicator|Typically SSH. The method of connection to the machine being configured.||
|Builder|Similar to a TF provider. Docker/VMWare/etc||
|Data Source|Similar to data source in TF. Provides access to external data in a template||
|Provisioner|Something that runs on/against or install software on the machine before the build is committed||
|Post Processor|Takes the result of a build or other post-processor and processes that to create a new artifact.||

# Links

1. [Ansible Provisioner](https://www.packer.io/plugins/provisioners/ansible/ansible): Run existing ansible roles/playbooks against the machine being configured with a dynamic inventory
1. [Git Datasources](https://www.packer.io/plugins/datasources/git): Load information about a commit or repository
1. [Docker Builder](https://www.packer.io/plugins/builders/docker): Build and export Docker images imperatively
1. [Proxmox Builder](https://www.packer.io/plugins/builders/proxmox/iso): Create a Proxmox image from an ISO file/URL
1. [SSH Datasource](https://www.packer.io/plugins/datasources/sshkey): Generates a public/private keyset
1. [GCP Builder](https://www.packer.io/plugins/builders/googlecompute): Create compute images
1. [AWS EBS AMI Builder](https://www.packer.io/plugins/builders/amazon/ebs): Build AMIs easily


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

# Show errors and try repairs
packer build -on-error=ask
# Show all underlying requests. Best for cloud builds. Exports a `instance.pem`
# private key so you can ssh into the instance for debugging (using a
# breakpoint?)
packer build -debug
packer build ./*.hcl

```

# Provisioners

```hcl
# Pause execution
provisioner "breakpoint" {
  disable = false
  note    = "this is a breakpoint"
}

# Upload file to machine
provisioner "file" {
  source = "app.tar.gz"
  destination = "/tmp/app.tar.gz"
}

# Run a command on the machine
provisioner "shell" {
    inline = ["echo foo"]
}

# Run a command on the machine _running_ packer
provisioner "shell-local" {
  inline = ["echo foo"]
}

```

# PostProcessors

```hcl
post-processor "artifice" {
  files = ["myfile"]
}

post-processor "compressor" {
  type = "compress"
  output = "log_{{.BuildName}}.gz"
  compression_level = 9
}

post-processor "manifest" {
    output = "manifest.json"
    strip_path = true
    custom_data = {
      my_custom_data = "example"
    }
}

post-processor "shell-local" {
  inline = ["echo foo"]
}
```

# Building Extensions

[Go lib explanation](https://www.packer.io/docs/plugins/creation/custom-builders)

