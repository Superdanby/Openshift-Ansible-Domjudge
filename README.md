# Openshift-Ansible-Domjudge

Setup [Domjudge](https://github.com/DOMjudge/domjudge-packaging) with Openshift(Origin) Ansible 3.11 on Fedora 30 machines

## Prerequisites

Configuration: 1 master node, 1 infra node with dns, multiple compute nodes

1. Prepare the environment for Openshift Ansible

## Configure and Deploy

1. Adjust `hosts_domjudge` and `hosts.domjudge` to suite your environment.
2. Download [Openshift Ansible](https://github.com/openshift/openshift-ansible) and `git checkout release-3.11` on master node
3. Put the files to the corresponding path. The path are written in the top of each file.
    - File mappings:
        - Master: hosts.domjudge, main.yml, dhclient.conf
        - Infra: dnsmasq.conf, hosts_domjudge, dhclient.conf
        - Compute: dhclient.conf
4. Restart `dnsmasq` on infra, add firewall rules to allow domain name queries
5. Rename all machines with `hostnamectl set-hostname [FQDN]`, e.g. `hostnamectl set-hostname master_02.domjudge` on `192.168.218.154`
6. Restart NetworkManager on all nodes & check if they are able to do dns queries correctly(both local and global queries)
7. On master:
    1. Stop `dnsmasq.service` and ensure no processes are using port 53
    2. Go to <path to openshift-ansible>
    3. `ansible-playbook -i inventory/hosts.domjudge playbooks/prerequisites.yml`
    4. `ansible-playbook -i inventory/hosts.domjudge playbooks/deploy_cluster.yml`

## Todo

Better documentation for configuring, deploying and DEBUGGING
