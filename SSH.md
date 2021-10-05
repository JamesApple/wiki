# SSH Tunneling
```sh
SERVER_IP=100.40.20.10
TO_PORT=2000
FROM_PORT=80
ssh-copy-id myuser@$SERVER_IP
ssh -f myuser@$SERVER_IP -L $TO_PORT:$SERVER_IP:$FROM_PORT -N

```
