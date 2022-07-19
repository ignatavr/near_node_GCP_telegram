### Step 1 - add new rules on firewall

we must monitoring our instance throught special exporters, which send information about specail ports, than add this port on your firewall rules, new instance coast about 15 $ per month 

3000 TCP/UDP - grafana with tags `grafana`
9090, 9093 TCP/UDP - Prometheus and alertmanager with tags `prometheus`

9100 TCP/UDP - Node exporter with tags `nodeexporter`
9333 TCP/UDP - Near exporter `nearexporter`

add rule `nodeexporter` and `nearexporter`  to near-validator instance

if you forgot how this do, pleace check this instruction](GCP-setting.md)

### Step 2 - Create Monitoring instance  

Name - near-monitoring
region - same ypour private zone (I choose europe-west-1)
Zone - c 
Serias - e2 
Machine type - e2-small

__Boot disk__
operation system - Ubuntu 
Version - 20.04 or 22.04 LTS 
Boot dist - new balanced persistent disk
size - 30 Gb

__Network__

network tags - ssh, grafana, prometheus, nodeexporter

network interfaces 
network - private 

**and Save**

![14](/screenshots/14.png)

 


