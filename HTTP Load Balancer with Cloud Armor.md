# Overview
GCP HTTP(S) load balancing is implemented at the edge of Google's network in Google's points of presence (POP) around the world. 
User traffic directed to an HTTP(S) load balancer enters the POP closest to the user and is then load balanced over Google's global network to the closest backend that has sufficient capacity available.

Cloud Armor IP blacklists/whitelists enable you to restrict or allow access to your HTTP(S) load balancer at the edge of the Google Cloud, as close as possible to the user and to malicious traffic. 
This prevents malicious users or traffic from consuming resources or entering your virtual private cloud (VPC) networks.

In this lab, you configure an HTTP Load Balancer with global backends, as shown in the diagram below. 
Then, you stress test the Load Balancer and blacklist the stress test IP with Cloud Armor.

![google](https://cdn.qwiklabs.com/7wJtCqbfTFLwKCpOMzUSyPjVKBjUouWHbduOqMpfRiM%3D)


# Objectives
In this lab, you learn how to perform the following tasks:

Create HTTP and health check firewall rules

Configure two instance templates

Create two managed instance groups

Configure an HTTP Load Balancer with IPv4 and IPv6

Stress test an HTTP Load Balancer

Blacklist an IP address to restrict access to an HTTP Load Balancer
