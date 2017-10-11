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

## Enabling the Linux Subsystem for Windows

Would it not be great if we literally had native Linux environment on Windows? Well it seems [pigs flew](https://en.wikipedia.org/wiki/When_pigs_fly)! Windows 10 64bit on the Anniversary edition has a beta Linux Subsystem available. When enabled it has a full featured Linux subsystem in your local user directory with an Ubuntu bash shell. On this shell you can do all Ubuntu command-line actions such as install apps using `apt-get`. What is also nice is that the Windows and Linux subsystems can see and communicate with each other (filesystem and networking). This makes it ideal to run Linux only services, commands and what not without having to install a Virtual Machine.

- You must have a 64-bit version of Windows 10 Anniversary Update or later (build 1607+)
- Follow the instructions on the [HowToGeek](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) site
- When asked to create a Unix username, do so and use a 1 word name
  - It would also be wise to use the same password as your windows password, unless you want to memorize 2 different passwords
- Follow the instructions and also install the Ubuntu Font
- Once the installation is all completed you will notice the terminal has some pretty nasty flurecent colors
  - This is because it's running on the Windows filesystem and UID and GID bits are set in many places

Try the following to install the Solarized Dark theme for the terminal

```bash
$ cd ~
$ wget --no-check-certificate https://raw.github.com/seebi/dircolors-solarized/master/dircolors.ansi-dark
$ mv dircolors.ansi-dark .dircolors
$ eval `dircolors ~/.dircolors`
```

You might want to decorate your terminal to show Git information as well since you will most likely store your git repo outside of the Linux sunsystem.

- Open the file `.bashrc` and replace the following code blocks

Add This

```bash
parse_git_branch() {
 git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
if [ "$color_prompt" = yes ]; then
 PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
else
 PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(parse_git_branch)\$ '
fi
unset color_prompt force_color_prompt
```

In place of this

```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt
```
