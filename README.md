# win-cross-dev
Cross Platform Development Setup for Windows

---

## Table of Contents

- [Enhancing PowerShell with Scoop](#powershell)
- [Enabling the Linux Subsystem for Windows](#linux)
- [EOL](#eol)
- [Configure Git](#git)
- [Configure IDE](#ide)

## Enhancing PowerShell with Scoop <a name="powershell"/>

[Scoop](http://scoop.sh) is a command line installer for Windows that runs ontop of the PowerShell. You could say it is like [Homebrew](https://brew.sh/) but for Windows. The nice thing is that it installs and manages windows applications and ported Linux tools all in your user environment. It also makes your interaction with PowerShell identical if not very close to a Linux bash shell.

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

#### Configuring the shell further

For a simple solarize-themed shell, run the following command:

```bash
$ concfg import solarized small
```

_**For those who still think powershell can never look as hacky as bash or even zsh, read the below, taken from [this blog post](http://cornelcocioaba.ro/wp/2017/04/28/make-powershell-look-like-cmder/)**_

- Create a `.json` theme file anywhere in your system, we'll call ours `mytheme.json`, and add the below code to it:

```json
{
    "black": "#272822",
    "dark_blue": "#01549e",
    "dark_green": "#74aa04",
    "dark_cyan": "#1a83a6",
    "dark_red": "#a70334",
    "dark_magenta": "#89569c",
    "dark_yellow": "#b6b649",
    "gray": "#cacaca",
    "dark_gray": "#7c7c7c",
    "blue": "#0383f5",
    "green": "#8dd006",
    "cyan": "#58c2e5",
    "red": "#f3044b",
    "magenta": "#a87db8",
    "yellow": "#cccc81",
    "white": "#ffffff",
    "screen_colors": "gray,black",
    "popup_colors": "dark_magenta,white",
    "font_face": "Lucida Console",
    "font_true_type": true,
    "font_size": "8x14",
    "font_weight": 0,
    "cursor_size": "small",
    "window_size": "80x25",
    "screen_buffer_size": "80x300",
    "command_history_length": 65,
    "num_history_buffers": 4,
    "quick_edit": true,
    "insert_mode": true,
    "fullscreen": false,
    "load_console_IME": true
}
```
- To use our new theme, we will use `concfg` - which we've previously installed using `scoop install` - by running:

```bash
concfg import \path\to\mytheme.json
```
> :checkered_flag: That's all we need to prettify our shell, but there is more we can do to customize the prompt using another utility we have installed called `pshazz`

- First, create a new theme by running the following command (name your theme however you want):

```bash
pshazz new mytheme
```

- This will open up a the newly generated theme file in `notepad` (your system default text editor). Replace the contents of this file with the following JSON code:

```JSON
{
    "plugins": [ "git", "ssh", "z", "aliases", "dircolors" ],
    "dircolors": {
        "dirs": [
            [".*", "blue", ""]
        ],
        "files": [
            ["(?ix).(7z|zip|tar|gz|rar)$",                        "darkcyan",   ""],
            ["(?ix).(exe|bat|cmd|py|pl|ps1|psm1|vbs|rb|reg)$",    "darkgreen",  ""],
            ["(?ix).(doc|docx|ppt|pptx|xls|xlsx|mdb|mdf|ldf)$",   "magenta",    ""],
            ["(?ix).(txt|cfg|conf|config|yml|ini|csv|log|json)$", "darkyellow", ""],
            ["(?ix).(sln|csproj|sqlproj|proj|targets)$",          "darkred",    ""],
            [".*",                                                "gray",   ""]
        ]
    },
    "prompt": [
        [ "green",  "", "$path" ],
        [ "red",   "", "$git_lbracket$git_branch$git_dirty$git_rbracket" ],
        [ "green", "", "$([char]0x3E)" ]
    ],
    "git": {
        "prompt_dirty":    "*",
        "prompt_lbracket": " (",
        "prompt_rbracket": ")"
    }
}
```

- Save the file, close it, and go back to Powershell and run the following command:

```bash
pshazz use mytheme
```

> :checkered_flag: That's it! Your shell is now colorful almost to a fault, and the results of your commands (such as `ls`) pretty print with custom colors.

## Enabling the Linux Subsystem for Windows  <a name="linux"/>

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

## EOL <a name="eol"/>
One of the most important aspect of cross-platform development is to be consistent with your EOLs in your files. The main objectives for this goal are the following:

- Use Linux style EOL '\n'
- Add EOL at the end of each file
- Use UTF-8 encoding for all text files
- Use tools to enforce these objectives

### Configure Git <a name="git"/>

Git comes with 3 settings

| Setting Name | Value | Description |
---------------|-------|-------------|
| core.autocrlf| true  | Convert all LF to CRLF when checking out and committing code. |
| core.autocrlf| false | Disables CRLF conversion, garbage in/garbage out |
| core.autocrlf| input | Convert all CRLF to LF when committing code |

Normally you want to set the `core.autocrlf` to `false` or `input`.

- When to set it to `false`?
  - This gives you ultimate control
  - Code editor controls EOLs for you
  - Tools such as eslint enforces Linux EOL
  - You have tools and settings that convert CRLF for you
- When to set it to `input`
  - Git has the last say in converting your EOL
  - Whatever happens it will always convert to Linux EOL when committing
  - It may auto-convert LF to CRLF on checkout (needs verification)
  - Code editor and tools may miss some files for enforcing Linux EOL

### Configure IDE <a name="ide"/>

All if not most code editors and IDEs will have an option to select what Line endings to use in your code or on newly created files.

- Set the IDE or code editor to exclusively use Linux EOL
- Set UTF-8 as the default file encoding
- Setup tools like eslint to enforce Linux EOL by flagging Windows EOL as errors
