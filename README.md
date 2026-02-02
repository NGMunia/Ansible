
# Ansible for Network Automation

## Overview
Ansible is an open-source automation tool that helps you manage and configure Network devices.  
In the world of **network automation**, Ansible makes it easy to automate repetitive tasks such as device configuration, across routers, switches, firewalls, and more.

Instead of logging into each device manually, you can use Ansible playbooks to push changes consistently and reliably.

---

## Why Use Ansible for Networks?
- **Agentless**: No need to install software on network devices (uses SSH).  
- **Simple YAML Playbooks**: Human-readable configuration files.  
- **Scalable**: Manage a few devices or thousands at once.  
- **Consistent**: Reduce human error by automating repetitive tasks.  
- **Vendor Support**: Works with Cisco, Juniper, Arista, Fortinet, and many others.

---

## Key Concepts
- **Inventory**: A list of devices (hosts) you want to manage.  
- **Modules**: Pre-built commands for specific tasks (e.g., configuring interfaces).  
- **Playbooks**: YAML files that describe automation tasks step by step.  
- **Tasks**: Individual actions within a playbook.  
- **Roles**: Organized collections of tasks, templates, and variables.

---

## Example Playbook
Here’s a simple playbook to verify OSPF in cisco router:

```yaml
---
- name: OSPF PROTOCOL INFORMATION
  hosts: edge_routers, stream_routers, firewalls
  gather_facts: no
  tasks:
   - name: "gathering OSPFv3 information..."
     cisco.ios.ios_facts:
       gather_network_resources:
         - ospfv3

   - name: "print gathered ospfv3 information"
     debug:
        msg:
          - hostname: "{{ansible_facts.net_hostname}}"
          - OSPF: "{{ansible_facts.network_resources}}"
 ```

## Getting Started

**Install Ansible:**
```bash
pip install ansible

```
**Define your Inventory**
This is the IP addresses or hostnames of devices that need to be automated.

```yaml
all:
  children:
    routers:
      children:
        edge_routers:
          hosts:
            edge_rtr_1:
              ansible_host: 2001:32:19:86::1
            edge_rtr_2:
              ansible_host: 2001:32:19:86::2
        stream_routers:
          hosts:
            rtr_1:
              ansible_host: 172.19.19.1
            rtr_2:
              ansible_host: 10.0.0.10
        vpn_routers:
          hosts:
            dmvpn_hub:
              ansible_host: 192.168.0.1
            rtr_tx_1:
              ansible_host: 192.168.0.4
            rtr_tx_2:
              ansible_host: 192.168.0.5
            rtr_br_1:
              ansible_host: 192.168.0.2
            rtr_br_2:
              ansible_host: 192.168.0.3
```
**Run your playbook:**

```bash
ansible-playbook playbook/ospf_verify.yml 
```

