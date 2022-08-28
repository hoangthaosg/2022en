ps ax | grep [s]sh-agent

ssh-keyscan github.com >> ~/.ssh/known_hosts

```
# Personal Github account 1
Host github.com
	HostName github.com
	User git
	AddKeysToAgent yes
	# UseKeychain yes
	IdentityFile ~/.ssh/id_ed25519_sg
	IdentitiesOnly yes
	PreferredAuthentications publickey
```