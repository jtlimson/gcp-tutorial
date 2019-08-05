## Start a Kubernetes Engine cluster
On the Google Cloud Platform menu, click Activate Cloud Shell (). If a dialog box appears, click Start Cloud Shell.

For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command:

`export MY_ZONE=`

followed by the zone that Qwiklabs assigned to you. Your complete command will look like this:

`export MY_ZONE=us-central1-f`

Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:

`gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2`

It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you.

After the cluster is created, check your installed version of Kubernetes using the kubectl version command:

`kubectl version`

The gcloud container clusters create command automatically authenticated kubectl for you.

View your running nodes in the GCP Console. On the Navigation menu, click Compute Engine > VM Instances.

Your Kubernetes cluster is now ready for use.

## Run and deploy a container

From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)

`kubectl run nginx --image=nginx:1.10.0`
In Kubernetes, all containers run in pods. This use of the kubectl run command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, you launched the default number of pods, which is 1.

View the pod running the nginx container:

`kubectl get pods`

Expose the nginx container outside Kubernetes:

`kubectl expose deployment nginx --port 80 --type LoadBalancer`

Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod.

View the new service:

`kubectl get services`

You can use the displayed external IP address to test and contact the nginx container remotely.

It may take a few seconds before the ExternalIP field is populated for your service. This is normal. Just re-run the kubectl get services command every few seconds until the field is populated.

Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed.

Scale up the number of pods running on your service:

`kubectl scale deployment nginx --replicas 3`

Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.

Confirm that Kubernetes has updated the number of pods:

`kubectl get pods`

Confirm that your external IP address has not changed:

`kubectl get services`

Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.
