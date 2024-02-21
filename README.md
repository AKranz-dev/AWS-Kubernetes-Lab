# Welcome to AWS-Kubernetes-Lab!

Hi! AWS-Kubernetes-Lab is an AWS CloudFormation stack template that bootstraps a 3-node Kubernetes cluster on EC2. AWS-Kubernetes-Lab makes it easy to begin working with an EC2-hosted kubernetes cluster in minutes. AWS-Kubernetes-Lab fully automates cluster provisioning and configuration, so you can immediately begin running kubectl commands when you SSH to the control plane. No manual configuration is required!


# How to use
- Simply deploy with CloudFormation. AWS-Kubernetes-Lab is written to allow deployments in any region, and does not require any parameters be given to run.
- Wait until all resources are created, and the CloudFormation stack enters the CREATE_COMPLETE state.
- Connect to the newly created k8s-control EC2 using Systems Manager. You are able to connect immediately as the agent is already installed.
  - Alternatively, you can you an SSH client of your choice. The SSH private key is stored in  AWS Systems Manager Parameter Store as: /ec2/keypair/{key_pair_id}.
- You can begin running kubectl commands immediately, with no further configuration required. Enjoy!



# How it works

- AWS-Kubernetes-Lab provisions 3 EC2 instances to host the kubernetes cluster (a control plane, and 2 worker nodes), and supporting resources.
- All system and Kubernetes configurations are scripted in the UserData section of each EC2 instance. 
- The UserData script on the control plane EC2 calls Systems Manager at the end of it's configuration to pass the cluster join command to the worker nodes.
- The CloudFormation stack is lightweight, only provisioning resources as absolutely necessary.



# Specifications
- **AWS-Kubernetes-Lab.yaml**
  - 3 EC2 instances
  - EC2 instance profile and IAM role
  - Security groups for the control node and worker nodes, and their ingress/egress rules.
  - An EC2 key pair
- **EC2 instances**:
   - OS: Ubuntu 20.04 Focal Fossa
   - Size: t2.medium
   - Packages:
        - curl
        - python3-pip
        - aws-cfn-bootstrap
        - awscliv2
        - unzip
        - kubeadm
        - kubectl
        - kubelet
- **Kubernetes**:
  - Version: latest
  - Container Runtime: containerd
  - Pod Networking Plugin: Calico



# Background
 For a weekend project, I wanted to write some IaC that would bootstrap all infarastructure and server configuration required to run a Kubernetes cluster on EC2. It sounded like a fun automation challenge, not to mention I now have a super easy way to spin up K8s clusters within minutes. 
 
 This is a great tool to have for my home lab, as it allows me to perform sandboxing in Kubernetes. I can experiment with different plugins and applications, easily standing up and tearing down clusters.

 Finally, in terms of cost, its a no-brainer. As long as you turn off your EC2 instances when you're not using them, you're looking at dollars and cents for a monthly cost. Well-worth the value that comes with hands-on experience with Kubernetes and it's related technologies.

