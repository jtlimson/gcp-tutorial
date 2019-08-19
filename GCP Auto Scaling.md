# Autoscaling

## Overview
Managed instance groups offer autoscaling capabilities that allow you to automatically add or remove instances from a managed instance group based on increases or decreases in load. Autoscaling helps your applications gracefully handle increases in traffic and reduces cost when the need for resources is lower. You just define the autoscaling policy, and the autoscaler performs automatic scaling based on the measured load.

Autoscaling works by scaling your instance group in or out. That is, it adds more instances to your instance group when there is more load (scaling out) and removes instances when the need for instances is lowered (scaling in).

#Create an Instance Template based on the custom image

1. In the left pane, click Instance templates.

2. Click Create instance template.

3. Specify the following, and leave the remaining settings as their defaults:

Property	Value (type value or select option as specified)
Name	webserver-template
Machine type	micro (1 shared vCPU)
For Boot disk, click Change.

4. Click Custom images.

5. Select mywebserver1.

6. Click Select.

7. For Firewall, enable Allow HTTP traffic and Allow HTTPS traffic.

8. Click Create.

You created an instance template from the custom image. Now you can use it in a Managed Instance Group.

## Create an instance template based on the custom image

### Create a managed instance group

1. In the left pane, click Instance groups.

2. Click Create instance group.

3. Specify the following, and leave the remaining settings as their defaults:

Property	Value (type value or select option as specified)
Name	mywebserver-group
Location	Multiple zones
Region	us-central1
Instance template	webserver-template
Autoscaling	On
Autoscaling policy	HTTP load balancing usage
Maximum number of instances	5
For Health check, select Create a health check.
For Name, type webserver-healthcheck.

4. Click Save and continue.

For Initial delay, type 60. This is how long the Instance Group waits after initializing the boot-up of a VM before it tries a health check. You don't want to wait 5 minutes for this during the lab, so you set it to 1 minute.

5. Click Create. A warning tells you that there is no load balancer. That's OK; you are going to create and attach one to the Managed Instance Group in the next section.

6. Click OK.

7. In the left pane, click VM instances.

Test the VM by clicking on the External IP address of the instance in the console.

Click through the warning to see the actual page. For example, in Chrome, click Advanced, and then click Proceed to External IP Address.
In this test setup, the instance is using self-signed certificates. Therefore, you see a warning in your browser the first time you access this new External IP address. Alternatively, you can copy the IP address and access the page in a new tab using http://<External IP address>/

## Create a managed instance group

### Create a load balancer

1. On the Navigation menu, click **Network services > Load balancing**.

2. Click Create load balancer.

In HTTP(S) Load Balancing, click Start configuration.

3. Click Frontend configuration.

For Name, type mywebserver-frontend.

Leave the remaining settings as their defaults, and click Done.

4. Click Backend configuration.

Click Create or select a backend service & backend buckets > Backend services > Create a backend service.
For Name, type mywebserver-backend.
In Backends > New backend, for Instance group, click mywebserver-group.
Leave the remaining settings as their defaults, and click Done.

5. For Health check, click webserver-healthcheck.

Click Create.
Enter a name for your HTTP(S) load balancer: webserver-load-balancer.
Click Create.
Note: Creating the backend automatically set a Host and path rule to deliver all traffic to the backend.
Click webserver-load-balancer.
Find the External IP that was assigned to the Frontend, which is later referred to as [YOUR_LB_IP].

6. On the Navigation menu (7a91d354499ac9f1.png), click Compute Engine > Instance groups.
mywebserver-group may show a red icon indicating that there is no backend attached to the group. It may take a minute or two for the backend configuration to register. Click Refresh, and it should change.

Now you should see a warning icon indicating that there is no traffic to the site yet. This is expected.

Open a new browser tab or window and browse to the load balancer's IP using http://[YOUR_LB_IP]/. You should see the Apache default page.
If you get a server error, wait 1 minute and refresh the page. There might be a delay in the load balancer response.



##Create a load balancer

### Stress test the Autoscaler
The entire configuration is working, as evidenced by the fact that you could browse to the load balancer's IP and view the default page on the web server. However, you need to see if the Autoscaler is working and will actually start new VMs in response to a load.

To test this you need software that can send repeated requests to a web server. Fortunately, free web server benchmarking software, called Apache Bench, is part of the Apache2 package.

That means that when you created the webserver custom image, you also installed and created the image with pre-installed software for a benchmark server.

1. In the GCP Console, on the Navigation menu click **Compute Engine > VM instances.**

2. Click Create instance.
Specify the following, and leave the remaining settings as their defaults:
Property	Value (type value or select option as specified)
Name	stress-test
Region	us-central1
Zone	us-central1-a
Machine type	micro (1 shared vCPU)
3. For Boot Disk, click Change.

4. Click Custom images.

5. Select mywebserver1.

6. Click Select, and then click Create.

7. On the VM instances page, for stress-test, click SSH to launch a terminal and connect.

8. To create an environment variable for your load balancer IP address, run the following command:

```Console
export LB_IP=<Enter [YOUR_LB_IP] here>
```

9. To place a load on the load balancer, run the following command:

```Console
ab -n 50000 -c 1000 http://$LB_IP/
```

10. In the GCP Console, in the left pane, click Instance groups.

Click mywebserver-group. Verify that new instances have been created.
Feel free to repeat the command a couple of times to create 5 instances (maximum number of instances defined in the Instance Group).
