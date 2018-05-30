# Getting Started with K8s On DC/OS 
The purpose of this repo is to help users get started using Kubernetes on DC/OS. All of this is based on the documentation for running Kubernetes on DC/OS which can be found at the URL Below. This takes you through how to setup an HA cluster with access to DC/OS PKI and is an advanced installation so please keep in mind that there a simpler way if desired. *You need to have the DC/OS CLI Installed* to install K8s from the CLI and so you can configure your kubectl CLI to use correct configs. The options.json installs the Kubernetes cluster in HA Mode across 4 nodes with 20 CPU, 20GB MEM and 100GB Disk on each worker. Feel free to modify to fit your needs. 

[Official Docs for Latest at time of writing](https://docs.mesosphere.com/services/kubernetes/1.0.3-1.9.7/)


### Installing in HA Mode with DC/OS PKI (Enterprise ONLY): 
Add the K8s Service Account so that use can interact with Secrets Store etc...

```sh
dcos security org service-accounts keypair private-key.pem public-key.pem
dcos security org service-accounts create -p public-key.pem -d 'Kubernetes service account' kubernetes
dcos security secrets create-sa-secret private-key.pem kubernetes kubernetes/sa
dcos security org groups add_user superusers kubernetes
```

### HA Mode with Service Account 
```sh
dcos package install kubernetes --options=options.json 
```
- The kube_cpus, kube_mem, kube_disk is the amount given on each worker node to run services on Kubernetes. Set this to fit your needs with the understanding that Kubernetes will use a little to run components such as kubelet. 

You should see multiple tasks deploy under the kubernetes service such as etcd, kubelet, scheduler, api, kube-controller, coredns, etc... as well as a kube-proxy service. It will take a few minutes to complete this. 

### URL for Kube Dashboard (Current)
http://${DCOS_URL}/service/kubernetes-proxy/

### Setting up kubectl
DC/OS CLI will automatically setup your kubectl
```sh 
dcos kubernetes kubeconfig
```
Will create your context based on your Cluster Name by default. You can set this as your default in the event you need to manage multiple Kubernetes clusters. 

```sh
kubectl get nodes
```

You should be able to see your worker nodes now!
