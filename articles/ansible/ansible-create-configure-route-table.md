---
title: Een Azure-routetabel maken, wijzigen of verwijderen met Ansible
description: Ontdek hoe u Ansible gebruikt voor het maken, wijzigen of verwijderen van een routetabel
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, networking, route, route table
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/17/2018
ms.openlocfilehash: 5d7a16ebda088d0ed42021e9def20a398e914ad9
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54158122"
---
# <a name="create-change-or-delete-an-azure-route-table-using-ansible"></a>Een Azure-routetabel maken, wijzigen of verwijderen met Ansible
Azure routeert automatisch verkeer tussen Azure-subnetten, virtuele netwerken en on-premises netwerken. Als u een van de standaardroutes van Azure wilt wijzigen, kan dat door een [routetabel](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) te maken.

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. In dit artikel leest u hoe u Azure-routetabellen maakt, wijzigt en verwijdert en hoe u routetabellen kunt toevoegen aan een subnet. 

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7 is vereist om de volgende voorbeeld-playbooks in deze zelfstudie uit te voeren.

## <a name="create-a-route-table"></a>Een routetabel maken
Dit gedeelte bevat een voorbeeld-Ansible-playbook waarmee een routetabel wordt gemaakt. Er kan maar een beperkt aantal routetabellen worden gemaakt voor elke Azure-locatie en elk Azure-abonnement. Zie [Netwerkenlimieten](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie. 

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

Sla dit playbook op als `route_table_create.yml`. Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook route_table_create.yml
```

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen
Een subnet kan worden gekoppeld aan geen enkele of één routetabel. Een routetabel kan worden gekoppeld aan geen enkele of meerdere subnetten. Aangezien routetabellen niet worden gekoppeld aan virtuele netwerken, moet u afzonderlijke routetabellen koppelen aan de gewenste subnetten. Al het verkeer dat het subnet verlaat, wordt doorgestuurd op basis van de routes die u in routetabellen hebt gemaakt, [standaardroutes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#default) en routes die zijn doorgegeven vanuit een on-premises netwerk, als het virtuele netwerk is verbonden met een gateway van een virtueel Azure-netwerk (ExpressRoute of VPN, als BGP wordt gebruikt met een VPN-gateway). U kunt routetabellen alleen koppelen aan subnetten in virtuele netwerken die zich op dezelfde Azure-locatie en in hetzelfde abonnement bevinden als de routetabel.

In dit gedeelte wordt een voorbeeld-Ansible-playbook gebruikt voor het maken van een virtueel netwerk en een subnet. Vervolgens wordt er een routetabel aan het subnet gekoppeld.

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

Sla dit playbook op als `route_table_associate.yml`. Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook route_table_associate.yml
```

## <a name="dissociate-a-route-table-from-a-subnet"></a>Een routetabel ontkoppelen van een subnet
Wanneer u een routetabel ontkoppelt van een subnet, hoeft u alleen de `route_table` in het subnet in te stellen op `None`. Hieronder ziet u een voorbeeld van een Ansible-playbook. 

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

Sla dit playbook op als `route_table_dissociate.yml`. Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook route_table_dissociate.yml
```

## <a name="create-a-route"></a>Een route maken
Dit gedeelte bevat een voorbeeld-Ansible-playbook waarmee een route onder de routetabel wordt gemaakt. Hiermee worden `virtual_network_gateway` als `next_hop_type` en `10.1.0.0/16` als `address_prefix` gedefinieerd. Het voorvoegsel kan niet in meer dan één route in de routetabel worden gedupliceerd, hoewel het voorvoegsel zich wel in een ander voorvoegsel kan bevinden. Zie [Routeringsverzicht](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) voor meer informatie over hoe Azure routes selecteert, met een gedetailleerde beschrijving van alle volgende hoptypen.

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
Sla dit playbook op als `route_create.yml`. Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook route_create.yml
```

## <a name="delete-a-route"></a>Een route verwijderen
Dit gedeelte bevat een voorbeeld-Ansible-playbook waarmee een route uit een routetabel wordt verwijderd.

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

Sla dit playbook op als `route_delete.yml`. Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook route_delete.yml
```

## <a name="get-information-of-a-route-table"></a>Informatie over een routetabel ophalen
U kunt de details van een routetabel bekijken via de Ansible-module `azure_rm_routetable_facts`. Via de feitenmodule wordt informatie over de routetabel geretourneerd, met alle routes eraan zijn gekoppeld.
Hieronder ziet u een voorbeeld van een Ansible-playbook. 

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

Sla dit playbook op als `route_table_facts.yml`. Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook route_table_facts.yml
```

## <a name="delete-a-route-table"></a>Een routetabel verwijderen
Als er een routetabel is gekoppeld aan subnetten, kan deze niet worden verwijderd. [Koppel een routetabel los](#dissociate-a-route-table-from-a-subnet) van alle subnetten voordat u deze probeert te verwijderen.

U kunt de routetabel verwijderen in combinatie met alle routes. Hieronder ziet u een voorbeeld van een Ansible-playbook. 

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

Sla dit playbook op als `route_table_delete.yml`. Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook route_table_delete.yml
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Ansible in Azure](https://docs.microsoft.com/azure/ansible/)