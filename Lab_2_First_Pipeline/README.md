# Lab 2. First Pipeline

In this lab, we will start to build our CI/CD pipeline on GitLab. 

## Hello World Pipeline

1. Create project if not already 
2. Create .gitlab-ci.yml with a single stage. 
3. Build second pipeline with multiple stages. 

```
stages: 
  - deploy

deploy testing:
  image: "ubuntu:22.04"
  stage: deploy
  script: 
    - echo "hello world"
```

## Create Python script with Netmiko 

1. Launch containerlab
2. Use script to do show version in codespace

```
<hosts.yaml>
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

<show version>
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

## Move Script to Pipeline

1. Construct a pipeline without installing nornir_util and nornir_netmiko (fail)
2. Construct a pipeline without tags to go to the correct runner (fail)
3. Final .gitlab-ci.yml

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




