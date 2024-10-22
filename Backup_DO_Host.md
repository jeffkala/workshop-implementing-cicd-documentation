# Backup Host in Digital Ocean

- IP: 142.93.185.168
 
```
root@autocon2-cicd-workshop:~# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.2 LTS
Release:	22.04
Codename:	jammy

root@autocon2-cicd-workshop:~# docker --version
Docker version 24.0.7, build 24.0.7-0ubuntu2~22.04.1

root@autocon2-cicd-workshop:~# containerlab version
    version: 0.57.5
     commit: fbca124e
       date: 2024-09-30T10:53:07Z
     source: https://github.com/srl-labs/containerlab
 rel. notes: https://containerlab.dev/rn/0.57/#0575

root@autocon2-cicd-workshop:~# ls clab_images/
clab-ceos.tar  clab-csr.tar  clab-nxos.tar  clab-vmx.tar
root@autocon2-cicd-workshop:~# ls clab/
ceos-simple.yml  clab-ceos-lab  startup-configs
```

> [!NOTE]
> Add public key to host and allow IP range for access

```
root@autocon2-cicd-workshop:~# adduser ericchou
root@autocon2-cicd-workshop:~# usermod -aG sudo ericchou

# Test sudo
root@autocon2-cicd-workshop:~# su - ericchou
ericchou@autocon2-cicd-workshop:~$ sudo ifconfig
```

