# Tektoncd Ansible Runner Examples

A repo to hold Ansible runner examples for the Tektoncd Task `ansible-runner`

## Namespace Context
```shell
kubectl create ns tekton-temp && \
kubectl config set-context --current --namespace=tekton-temp
```

## Common Tasks

```shell
tkn hub install task git-clone --version 0.9
tkn hub install task ansible-runner --version 0.2

kubectl apply -f https://raw.githubusercontent.com/micha-aucoin/ansible-runner-temp-db/master/playbooks-pvc.yaml
```

## Examples

Run the following Task to clone this repository

```shell
tkn task start git-clone \
  --workspace=name=output,claimName=ansible-playbooks \
  --param=url=https://github.com/micha-aucoin/ansible-runner-temp-db.git \
  --param=revision=master \
  --param=deleteExisting=true \
  --showlog
```

### Service Account

You need proper RBAC in Kubernetes to allow it to perform the example tasks:

```shell
kubectl apply -f https://raw.githubusercontent.com/micha-aucoin/ansible-runner-temp-db/master/kubernetes/ansible-deployer.yaml
```

### Listing pods

```shell
 tkn task start ansible-runner \
   --serviceaccount ansible-deployer-account \
   --param=project-dir=kubernetes \
   --param=args=-p,list-pods.yml \
   --workspace=name=runner-dir,claimName=ansible-playbooks \
   --showlog
```

### Create Deployment

```shell
 tkn task start ansible-runner \
   --serviceaccount ansible-deployer-account \
   --param=project-dir=kubernetes \
   --param=args=-p,create-deployment.yml \
   --workspace=name=runner-dir,claimName=ansible-playbooks \
   --showlog
```

### Create Service

```shell
 tkn task start ansible-runner \
   --serviceaccount ansible-deployer-account \
   --param=project-dir=kubernetes \
   --param=args=-p,create-service.yml \
   --workspace=name=runner-dir,claimName=ansible-playbooks \
   --showlog
```

### Delete Deployment

```shell
 tkn task start ansible-runner \
   --serviceaccount ansible-deployer-account \
   --param=project-dir=kubernetes \
   --param=args=-p,delete-deployment.yml \
   --workspace=name=runner-dir,claimName=ansible-playbooks \
   --showlog
```

### Delete Service

```shell
 tkn task start ansible-runner \
   --serviceaccount ansible-deployer-account \
   --param=project-dir=kubernetes \
   --param=args=-p,delete-service.yml \
   --workspace=name=runner-dir,claimName=ansible-playbooks \
   --showlog
```
