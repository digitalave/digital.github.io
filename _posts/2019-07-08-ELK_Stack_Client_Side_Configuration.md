---
layout: post
authors: [dimuthu_daundasekara]
title: 'ELK Stack Client Side Configuration Using Auditbeat'
image: /assets/img/post-imgs/elk-auditbeat/audit2.png
tags: [pfSense, Firewall, OpenVPN, VPN]
category: DevOps
comments: true
last_modified_at: 2020-01-31
---

<style>
.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
</style>
<div class='embed-container'>
    <iframe src='https://www.youtube.com/embed/rWqUyGP_NwE?&autoplay=1' frameborder='0' allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
    </iframe>
</div>

## Introduction:

#### Beats:-

Beats is the platform for single purpose data shippers. 
Bests send data to Logstash or Elasticsearch.

All the Beats data ships to Elasticsearch or Logstash. And Visualize in Kibana.

#### Beat Types:-

There are number of types of Beats available according to your requirement.

**1. Filebeat**

You don't need to dealing with thousands of server for log monitoring and analizing using using traditional SSH.  

Filebeat forwarding all logs into centralized server.

**2. Auditbeat**

Lightweight shipper for Audit Data. 
You can keep eye on Linux Systems by monitoring user activities and processes and analyze even without toughing auditd daemon.
Auditbeat directly communicate with the Linux Audit Framework. 
So, You don't need to install of configure auditd packages manually.

**3. Metricbeat**

**4. Pakcetbeat**

**5. Winlogbeat**

**6. Heatbeat**

**7. Functionbeat**

In this tutorial, I'm going to demonstrate Auditbeat only.


#### INSTALL & CONFIGURE AUDITBEAT 

##### STEP 01: INSTALL Auditbeat

Download Relevant Package for your Operating System.
Install Auditbeat on all servers you want to monitor.

Ref: <a href="https://www.elastic.co/downloads/beats/filebeat" target="_blank">https://www.elastic.co/downloads/beats/filebeat</a>

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-7.1.1-amd64.deb
```


```bash
sudo dpkg -i auditbeat-7.1.1-amd64.deb
```


##### STEP 02: Configure Auditbeat

Auditbeat main configuration file (auditbeat.yml) resides in directory (/etc/auditbeat/) 

Ref: <a href="https://www.elastic.co/guide/en/beats/auditbeat/6.0/auditbeat-configuration.html" target="_blank">https://www.elastic.co/guide/en/beats/auditbeat/6.0/auditbeat-configuration.html</a>


`vim /etc/auditbeat/auditbeat.yml`


```yaml
#==========================  Modules configuration =============================
auditbeat.modules:

- module: auditd
  # Load audit rules from separate files. Same format as audit.rules(7).
  audit_rule_files: [ '${path.config}/audit.rules.d/*.conf' ]
  audit_rules: |

  # Add Custom Audit Rules According to Your Requirement
     -a always,exclude -F msgtype=AVC
     -a always,exclude -F msgtype=CWD
     -a always,exclude -F msgtype=EOE
     -a always,exclude -F msgtype=PROCTITLE
     -a always,exclude -F msgtype=CRED_REFR
     -a always,exclude -F msgtype=CRED_ACQ
     -a always,exit -F arch=b64 -F dir=/DATA -S rmdir -S unlink -S unlinkat -S rename -S renameat -F uid=0 -k delete_data_root
     -a always,exit -F arch=b64 -F dir=/DATA -S rmdir -S unlink -S unlinkat -S rename -S renameat -F uid>=1000 -k delete_data_non_root
     -a always,exit -F arch=b32 -F dir=/DATA -S rmdir -S unlink -S unlinkat -S rename -S renameat -F uid=0 -k delete_data_root
     -a always,exit -F arch=b32 -F dir=/DATA -S rmdir -S unlink -S unlinkat -S rename -S renameat -F uid>=1000 -k delete_data_non_root

- module: file_integrity
  # Here You Can Add Custom Dorectories To Be Monitored
  paths:
  - /bin
  - /usr/bin
  - /sbin
  - /usr/sbin
  - /etc

- module: system
  datasets:
    - host    # General host information, e.g. uptime, IPs
    - login   # User logins, logouts, and system boots.
    - package # Installed, updated, and removed packages
    - process # Started and stopped processes
    - socket  # Opened and closed sockets
    - user    # User information

  state.period: 12h
  user.detect_password_changes: true

  # File patterns of the login record files.
  login.wtmp_file_pattern: /var/log/wtmp*
  login.btmp_file_pattern: /var/log/btmp*

#==================== Elasticsearch template setting ==========================
setup.template.settings:
  index.number_of_shards: 1

#============================== Kibana =====================================
setup.kibana:

# Comment If not using "elasticsearch output" section

#----------------------------- Logstash output --------------------------------
output.logstash:
  # The Logstash hosts
  hosts: ["192.168.0.248:5044"]

#================================ Processors =====================================

processors:
  - add_host_metadata: ~
  - add_cloud_metadata: ~
```


```bash
Start & Enable at system boot:
```


```bash
systemctl enable auditbeat.service
```


```bash
systemctl start auditbeat.service
```

REF: <a href="https://www.elastic.co/downloads/beats/filebeat" target="_blank">https://www.elastic.co/downloads/beats/filebeat</a>

**Note: You also can define any compatible auditd rules according to your requirement. You have to include audit rules under the " audit_rules: |" section of auditbeat.yml file or "/etc/auditbeat/audit.rules.d/" directory.**


[<img src="Docker-Installation/sub.gif">](https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ?sub_confirmation=1) 

