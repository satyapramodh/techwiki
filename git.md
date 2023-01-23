## Bash
<pre>
alias gs='git status'
alias gb='git branch' 
alias gf='git fetch' 
alias gap='git add -p' 
alias gal='git add .' 
alias gall='git add . --all' 
alias glog='git log'  
alias gpush='git push $1 $2' 
alias gpr='git pull -r $1 $2' 
alias gck='git checkout $1'
alias gcm='git commit -m $1'
alias gca='git commit --amend'
alias gck='git checkout $1'
alias gbd='git branch -D $1'
alias gckb='git checkout -b $1'
alias gstash='git stash'
alias gstashp='git stash pop'
</pre>  

Git rebase `ours`, `theirs`

https://stackoverflow.com/questions/8146289/how-to-get-their-changes-in-the-middle-of-conflicting-git-rebase


changes to .gitconfig if github is not then only git hosting site you use:
> https://bl.ocks.org/rosswd/e1afd2b0b0d515517eac


## PowerShell
<pre>
# git aliases
function Get-GitStatus { & git status -sb $args }
New-Alias -Name gs -Value Get-GitStatus -Force -Option AllScope
function Get-GitCommit { & git commit -m $args }
New-Alias -Name gcm -Value Get-GitCommit -Force -Option AllScope
function Get-GitCommitAmend { & git commit --amend $args }
New-Alias -Name gca -Value Get-GitCommitAmend -Force -Option AllScope
function Get-GitAdd { & git add . $args }
New-Alias -Name gal -Value Get-GitAdd -Force -Option AllScope
function Get-GitAddP { & git add -p $args }
New-Alias -Name gap -Value Get-GitAddP -Force -Option AllScope
function Get-GitTree { & git log --graph --decorate $args }
New-Alias -Name glog -Value Get-GitTree -Force -Option AllScope
function Get-GitPush { & git push origin $args }
New-Alias -Name gpush -Value Get-GitPush -Force -Option AllScope
function Get-GitPull { & git pull origin $args }
New-Alias -Name gplo -Value Get-GitPull -Force -Option AllScope
function Get-GitFetch { & git fetch $args }
New-Alias -Name gf -Value Get-GitFetch -Force -Option AllScope
function Get-GitCheckout { & git checkout $args }
New-Alias -Name gck -Value Get-GitCheckout -Force -Option AllScope
function Get-GitCheckoutNewBranch { & git checkout -b $args }
New-Alias -Name gckb -Value Get-GitCheckoutNewBranch -Force -Option AllScope
function Get-GitCheckoutDelBranch { & git branch -D $args }
New-Alias -Name gbd -Value Get-GitCheckoutDelBranch -Force -Option AllScope
function Get-GitBranch { & git branch $args }
New-Alias -Name gb -Value Get-GitBranch -Force -Option AllScope
function Get-GitRemote { & git remote -v $args }
New-Alias -Name grv -Value Get-GitRemote -Force -Option AllScope
function Get-GitStash { & git stash $args }
New-Alias -Name gstash -Value Get-GitStash -Force -Option AllScope
function Get-GitStashPop { & git stash pop $args }
New-Alias -Name gstashp -Value Get-GitStashPop -Force -Option AllScope

</pre>
