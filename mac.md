## For terminal autocomplete
Put this in your `~/.inputrc`:

`set show-all-if-ambiguous on`

You'll need to restart your shell (for example by opening a new terminal window or typing exec /bin/bash).

## Happy git env
https://gist.github.com/trey/2722934

### .bash_profile

```shell
export PATH="$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"

if [ -f $(brew --prefix)/etc/bash_completion ]; then
. $(brew --prefix)/etc/bash_completion
fi

[ -f /usr/local/etc/bash_completion ] && . /usr/local/etc/bash_completion

### https://github.com/magicmonty/bash-git-prompt
if [ -f "/usr/local/opt/bash-git-prompt/share/gitprompt.sh" ]; then
    __GIT_PROMPT_DIR="/usr/local/opt/bash-git-prompt/share"
    source "/usr/local/opt/bash-git-prompt/share/gitprompt.sh"
fi

### git autocomplete
if [ -f `brew --prefix`/etc/bash_completion.d/git-completion.bash ]; then
  . `brew --prefix`/etc/bash_completion.d/git-completion.bash
fi


GIT_PS1_SHOWDIRTYSTATE=true
export PS1='[\u@mbp \w$(__git_ps1)]\$ '
```
