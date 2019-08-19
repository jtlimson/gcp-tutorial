# How-To Create and prepare the VPN gateways

Create the VPN gateways and do all the setup work to establish the VPN tunnels. You will be doing this from the command line using Cloud Shell. Cloud Shell is used instead of the GCP Console so you can learn about the available options and how they fit together. The GCP Console conceals much of the complexity.

Create two VPN gateways, one in each region. Create forwarding rules for EPS, UDP:500, and UDP:4500 for each gateway.

**Project ID**

1. In the GCP Console, on the Navigation menu, click Home.
2. Note the Project ID; it is referred to as [PROJECT_ID] in the following steps.
3. Click Activate Cloud Shell. If prompted, click Start Cloud Shell.
4. To verify that gcloud is configured to [PROJECT_ID], run the following command:

```Console
gcloud config list project
```
 
 ```
If the project ID is undefined or does not match [PROJECT_ID], update it using
gcloud config set project <Enter PROJECT_ID here>
 ```

### Set up the VPN for both networks

1. In Cloud Shell, to create the vpn-1 gateway, run the following command:

```Console
gcloud compute target-vpn-gateways \
create vpn-1 \
--network vpn-network-1  \
--region us-central1
```

2. To create the vpn-2 gateway, run the following command:

```Console
gcloud compute target-vpn-gateways \
create vpn-2 \
--network vpn-network-2  \
--region europe-west1
```

### Reserve a static IP for each network

1. To reserve a Static IP for the vpn-1 gateway, run the following command:

```Console
gcloud compute addresses create --region us-central1 vpn-1-static-ip
```

2. To view the Static IP for the vpn-1 gateway, run the following command:

```Console
gcloud compute addresses list
```

3. To store the Static IP for the **vpn-1** gateway, in an environment variable, run the following command, and replace the IP address with the address from the output of the last command:

```Console
export STATIC_IP_VPN_1=<Enter IP address for vpn-1 here>
```

4. To reserve a Static IP for the **vpn-2** gateway, run the following command:

```Console
gcloud compute addresses create --region europe-west1 vpn-2-static-ip
```

5. To view the Static IP for the **vpn-2** gateway, run the following command:

```Console
gcloud compute addresses list
```

6.To store the Static IP for the **vpn-2** gateway, in an environment variable, run the following command, and replace the IP address with the address from the output of the last command:

```Console
export STATIC_IP_VPN_2=<Enter IP address for vpn-2 here>
```

### Create forwarding rules for both vpn gateways

The forwarding rules forward traffic arriving on the external IP to the VPN gateway. It connects them together. Create three forwarding rules for the protocols necessary for VPN.

1. To create ESP forwarding for **vpn-1**, run the following command:

```Console
gcloud compute \
forwarding-rules create vpn-1-esp \
--region us-central1  \
--ip-protocol ESP  \
--address $STATIC_IP_VPN_1 \
--target-vpn-gateway vpn-1
```

2. To create ESP forwarding for **vpn-2**, run the following command:

```Console
gcloud compute \
forwarding-rules create vpn-2-esp \
--region europe-west1  \
--ip-protocol ESP  \
--address $STATIC_IP_VPN_2 \
--target-vpn-gateway vpn-2
```

3. To create UDP500 forwarding for **vpn-1**, run the following command:

```Console
gcloud compute \
forwarding-rules create vpn-1-udp500  \
--region us-central1 \
--ip-protocol UDP \
--ports 500 \
--address $STATIC_IP_VPN_1 \
--target-vpn-gateway vpn-1
```

4. To create UDP500 forwarding for **vpn-2**, run the following command:

```Console
gcloud compute \
forwarding-rules create vpn-2-udp500  \
--region europe-west1 \
--ip-protocol UDP \
--ports 500 \
--address $STATIC_IP_VPN_2 \
--target-vpn-gateway vpn-2
```

5. To create UDP4500 forwarding for **vpn-1**, run the following command:

```Console
gcloud compute \
forwarding-rules create vpn-1-udp4500  \
--region us-central1 \
--ip-protocol UDP --ports 4500 \
--address $STATIC_IP_VPN_1 \
--target-vpn-gateway vpn-1
```

6. To create UDP4500 forwarding for **vpn-2**, run the following command:

```Console
gcloud compute \
forwarding-rules create vpn-2-udp4500  \
--region europe-west1 \
--ip-protocol UDP --ports 4500 \
--address $STATIC_IP_VPN_2 \
--target-vpn-gateway vpn-2
```

### Verify the external IP addresses and VPN gateways

1. They should be in use by the forwarding rules you just created.

2. In the GCP Console, on the **Navigation menu**, click **VPC network > External IP addresses.**

```
Verify that both regions have an external IP address reserved and that all three forwarding rules are displayed in the In use by column.
Alternatively, you can reserve static addresses and set forwarding rules through this section of the GCP Console.
```

3. In Cloud Shell, to verify the VPN gateways, run the following command:

```Console
gcloud compute target-vpn-gateways list
```

You should see the VPN gateways.

