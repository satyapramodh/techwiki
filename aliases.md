# Alias keyboard shortcuts

```
alias k="kubectl" 
alias kctx="kubectx"
alias kns="kubens"
alias ti="terraform init"
alias tiu="terraform init --upgrade"
alias tp="terraform plan"

# kubectl
alias kdp='k describe pod -n NAMESPACE $1'
alias klogs='k logs -n NAMESPACE $1'
alias kgp='k get pods -n NAMESPACE'
alias kdelp='k delete pod $1 --namespace NAMESPACE'

alias awsssm='aws ssm start-session --target $1'

```
