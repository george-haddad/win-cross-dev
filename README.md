# win-cross-dev
Cross Platform Development Setup for Windows

## Enhancing PowerShell with [Scoop](http://scoop.sh)

Scoop is a command line installer for Windows that runs ontop of the PowerShell. You could say it is like [Homebrew](https://brew.sh/) but for Windows. The nice thing is that it installs and manages windows applications and ported Linux tools all in your user environment. It also makes your interaction with PowerShell identical if not very close to a Linux bash shell.

- Open Windows PowerShell
- Run `Set-ExecutionPolicy RemoteSigned -scope CurrentUser`
- Run `iex (new-object net.webclient).downloadstring('https://get.scoop.sh')`
- Add the scoop extras bucket to access more apps

```bash
$ scoop bucket add extras https://github.com/lukesampson/scoop-extras.git
```

- Run the following list of commands in order to install essential packages

```bash
$ scoop install 7zip
$ scoop install curl
$ scoop install cacert
$ scoop install git
$ scoop install grep
$ scoop install gzip
$ scoop install less
$ scoop install sudo
$ scoop install tar
$ scoop install touch
$ scoop install wget
$ scoop install which
$ scoop install sed
```

- Run the following commands to make your PowerShell look awesome

```bash
$ scoop install concfg
$ concfg import solarized small
$ scoop install pshazz
```

- Install essential development tools

```bash
$ scoop install nodejs
$ scoop install yarn
$ scoop install php
$ scoop install openjdk
$ scoop install vcredist2017
$ scoop install openssh
```

- Install some apps to impress your friends

```bash
$ scoop install cowsay
$ scoop install figlet
```

You should now have a very sophisticated PowerShell terminal with a Linux like interaction and package management. Below are some essential `scoop` commands you will be using often.

| Command  | Description                                |
|----------|--------------------------------------------|
| cleanup  | Cleanup apps by removing old versions      |
| depends  | List dependencies for an app               |
| help     | Show help for a command                    |
| install  | Install apps                               |
| list     |  List installed apps                       |
| search   | Search available apps                      |
|  status  | Show status and check for new app versions |
| uninstall| Uninstall an app                           |
| update   | Update apps, or Scoop itself               |
| which    |  Locate a program path                     |

