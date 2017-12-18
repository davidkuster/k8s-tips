# k8s-tips
Notepad for possible best practices / gotchas / etc with Kubernetes

## kubectl aliases

My preference is to never use `kubectl` directly, except when using [Minikube](https://github.com/kubernetes/minikube). Instead I set up bash aliases for each cluster/namespace I access, which ensures I know what environment each kubectl command is running against. This also allows each command to run against a different environment without needing to run an intermediary command in between to change envs.

Your setup will be dependent upon the specific k8s setup for your org and team(s) of course. I currently access 4 different clusters across 2 different teams. My current preference is to use 2-char abbreviations to indicate team. In this scenario we'll call them teams A and B, and for the example use 2 char abbreviations `aa` and `bb` to distinguish.

Possible alias setup per team might be:

    # Team A k8s env shortcuts
    alias k8s-aa-dev="kubectl --kubeconfig=${HOME}/.kube/config.aa-dev --namespace=dev"
    alias k8s-aa-qa="kubectl --kubeconfig=${HOME}/.kube/config.aa-dev --namespace=qa"
    alias k8s-aa-stage="kubectl --kubeconfig=${HOME}/.kube/config.aa-prod --namespace=stage"
    alias k8s-aa-production="kubectl --kubeconfig=${HOME}/.kube/config.aa-prod --namespace=production"

    # Team A k8s default namespaces
    alias k8s-aa-dev-default="kubectl --kubeconfig=${HOME}/.kube/config.aa-dev --namespace=default"
    alias k8s-aa-prod-default="kubectl --kubeconfig=${HOME}/.kube/config.aa-prod --namespace=default"

    # Team A k8s system namespaces
    alias k8s-aa-dev-sys="kubectl --kubeconfig=${HOME}/.kube/config.aa-dev --namespace=kube-system"
    alias k8s-aa-prod-sys="kubectl --kubeconfig=${HOME}/.kube/config.aa-prod --namespace=kube-system"

    # Team B setup - basically the same as above, customized for the variations specific to team B

Note:

- If config files and namespaces are standard a bash function might be simpler.

- If you only access k8s for a single team, the `-aa` or `-bb` differentiator is probably unnecessary.

- If you'd prefer a couple less keystrokes per command something like `k-*` or `k*` instead of `k8s-*` is viable. However, note that in the example above the alias for prod is `k8s-aa-production`. I intentionally made it longer to not only accurately match the real namespace, but also to give myself a couple extra mental cycles as I type it out to ensure that, actually yes, I do want to run this command against _production_. :)

Regardless, the idea of the command indicating the env is to me the key part of this.
