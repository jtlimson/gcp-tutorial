

```YAML
resources:
- name: appserver
  type: compute.v1.instance
  properties:
    zone: us-west2-c
    machineType: https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-ced3587d8171e74d/zones/us-west2-c/machineTypes/f1-micro
    networkInterfaces:
    - network: https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-ced3587d8171e74d/regions/us-west2/subnetworks/default
      accessConfigs:
      - name: External_NAT
        type: ONE_TO_ONE_NAT
    disks:
    - type: PERSISTENT
      deviceName: boot
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-9-stretch-v20190813
```

## getting the resources: 
  `gcloud deployment-manager types list`
  choose and paste it to the type
## Getting the properties
  1. choose a zone
  
  `gcloud compute zones list`
  
  copy paste paste the `zone`
  
  2. `gcloud compute machine-types describe f1-micro --zone [YOUR_ZONE]`
  
  copy and paste the selfLink to the the `machineType`
  
  3. `networkInterfaces` properties
  
  `gcloud compute networks describe default`
  
  copy and paste theh selfLink to the `network` from netWorkInterfaces
  
    accessConfigs: to be explored  
    
  4. image `disk`
  
  choose an image from the list outputed below
  
    `gcloud compute images list`
    
    `gcloud compute images list | grep debian` narrowing the search
    
    get the uri of the image from the desired image 
    
    `gcloud compute images list --uri | grep debian`
    
    copy and paste the desired image uri to the `sourceImage`
    
    
  
