git-color-scheme
================

Bash Color Scheme for GIT

No files for this repo, only simple instructions on how to sintax highlight your favorite git repos.


You can check if the .bash_profile file exists by (MACOSX and I believe most Linux distros):

```
cat /Users/yourmachineuser/.bash_profile
```

If the cat returns empty, then you will need to create a new file:

```
touch /Users/yourmachineuser/.bash_profile
```

Now that you have the file created, edit it and add the following:

```
if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
  c_reset=`tput sgr0`
  c_user=`tput setaf 2; tput bold`
  c_path=`tput setaf 4; tput bold`
  c_git_clean=`tput setaf 2`
  c_git_dirty=`tput setaf 1`
else
  c_reset=
  c_user=
  c_path=
  c_git_clean=
  c_git_dirty=
fi

git_prompt ()
{
  if ! git rev-parse --git-dir > /dev/null 2>&1; then
    return 0
  fi

  git_branch=$(git branch 2>/dev/null| sed -n '/^\*/s/^\* //p')

  if git diff --quiet 2>/dev/null >&2; then
    git_color="${c_git_clean}"
  else
    git_color=${c_git_dirty}
  fi

  echo " [$git_color$git_branch${c_reset}]"
}
export PS1="\[\033[0;32m\]\u@\h\[\033[0;39m\]:\[\033[0;35m\]\w\[\033[0;39m\]\$"
#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Shell prompt format => [1:0:502][$USER@hostname:][~]
#=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AAPS[0]="\n\e[1;30m[\e[0;37m${SHLVL}\e[1;30m:\e[0;37m\j:\!\e[1;30m][\e[1;34m\u\e[0;34m@\e[1;34m\h\e[1;30m:\e[1;37m${SSHTTY/\/dev\/}\e[1;30m]\e[0;37m[\e[0;37m\w\e[0;37m]\$(git_prompt)\e[1;37m\n\[${R}\]\$ ";
AAPS[1]='\n\e[1;30m[\e[0;37m${SHLVL}\e[1;30m:\e[0;37m\j:\!\e[1;30m][\e[0;32m\u\e[1;32m@\e[0;32m\h\e[1;30m:\e[1;37m${SSHTTY/\/dev\/}\e[1;30m]\e[0;37m[\e[0;37m\w\e[0;37m]\e[1;37m\n\[${R}\]\$ ';
AAPS[2]='\n\e[1;30m[\e[0;37m${SHLVL}\e[1;30m:\e[0;37m\j:\!\e[1;30m][\e[0;35m\u\e[1;35m@\e[0;35m\h\e[1;30m:\e[1;37m${SSHTTY/\/dev\/}\e[1;30m]\e[0;37m[\e[0;37m\w\e[0;37m]\e[1;37m\n\[${R}\]\$ ';
: ${PLVL=0};
[[ "${#AAPS[@]}" -lt "$PLVL" || "${#AAPS[@]}" -eq "$PLVL" ]] && PLVL=0;
export PS1=${AAPS[$PLVL]} && (( PLVL++ )) && export PLVL;

```

Restart your terminal and change directories to one of your git repos. You will see that the format has changed to:

```
[time][username@machine:][currentdir][branch]
```

Example:

```
[1:00:169][username@machine:][~/www/git-color-scheme][master]
```

The colors of the branch will change to RED when the branch has changes and to GREEN when there are no changes or it is stable.

The original bash_profile has been created by: https://github.com/kryptek/
