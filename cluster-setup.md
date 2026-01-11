# Amazon EKS Cluster Setup Guide

This document provides step-by-step instructions to set up an
Amazon EKS (Elastic Kubernetes Service) cluster on AWS using
eksctl, kubectl, and AWS CLI.

--------------------------------------------------

Prerequisites

- AWS Account
- One EC2 instance (Ubuntu recommended)
- IAM user with EKS, EC2, IAM, VPC permissions
- Internet access
- Docker installation
- Create container
- Create image using containerID
- Give a name to the "Untagged image"
- login
- Push on your docker.hub repo
--------------------------------------------------

Step 1: Launch an EC2 Instance

Launch one Ubuntu EC2 instance and connect to it via SSH.

--------------------------------------------------

Step 2: Install eksctl CLI

eksctl is a command-line tool for creating and managing EKS clusters.

Command:

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin

Verify:

eksctl version

--------------------------------------------------

Step 3: Install kubectl

kubectl is used to interact with Kubernetes clusters.

Command:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

Optional (local user setup):

chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl

Verify:

kubectl version --client

--------------------------------------------------

Step 4: Install AWS CLI on Ubuntu

Command:

sudo apt update
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

Verify:

aws --version

--------------------------------------------------

Step 5: Configure AWS CLI

Command:

aws configure

Enter:
- AWS Access Key ID
- AWS Secret Access Key
- Default region (example: ap-southeast-1)
- Output format: json

--------------------------------------------------

Step 6: Create Amazon EKS Cluster

Command:

eksctl create cluster \
--name eks-oncdecb31 \
--region ap-southeast-1 \
--version 1.32 \
--nodegroup-name linux-nodes \
--node-type t2.medium \
--nodes 2

Note:
Cluster creation may take 10–15 minutes.

--------------------------------------------------

Step 7: Connect to the EKS Cluster

Command:

aws eks update-kubeconfig --name eks-oncdecb31 --region ap-southeast-1

Verify:

kubectl get nodes

--------------------------------------------------

Step 8: Delete EKS Cluster (Cleanup)

Command:

eksctl delete cluster --name eks-oncdecb31 --region ap-southeast-1

--------------------------------------------------

Conclusion

You have successfully:
- Installed eksctl, kubectl, and AWS CLI
- Created and connected to an Amazon EKS cluster
- Deleted the cluster safely to avoid charges

--------------------------------------------------

Recommended GitHub File Path:

docs/eks-cluster-setup.md
