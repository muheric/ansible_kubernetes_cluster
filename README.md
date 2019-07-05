# ansible_kubernetes_cluster
Introduction

Kubernetes is a container orchestration system that manages containers at scale. Initially developed by Google based on its experience running containers in production, Kubernetes is open source and actively developed by a community around the world.

Note: This tutorial uses version 1.14 of Kubernetes, the official supported version at the time of this article's publication. For up-to-date information on the latest version, please see the current release notes in the official Kubernetes documentation.

Kubeadm automates the installation and configuration of Kubernetes components such as the API server, Controller Manager, and Kube DNS. It does not, however, create users or handle the installation of operating-system-level dependencies and their configuration. For these preliminary tasks, it is possible to use a configuration management tool like Ansible or SaltStack. Using these tools makes creating additional clusters or recreating existing clusters much simpler and less error-prone.

In this guide, you will set up a Kubernetes cluster from scratch using Ansible and Kubeadm, and then deploy a containerized Nginx application to it.
Goals

Your cluster will include the following physical resources:

    One master node

The master node (a node in Kubernetes refers to a server) is responsible for managing the state of the cluster. It runs Etcd, which stores cluster data among components that schedule workloads to worker nodes.

    Two worker nodes

Worker nodes are the servers where your workloads (i.e. containerized applications and services) will run. A worker will continue to run your workload once they're assigned to it, even if the master goes down once scheduling is complete. A cluster's capacity can be increased by adding workers.

After completing this guide, you will have a cluster ready to run containerized applications, provided that the servers in the cluster have sufficient CPU and RAM resources for your applications to consume. Almost any traditional Unix application including web applications, databases, daemons, and command line tools can be containerized and made to run on the cluster. The cluster itself will consume around 300-500MB of memory and 10% of CPU on each node.

Once the cluster is set up, you will deploy the web server Nginx to it to ensure that it is running workloads correctly.
Prerequisites

    An SSH key pair on your local Linux/macOS/BSD machine. If you haven't used SSH keys before, you can learn how to set them up by following this explanation of how to set up SSH keys on your local machine.

    Three servers running CentOS 7 with at least 2GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user with your SSH key pair. Be sure to also add your public key to the centos user's account on the master node. If you need guidance on adding an SSH key to a particular user account, see this tutorial on How To Set Up SSH Keys on CentOS7.

    Ansible installed on your local machine. For installation instructions, follow the official Ansible installation documentation.

    Familiarity with Ansible playbooks. For review, check out Configuration Management 101: Writing Ansible Playbooks.

    Knowledge of how to launch a container from a Docker image. Look at "Step 5 — Running a Docker Container" in How To Install and Use Docker on CentOS 7 if you need a refresher.

Step 1 — Setting Up the Workspace Directory and Ansible Inventory File

In this section, you will create a directory on your local machine that will serve as your workspace. You will also configure Ansible locally so that it can communicate with and execute commands on your remote servers. To do this, you will create a hosts file containing inventory information such as the IP addresses of your servers and the groups that each server belongs to.

Out of your three servers, one will be the master with an IP displayed as master_ip. The other two servers will be workers and will have the IPs worker_1_ip and worker_2_ip.

Create a directory named ~/kube-cluster in the home directory of your local machine and cd into it:


for more info read : 

https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-centos-7
