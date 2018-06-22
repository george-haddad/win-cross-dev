# win-cross-dev
[![All Contributors](https://img.shields.io/badge/all_contributors-0-orange.svg?style=flat-square)](#contributors)

Cross Platform Development Setup for Windows

---

## Table of Contents

* [Enhancing PowerShell with Scoop](#powershell)
* [Enabling the Linux Subsystem for Windows](#linux)
* [EOL](#eol)
* [Configure Git](#git)
* [Configure IDE](#ide)
* [Node Modules](#node)

## Enhancing PowerShell with Scoop <a name="powershell"/>

[Scoop](http://scoop.sh) is a command line installer for Windows that runs ontop of the PowerShell. You could say it is like [Homebrew](https://brew.sh/) but for Windows. The nice thing is that it installs and manages windows applications and ported Linux tools all in your user environment. It also makes your interaction with PowerShell identical if not very close to a Linux bash shell.

* Open Windows PowerShell
* Run `Set-ExecutionPolicy RemoteSigned -scope CurrentUser`
* Run `iex (new-object net.webclient).downloadstring('https://get.scoop.sh')`
* Add the scoop extras bucket to access more apps

```bash
scoop install git
# or you can get it bundled with openssh
scoop install git-with-opensssh
scoop bucket add extras https://github.com/lukesampson/scoop-extras.git
```

* Run the following list of commands in order to install essential packages

```bash
scoop install 7zip
scoop install curl
scoop install cacert
scoop install grep
scoop install gzip
scoop install less
scoop install sudo
scoop install tar
scoop install touch
scoop install wget
scoop install which
scoop install sed
```

* There are also some interesting packages to install that contain a group of binaries

```bash
scoop install coreutils
scoop install diffutils
scoop install findutils
scoop install pciutils
```

* Run the following commands to make your PowerShell look awesome

```bash
scoop install concfg
scoop install pshazz
```

* Install essential development tools
* Note that the -g option is to install globally instead of locally only to user

```bash
scoop install -g nodejs-lts
scoop install -g yarn
scoop install -g php
scoop install -g openjdk
scoop install -g vcredist2017
```

* Install some apps to impress your friends

```bash
scoop install cowsay
scoop install figlet
```

You should now have a very sophisticated PowerShell terminal with a Linux like interaction and package management. Below are some essential `scoop` commands you will be using often.

| Command   | Description                                |
| --------- | ------------------------------------------ |
| cleanup   | Cleanup apps by removing old versions      |
| depends   | List dependencies for an app               |
| help      | Show help for a command                    |
| install   | Install apps                               |
| list      | List installed apps                        |
| search    | Search available apps                      |
| status    | Show status and check for new app versions |
| uninstall | Uninstall an app                           |
| update    | Update apps, or Scoop itself               |
| which     | Locate a program path                      |

### Configuring the shell further

_**For those who still think powershell can never look as hacky as bash or even zsh, allow me to prove you wrong:**_

* First, use the utility `concfg` we've installed previously to search for a nice theme preset:

```bash
concfg presets
```

* You can try all of these themes using the command below. The `powershell-defaults` one works great if you're not sure.

```bash
concfg import powershell-defaults
```

> :checkered_flag: That's all we need to prettify our shell, but there is more we can do to customize the prompt using another utility we have installed called `pshazz`

* There are a lot of default themes built into pshazz, and it is fairly easy to create you own by running:

```bash
pshazz new mytheme
```

where `mytheme` is any name you want.

* Once you are done editing your theme's colors and custom prompt text, you can use it with the following command:

```bash
pshazz use mytheme
```

A theme developed by @Faultless kicks it up a notch. You can get it by downloading the JSON file:

```bash
curl -O https://raw.githubusercontent.com/Faultless/pshazz/master/themes/xpander.json
mv ~/xpander.json ~/pshazz/xpander.json
```

And then use the theme:

```bash
pshazz use xpander
```

_**Concerning the powershell font**, you will need to download and install the [Patched Deja Vu Sans Mono font for Powerline](https://github.com/powerline/fonts/blob/master/DejaVuSansMono/DejaVu%20Sans%20Mono%20for%20Powerline.ttf) in order to display special characters required by some themes (e.g. pshazz, agnoster).
You can use this font in powershell from the **top left menu** -> **Defaults Menu** -> **Font**_

> :checkered_flag: That's it! Your shell is now colorful almost to a fault, and the prompt has that \*nix look and feel we all love.

![pimped-powershell](https://user-images.githubusercontent.com/3069650/32433425-8e816f28-c2e3-11e7-9763-6ffec75117f8.png)

## Enabling the Linux Subsystem for Windows <a name="linux"/>

Would it not be great if we literally had native Linux environment on Windows? Well it seems [pigs flew](https://en.wikipedia.org/wiki/When_pigs_fly)! Windows 10 64bit on the Anniversary edition has a beta Linux Subsystem available. When enabled it has a full featured Linux subsystem in your local user directory with an Ubuntu bash shell. On this shell you can do all Ubuntu command-line actions such as install apps using `apt-get`. What is also nice is that the Windows and Linux subsystems can see and communicate with each other (filesystem and networking). This makes it ideal to run Linux only services, commands and what not without having to install a Virtual Machine.

* You must have a 64-bit version of Windows 10 Anniversary Update or later (build 1607+)
* Follow the instructions on the [HowToGeek](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) site
* When asked to create a Unix username, do so and use a 1 word name
  * It would also be wise to use the same password as your windows password, unless you want to memorize 2 different passwords
* Follow the instructions and also install the Ubuntu Font
* Once the installation is all completed you will notice the terminal has some pretty nasty flurecent colors
  * This is because it's running on the Windows filesystem and UID and GID bits are set in many places

Try the following to install the Solarized Dark theme for the terminal

```bash
cd ~
wget --no-check-certificate https://raw.githubusercontent.com/seebi/dircolors-solarized/master/dircolors.256dark
mv dircolors.256dark .dircolors
eval `dircolors ~/.dircolors`
```

You might want to decorate your terminal to show Git information as well since you will most likely store your git repo outside of the Linux sunsystem.

* Open the file `.bashrc` and replace the following code blocks

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

Your console window should look like this
![linux console preview](https://user-images.githubusercontent.com/3069650/32433129-a16c7e26-c2e2-11e7-9fd3-2f9b572d0980.gif)

## EOL <a name="eol"/>

One of the most important aspect of cross-platform development is to be consistent with your EOLs in your files. The main objectives for this goal are the following:

* Use Linux style EOL '\n'
* Add EOL at the end of each file
* Use UTF-8 encoding for all text files
* Use tools to enforce these objectives

### Configure Git <a name="git"/>

Git comes with 3 settings

| Setting Name  | Value | Description                                                   |
| ------------- | ----- | ------------------------------------------------------------- |
| core.autocrlf | true  | Convert all LF to CRLF when checking out and committing code. |
| core.autocrlf | false | Disables CRLF conversion, garbage in/garbage out              |
| core.autocrlf | input | Convert all CRLF to LF when committing code                   |

Normally you want to set the `core.autocrlf` to `false` or `input`.

* When to set it to `false`?
  * This gives you ultimate control
  * Code editor controls EOLs for you
  * Tools such as eslint enforces Linux EOL
  * You have tools and settings that convert CRLF for you
* When to set it to `input`
  * Git has the last say in converting your EOL
  * Whatever happens it will always convert to Linux EOL when committing
  * It may auto-convert LF to CRLF on checkout (needs verification)
  * Code editor and tools may miss some files for enforcing Linux EOL

### Configure IDE <a name="ide"/>

All if not most code editors and IDEs will have an option to select what Line endings to use in your code or on newly created files.

* Set the IDE or code editor to exclusively use Linux EOL
* Set UTF-8 as the default file encoding
* Setup tools like eslint to enforce Linux EOL by flagging Windows EOL as errors

### Node Modules <a name="node"/>

There are some useful modules on npm that make cross platform development much easier. Here are a few with their core uses

* [cross-env](https://www.npmjs.com/package/cross-env)
  * Use environment variables without worrying what platform you are on
* [shx](https://www.npmjs.com/package/shx)
  * Writing one-off unix commands that can run on multi-platforms

## Contributors

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
<!-- ALL-CONTRIBUTORS-LIST:END -->