---
title: 'Zelfstudie: configureren van peering op virtueel netwerk met behulp van Ansible | Microsoft Docs'
description: Informatie over het gebruik van Ansible voor virtuele netwerken verbinden met virtueel-netwerkpeering.
keywords: ansible, azure, devops, bash, playbook, netwerken, peering
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: d3d1aab17a94e928d113246f280c3391cae85655
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767087"
---
# <a name="tutorial-configure-azure-virtual-network-peering-using-ansible"></a>Zelfstudie: Peering op virtueel netwerk met behulp van Ansible configureren

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[Peering op virtueel netwerk (VNet)](/azure/virtual-network/virtual-network-peering-overview) kunt u naadloos verbinding maken met twee virtuele Azure-netwerken. Zodra aan elkaar gekoppeld, worden de twee virtuele netwerken weergegeven als een voor verbindingsdoeleinden. 

Verkeer wordt gerouteerd tussen virtuele machines in hetzelfde virtuele netwerk via privé-IP-adressen. Op deze manier wordt verkeer tussen virtuele machines in een gekoppeld virtueel netwerk doorgestuurd via de Microsoft-backbone-infrastructuur. Als gevolg hiervan kunnen virtuele machines in verschillende virtuele netwerken met elkaar communiceren.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Twee virtuele netwerken maken
> * De twee virtuele netwerken peeren
> * De peering tussen de twee netwerken verwijderen

## <a name="prerequisites"></a>Vereisten

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-two-resource-groups"></a>Maak twee resourcegroepen

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Maak twee resourcegroepen 

```yml
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create secondary resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group_secondary }}"
      location: "{{ location }}"
```

## <a name="create-the-first-virtual-network"></a>De eerste virtuele netwerk maken

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Een virtueel netwerk maken
- Een subnet binnen het virtuele netwerk maken

```yml
  - name: Create first virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefix: "10.0.0.0/24"
      virtual_network: "{{ vnet_name1 }}"
```

## <a name="create-the-second-virtual-network"></a>Het tweede virtuele netwerk maken

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Een virtueel netwerk maken
- Een subnet binnen het virtuele netwerk maken

```yml
  - name: Ceate second virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ vnet_name2 }}"
      address_prefixes: "10.1.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name2 }}"
      address_prefix: "10.1.0.0/24"
      virtual_network: "{{ vnet_name2 }}"
```

## <a name="peer-the-two-virtual-networks"></a>De twee virtuele netwerken peeren

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Initialiseren van peering in virtuele netwerken
- Peer-twee virtuele netwerken eerder hebt gemaakt

```yml
  - name: Initial vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group_secondary }}"
        name: "{{ vnet_name2 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Connect vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name2 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group }}"
        name: "{{ vnet_name1 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true
```

## <a name="delete-the-virtual-network-peering"></a>Het virtuele netwerk-peering verwijderen

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Verwijder de peering tussen de twee virtuele netwerken eerder hebt gemaakt

```yml
  - name: Delete vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      state: absent
```

## <a name="get-the-sample-playbook"></a>De voorbeeld-playbook ophalen

Er zijn twee manieren om de playbook compleet voorbeeld:

- [Download de playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vnet_peering.yml) en sla deze op `vnet_peering.yml`.
- Maak een nieuw bestand met de naam `vnet_peering.yml` en kopieer naar het de volgende inhoud:

```yml
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 1000 | random }}"
      run_once: yes

- name: Connect virtual networks with virtual network peering
  hosts: localhost
  connection: local
  vars:
    resource_group: "{{ resource_group_name }}"
    resource_group_secondary: "{{ resource_group_name }}2"
    vnet_name1: "myVnet{{ rpfx }}"
    vnet_name2: "myVnet{{ rpfx }}2"
    peering_name: peer1
    location: eastus2
  tasks:
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create secondary resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group_secondary }}"
      location: "{{ location }}"
  - name: Create first virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefix: "10.0.0.0/24"
      virtual_network: "{{ vnet_name1 }}"
  - name: Ceate second virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ vnet_name2 }}"
      address_prefixes: "10.1.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name2 }}"
      address_prefix: "10.1.0.0/24"
      virtual_network: "{{ vnet_name2 }}"
  - name: Initial vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group_secondary }}"
        name: "{{ vnet_name2 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Connect vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name2 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group }}"
        name: "{{ vnet_name1 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Delete vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      state: absent
```

## <a name="run-the-sample-playbook"></a>De voorbeeld-playbook uitvoeren

De voorbeeldcode van de playbook in deze sectie wordt gebruikt voor het testen van verschillende functies die worden weergegeven in deze zelfstudie.

Hier volgen een aantal belangrijke opmerkingen bij het werken met de voorbeeld-playbook:

- In de `vars` sectie, vervangt de `{{ resource_group_name }}` tijdelijke aanduiding door de naam van uw resourcegroep.

De playbook die met de opdracht ansible-playbook uitvoeren:

```bash
ansible-playbook vnet_peering.yml
```

Nadat de playbook is uitgevoerd, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Prepare random postfix] 
ok: [localhost]

PLAY [Connect virtual networks with virtual network peering] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create a resource group] 
changed: [localhost]

TASK [Create secondary resource group] 
changed: [localhost]

TASK [Create first virtual network] 
changed: [localhost]

TASK [Add subnet] 
changed: [localhost]

TASK [Ceate second virtual network] 
changed: [localhost]

TASK [Add subnet] 
changed: [localhost]

TASK [Initial vnet peering] 
changed: [localhost]

TASK [Connect vnet peering] 
changed: [localhost]

TASK [Delete vnet peering] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=12   changed=9    unreachable=0    failed=0    skipped=0   rescued=0    ignored=0
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer het niet meer nodig hebt, verwijdert u de resources die in dit artikel is gemaakt. 

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Verwijder de twee resourcegroepen eerder hebt gemaakt

Sla het volgende playbook op als `cleanup.yml`:

```bash
- hosts: localhost
  vars:
    resource_group: "{{ resource_group_name-1 }}"
    resource_group_secondary: "{{ resource_group_name-2 }}"
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        force_delete_nonempty: yes
        state: absent

    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group_secondary }}"
        force_delete_nonempty: yes
        state: absent
```

Hier volgen een aantal belangrijke opmerkingen bij het werken met de voorbeeld-playbook:

- Vervang de `{{ resource_group_name-1 }}` tijdelijke aanduiding door de naam van de eerste resourcegroep gemaakt.
- Vervang de `{{ resource_group_name-2 }}` tijdelijke aanduiding door de naam van de tweede resourcegroep gemaakt.
- Alle resources in de twee opgegeven resourcegroepen worden verwijderd.

De playbook die met de opdracht ansible-playbook uitvoeren:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Ansible in Azure](/azure/ansible/)