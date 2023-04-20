
# How to create own GitLab Runner on Ubuntu

Sources:
- https://docs.docker.com/engine/install/ubuntu/
- https://www.fosstechnix.com/how-to-install-gitlab-runner-on-ubuntu/

> **_NOTE:_**  For run commands become `root` or use `sudo` (not included in commands).


## Ubuntu

Get Ubuntu virtual machine.


## Install Docker

`apt update`

`apt install curl gnupg ca-certificates`

Add apt sources:

`install -m 0755 -d /etc/apt/keyrings`  
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg`  
`chmod a+r /etc/apt/keyrings/docker.gpg`    
`echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu jammy stable" | tee /etc/apt/sources.list.d/docker.list`

`apt update`

`apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

Verify it is working: `docker run hello-world`


## Install Runner

Add apt sources: `curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash`

`apt install gitlab-runner`

Verify it is working:

`gitlab-runner -version`  
`gitlab-runner status`  
