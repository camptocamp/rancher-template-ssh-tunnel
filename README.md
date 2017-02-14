# SSH Tunnel

This stacks deploys an SSH tunnel service.


## Whitelisting services

By default, the SSH service forbids tunneling to all services. In order to whitelist a service, you must add an `ssh_tunnel.port` label to it, with the port you wish to allow.

For example, if you deploy:

```yaml
version: '2'
services:
   hello:
     image: tutum/hello-world
     labels:
       ssh_tunnel.port: '80'
```

to a stack named `foo`, the SSH tunnel service will allow tunneling to `hello.foo:80`, as well as `foo-hello-1:80` (and `foo-hello-2`... if you scale the service).


## Using the tunnel

It might be useful to setup an SSH client configuration for your tunnel in `~/.ssh/config`, e.g.:

```
Host infra-tunnel
  HostName tunnel.ssh.<env_domain>
  Port 2222
  LocalForward 8080 hello.raphink:80
  LocalForward 8081 raphink-hello-1:80
  LocalForward 8082 raphink-hello-2:80
  User tunnel
```

where `<env_domain>` is the external DNS domain for the Rancher environment you deployed to.
