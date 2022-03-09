# Visual Studio Code `devcontainer` Templates

- [Python Development](python/)

## Windows Setup

Install WSL 2 with the default Ubuntu distribution in PowerShell:

```powershell
wsl --install
```

Install [Docker Desktop](https://www.docker.com/products/docker-desktop) or using Chocolately in PowerShell:

```powershell
choco install -y docker-desktop
```

Right click on the Docker icon in the taskbar and select `Settings`. In `General`, ensure that `Use the WSL 2 based engine` is checked.

Install [Visual Studio Code](https://code.visualstudio.com/download) or using Chocolately in PowerShell:

```powershell
choco install -y vscode
```

Install the [Remote Development Extension Pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack).

### Git (Optional)

Install [Git](https://git-scm.com/download/win) or using Chocolately in PowerShell:

```powershell
choco install -y git
```

Restart PowerShell if Chocolately was used to install. In PowerShell, set the credential manager, then try an action that requires authentication, such as cloning one of your private repositories:

```powershell
git config --global credential.helper
git clone https://github.com/sears-s/private
```

A prompt should appear asking for your GitHub credentials. After logging in, click `Sign in with your browser`. In the browser, click `Authorize GitCredentialManager`. Future actions won't require your GitHub credentials again.

Set your name and email address in PowerShell (replace with your own):

```powershell
git config --global user.name "Sears Schulz"
git config --global user.email "36250748+sears-s@users.noreply.github.com"
```

You can find your private GitHub email address [here](https://github.com/settings/emails).

To use Git within devcontainers, the `git` package must be installed. This can be done within the Dockerfile:

```Dockerfile
RUN apt-get update && apt-get install git -y
```

### GPG Commit Signing (Optional)

Install [Gpg4win](https://gpg4win.org/get-gpg4win.html) or using Chocolately:

```powershell
choco install -y gpg4win
```

Restart PowerShell if Chocolately was used to install. In PowerShell, generate a new GPG key:

```powershell
gpg --full-generate-key
```

The kind of key should be `RSA and RSA` (default) and the key size should be `4096`. The email address must be the same as your GitHub private email address. Alternatively, a key can be imported:

```powershell
gpg --export-secret-keys -a KEYID > my-key.asc
gpg --import my-key.asc
gpg --edit-key KEYID
gpg> trust
Your decision? 5
```

To get the key ID, use the following command:

```powershell
gpg --list-secret-keys --keyid-format LONG
```

On the line with `sec`, the key ID is the hex string after `rsa4096/`. Export the public key:

```powershell
gpg --armor --export KEYID
```

Copy the output, from `-----BEGIN PGP PUBLIC KEY BLOCK-----` to `-----END PGP PUBLIC KEY BLOCK-----`, inclusive. Back on [GitHub](https://github.com/settings/gpg/new), paste this, then click `Add GPG key`.

In PowerShell, enable Git GPG signing:

```powershell
git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"
git config --global user.signingkey KEYID
git config --global commit.gpgsign true
```

To sign commits within devcontainers, the `gnupg2` package must be installed. This can be done within the Dockerfile:

```Dockerfile
RUN apt-get update && apt-get install gnupg2 -y
```

Additionally, the following line will need to be included in `devcontainer.json`:

```json
"postStartCommand": "git config --global gpg.program \"$(which gpg)\""
```
