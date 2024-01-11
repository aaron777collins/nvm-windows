<h1 align="center">A Git Bash Usable NVM for Windows</h1>

<p><b>This version of NVM ignores which terminal you're using but should otherwise work the same as the original <a href="https://github.com/coreybutler/nvm-windows">nvm for windows</a>. It's important to note that the original author disabled this out of fear that some NVM features wouldn't work properly. However, the basic <i>install</i> and <i>use</i> features should work as intended.</b></p>

## Overview

Manage multiple installations of node.js on a Windows computer.

**tl;dr** Similar (not identical) to [nvm](https://github.com/creationix/nvm), but for Windows. Has an installer. [Download Now](https://github.com/aaron777collins/nvm-windows/releases)!

This has always been a node version manager, not an io.js manager, so there is no back-support for io.js. Node 4+ is supported. Remember when running `nvm install` or `nvm use`, Windows usually requires administrative rights (to create symlinks).

![NVM for Windows](https://github.com/coreybutler/staticassets/raw/master/images/nvm-1.1.8-screenshot.jpg)

There are situations where the ability to switch between different versions of Node.js can be very useful. For example, if you want to test a module you're developing with the latest bleeding edge version without uninstalling the stable version of node, this utility can help.

![Switch between stable and unstable versions.](https://github.com/coreybutler/staticassets/raw/master/images/nvm-usage-highlighted.jpg)

### Installation & Upgrades

#### :star: :star: Uninstall any pre-existing Node installations!! :star: :star:

The simplest (recommended) way to get NVM for Windows running properly is to uninstall any prior Node installation _before_ installing NVM for Windows. It avoids all of the pitfalls listed below. However; you may not wish to nuke your Node installation if you've highly customized it. NVM for Windows _can_ assume management of an existing installation, but there are nuances to this (dependent entirely on the permissions of the user running the installation). If you have an administrative account, it's relatively safe to install NVM for Windows before uninstalling the original Node version. If you are working in a closed environment, such as a corporate Active Directory environment where installations/uninstallations are controlled by group policy, you should really consider removing the original version of Node before installing NVM4W.

_Permission Problems_
For security reasons, Windows will not allow an application from one vendor to "uninstall" an application from a different vendor. The official NVM4W installer will attempt assume management of an existing installation of Node., but it cannot actually uninstall the original Node.js version. To work around this, NVM for Windows installer attempts to copy the original Node.js installation files to the NVM root. This includes global npm modules and configurations. Once this process is complete, the original Node.js installation can be uninstalled without losing data.

_PATH Installation Problems_
If you attempt to configure the `NVM_SYMLINK` to use an existing directory (like `C:\Program Files\nodejs`), it will fail because a symlink cannot overwrite a physical directory. This is not a problem if you choose a different symlink path (such as `C:\nvm\node`).

_PATH Conflicts_
If you do not uninstall the original version, running `nvm use` may appear to do nothing at all. Running `node -v` will always show the original installation version. This is due to a [`PATH` conflict](https://github.com/coreybutler/nvm-windows/wiki/Common-Issues#why-do-i-need-to-uninstall-nodejs-before-installing-nvm-for-windows) that presents when the same application is installed multiple times. In NVM4W 1.1.11+, run `nvm debug` to determine if you have a `PATH` conflict.

For simpliciy, we recommend uninstalling any existing versions of Node.js before using NVM for Windows. Delete any existing Node.js installation directories (e.g., `%ProgramFiles%\nodejs`) that might remain. NVM's generated symlink will not overwrite an existing (even empty) installation directory. 

:eyes: **Backup any global `npmrc` config** :eyes:
(e.g. `%AppData%\npm\etc\npmrc`)

Alternatively, copy the settings to the user config `%UserProfile%\.npmrc`. Delete the existing npm install location (e.g. `%AppData%\npm`) to prevent global module conflicts.

#### Install nvm-windows

Use the [latest installer](https://github.com/aaron777collins/nvm/releases) (comes with an uninstaller). Alternatively, follow the original [manual installation](https://github.com/coreybutler/nvm-windows/wiki#manual-installation) guide. **Note this is the guide from the original and the download links are to the original version, not the patched one.**

_If NVM4W doesn't appear to work immediately after installation, restart the terminal/powershell (not the whole computer)._

![NVM for Windows Installer](https://github.com/coreybutler/staticassets/raw/master/images/nvm-installer.jpg)

#### Reinstall any global utilities

After install, reinstalling global utilities (e.g. yarn) will have to be done for each installed version of node:

```
nvm use 14.0.0
npm install -g yarn
nvm use 12.0.1
npm install -g yarn
```

### Usage

**nvm-windows runs in an Admin shell**. You'll need to start `powershell` or Command Prompt as Administrator to use nvm-windows

NVM for Windows is a command line tool. Simply type `nvm` in the console for help. The basic commands are:

- **`nvm arch [32|64]`**: Show if node is running in 32 or 64 bit mode. Specify 32 or 64 to override the default architecture.
- **`nvm check`**: Check the NVM4W process for known problems.
- **`nvm current`**: Display active version.
- **`nvm install <version> [arch]`**:  The version can be a specific version, "latest" for the latest current version, or "lts" for the most recent LTS version. Optionally specify whether to install the 32 or 64 bit version (defaults to system arch). Set [arch] to "all" to install 32 AND 64 bit versions. Add `--insecure` to the end of this command to bypass SSL validation of the remote download server.
- **`nvm list [available]`**: List the node.js installations. Type `available` at the end to show a list of versions available for download.
- **`nvm on`**: Enable node.js version management.
- **`nvm off`**: Disable node.js version management (does not uninstall anything).
- **`nvm proxy [url]`**: Set a proxy to use for downloads. Leave `[url]` blank to see the current proxy. Set `[url]` to "none" to remove the proxy.
- **`nvm uninstall <version>`**: Uninstall a specific version.
- **`nvm use <version> [arch]`**: Switch to use the specified version. Optionally use `latest`, `lts`, or `newest`. `newest` is the latest _installed_ version. Optionally specify 32/64bit architecture. `nvm use <arch>` will continue using the selected version, but switch to 32/64 bit mode. For information about using `use` in a specific directory (or using `.nvmrc`), please refer to [the original nvm-windows issue #16](https://github.com/coreybutler/nvm-windows/issues/16).
- **`nvm root <path>`**: Set the directory where nvm should store different versions of node.js. If `<path>` is not set, the current root will be displayed.
- **`nvm version`**: Displays the current running version of NVM for Windows.
- **`nvm node_mirror <node_mirror_url>`**: Set the node mirror.People in China can use *https://npmmirror.com/mirrors/node/*
- **`nvm npm_mirror <npm_mirror_url>`**: Set the npm mirror.People in China can use *https://npmmirror.com/mirrors/npm/*

### :warning: Gotcha!

Please note that any global npm modules you may have installed are **not** shared between the various versions of node.js you have installed. Additionally, some npm modules may not be supported in the version of node you're using, so be aware of your environment as you work.

### :name_badge: Antivirus

Users have reported some problems using antivirus, specifically McAfee. It appears the antivirus software is manipulating access to the VBScript engine. See [issue #133](https://github.com/coreybutler/nvm-windows/issues/133) for details and resolution.

**v1.1.3 is not signed** since this is a fork and I haven't purchased a certificate.

### Build from source

- Install go from http://golang.org
- Download source / Git Clone the repo
- Change GOARCH to amd64 in build.bat if you feel like building a 64-bit executable
- Fire up a Windows command prompt and change directory to project dir
- Execute `go get github.com/blang/semver`
- Execute `go get github.com/olekukonko/tablewriter`
- Execute `build.bat`
- Check the `dist`directory for generated setup program.

---

**For more information about using nvm-windows, reference the [original](https://github.com/coreybutler/nvm-windows) docs**