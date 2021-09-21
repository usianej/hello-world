# Integrating Kubernetes cluster with Ansible

1. Login to ansible server and copy public key onto kubernetes cluseter master account 
from the k8 maagement console ssh into the k8 master node --> sudo and become root, go to the ~/.ssh/authorized_key and paste the id_rsa.pub from the ansible server on it.
# you must ssh as the paticular user u used the key from
ssh -i ~/.ssh/id_rsa root@3.15.196.52  # from ansible server

# ssh -i ~/.ssh/id_rsa admin@api.demo.k8.ehonor.com #from kuberneties mht console to master server



1. Update hosts file with new group called kubernetes and add kubernetes master in that. 

1. Create ansible playbooks to create **[deployment](https://github.com/yankils/Simple-DevOps-Project/blob/master/Kubernetes/kubernetes-valaxy-deployment.yml)** and **[services](https://github.com/yankils/Simple-DevOps-Project/blob/master/Kubernetes/kubernetes-valaxy-service.yml)** 
		
1.  Check for pods, deployments and services on kubernetes master
    ```sh 
    kubectl get pods -o wide 
    kubectl get deploy -o wide
    kubectl get service -o wide
    ```
	
1. Access application suing service IP
   ```sh
   wget <kubernetes-Master-IP>:31200
   ```
 
