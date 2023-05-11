
# How to create own GitLab Runner on Ubuntu

Sources:
- https://docs.docker.com/engine/install/ubuntu/
- https://www.fosstechnix.com/how-to-install-gitlab-runner-on-ubuntu/

> **_NOTE:_**  For run commands become `root` or use `sudo` (not included in text).


## Install Docker

Get Ubuntu virtual machine.

`apt update`

`apt install curl gnupg ca-certificates`

Add apt sources:
```
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu jammy stable" | tee /etc/apt/sources.list.d/docker.list
apt update
apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Verify it is working: `docker run hello-world`


## Install Runner

Add apt sources: `curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash`

`apt install gitlab-runner`

Verify it is working:

`gitlab-runner -version`  
`gitlab-runner status`  


## Add runner to Gitlab and register runner

Source https://docs.gitlab.com/ee/ci/runners/register_runner.html

Go to Gitlab and ...
 On the top bar, select Main menu > Groups and find your group.
 On the left sidebar, select CI/CD > Runners.
 Select New group runner.
 Select a platform.
 Optional. Enter configurations for the runner.
 Select Submit.

You get CLI command which must be run on runner's machine.

eg. `gitlab-runner register --url https://gitlab.com --token xxxxx`

After running the command, a few questions must be answered.


## Runner and job tags

When you added runner into Gitlab you could set tags which define runner's ability (kind of OS, compiler version etc.)

You can mark pipeline job to match runner's tag like this ...
```
image: golang:latest

compile:
  tags:
    - runner1
  stage: build
  script:
    - pwd
```

You can verify that workload really do on new runner by checking job output

```
...
Preparing environment
Running on runner-imchus3qc-project-34891567-concurrent-0 via runner1...
...
```
The important part is "**via runner1**".


## Using runner in multiple groups and projects

One runner can be used by multiple groups and projects but each time must be registered again.


## IPv6 in docker

<ins>**Enable in Docker**</ins>
- Create file daemon.json if not exist `touch /etc/docker/daemon.json`
- Make content of file
```
{
  "ipv6": true,
  "fixed-cidr-v6": "2001:470:6f:4ee:1::/80"
}
```
- Restart service `systemctl restart docker`
- Check network interface docker0 ... should have IPv6 address

Sources:
- https://docs.gitlab.com/runner/executors/docker.html
- https://docs.docker.com/config/daemon/ipv6/


<ins>**Enable in Runner**</ins>

- Find file /etc/gitlab-runner/config.toml
- Add `enable_ipv6 = true` into section `[runners.docker]`
- Restart service `systemctl restart gitlab-runner`
- Verify IPv6 address in docker `docker run -it ubuntu bash -c "ip -6 addr show dev eth0; ip -6 route show"`
- Check `sysctl net.ipv6.conf.default.forwarding` should be 1
- Check `sysctl net.ipv6.conf.all.forwarding` should be 1
- Check route table `route -6n` should be there route `2001:470:6f:4ee:1::/80` to dev `docker0`

Runner config for example:
```
concurrent = 1
check_interval = 0
shutdown_timeout = 0

[session_server]
  session_timeout = 1800

[[runners]]
  name = "runner1"
  url = "https://gitlab.com"
  id = 111
  token = "xxxxx"
  token_obtained_at = 2023-05-11T19:09:58Z
  token_expires_at = 0001-01-01T00:00:00Z
  executor = "docker"
  [runners.cache]
    MaxUploadedArchiveSize = 0
  [runners.docker]
    tls_verify = true
    image = "golang:latest"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0
    enable_ipv6 = true
```

>  **_Comment from source code:_**  
> To enable IPv6 support for this network, set `enable_ipv6` to `true` inside the Docker config.
> This feature works only when the Docker daemon is configured with IPv6 enabled.
> To enable IPv6 support on your host, see the [Docker documentation](https://docs.docker.com/config/daemon/ipv6/).

Source: https://gitlab.com/gitlab-org/gitlab-runner/-/commit/01dfa2b05741960888a78a2a110b8cf1ef0d7afb


<ins>**Let neighbors know where to find dockers**</ins>

Now should works ping between Docker host and Docker guest network interface.
The trouble is nobody knows about this network. It is resulting in the state when packets go out but never come back (they do not know where).

There are two ways ... use NDP proxying or distribute route using "Router Advertisement"(RA)

I choose Router Advertisement  
- Create config file `touch /etc/radvd.conf`
- Make content of file
```
interface ens18 {
    AdvSendAdvert on;
    AdvDefaultLifetime 0;
    MinRtrAdvInterval 30;
    MaxRtrAdvInterval 100;

    route 2001:470:6f:4ee:1::/80 {
        AdvRouteLifetime 6000;
    };
};
```
- Install RA daemon `apt install radvd`
- Check route tables on others computers ... should be there route "2001:470:6f:4ee:1::/80" to gateway "Docker host IP"
- Manually add route "2001:470:6f:4ee:1::/80" to "Docker host IP" into the (Mikrotik) router (it doesn't load RA I don't know why)

Sources:
- https://linux.die.net/man/5/radvd.conf
- https://colinpaice.blog/2022/12/16/understanding-radvd-with-ipv6-on-linux/
- https://gdevillele.github.io/engine/userguide/networking/default_network/ipv6/
- https://labs.ripe.net/author/tugzrida/docker-ipv6-networking-routing-and-ndp-proxying/
- https://superuser.com/questions/1263626/subnet-with-two-ipv6-routers-how-to-get-default-route-correct

> **_Not used but can be handy:_**  
> In case `sysctl net.ipv6.conf.eth0.accept_ra` get 1 then should be set to 2 to awoid collision during using ipv6 forwarding and also use ipv6 SLAAC.
> Changing it to 1 will allow you to automatically get a ipv6 Global or ULA address, ONLY if ipv6 forwarding is set to 0.
