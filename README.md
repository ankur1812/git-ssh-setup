# GIT SSH SETUP
SSH setup for git

## MacOS

### 1. Check for existing keys
```
ls  ~/.ssh
```
If the SSH key exists the generating new key in Step 2 won't be needed. 


### 2. Generate a New SSH Key: 
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

```
% ssh-keygen -t ed25519 -C "your_email@example.com" 
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/ankur1812/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/ankur1812/.ssh/id_ed25519
Your public key has been saved in /Users/ankur1812/.ssh/id_ed25519_testing.pub
The key fingerprint is:
SHA256:JI1GDpl/F6Pb71H1n3RrE6fMQ/rbSWtmxAIVgq2kYj8 your_email@example.com
The key's randomart image is:
+--[ED25519 256]--+
|    .o.   o. ..  |
|    o+ o oo...   |
|     .= =.oo.   .|
|     +.+o..o   ..|
|    . o.S+  . +o+|
|       E. .  B.+B|
|        .  .. B*.|
|            ..o*+|
|           .. *+.|
+----[SHA256]-----+
```
This will generate 2 files **id_ed25519** & **id_ed25519.pub** at location ~/.ssh ( same as /Users/<user>/.ssh ). 
If the files already exist enter the file name e.g id_ed25519_latest.

** To verify if the keys were generated use the command `ls  ~/.ssh`


( NOTE: If using older systems use `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`)


### 3. Adding SSH key to to the ssh-agent

3.1. Start the ssh-agent in the background.
```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

3.2. Create or update the `~/.ssh/config` file to automatically load they key to the ssh-agent and store the passphrase in your keychain
```
$ open ~/.ssh/config
-- OR --
$ touch ~/.ssh/config
```
3.3 Add the ssh key file to the config
```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```
3.4 Add your SSH private key to the ssh-agent and store your passphrase in the keychain

`ssh-add --apple-use-keychain ~/.ssh/id_ed25519`

(`ssh-add ~/.ssh/id_ed25519` is the command, to make sure it works, use the `--apple-use-keychain` flag)


### 4. Add the SSH key to your github profile
Copy the contents of the public key stored in **ed_id25519.pub**
```
pbcopy < ~/.ssh/id_ed25519.pub
-- OR --
cat ~/.ssh/id_ed25519.pub
```
Add the key to your github profile [here](https://github.com/settings/keys)

### 5. Test if the connected SSH keys works
```
% ssh -T git@github.com

Hi ankur1812! You've successfully authenticated, but GitHub does not provide shell access.

```
