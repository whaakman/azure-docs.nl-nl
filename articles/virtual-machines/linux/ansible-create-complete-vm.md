---
title: Ansible gebruiken voor het maken van een volledige Linux VM in Azure | Microsoft Docs
description: Informatie over het gebruik van Ansible maken en beheren van een omgeving met volledige Linux virtuele machines in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/18/2017
ms.author: iainfou
ms.openlocfilehash: 88e1f17184be07ec8499ad3049f7210b56fdfc15
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Maken van een omgeving met volledige Linux virtuele machines in Azure met Ansible
Ansible kunt u de implementatie en configuratie van resources in uw omgeving automatiseren. U kunt Ansible gebruiken voor het beheren van uw virtuele machines (VM's) in Azure, net als elke andere bron dezelfde. In dit artikel leest u hoe een volledige Linux-omgeving en de ondersteunende resources met Ansible maken. U kunt ook meer te weten hoe [maken van een basis-VM met Ansible](ansible-create-vm.md).


## <a name="prerequisites"></a>Vereisten
Voor het beheren van Azure-resources met Ansible, moet u het volgende:

- Ansible en de Azure Python SDK-modules die op uw hostsysteem zijn geïnstalleerd.
    - Installeer Ansible op [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), en [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Azure-referenties en Ansible geconfigureerd ze gebruiken.
    - [Azure-referenties maken en configureren van Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. 
    - Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). U kunt ook [Cloud Shell](/azure/cloud-shell/quickstart) vanuit de browser.


## <a name="create-virtual-network"></a>Virtueel netwerk maken
De volgende sectie in een playbook Ansible maakt een virtueel netwerk met de naam *myVnet* in de *10.0.0.0/16* adresruimte:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

Als u wilt toevoegen in een subnet, maakt de volgende sectie een subnet met de naam *mySubnet* in de *myVnet* virtueel netwerk:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Openbare IP-adres maken
Toegang krijgen tot bronnen via het Internet, maken en een openbare IP-adres toewijzen aan uw virtuele machine. De volgende sectie in een playbook Ansible maakt een openbaar IP-adres met de naam *myPublicIP*:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Netwerkbeveiligingsgroep maken
Netwerkbeveiligingsgroepen regelen de stroom van netwerkverkeer binnen en buiten uw virtuele machine. De volgende sectie in een playbook Ansible maakt een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* en definieert u een regel voor SSH-verkeer op TCP-poort 22:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: TCP
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-virtual-network-interface-card"></a>Maken van de virtuele netwerkinterfacekaart
Een virtuele netwerkinterfacekaart (NIC) verbindt uw virtuele machine met een opgegeven virtuele netwerk, het openbare IP-adres en de netwerkbeveiligingsgroep. De volgende sectie in een playbook Ansible maakt u een virtuele NIC met de naam *myNIC* verbonden met de virtuele netwerken bronnen die u hebt gemaakt:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```


## <a name="create-virtual-machine"></a>Virtuele machine maken
De laatste stap is het maken van een virtuele machine en gebruik van alle bronnen die zijn gemaakt. De volgende sectie in een playbook Ansible maakt u een virtuele machine met de naam *myVM* en koppelt u de virtuele NIC met de naam *myNIC*. Voer uw eigen volledig openbare sleutel gegevens in de *key_data* koppelen als volgt:

```yaml
- name: Create VM
  azure_rm_virtualmachine:
    resource_group: myResourceGroup
    name: myVM
    vm_size: Standard_DS1_v2
    admin_username: azureuser
    ssh_password_enabled: false
    ssh_public_keys: 
      - path: /home/azureuser/.ssh/authorized_keys
        key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.3'
      version: latest
```

## <a name="complete-ansible-playbook"></a>Ansible playbook voltooien
Maak een Ansible playbook met de naam voor het maken van alle deze secties samen, *azure_create_complete_vm.yml* en plak de volgende inhoud. Voer uw eigen volledig openbare sleutel gegevens in de *key_data* paar:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys: 
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

Ansible moet een resourcegroep voor het implementeren van uw resources in. Maak een resourcegroep maken met [az group create](/cli/azure/vm#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie:

```azurecli
az group create --name myResourceGroup --location eastus
```

Als de volledige VM-omgeving maken met Ansible, voert u de playbook als volgt uit:

```bash
ansible-playbook azure_create_complete_vm.yml
```

De uitvoer lijkt op het volgende voorbeeld ziet u dat de virtuele machine is gemaakt:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create virtual network] *********************************************
changed: [localhost]

TASK [Add subnet] *********************************************************
changed: [localhost]

TASK [Create public IP address] *******************************************
changed: [localhost]

TASK [Create Network Security Group that allows SSH] **********************
changed: [localhost]

TASK [Create virtual network inteface card] *******************************
changed: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=7    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld maakt u een volledige VM-omgeving met inbegrip van de vereiste resources voor virtuele netwerken. Zie voor een meer rechtstreekse voorbeeld voor het maken van een virtuele machine in de bestaande netwerkbronnen met de standaardopties [een virtuele machine maken](ansible-create-vm.md).
