# Setup Github or Gitlab
### Useful instructions to setup your dev environment

## Basic git setup commands
Setup email and username

```
git config --global user.name "[Your name]"
git config --global user.email [Your e-mail]
```

## Create SSH keys for access


## SSH keys for multiple servers


## Use Git Credential Manager with encrypted keys
Particularly useful to manage tokens for multiple repos.

#### Setup Git Credential manager

Download the latest [*.deb package](https://github.com/git-ecosystem/git-credential-manager/releases/latest)

you can copy the link and download it using curl, for example:

```
curl -LO https://github.com/git-ecosystem/git-credential-manager/releases/download/v2.6.1/gcm-linux_amd64.2.6.1.deb 
```

The -L helps following redirected links.

, and run the following:

```
sudo dpkg -i <path-to-package> 
```

Setup of the GCM as main credentials helper.

```
git-credential-manager configure
```

### Configure GCM to use GPG and PASS to store encrypted passwords
This credential store uses GPG to encrypt files containing credentials which are stored in your file system. The file structure is compatible with the popular pass tool. By default files are stored in ~/.password-store but this can be configured using the pass environment variable PASSWORD_STORE_DIR.

#### Setup PASS and GPG
Normally the pass utility does not come with Ubuntu distro, so you need to install.
```
sudo apt install pass 
```

In case GPG is not install already, you will need to run:
```
sudo apt install gnupg
```

#### Initalise PASS using GPG key pair
Create a new GPG key pair by running:

```
gpg --gen-key
```

...follow the promts

Copy the ``` <gpg-id> ``` (which is a long string), and initealise the Pass Store running:

```
pass init <gpg-id>
```

#### Set Git to use correct Credential Storage

```
git config --global credential.credentialStore gpg 
```

#### Headless/TTY-only sessions 

Configure GPG to use a TTY-only environment (no GUI)

```
export GPG_TTY=$(tty)
```

*It will be convinient to add it to the ``` ~/.bashrc ```... 
```
nano ~/.bashrc
```

Prevent GitHub from asking credentials via dialog.

```
git config --global credential.interactive never
```

#### Configure GCM for each repo

Necessary if you use different credentials (e.g. tokens) for different repos

```
git config --global credential.["GitServer URL"].useHttpPath true
```

(Optional) If you want to store a default username for a server by using attaching directly the the Git Server ```[URL]``` direcly.

```
git config --global credential."[URL]".username [username]
```

### Try it out

You can clone a repository from the server you setup, and it will ask you for the credentials.

After that you can run,

```
pass show
``` 

to check your credentials are stored in the Pass Store, including the full path if you enabled the option to use different tokens for different repos.

You can also verify that your key and passphrase were stored correctly, by using the tab key to autocomplete the following command with the full ``` <key-path> ```,

```
pass show <key-path>
```
