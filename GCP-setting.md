## 1. Create private network on GCP
1. Login to GCP Console  https://console.cloud.google.com/ and chose your project 
2. Got to VPS Network and VPS networks 
![3](/screenshots/3.png)

3. Click on CREATE VPS NETWORK
4. Name - "private", VPC network ULA internal IPv6 range - disable, subnet - custom, name subnet - private, region - (I prefe europe-west-1), IP stack type - IPv4, Private Google Access - off, flow logs - off, Dynamic routing mode - regional

 Also you might create this with help gcloud 

    gcloud compute networks create private --project=<your_project> --subnet-mode=custom --mtu=1460 --bgp-routing-mode=regional
    gcloud compute networks subnets create NAME --project=<your_project> --range=IP_RANGE --stack-type=IPV4_ONLY --network=private --region=REGION


### create firewall rules
 go to Firewall and create firewall rule 

 ![4](/screenshots/4.png)

Now we creating two firewall rules for our node for ssh and node 
Name - ssh / validator
logs - off
network - private 
Direction of traffic - Ingress
Action on match  - Allow
Targets - specified target tags 
Target tags - ssh / validator 

protocol and port 
for ssh - tcp, port - 22 
for validator - tcp port - 3030, 24567
![5](/screenshots/5.png)
![6](/screenshots/6.png)


    gcloud compute --project=<your_project> firewall-rules create ssh --direction=INGRESS --priority=1000 --network=private --action=ALLOW --rules=tcp:22 --target-tags=ssh
    gcloud compute --project=<your_project> firewall-rules create validator --direction=INGRESS --priority=1000 --network=private --action=ALLOW --rules=tcp:3030,tcp:24567 --target-tags=validator
## 2. Create  instance 
1. Login to GCP Console  https://console.cloud.google.com/ and chose your project 
![1](/screenshots/1.png)
2. Go to Computer Engine
3. Chose "VM instances"
4. And "Create instancer"
![2](/screenshots/2.png)

#### Next we setup hardware and network settings 
Name - near-validator
region - same ypour private zone (I choose europe-west-1)
Zone - c 
Serias - N2 
Machine type - n2-standart-4

__Boot disk__
operation system - Ubuntu 
Version - 20.04 or 22.04 LTS 
Boot dist - SSD persistent disk
size - 500 Gb

__Network__

network tags - ssh, validator

network interfaces 
network - private 
![7](/screenshots/7.png)
![8](/screenshots/8.png)
![9](/screenshots/9.png)


__and Save__


Also ypu might create instance with gcloud, for example

    gcloud compute instances create near-producer --project=<your_project> --zone=europe-west1-b --machine-type=n2-standard-4 --network-interface=network-tier=STANDARD,subnet=private --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=168734249521-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --create-disk=auto-delete=yes,boot=yes,device-name=near-producer,image=projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20220712,mode=rw,size=500,type=projects/<your_project>/zones/europe-west1-b/diskTypes/pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any


Afret several minustes our instance will be done, and next pleace connect at this woith help terminal 
    gcloud compute ssh --zone "<your time zone>" "near-validator"  --project "<id your project>"

also you check this command on instance menu 

![10](/screenshots/10.png)

Besides of you may use another ssh client - Cloud shell or browser window, but I reccomend use terminal 

