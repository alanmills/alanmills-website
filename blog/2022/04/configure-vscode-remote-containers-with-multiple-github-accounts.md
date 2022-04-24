# Configure Visual Studio Code Remote - Containers with multiple GitHub accounts

**Author:** [Alan Mills]
**Date:** [23 April 2022 22:53]
**Tags:** [Visual Studio Code, Git]
**Status**: Publish

## Overview

I have been exploring the [Visual Studio Code Remote Development inside a Docker Container][2], and I was running into issues preventing me from working with Git from within the Development Container as a result of my [local setup to support multiple Git accounts][1].

My setup uses two items that were not working with the default Development Container setup:

1. Using Git Profiles `includeIf` to specify the Git name, email, and GPG signing key based on code location in the file system.
2. Using SSH to authenticate with Github using SSH Key files

To fix this, I need to:

1. [Sharing SSH and Git config with Development Container](#sharing-ssh-and-git-config-with-development-container) 
2. [Triggering Git config rules by changing the default source code mount](#triggering-git-config-rules-by-changing-the-default-source-code-mount)
3. [Ensuring that Git profiles are not tied to your local path](#ensuring-that-git-profiles-are-not-tied-to-your-local-path)
4. [Sharing SSH Config file between Mac and Linux - Bad configuration option: usekeychain](#sharing-ssh-config-file-between-mac-and-linux---bad-configuration-option-usekeychain)
5. [Testing things work](#testing-things-work)

## Sharing SSH and Git config with Development Container

By default` VSCode shares the Global Git config and forward your local SSH agent; however, this was not working for me.  So, to ensure that the container has access to my SSH and Git config without storing these files in the container, I mounted the local files to the running development container.

In this example, the Development Container is not running with `root` instead using the `vscode` user, with the `HOME` path of `/home/vscode`.

```json
"mounts": [
  "source=${localEnv:HOME}/.ssh,target=/home/vscode/.ssh,type=bind,consistency=cached",
  "source=${localEnv:HOME}/.git,target=/home/vscode/.git,type=bind,consistency=cached"
]
```

## Triggering Git config rules by changing the default source code mount

By default, Visual Studio Code mounts the root of the VSCode Workspace to `/workspace` within the container.  To trigger the [Git profile][1], we need to add two lines of configuration to `devcontainer.json` to:

1. `workspaceFolder` - the source code location within the Docker Container that aligns with the Git profile rules, e.g. `/github/company1`.
2. `workspaceMount` - configure VSCode to mount the local source code folder with the `workspaceFolder`.

```json
"workspaceFolder": "/github/company1",
"workspaceMount": "source=${localWorkspaceFolder},target=/github/company1,type=bind",
```

## Ensuring that Git profiles are not tied to your local path

When I [set up local support for multiple Git accounts][1], I set the `includeif` to my `~/code/github/company1` path; however, with the `workspaceFolder`, we need to update this to a more generic matching pattern to give us more flexibility where we mount the **Workspace** in the **Container**.

```bash
~/.gitconfig
[includeIf "gitdir:**/github/company1/**"]
  path=~/.git/github-company1.conf
```

## Sharing SSH Config file between Mac and Linux - Bad configuration option: usekeychain

After configuring the Development Container to use my Git and SSH configuration, I ran into the issue that when sharing the SSH configuration between Mac and Linux, Linux does not understand the SSH configuration `UseKeyChain`, and therefore, you will receive errors when you use SSH within the Development Container.

```bash
/home/vscode/.ssh/config: line 3: Bad configuration option: usekeychain
/home/vscode/.ssh/config: terminating, 1 bad configuration options
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

To resolve this, use the SSH command `IgnoreUnknown UseKeychain` to tell Linux to ignore without warning any instances of `UseKeychain` within the rest of the SSH `config` file.

### ~/.ssh/config

```bash
AddKeysToAgent yes
IgnoreUnknown UseKeychain
UseKeychain yes

Host *
  IdentityFile ~/.ssh/id_rsa

Host github_company1
  Host github.com
  IdentityFile ~/.ssh/id_github_comany1_ed25519
  User alan@company1.com
  IdentityOnly yes
```

## Testing things work

Open your VSCode project configured to use a Remote Development Docker Container.  Once started, open a Terminal and type the commands:

```bash
vscode ➜ /github/company1 (main ✗) $ ssh -T git@github_compan1
Hi Alan!  You’ve successfully authenticated, but GitHub does not provide shell access.
vscode ➜ /github/company1 (main ✗) $ git pull
Already up to date.
```

## Full VSCode Container Configuration

For my setup of VSCode, I am using the VSCode Go `1.17-bullseye` Docker Image for `arm64/Apple Silicon` as I am using an Apple M1 Mac.

VSCode stores the files `devcontainer.json` and `Dockerfile` in the `.devcontainer` folder.

### devcontainer.json

```json
// For format details, see https://aka.ms/devcontainer.json. For config options, see the README at:
// https://github.com/microsoft/vscode-dev-containers/tree/v0.231.6/containers/go
{
 "name": "container-with-multiple-github-accounts",
 "build": {
  "dockerfile": "Dockerfile",
  "args": {
   // Update the VARIANT arg to pick a version of Go: 1, 1.18, 1.17
   // Append -bullseye or -buster to pin to an OS version.
   // Use -bullseye variants on local arm64/Apple Silicon.
   "VARIANT": "1.17-bullseye",
   // Options
   "NODE_VERSION": "none"
  }
 },
 "runArgs": [ "--cap-add=SYS_PTRACE", "--security-opt", "seccomp=unconfined" ],

 // Set *default* container specific settings.json values on container create.
 "settings": {
  "go.toolsManagement.checkForUpdates": "local",
  "go.useLanguageServer": true,
  "go.gopath": "/go"
 },

 // Add the IDs of extensions you want installed when the container is created.
 "extensions": [
  "golang.Go"
 ],

 // Use 'forwardPorts' to make a list of ports inside the container available locally.
 // "forwardPorts": [],

 // Use 'postCreateCommand' to run commands after the container is created.
 // "postCreateCommand": "go version",

 // Comment out to connect as root instead. More info: https://aka.ms/vscode-remote/containers/non-root.
 "remoteUser": "vscode",
 "features": {
  "github-cli": "latest"
 },
 // "workspaceMount": "source=${localWorkspaceFolder},target=/go/src/github.com/cadamus,type=bind",
 // "workspaceFolder": "/go/src/github.com/cadamus",
 "workspaceMount": "source=${localWorkspaceFolder},target=/github/company1,type=bind",
 "workspaceFolder": "/github/company1",
 "mounts": [
  "source=${localEnv:HOME}/.ssh,target=/home/vscode/.ssh,type=bind,consistency=cached",
  "source=${localEnv:HOME}/.git,target=/home/vscode/.git,type=bind,consistency=cached"
 ]
}
```

### Dockerfile

```Dockerfile
# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.231.6/containers/go/.devcontainer/base.Dockerfile

# [Choice] Go version (use -bullseye variants on local arm64/Apple Silicon): 1, 1.16, 1.17, 1-bullseye, 1.16-bullseye, 1.17-bullseye, 1-buster, 1.16-buster, 1.17-buster
ARG VARIANT=” 1.18-bullseye”
FROM mcr.microsoft.com/vscode/devcontainers/go:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment the next lines to use go get to install anything else you need
# USER vscode
# RUN go get -x <your-dependency-or-tool>

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1
```

## References

* [blog - Setting up multiple GitHub accounts][1]
* [VSCode - Developing inside a Continer][2]
* [VSCode - Creating a Development Container](https://code.visualstudio.com/docs/remote/create-dev-container)
* [VSCode - Change the default source code mount](https://code.visualstudio.com/remote/advancedcontainers/change-default-source-mount)
* [VSCode - How to add a folder from your local file system to a dev container](https://youtu.be/L1-dx-ZD0Ao)
* [Unix Tutorials - SSH: Bad configuration option: usekeychain](https://www.unixtutorial.org/ssh-bad-configuration-option-usekeychain/)

[1]: <../../2020/12/setting-up-multiple-github-accounts.md>
[2]: <https://code.visualstudio.com/docs/remote/containers>
