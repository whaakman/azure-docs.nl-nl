---
title: Ansible gebruiken voor het maken van een basis Linux VM in Azure | Microsoft Docs
description: Informatie over het gebruik van Ansible maken en beheren van een eenvoudige virtuele Linux-machine in Azure
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
ms.openlocfilehash: 184a30c91de0d4141d6bd8a8b9db93c539e083b5
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Een eenvoudige virtuele machine maken in Azure met Ansible
Ansible kunt u de implementatie en configuratie van resources in uw omgeving automatiseren. U kunt Ansible gebruiken voor het beheren van uw virtuele machines (VM's) in Azure, net als elke andere bron dezelfde. In dit artikel leest u hoe een basis-VM met Ansible maken. U kunt ook meer te weten hoe [maakt een complete VM-omgeving met Ansible](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Vereisten
Voor het beheren van Azure-resources met Ansible, moet u het volgende:

- Ansible en de Azure Python SDK-modules die op uw hostsysteem zijn geïnstalleerd.
    - Installeer Ansible op [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), en [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Azure-referenties en Ansible geconfigureerd ze gebruiken.
    - [Azure-referenties maken en configureren van Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. 
    - Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). U kunt ook [Cloud Shell](/azure/cloud-shell/quickstart) vanuit de browser.


## <a name="create-supporting-azure-resources"></a>Ondersteuning van Azure-resources maken
In dit voorbeeld maakt u een runbook dat een virtuele machine in een bestaande infrastructuur implementeert. Maak eerst een resourcegroep met [az groep maken](/cli/azure/vm#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie:

```azurecli
az group create --name myResourceGroup --location eastus
```

Een virtueel netwerk maken voor uw virtuele machine met [az network vnet maken](/cli/azure/network/vnet#create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* en een subnet met de naam *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Maken en uitvoeren van Ansible playbook
Maken van een Ansible playbook met de naam *azure_create_vm.yml* en plak de volgende inhoud. In dit voorbeeld één VM maakt en configureert u SSH-referenties. Voer uw eigen volledig openbare sleutel gegevens in de *key_data* koppelen als volgt:

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
        sku: '7.3'
        version: latest
```

Als de virtuele machine maken met Ansible, voert u de playbook als volgt uit:

```bash
ansible-playbook azure_create_vm.yml
```

De uitvoer lijkt op het volgende voorbeeld ziet u dat de virtuele machine is gemaakt:

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
In dit voorbeeld maakt een virtuele machine in een bestaande resourcegroep en met een virtueel netwerk al is geïmplementeerd. Zie voor een uitgebreider voorbeeld over het gebruik van Ansible ondersteunende resources zoals een virtueel netwerk en de Netwerkbeveiligingsgroep regels maken [maakt een complete VM-omgeving met Ansible](ansible-create-complete-vm.md).
