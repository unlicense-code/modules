Note Ignores global ssh config even if it exists.
```
GIT_SSH_COMMAND='ssh -i $HOME/.ssh/id_ed25519 -o IdentitiesOnly=yes -F /dev/null' git push origin c13_training_network
```

Match Rules not Host Rules

.ssh/config

```
Match Exec "[ git@git.unlicensed-code.dspeed.eu:gitolite-admin = $(git config --get remote.origin.url)'' ]"
  IdentityFile ~/.ssh/gitolite-admin
  IdentitiesOnly yes
  ForwardAgent no
  ForwardX11 no
  ForwardX11Trusted no

Match Exec "[ git@git.unlicensed-code.dspeed.eu:some_repo = $(git config --get remote.origin.url)'' ]"
  IdentityFile ~/.ssh/yourOwnPrivateKey
  IdentitiesOnly yes
  ForwardAgent no
  ForwardX11 no
  ForwardX11Trusted no
```
