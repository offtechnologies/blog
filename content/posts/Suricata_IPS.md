---
title: "Ubuntu and Suricata in IPS mode - 8 simple steps"
date: 2018-02-19T12:42:11+01:00
draft: false
tags: [ "Ubuntu", "Security", "IPS", "Suricata" ]
---
Suricata is a free and open source, mature, fast and robust network threat detection engine. <!--more-->


The Suricata engine is capable of real time intrusion detection (IDS), inline intrusion prevention (IPS), network security monitoring (NSM) and offline pcap processing.

Suricata inspects the network traffic using a powerful and extensive rules and signature language.


#### Step 1. Install Suricata
```
sudo apt-get -y install suricata
```
#### Step 2. Allow Suricata File Logging
```
sudoedit /etc/suricata/suricata-debian.yaml
```
Edit:
```
outputs:
 - console:
   enabled: no
 - file:
   enabled: yes
   filename: /var/log/suricata/suricata.log
```
#### Step 3. Configure Daemon
```
sudoedit /etc/default/suricata
```
Edit:
```
RUN=yes
```
#### Step 4. Download Latest Rules with OinkMaster
```
sudoedit /etc/oinkmaster.conf
```
Edit:
```
url=http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
```
Run:
```
sudo oinkmaster -o /etc/suricata/rules/
```
#### Step 5. Add NFQUEUE Rules to IpTables Firewall
```
sudo iptables -I INPUT -j NFQUEUE
sudo iptables -I OUTPUT -j NFQUEUE
sudo netfilter-persistent save
```

#### Step 6. Restart Suricata
```
sudo systemctl restart suricata
```
#### Step 7. Add Oinkmaster cronjob to automatically download rules - every day at 12
```
sudo crontab -e
```
Edit:
```
0 12 * * * oinkmaster -C /etc/oinkmaster.conf -o /etc/suricata/rules
```

#### Step 8. Test
```
curl -A BlackSun google.com
```
```
sudo tail /var/log/suricata/fast.log
```
