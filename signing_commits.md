
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
