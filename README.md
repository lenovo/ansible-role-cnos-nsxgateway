# Ansible Role: cnos-nsxgateway – NSX VXLAN Gateway Configuration on Lenovo CNOS
---
<add role description below>
NSX VXLAN Gateway feature on Lenovo switches running CNOS support the following:<br /><br />
<li>Configuration of Lenovo CNOS switch as a NSX VXLAN Gateway (VTEP)</li>
<li>Packet counters for virtual ports and networks associated with the VXLAN Gateway</li>
<li>Open vSwitch Database (OVSDB) Protocol for orchestration from SDN Controller Node</li>
<li>Bidirectional Forwarding Detection (BFD) to ensure SDN Replication Cluster availability</li>
<li>Line rate packet forwarding for both VXLAN and non‐VXLAN packets</li>
<br />
The Lenovo VXLAN Gateway supports Virtual Link Aggregation Group (vLAG) and Equal Cost Multiple Paths (ECMP) to provide an active-active, fully redundant high availability solution. The VXLAN solution is configured and administered by the VMware NSX Manager application through the Management Network. Traffic that is transmitted across this network is used by NSX to manage each device that is part of the VXLAN solution, such as virtual machines and switches.<br />

<br />Ansible role for Standalone NSX VXLAN Gateway Configuration for VMware NSX is a template driven playbook.

The role enables VXLAN Gateway on the switch making it a HSC – Hardware Switch Controller, that allows it to run in VXLAN Tunnel Endpoint (VTEP) mode. Below are the steps that it follows:
1.  Configuring IP address for the VTEP, also referred to as Tunnel IP.
2. Configure VTEP to use VMware NSX as controller provider
3. Provide NSX Controller IP Address  and TCP Port to VTEP
4. Configure Virtual Routing and Forwarding (VRF) instance used by NSX Controller

The configuration commands and their results can be verified in the *commands* and *results* directories.

For more details, see [Configuring a NSX Gateway Standalone](
https://systemx.lenovofiles.com/help/topic/com.lenovo.switchmgt.ansible.doc/Ansible_User_Guide_2-8_rev1.pdf).


## Requirements
---
<add role requirements information below>

- Ansible version 2.8 or later ([Ansible installation documentation](http://docs.ansible.com/ansible/intro_installation.html))
- Lenovo switches running CNOS version 10.10.1.0 or later
- an SSH connection to the Lenovo switch (SSH must be enabled on the network device)


## Role Variables
---
<add role variables information below>
Available variables are listed below, along with description.

The following are mandatory inventory variables:

Variable | Description
--- | ---
`ansible_connection` | Has to be `network_cli`
`ansible_network_os` | Has to be `enos`
`ansible_ssh_user` | Specifies the username used to log into the switch
`ansible_ssh_pass` | Specifies the password used to log into the switch
`deviceType` | Specifies the type of the CNOS switch

The values of the variables used need to be modified to fit the specific scenario in which you are deploying the solution. To change the values of the variables, you need to visits the *vars* directory of each role and edit the *main.yml* file located there. The values stored in this file will be used by Ansible when the template is executed.

The syntax of *main.yml* file for variables is the following:

```
<template variable>:<value>
```

You will need to replace the `<value>` field with the value that suits your topology. The `<template variable>` fields are taken from the template and it is recommended that you leave them unchanged.

Available variables are listed below, along with description:

Variable | Description
--- | ---
`dns_server_address` | Configure DNS server addresses
`ntp_server_address` | Configure NTP server address
`interface_ecmp_1` | Configure routed interface 1 for ECMP.
`ip_address_with_mask_1` | IP Address of the routed interface 1 with mask
`interface_ecmp_2` | Configure routed interface 2 for ECMP.
`vtep_interface` | `Interface assigned to VTEP IP address.
`ip_address_with_mask_vtep` | IP Address that assign to hardware VTEP. Supply with mask.
`controller_ip_address  ` | Controller IP Address
`controller_port` | Controller port number
`vxlan_tep_ip_address` | Configure the local VXLAN TEP IP address
`vtep_instance_id` | VTEP instance id.
`vxlan_interfaces` | Switch ports which physically participate in virtual network.
`vtep_username` | Configure username of the VTEP instance (*1-64*)
`vtep_password` | Configure password of the VTEP instance (*1-64*)


## Dependencies
---
<add dependencies information below>

- username.iptables - Configures the firewall and blocks all ports except those needed for web server and SSH access.
- username.common - Performs common server configuration.
- /etc/ansible/hosts - You must edit the */etc/ansible/hosts* file with the device information of the switches designated as spine switches. You may refer to *cnos-nsxgateway-hosts* for a sample configuration.

Ansible keeps track of all network elements that it manages through a hosts file. Before the execution of a playbook, the hosts file must be set up.

Open the */etc/ansible/hosts* file with root privileges. Most of the file is commented out by using **#**. You can also comment out the entries you will be adding by using **#**. You need to copy the content of the hosts file for the role into the */etc/ansible/hosts* file. The sample hosts file for the role is located in the main directory.
```
[cnos-nsxgateway]
10.241.5.177   ansible_network_os=cnos ansible_ssh_user=<username> ansible_ssh_pass=<password> deviceType=G8272
```
**Note:** You need to change the IP addresses, to fit your specific topology. You also need to change the `<username>` and `<password>` to the appropriate values used to log into the specific Lenovo network devices.


## Example Playbook
---
<add playbook samples below>

To execute an Ansible playbook, use the following command:

```
ansible-playbook cnos-nsxgateway.yml -vvv
```

`-vvv` is an optional verbos command that helps identify what is happening during playbook execution. The playbook for each role of the NSX Gateway configuration solution is located in the main directory of the solution.
```
- hosts: cnos-nsxgateway
  roles:
    - cnos-nsxgateway
```

## License
---
<add license information below>
Copyright (C) 2019 Lenovo, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
