---
title: Ansible gebruiken voor het maken van een volledige Linux-VM in Azure | Microsoft Docs
description: Leer hoe u Ansible gebruikt voor het maken en beheren van een volledige omgeving van Linux virtuele machine in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 63228f8bf8729f1bf3796a77516490ae7088d5ed
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930841"
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Een volledige omgeving van de Linux-virtuele machine maken in Azure met Ansible
Ansible kunt u de implementatie en configuratie van bronnen in uw omgeving automatiseren. U kunt Ansible gebruiken voor het beheren van uw virtuele machines (VM's) in Azure, net als elke andere resource hetzelfde. Dit artikel ziet u hoe u een volledige Linux-omgeving en de ondersteunende resources met Ansible maakt. U leert ook hoe u [een basis-VM maken met Ansible](ansible-create-vm.md).


## <a name="prerequisites"></a>Vereisten
Voor het beheren van Azure-resources met Ansible, moet u het volgende:

- Ansible en de Azure Python SDK-modules geïnstalleerd op uw hostsysteem.
    - Ansible installeren op [CentOS 7.4](ansible-install-configure.md#centos-74), [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), en [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Azure-referenties en Ansible geconfigureerd voor het gebruik ervan.
    - [Azure-referenties maken en configureren van Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. 
    - Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). U kunt ook [Cloud Shell](/azure/cloud-shell/quickstart) vanuit uw browser.


## <a name="create-virtual-network"></a>Virtueel netwerk maken
Laten we kijken naar elke sectie van een Ansible-playbook en de afzonderlijke Azure-resources maken. Zie voor de volledige playbook [deze sectie van het artikel](#complete-ansible-playbook).

De volgende sectie in een Ansible-playbook maakt een virtueel netwerk met de naam *myVnet* in de *10.0.0.0/16* adresruimte:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.0.0.0/16"
```

Als u wilt toevoegen een subnet, de volgende sectie maakt u een subnet met de naam *mySubnet* in de *myVnet* virtueel netwerk:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Openbare IP-adres maken
Voor toegang tot resources via het Internet, maken en een openbaar IP-adres toewijzen aan uw virtuele machine. De volgende sectie in een Ansible-playbook maakt u een openbaar IP-adres met de naam *myPublicIP*:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Netwerkbeveiligingsgroep maken
Netwerkbeveiligingsgroepen beheren van de gegevensoverdracht van netwerkverkeer naar en uit uw virtuele machine. De volgende sectie in een Ansible-playbook maakt u een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* en definieert u een regel voor SSH-verkeer op TCP-poort 22:

```yaml
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
```


## <a name="create-virtual-network-interface-card"></a>Maken van virtuele netwerkinterfacekaart
Een virtuele netwerkinterfacekaart (NIC) verbindt de virtuele machine met een bepaald virtueel netwerk, het openbare IP-adres en de netwerkbeveiligingsgroep. De volgende sectie in een Ansible-playbook maakt u een virtuele NIC met de naam *myNIC* verbonden met de virtuele netwerken bronnen die u hebt gemaakt:

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
De laatste stap is het maken van een virtuele machine en gebruiken van alle resources die zijn gemaakt. De volgende sectie in een Ansible-playbook maakt u een virtuele machine met de naam *myVM* en koppelt u de virtuele NIC met de naam *myNIC*. Voer uw eigen volledig openbare sleutel gegevens in de *key_data* worden gekoppeld als volgt:

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
      sku: '7.5'
      version: latest
```

## <a name="complete-ansible-playbook"></a>Ansible-playbook uitvoeren
Als u wilt alle deze secties samenbrengen, maakt u een Ansible-playbook met de naam *azure_create_complete_vm.yml* en plak de volgende inhoud. Voer uw eigen volledig openbare sleutel gegevens in de *key_data* paar:

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
        sku: '7.5'
        version: latest
```

Ansible moet een resourcegroep om alle resources in te implementeren. Maak een resourcegroep maken met [az group create](/cli/azure/group#az-group-create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Als u wilt de volledige VM-omgeving maken met Ansible, voert u de playbook als volgt uit:

```bash
ansible-playbook azure_create_complete_vm.yml
```

De uitvoer lijkt op het volgende voorbeeld waarin dat de virtuele machine is gemaakt:

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
In dit voorbeeld maakt u een volledige VM-omgeving, met inbegrip van de vereiste resources voor virtuele netwerken. Zie voor een meer rechtstreekse voorbeeld maakt u een virtuele machine in bestaande netwerkbronnen met standaardopties [maken van een virtuele machine](ansible-create-vm.md).
