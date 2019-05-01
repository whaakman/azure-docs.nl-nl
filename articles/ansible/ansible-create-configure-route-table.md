---
title: Zelfstudie - Azure-routetabellen met Ansible configureren | Microsoft Docs
description: Meer informatie over het maken, wijzigen en verwijderen van Azure-routetabellen met Ansible
keywords: ansible, azure, devops, bash, playbook, networking, route, route table
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 3d20a7bb98ba266850baa0512f5b767f8b649767
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707871"
---
# <a name="tutorial-configure-azure-route-tables-using-ansible"></a>Zelfstudie: Wanneer u Ansible gebruikt Azure-routetabellen configureren

[!INCLUDE [ansible-27-note.md](../../includes/ansible-28-note.md)]

Azure routeert automatisch verkeer tussen Azure-subnetten, virtuele netwerken en on-premises netwerken. Als u meer controle over de routering van uw omgeving nodig hebt, kunt u een [routetabel](/azure/virtual-network/virtual-networks-udr-overview). 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> Maken van een routetabel maken een virtueel netwerk en subnet een routetabel aan een subnet op de koppeling verbreken een routetabel van een subnet maken koppelen en verwijderen stuurt Query een routetabel een routetabel verwijderen

## <a name="prerequisites"></a>Vereisten

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-route-table"></a>Een routetabel maken

De playbook-code in deze sectie maakt een routetabel. Zie voor meer informatie over limieten voor route-table [Azure-limieten](/azure/azure-subscription-service-limits#azure-resource-manager-virtual-networking-limits). 

Sla het volgende playbook op als `route_table_create.yml`:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook route_table_create.yml
```

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

De playbook-code in deze sectie:

* Hiermee maakt u een virtueel netwerk
* Hiermee maakt u een subnet binnen het virtuele netwerk
* Wordt gekoppeld aan een routetabel aan het subnet

Routetabellen zijn niet gekoppeld aan virtuele netwerken. In plaats daarvan zijn routetabellen gekoppeld aan het subnet van een virtueel netwerk.

Het virtuele netwerk en route-tabel moet worden gecombineerd in dezelfde Azure-locatie en abonnement.

Subnetten en routetabellen hebben een een-op-veel-relatie. Een subnet kan worden gedefinieerd zonder dat bijbehorende routetabel of één routetabel. Routetabellen kunnen worden gekoppeld aan geen, een of veel subnetten. 

Verkeer vanuit het subnet wordt doorgestuurd op basis van:

- routes zijn gedefinieerd binnen routetabellen
- [Standaardroutes](/azure/virtual-network/virtual-networks-udr-overview#default)
- routes doorgegeven vanuit een on-premises netwerk

Het virtuele netwerk moet zijn verbonden met de gateway van een Azure-netwerk. De gateway kan ExpressRoute of VPN zijn als het gebruik van BGP met een VPN-gateway.

Sla het volgende playbook op als `route_table_associate.yml`:

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create virtual network
      azure_rm_virtualnetwork:
        name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
        - 10.1.0.0/16
        - 172.100.0.0/16
        dns_servers:
        - 127.0.0.1
        - 127.0.0.3
    - name: Create a subnet with route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
        route_table: "{ route_table_name }"
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook route_table_associate.yml
```

## <a name="dissociate-a-route-table-from-a-subnet"></a>Een routetabel ontkoppelen van een subnet

De code playbook in deze sectie dissociates een routetabel van een subnet.

Wanneer u een routetabel van een subnet ontkoppelen, stelt de `route_table` voor het subnet `None`. 

Sla het volgende playbook op als `route_table_dissociate.yml`:

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    resource_group: myResourceGroup
  tasks:
    - name: Dissociate a route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook route_table_dissociate.yml
```

## <a name="create-a-route"></a>Een route maken

De playbook-code in deze sectie een route in een routetabel. 

Sla het volgende playbook op als `route_create.yml`:

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        next_hop_type: virtual_network_gateway
        address_prefix: "10.1.0.0/16"
        route_table_name: "{{ route_table_name }}"
```

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:

* `virtual_network_gateway` is gedefinieerd als `next_hop_type`. Zie voor meer informatie over hoe Azure routes selecteert, [routeringoverzicht](/azure/virtual-network/virtual-networks-udr-overview).
* `address_prefix` is gedefinieerd als `10.1.0.0/16`. Het voorvoegsel kan niet worden gedupliceerd in de routetabel.

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook route_create.yml
```

## <a name="delete-a-route"></a>Een route verwijderen

De playbook-code in deze sectie wordt een route verwijderen uit een routetabel.

Sla het volgende playbook op als `route_delete.yml`:

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Remove route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        route_table_name: "{{ route_table_name }}"
        state: absent
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook route_delete.yml
```

## <a name="get-route-table-information"></a>Ophalen van informatie over de route-table

De code playbook in deze sectie maakt gebruik van de module Ansible `azure_rm_routetable_facts` ophalen van informatie over de route-table.

Sla het volgende playbook op als `route_table_facts.yml`:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks: 
    - name: Get route table information
      azure_rm_routetable_facts:
         resource_group: "{{ resource_group }}"
         name: "{{ route_table_name }}"
      register: query
    
    - debug:
         var: query.route_tables[0]
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook route_table_facts.yml
```

## <a name="delete-a-route-table"></a>Een routetabel verwijderen

De playbook-code in deze sectie een routetabel.

Wanneer een routetabel wordt verwijderd, worden alle bijbehorende routes ook verwijderd.

Een routetabel kan niet worden verwijderd als deze is gekoppeld aan een subnet. [Ontkoppelen van de routetabel van alle subnetten](#dissociate-a-route-table-from-a-subnet) voordat u probeert te verwijderen van de routetabel. 

Sla het volgende playbook op als `route_table_delete.yml`:

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
        state: absent
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook route_table_delete.yml
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Ansible in Azure](/azure/ansible/)