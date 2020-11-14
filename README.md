# k8sPi-inTheSky
Repository to store anything used for turning rasberry pi's running HypriotOS into a K3s cluster, minecraft server, and setup monitoring

## K3s Cluster Setup
To setup up K3s on the rasberry pi's, run through the following steps:
- Download the k3s-ansible-master folder contents
- Edit the Ansible inventory file k3s-ansible-master/inventory/sample/hosts.ini, and replace the IPs of your master and nodes
- Edit the k3s-ansible-master/inventory/sample/group_vars/all.yml file and change the ansible_user to the correct username
- Run ansible-playbook site.yml -i k3s-ansible-master/inventory/hosts.ini and wait

To connect to the cluster from your local machine, once it's built, you need to grab the kubectl configuration from the master node: scp <username>@<masterNodeIPValue>:~/.kube/config ~/.kube/<configFileName>

The original source can be found here: https://github.com/rancher/k3s-ansible

## Minecraft Server Setup
To setup the K3s cluster as a Minecraft server, download kubernetes-pi-minecraft.yaml, and run it as an Ansible playbook

The Helm chart that is used in the playbook to setup the cluster as a minecraft server can be found here: https://github.com/itzg/minecraft-server-charts/tree/master/charts/minecraft 

## Prometheus and Grafana Setup
To setup monitoring for the K3s cluster and everything running on the cluster, go through these steps:
- Log into the master K3s node: ssh <username>@<masterNodeIPValue>
- Switch to the root user
- Install prerequisite tools: apt-get update && apt-get install -y build-essential golang
- Download the cluster-monitoring-master folder contents
- Edit the vars.jsonnet file, tweaking the IP addresses to servers in your cluster, and enabling the k3s option and armExporter
- Run make vendor && make
- Run the final commands in the Quickstart (see below) for K3s guide to kubectl apply manifests, and wait for everything to roll out

The original source can be found here: https://github.com/carlosedp/cluster-monitoring