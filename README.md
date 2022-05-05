# SSH Keys

The file `~/.ssh/config` contains the required information for the `SSH` to operate smoothly.

One easy way to `ssh` to a `private` terminal via a `public` terminal, is also given here. In this way, there is no need to go through multiple logins.

The `push` or `pull` to [github](github.com) is a bit different.

Check all, then write your own `config` file.

## The contents

The following keeps the `SSH` to stop failing if the terminal is kept idle too long.
```
ServerAliveInterval 60
```

### The following is the way to `ssh` to a `public` terminal:

The following pieace of code goes in the `~/.ssh/config` file.
```
Host remote1
     User myusername
     Hostname host.public.ip.or.url
     IdentityFile ~/.ssh/id_rsa_remote1
```

The point of all of this is that one can just `ssh remote1`, instead of `ssh myusername@host.public.ip.or.url`.

Now the `id_rsa_remote1` file needed to be created.

In simple, a pair of `public` and `private` keys are created. The `public` key is then copied to the `remote1` terminal.

Generate the key using,
```
ssh-keygen -C "my_laptop"
```
Give the `path/to/file` as `~/.ssh/id_rsa_remote1`.

*Note*: Never keep the passphrase empty. Keep it different than the `ssh user password`.

Now, copy the `public` key to the `remote1` using the following command.
```
ssh-copy-id -i ~/.ssh/id_rsa_remote1 remote1
```
It may ask user password multiple times, don't worry.

After this, whenever you do `ssh` to `remote1`, it should not ask for ssh user password. You need to provide the `ssh passphrase` once after rebooting the terminal.

### The following is the way to `ssh` to a `private` terminal via `public` termina:

Add the following pieace of code in the `~/.ssh/config` file.
```
Host remote2
     User myotherusername
     Hostname host.private.ip.or.url
     IdentityFile ~/.ssh/id_rsa_remote2
     ProxyJump remote1
```

The `ProxyJump` does all the magic.

Generate the key in the same way, only change the path to `~/.ssh/id_rsa_remote2`.

Then, copy the `public` key to the `remote2` using the following key.
```
ssh-copy-id -i ~/.ssh/id_rsa_remote2 remote2
```
