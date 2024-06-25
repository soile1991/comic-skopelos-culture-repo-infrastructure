<!-- https://fluxcd.io/flux/guides/image-update/ -->

# Single node kubernetes cluster

### Step 1 install k3s

```
curl -sfL https://get.k3s.io | sh -
```


### Step 2 use k3s without sudo

```
export KUBECONFIG=~/.kube/config
mkdir ~/.kube 2> /dev/null
sudo k3s kubectl config view --raw > "$KUBECONFIG"
chmod 600 "$KUBECONFIG"
```

note that You can add KUBECONFIG=~/.kube/config to your ~/.profile or ~/.bashrc to make it persist on reboot.

https://devops.stackexchange.com/questions/16043/error-error-loading-config-file-etc-rancher-k3s-k3s-yaml-open-etc-rancher

### Step 3 install flux


```
curl -s https://fluxcd.io/install.sh | sudo bash
```

### Step 4 configure flux with auto updates

```
export GITHUB_USER=soile1991
```

if you update flux
```
flux install
flux uninstall
```

```
flux bootstrap github \
  --components-extra=image-reflector-controller,image-automation-controller \
  --owner=$GITHUB_USER \
  --repository=comic-vatopedi-culture-repo-infrastructure \
  --branch=main \
  --path=clusters/cultural-repo-cluster \
  --read-write-key \
  --personal \
  --token-auth
  
  ```


for registry credentials

https://chris-vermeulen.com/using-gitlab-registry-with-kubernetes/



### Show available resurces 
```
kubectl api-resources
```

### flux reconcile
```
flux reconcile kustomization flux-system --with-source
```

