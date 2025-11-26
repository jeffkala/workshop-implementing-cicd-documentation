## Lab Setup Walkthrough Video

Here are video walkthrough to help with illustrate the lab setup. 

Video 1. Overview and Software Download

[![video_step_1](images/video_step_1.png)](https://www.youtube.com/watch?v=p7FcvGOZHuY)

Video 2. Codespace Launch and Preparation

[![video_step_2](images/video_step_2.png)](https://www.youtube.com/watch?v=FbbZD1IWgFA)

Video 3. Create Gitlab Project

[![video_step_3](images/video_step_3.png)](https://www.youtube.com/watch?v=Rqtnxg9mPmM)

Video 4. Runner Registration

[![video_step_4](images/video_step_4.png)](https://www.youtube.com/watch?v=I3ng43OSUjc)

That is it, having gone thru the steps will ensure we can jump right into the workshop lab at Autocon2. 

Below is an optional step for those who are somewhat familiar with Gitlab and want to check the end-to-end setup. 

## (Optional) Checking for end-to-end Lab Setup

This is complete optional and we will go over it in the workshop as our first lab, but if you are up for some testing, we can test the end-to-end lab setup with the following steps. 

- Start containerlab

```
@ericchou1 ➜ /workspaces/workshop-implementing-cicd/clab (main) $ sudo containerlab deploy --topo ceos-lab.clab.yml 
INFO[0000] Containerlab v0.58.0 started                 
INFO[0000] Parsing & checking topology file: ceos-lab.clab.yml 
WARN[0000] Unable to init module loader: stat /lib/modules/6.5.0-1025-azure/modules.dep: no such file or directory. Skipping... 
INFO[0000] Creating lab directory: /workspaces/workshop-implementing-cicd/clab/clab-ceos-lab 
INFO[0000] Creating container: "ceos-02"                
INFO[0000] Creating container: "ceos-01"                
INFO[0000] Running postdeploy actions for Arista cEOS 'ceos-01' node 
INFO[0000] Created link: ceos-01:eth1 <--> ceos-02:eth1 
INFO[0000] Running postdeploy actions for Arista cEOS 'ceos-02' node 
INFO[0042] Adding containerlab host entries to /etc/hosts file 
INFO[0042] Adding ssh config for containerlab nodes     
+---+---------+--------------+--------------+------+---------+---------------+--------------+
| # |  Name   | Container ID |    Image     | Kind |  State  | IPv4 Address  | IPv6 Address |
+---+---------+--------------+--------------+------+---------+---------------+--------------+
| 1 | ceos-01 | 98ff98926159 | ceos:4.32.0F | ceos | running | 172.17.0.3/16 | N/A          |
| 2 | ceos-02 | 73b94e874b6c | ceos:4.32.0F | ceos | running | 172.17.0.2/16 | N/A          |
+---+---------+--------------+--------------+------+---------+---------------+--------------+
```
 - Create a test project with the following CI file .gitlab-ci.yml

 ```
stages: 
  - deploy

deploy testing:
  image: "python:3.10"
  stage: deploy
  tags: 
    - "ericchou-1"
  script: 
    - pip3 install nornir_utils nornir_netmiko
    - python3 show_version.py
 ```

- Create the following hosts.yaml file

```
---
eos-1:
    hostname: '172.17.0.2'
    port: 22
    username: 'admin'
    password: 'admin'
    platform: 'arista_eos'

eos-2:
    hostname: '172.17.0.3'
    port: 22
    username: 'admin'
    password: 'admin'
    platform: 'arista_eos'
```

- Create the following show_version.py file

```
#!/usr/bin/env python
from nornir import InitNornir
from nornir_netmiko import netmiko_send_command
from nornir_utils.plugins.functions import print_result

# Initialize Nornir, by default it will look for the 
# hosts.yaml file in the same directory. 
nr = InitNornir()

# Run the show version command for each of the devices. 
# store the value in the results variable. 
result = nr.run(
    task=netmiko_send_command,
    command_string="show version"
)

# print the results in 
print_result(result)
```

- You should see the following result: 

![optional_first_pipeline](images/optional_fisrt_pipeline.png)
