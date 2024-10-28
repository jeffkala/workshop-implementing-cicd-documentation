## Lab 4 Configuration Generation and Source Code Checks

Due to the nature of GitHubs codespaces; and the docker networking within; there is an initial requirement to update the Nornir inventory file.

Step 1 is to "fork" the GitLab repository that holds the CICD repo.

TODO: Jeff to get this in a branch as a starting point.

Execute the containerlab deploy command which based on the topology file will auto assign mgmt interfaces to the lab equipment.

Now that the lab has been deployed in the Codespace environment, we have the mgmt IPs of the equipment that we can update the Inventory host file with

For example:
```
@jeffkala ➜ /workspaces/autocon2-cicd-workshop-dev/clab (jkala-work) $ sudo containerlab deploy --topo ceos-lab.clab.yml 
INFO[0000] Containerlab v0.59.0 started                 
INFO[0000] Parsing & checking topology file: ceos-lab.clab.yml 
< ommited >
INFO[0083] Adding containerlab host entries to /etc/hosts file 
INFO[0083] Adding ssh config for containerlab nodes     
+---+---------+--------------+--------------+------+---------+---------------+--------------+
| # |  Name   | Container ID |    Image     | Kind |  State  | IPv4 Address  | IPv6 Address |
+---+---------+--------------+--------------+------+---------+---------------+--------------+
| 1 | ceos-01 | 45ff3e72dbee | ceos:4.32.0F | ceos | running | 172.17.0.3/16 | N/A          |
| 2 | ceos-02 | b9782cdb4d60 | ceos:4.32.0F | ceos | running | 172.17.0.6/16 | N/A          |
| 3 | ceos-03 | 0796d1302ef1 | ceos:4.32.0F | ceos | running | 172.17.0.4/16 | N/A          |
| 4 | ceos-04 | 90779f1b05fa | ceos:4.32.0F | ceos | running | 172.17.0.5/16 | N/A          |
+---+---------+--------------+--------------+------+---------+---------------+--------------+
```

Now clone the GitLab repo in to your codespace environment. This will allow everythign to be managed from this single place.

```
@jeffkala ➜ /workspaces/autocon2-cicd-workshop-dev (jkala-work) $ git clone https://gitlab.com/jeffkala/ac2-cicd-workshop.git
Cloning into 'ac2-cicd-workshop'...
remote: Enumerating objects: 597, done.
remote: Counting objects: 100% (484/484), done.
remote: Compressing objects: 100% (467/467), done.
remote: Total 597 (delta 298), reused 0 (delta 0), pack-reused 113 (from 1)
Receiving objects: 100% (597/597), 212.95 KiB | 9.68 MiB/s, done.
Resolving deltas: 100% (324/324), done.
```

Now navigate to the cloned repository --> ac2_cicd_workshop --> inventory --> hosts.yml

Update the host definitions

## Destorying the Lab
```
@jeffkala ➜ /workspaces/autocon2-cicd-workshop-dev/clab (jkala-work) $ sudo containerlab destroy
INFO[0000] Parsing & checking topology file: ceos-lab.clab.yml 
INFO[0000] Parsing & checking topology file: ceos-lab.clab.yml 
INFO[0000] Destroying lab: ceos-lab                     
INFO[0001] Removed container: ceos-02                   
INFO[0001] Removed container: ceos-04                   
INFO[0001] Removed container: ceos-03                   
INFO[0001] Removed container: ceos-01                   
INFO[0001] Removing containerlab host entries from /etc/hosts file 
INFO[0001] Removing ssh config for containerlab nodes   
```

```
@jeffkala ➜ /workspaces/autocon2-cicd-workshop-dev (jkala-work) $ docker ps
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS          PORTS      NAMES
9d471c69cb01   9ec1cdbafe6e                  "sh -c 'if [ -x /usr…"   27 seconds ago   Up 26 seconds              runner-t3gdm4gs-project-60671817-concurrent-0-7747588731308731-build
7ac59cf07ec1   489359c79c83                  "java -XX:-UseCompre…"   2 minutes ago    Up 2 minutes    9996/tcp   runner-t3gdm4gs-project-60671817-concurrent-0-7747588731308731-batfish__batfish-0
b9782cdb4d60   ceos:4.32.0F                  "bash -c '/mnt/flash…"   16 minutes ago   Up 16 minutes              ceos-02
0796d1302ef1   ceos:4.32.0F                  "bash -c '/mnt/flash…"   16 minutes ago   Up 16 minutes              ceos-03
90779f1b05fa   ceos:4.32.0F                  "bash -c '/mnt/flash…"   16 minutes ago   Up 16 minutes              ceos-04
45ff3e72dbee   ceos:4.32.0F                  "bash -c '/mnt/flash…"   16 minutes ago   Up 16 minutes              ceos-01
7102edeb6f05   gitlab/gitlab-runner:latest   "/usr/bin/dumb-init …"   27 minutes ago   Up 27 minutes              gitlab-runner
```