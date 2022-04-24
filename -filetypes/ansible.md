# Articles

[Ansible Tricks](https://zwischenzugs.com/2021/08/27/five-ansible-techniques-i-wish-id-known-earlier/)

# Run arbitrary commands

`ansible-doc ping`

`ansible localhost -m ping`

`ansible localhost -m shell -a "cat ./a-file"`

`ansible localhost -m uri -a 'url=https://google.com return_content=true'`

`ansible all --list-hosts`

# Simple localhost playbook

`ansible-playbook -e "my_var=123 other_var=345" ./deploy.yaml`

```yaml
---
- hosts: localhost
  vars_prompt:
    - name: 'env'
      prompt: 'Environment: nonprod | prod'
      private: no
  vars_files:
    - './vars/env-all.yaml'
    - './vars/env-{{ env }}-{{ instance }}.yaml'

  vars:
  tasks:
    - name: 'Assume to k8s'
      command: |
        aws eks update-kubeconfig --region ap-southeast-2 --profile {{ eks_profile }} --name {{ eks_cluster }}
    - name: 'Deploy {{ namespace }} ingress'
      command: |
        kubectl --namespace={{ namespace }} apply -f -
      args:
        stdin: '{{ lookup("template", "./manifests/my-deployment.yaml.j2") }}'
```

# Default

```yaml
# Can prevent exceptions
{{ item.val | default(100) }}
```


# Loops

Avoid `with_*` keywords

```yaml
- name: basic loop
    debug:
      msg: "{{ item }}"
    loop:
    - a
    - b
- name: hello
    debug:
        msg: "{{ item.val }}"
    loop:
    - val: a
    - val: b
    
# When clause is processed separately for each
- name: hello
    debug:
      msg: "{{ item.val }}"
    when: item.val > 10
    loop:
    - val: 10
    - val: 29
    -
# Dict loop
- hosts: localhost
  vars:
    my_dict:
      a: true
  tasks:
    - name: hello
      debug:
        msg: "{{ item }}"
      loop: "{{ my_dict | dict2items }}"

# Loop over commands and throw if nonzero rc
  tasks:
    - shell: "echo {{ item }}"
      loop:
        - "one"
        - "two"
      register: echo

    - name: Fail if return code is not 0
      fail:
        msg: "The command ({{ item.cmd }}) did not have a 0 return code"
      when: item.rc != 0
      loop: "{{ echo.results }}"

# Do until
- shell: /usr/bin/foo
  register: result
  until: result.stdout.find("all systems go") != -1
  retries: 5
  delay: 10

```

# Error handling

```yaml
name: Perform possible failing process
block:
  - ...task
rescue:
  - ...task
always:
  - ...task
```

# Task args


```yaml
- tasks: 
  name: me
  changed_when: 'changed' in me.stdout
  failed_when: 'failed' in me.stderr
  when: true

```

# Handlers

## Basic direct handler
```yaml
tasks:
  - name: write config
    template:
      src: template.j2
      dest: /etc/file.foo
    notify:
      - handler name

handlers: 
  - name: handler name
    service: nginx
    state: restarted
```

## Publish subscribe as a handler
```yaml
tasks:
  - name:
    notify: "restart everything"

handlers:
  - name: a consumer
    ...
    listen: "restart everything"
  - name: another consumer
    ...
    listen: "restart everything"
```
# Debugging

Set `ANSIBLE_STRATEGY=debug` to run a playbook and debug on error

Debugger can be used on any block with a `name`
```yaml
- name: Execute a command
  command: false
  debugger: on_failed # or on_skipped 
```
```
p task
p task.args
p task_vars["my-var"]
p host
p result._result

task.args['my-arg'] = 'new val'
redo

task_vars["my-vars"] = 'new val'
update_task
redo

continue

quit

```

# Setting an environment variable

```yaml
  vars:
    proxy_env:
      http_proxy: http://proxy.example.com:8080

  tasks:

    - name: Install cobbler
      package:
        name: cobbler
        state: present
      environment: "{{ proxy_env }}"
```

# Testing


## Wait for a response from a url
```yaml
  - wait_for:
      host: "{{ inventory_hostname }}"
      port: 22
    delegate_to: localhost
```

## Curl and check a webpage
```yaml
  - action: uri url=https://google.com return_content=yes
    register: webpage

  - fail:
      msg: "google is down"
    when: 'google' not in webpage.content
```

## Check results of a shell command

```yaml
   - shell: ls
     register: cmd_result

   - assert:
       that:
         - "'not ready' not in cmd_result.stderr"
         - "'gizmo enabled' in cmd_result.stdout"
```

## Check for existence of an unmanaged file
```yaml
   - stat:
       path: /path/to/something
     register: p

   - assert:
       that:
         - p.stat.exists and p.stat.isdir
```

# Ansible config

* ANSIBLE_CONFIG (environment variable)
* ansible.cfg (per directory)
* ~/.ansible.cfg (home directory)
* /etc/ansible/ansible.cfg (global)

# Ansible galaxy
`ansible-galaxy install -r requirements.yml`
`ansible-galaxy search elasticsearch --author geerlingguy`
`ansible-galaxy info username.role_name`

`requirements.yml`
```yaml

# from galaxy
- src: yatesr.timezone

# from GitHub
- src: https://github.com/bennojoy/nginx

# from GitHub, overriding the name and specifying a specific tag
- src: https://github.com/bennojoy/nginx
  version: master
  name: nginx_role

# from a webserver, where the role is packaged in a tar.gz
- src: https://some.webserver.example.com/files/master.tar.gz
  name: http-role

# from Bitbucket
- src: git+http://bitbucket.org/willthames/git-ansible-galaxy
  version: v1.4

# from Bitbucket, alternative syntax and caveats
- src: http://bitbucket.org/willthames/hg-ansible-galaxy
  scm: hg

# from GitLab or other git-based scm
- src: git@gitlab.company.com:mygroup/ansible-base.git
  scm: git
  version: "0.1"  # quoted, so YAML doesn't parse this as a floating-point value
```
