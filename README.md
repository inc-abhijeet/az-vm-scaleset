# Ansible role: az-vm
[![Build Status](https://travis-ci.org/rdvansloten/az-vm.svg?branch=master)](https://travis-ci.org/rdvansloten/az-vm)

az-vm is an Ansible role for deploying VMs to Microsoft Azure.

## Requirements
- Install [Python 3.x](https://www.python.org/downloads/)
- Install [pip](https://pip.pypa.io/en/stable/installing/)
-  Install [Ansible 2.8+](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- Install the [Azure Resource Manager modules](https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html) for Ansible

## Installation

Use [Ansible Galaxy](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) to install this role.

```bash
ansible-galaxy install rdvansloten.az_vm
```

## Variables

### Mandatory
```YAML
rg_name: [string]  # Name of the Resource Group, both cases, dashes and underscores only
vm_name: [string]  # Name of the VM, 15-character limit, no special characters
sa_name: [string]  # Name of the Storage Account, lowercase letters only
vn_name: [string]  # Name of the Virtual Network, both cases, dashes and underscores only
nsg_name: [string] # Name of the Network Security Group, both cases, dashes and underscores only
```

### Optional
```YAML
vm_username: [string]               # Name of the initial VM user
ssh_keys: 
  - path: [string]                  # Defaults to "/home/{{ vm_username }}/.ssh/authorized_keys"
    key_data: [string]              # Your public SSH key
vm_os_type: [string]                # "Linux" / "Windows"
vm_disk_type: [string]              # HDD ("Standard_LRS), Standard SSD ("StandardSSD_LRS") or Premium SSD ("Premium_LRS")
vm_location: [string]               # Region, defaults to "westeurope"
vm_size: [string]                   # Size of the VM (example: "Standard_F1s")
vm_image: 
  offer: [string]                   # Offer type. For example: "WindowsServer" / "UbuntuServer"
  publisher: [string]               # Publisher of the offer. For example: "MicrosoftWindowsServer" / "Canonical"
  sku: [string]                     # SKU of the OS. For example: "2019-Datacenter", "18.04-LTS"
  version: [string]                 # Version of the image, usually "latest"
vn_rg_name: [string]                # Resource Group for the Virtual Network. Use this when the vnet is in a different Resource Group.
nsg_rules:
 - name: [string]                   # Firewall rule name
    protocol: [string]              # Firewall protocol name, case sensitive ("Tcp"/"Udp"/*)
    source_address_prefix: [string] # IP address or range
    destination_port_range: [int]   # Port number or range, delimited by a dash
    access:  [string]               # "Allow" or "Deny"
    priority: [int]                 # Priority, must be a unique value
    direction: [string]             # Direction of traffic. "Inbound" / "Outbound"
```

## Usage

You can run the role using the following example playbook.

```YAML
---
- hosts: localhost
  connection: local
  vars:
    rg_name: "rdvanslotengroup"
    vm_name: "rdvanslotenvm"
    sa_name: "rdvsstoragetest"
    vn_name: "rdvs-test-vnet01"
    nsg_name: "rdvanslotenvm-nsg01"

  roles:
    - rdvansloten.az_resource_group
    - rdvansloten.az_storage_account
    - rdvansloten.az_public_ip
    - rdvansloten.az_virtual_network
    - rdvansloten.az_network_interface
    - rdvansloten.az_network_security_group
    - rdvansloten.az_vm
    - rdvansloten.az_managed_disk
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.