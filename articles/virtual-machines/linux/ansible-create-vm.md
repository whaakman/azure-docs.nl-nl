---
title: Ansible gebruiken voor het maken van een eenvoudige Linux-VM in Azure | Microsoft Docs
description: Leer hoe u Ansible gebruikt om te maken en beheren van een eenvoudige Linux-machine in Azure
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
ms.openlocfilehash: 35dfe8348718e0edf8683f7eeddf286831697d89
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931426"
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Een eenvoudige virtuele machine maken in Azure met Ansible
Ansible kunt u de implementatie en configuratie van bronnen in uw omgeving automatiseren. U kunt Ansible gebruiken voor het beheren van uw virtuele machines (VM's) in Azure, net als elke andere resource hetzelfde. Dit artikel ziet u hoe u een basis-VM maken met Ansible. U leert ook hoe u [een volledige VM-omgeving maken met Ansible](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Vereisten
Voor het beheren van Azure-resources met Ansible, moet u het volgende:

- Ansible en de Azure Python SDK-modules geïnstalleerd op uw hostsysteem.
    - Ansible installeren op [CentOS 7.4](ansible-install-configure.md#centos-74), [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), en [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Azure-referenties en Ansible geconfigureerd voor het gebruik ervan.
    - [Azure-referenties maken en configureren van Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. 
    - Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). U kunt ook de [Azure Cloud Shell](/azure/cloud-shell/quickstart) vanuit de webbrowser.


## <a name="create-supporting-azure-resources"></a>Ondersteuning van Azure-resources maken
In dit voorbeeld maakt u een runbook dat een virtuele machine in een bestaande infrastructuur implementeert. Maak eerst een resourcegroep met [az-groep maken](/cli/azure/group#az-group-create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Een virtueel netwerk maken voor uw virtuele machine met [az network vnet maken](/cli/azure/network/vnet#az-network-vnet-create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* en een subnet met de naam *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Maken en uitvoeren van Ansible-playbook
Maken van een Ansible-playbook met de naam *azure_create_vm.yml* en plak de volgende inhoud. In dit voorbeeld maakt een enkele virtuele machine en Hiermee configureert u SSH-referenties. Voer uw eigen volledig openbare sleutel gegevens in de *key_data* worden gekoppeld als volgt:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
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
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.5'
        version: latest
```

Voor het maken van de virtuele machine met Ansible, voert u de playbook als volgt uit:

```bash
ansible-playbook azure_create_vm.yml
```

De uitvoer lijkt op het volgende voorbeeld waarin dat de virtuele machine is gemaakt:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld maakt een virtuele machine in een bestaande resourcegroep en met een virtueel netwerk al is geïmplementeerd. Zie voor een uitgebreider voorbeeld over het gebruik van Ansible om ondersteunende resources, zoals een virtueel netwerk en netwerkbeveiligingsgroepsregels te maken, [een volledige VM-omgeving maken met Ansible](ansible-create-complete-vm.md).
