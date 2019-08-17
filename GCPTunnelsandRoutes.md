# GCP Tunnels and Static Routes

## Create Tunnels

Create the tunnels between the VPN gateways. After the tunnels exist, create a static route to enable traffic to be forwarded into the tunnel. If this is successful, you can ping a local VM in one location on its internal IP from a VM in a different location.

1. To create the tunnel for traffic from Network-1 to Network-2, run the following command:

```Console
gcloud compute \
vpn-tunnels create tunnel1to2  \
--peer-address $STATIC_IP_VPN_2 \
--region us-central1 \
--ike-version 2 \
--shared-secret gcprocks \
--target-vpn-gateway vpn-1 \
--local-traffic-selector 0.0.0.0/0 \
--remote-traffic-selector 0.0.0.0/0
```

2. To create the tunnel for traffic from Network-2 to Network-1, run the following command:

```Console
gcloud compute \
vpn-tunnels create tunnel2to1 \
--peer-address $STATIC_IP_VPN_1 \
--region europe-west1 \
--ike-version 2 \
--shared-secret gcprocks \
--target-vpn-gateway vpn-2 \
--local-traffic-selector 0.0.0.0/0 \
--remote-traffic-selector 0.0.0.0/0
```

3. To verify that the tunnels are created, run the following command:

```Console
gcloud compute vpn-tunnels list
```

```
It may take a couple of minutes for the VPNs to connect to their peers. If the connection fails, it means something was entered incorrectly in the previous commands. Be very careful about spaces or copy-paste errors.

At this point, the gateways are connected and communicating. But there is no method to direct traffic from one subnet to the other. You must establish static routes.
```

## Create static routes

1. To create a static route from Network-1 to Network-2, run the following command:

```Console
gcloud compute  \
routes create route1to2  \
--network vpn-network-1 \
--next-hop-vpn-tunnel tunnel1to2 \
--next-hop-vpn-tunnel-region us-central1 \
--destination-range 10.1.3.0/24
```

2. To create a static route from Network-2 to Network-1, run the following command:

```Console
gcloud compute  \
routes create route2to1  \
--network vpn-network-2 \
--next-hop-vpn-tunnel tunnel2to1 \
--next-hop-vpn-tunnel-region europe-west1 \
--destination-range 10.5.4.0/24
```
