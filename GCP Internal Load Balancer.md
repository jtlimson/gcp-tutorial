# Overview
GCP offers Internal Load Balancing for your TCP/UDP-based traffic. Internal Load Balancing enables you to run and scale your services behind a private load balancing IP address that is accessible only to your internal virtual machine instances.

In this lab, you create two managed instance groups in the same region. Then, you configure and test an Internal Load Balancer with the instances groups as the backends, as shown in this network diagram:

 ![google](https://cdn.qwiklabs.com/k3u04mphJhk%2F2yM84NjgPiZHrbCuzbdwAQ98vnaoHQo%3D)
 
# Objectives
In this lab, you learn how to perform the following tasks:

Create HTTP and health check firewall rules

Configure two instance templates

Create two managed instance groups

Configure and test an internal load balancer

