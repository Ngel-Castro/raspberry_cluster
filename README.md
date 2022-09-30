# Ansible playbook for Raspberry local cluster

## How to test connectivity to local cluster

`export CLUSTER_PASSWORD="yourpassword"`

It would be recommended to create a file named `.secrets` with all the exported secrets for later use. **BECAREFUL NOT TO LEAK SECRETS**

`source .secrets`

### Calling all servers
```
ansible all -m ping -v -i ansible/inventory --extra-vars "cluster_password=$CLUSTER_PASSWORD"
```

### Calling nodes/cluster separalely

```
ansible raspberry_cluster -m ping -v -i ansible/inventory --extra-vars "cluster_password=$CLUSTER_PASSWORD"
ansible controllers -m ping -v -i ansible/inventory --extra-vars "cluster_password=$CLUSTER_PASSWORD"
ansible nodes -m ping -v -i ansible/inventory --extra-vars "cluster_password=$CLUSTER_PASSWORD"
```