# Automating the creation and deployment of a Kubernetes cluster on AWS EC2 instances ⚡
![maxresdefault](https://https://github.com/paularinzee/Ansible-Terraform-K8S-Ec2-Deployment)

---

## Overview ☀️
This repository contains the necessary automation scripts to provision a Kubernetes cluster from scratch on AWS EC2 instances. It leverages Terraform for infrastructure provisioning and Ansible for automating Kubernetes cluster deployment.

## Architecture 🔻

```
loco@loco-Inspiron-3576:~/projects/Ansible-terraform$ tree
.
├── ansible.cfg
├── Cluster-with-Bash
│   ├── basicscript.sh
│   └── masterscript.sh
├── inventory
│   └── hosts.txt
├── main-playbook.yaml
├── roles
│   ├── master-node
│   │   └── tasks
│   │       └── main.yaml
│   └── worker-node
│       └── tasks
│           └── main.yaml
└── terraform
    ├── backend.tf
    ├── instance.tf
    ├── outputs.tf
    ├── provider.tf
    ├── rt.tf
    ├── sg.tf
    ├── variables.tf
    └── vpc.tf

```




# Prerequisites ✔️
- AWS Account with access to EC2, VPC.
- Terraform installed on your local machine.
- Ansible installed on your local machine.
-  #### SSH keypair for EC2 instance access. This is Important Because i have downloaded the keypair in this path
-  https://github.com/paularinzee/Ansible-Terraform-K8S-Ec2-Deployment/blob/dc08fadba91664ec370d80287c7bc364e0a093c8/roles/master-node/tasks/main.yaml#L155
-  #### Please Change this location in the project for your desired path.
   
- S3 bucket to save the statefile in, in mycase you can check 

## Quick Start 🤜

1. Clone the repository:
   `git clone https://github.com/paularinzee/Ansible-Terraform-K8S-Ec2-Deployment`

2. Navigate to the Terraform directory 
   ` cd terraform `
   
3. Create S3 bucket and Change the name of variable `BUCKETNAME` in `/terraform/variables.tf`  
   
4. initialize the Terraform environment
   ` terraform init `
5. Now Everything is read to apply
   `terraform apply --auto-approve`
   
6. Grab a cup of coffee until this ends. ☕


## Once the infrastructure is provisioned, update the Ansible hosts file with the new EC2 instance IP addresses:

- You can Navigate to the hosts file by this command
  `vi ../inventory/hosts.txt`
- Assuming you are in terraform directory

### Update the hosts file <a href="https://github.com/paularinzee/Ansible-Terraform-K8S-Ec2-Deployment/blob/main/inventory/hosts.txt" target="_blank">Here</a>

#### If you cleared the shell by wrong you still can get the IPs by the following commands
```
terraform output -raw master-node-ip 
terraform output -raw worker-node-ip
```

## Action Time ! ✴️ 

- First Navigate to main project directory
- `cd ..` if you are still in terraform directory.
  
- ### Then Run the Magical Command 🌠
  ```
   ansible-playbook main-playbook.yaml -i inventory/hosts.txt
  ```
- Now give him like 10 Mins to Get everything done.


### Go to your Terminal and ssh to the master node 
```
chmod 400 <keypair file location>
ssh -i <keypair location> ubuntu@<instanceip>
```
### then check the kubernetes nodes
```
sudo kubectl get nodes
```

The end 👷