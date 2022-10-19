# Ansible playbook for Raspberry local cluster

This is a fork from https://github.com/k3s-io/k3s-ansible I don't have the intention to give product support. please refer to main repository for support.

## How to test connectivity to local cluster

`export CLUSTER_PASSWORD="yourpassword"`
`export cluster="raspberry_cluster"`

It would be recommended to create a file named `.secrets` with all the exported secrets for later use. **BECAREFUL NOT TO LEAK SECRETS**

`source .secrets`

If needed, you can also edit `inventory/${cluster}/group_vars/all.yml` to match your environment.

### Calling all servers
```
ansible all -m ping -v -i ansible/inventory --extra-vars "cluster_password=$CLUSTER_PASSWORD"
```

### Calling nodes/cluster separalely

```
ansible raspberry_cluster -m ping -v -i ansible/inventory --extra-vars "cluster_password=$CLUSTER_PASSWORD"
ansible controller -m ping -v -i ansible/inventory --extra-vars "cluster_password=$CLUSTER_PASSWORD"
ansible node -m ping -v -i ansible/inventory --extra-vars "cluster_password=$CLUSTER_PASSWORD"
```

## Execute Playbooks

```
ansible-playbook site.yml -i ansible/inventory/raspberry_cluster/inventory.yml --extra-vars "cluster_password=$CLUSTER_PASSWORD"
```


# Troubleshooting errors

currently trying to solve issue with k3s nodes

```
Oct 02 15:55:36 raspberrypi k3s[13542]: time="2022-10-02T15:55:36-04:00" level=error msg="unable to verify hash for node 'raspberrypi': hash does not match"
Oct 02 15:55:41 raspberrypi k3s[13542]: time="2022-10-02T15:55:41-04:00" level=error msg="unable to verify hash for node 'raspberrypi': hash does not match"

```


## Reset cluster

```
ansible-playbook reset.yml -i ansible/inventory/raspberry_cluster/inventory.yml --extra-vars "cluster_password=$CLUSTER_PASSWORD"
```


## Install ArgoCD

```
kubectl create namespace argocd
kubectl apply -f argocd/manifests/install.yaml
```

## Install metalLB

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.7/config/manifests/metallb-native.yaml

```