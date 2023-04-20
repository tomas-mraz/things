# Things
... to remember

- How to make my own GitLab runner with IPv6 [link](./gitlab-runner.md) 

## Docs
- [Mermaid](https://mermaid-js.github.io/mermaid/) - Graphs in markdown
- https://stackedit.io - Markdown online editor

## Code
- [Swagger/OpenAPI](https://swagger.io/) - API Designer 

## Networking 

Ubuntu Netplan IPv4 and IPv6 settings [sfsfsdf](./netplan.md) 

Raspberry PI [manual setting IPv4 and IPv6 address](./rpi_static_ip_addresses.md)

## Grafana

Prometheus exporter pro Raspberry Pi

https://github.com/lukasmalkmus/rpi_exporter


## Signing commits

I am using app called [SmartGit](https://www.syntevo.com/smartgit/) and [manual](https://docs.syntevo.com/SmartGit/HowTos/Sign-Tags-and-Commits.html) says
- if you are using Windows, please install [Gpg4win](https://gpg4win.org) (verified with Gpg4win 3.1.16)
- run gpa.exe (usually found in `C:\Program Files (x86)\Gpg4win\bin`) and create a key pair securing it with a passphrase
- in SmartGit Repository | Settings, tab Signing configure the full path to the gpg.exe and enter the Key ID of your created key pair
- if necessary, select Sign all commits
- when committing a file or tagging, a popup of GPG will occur and ask you for the key's passphrase (actually just the first time in the app's lifetime)

GitHub [documentation](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key):

- run `gpg --full-generate-key` to generate key pair
- run `gpg --list-secret-keys --keyid-format=long` to get key ID 
- run `gpg --armor --export $USE_ID_YOU_GET` to get public key to copy into GitHub profile settings

