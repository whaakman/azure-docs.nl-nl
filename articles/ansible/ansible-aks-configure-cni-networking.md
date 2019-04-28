---
title: 'Zelfstudie: Azure CNI netwerken configureren in Azure Kubernetes Service (AKS) wanneer u Ansible gebruikt | Microsoft Docs'
description: Meer informatie over het gebruik van Ansible voor het configureren van kubenet netwerken in Azure Kubernetes Service (AKS)-cluster
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, container, aks, kubernetes
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: e4d5e68d13f00b79a309fab5e315747e934a0577
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767220"
---
# <a name="tutorial-configure-azure-cni-networking-in-azure-kubernetes-service-aks-using-ansible"></a>Zelfstudie: Configureren van Azure CNI netwerken in Azure Kubernetes Service (AKS) met Ansible

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

Met behulp van AKS, kunt u een cluster met behulp van de volgende netwerk-modellen implementeren:

- [Kubenet netwerken](/azure/aks/configure-kubenet) -netwerkbronnen gewoonlijk worden gemaakt en geconfigureerd, zoals het AKS-cluster is geïmplementeerd.
- [Netwerken van Azure CNI](/azure/aks/configure-azure-cni) -AKS-cluster is verbonden met bestaande resources van het virtuele netwerk (VNET) en configuraties.

Zie voor meer informatie over netwerken voor uw toepassingen in AKS [concepten voor toepassingen in AKS netwerk](/azure/aks/concepts-network).

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Een AKS-cluster maken
> * Azure CNI-netwerken configureren

## <a name="prerequisites"></a>Vereisten

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en een subnet maken

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Een virtueel netwerk maken
- Een subnet binnen het virtuele netwerk maken

Sla het volgende playbook op als `vnet.yml`:

```yml
- name: Create vnet
  azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      address_prefixes_cidr:
          - 10.0.0.0/8

- name: Create subnet
  azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      address_prefix_cidr: 10.240.0.0/16
      virtual_network_name: "{{ name }}"
  register: subnet
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Een AKS-cluster maken in het virtuele netwerk

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Een AKS-cluster in een virtueel netwerk maken.

Sla het volgende playbook op als `aks.yml`:

```yml
- name: List supported kubernetes version from Azure
  azure_rm_aks_version:
      location: "{{ location }}"
  register: versions

- name: Create AKS cluster within a VNet
  azure_rm_aks:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      dns_prefix: "{{ name }}"
      kubernetes_version: "{{ versions.azure_aks_versions[-1] }}"
      agent_pool_profiles:
        - count: 3
          name: nodepool1
          vm_size: Standard_D2_v2
          vnet_subnet_id: "{{ vnet_subnet_id }}"
      linux_profile:
          admin_username: azureuser
          ssh_key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
      service_principal:
          client_id: "{{ lookup('ini', 'client_id section=default file=~/.azure/credentials') }}"
          client_secret: "{{ lookup('ini', 'secret section=default file=~/.azure/credentials') }}"
      network_profile:
          network_plugin: azure
          docker_bridge_cidr: 172.17.0.1/16
          dns_service_ip: 10.2.0.10
          service_cidr: 10.2.0.0/24
  register: aks
```

Hier volgen een aantal belangrijke opmerkingen bij het werken met de voorbeeld-playbook:

- Gebruik de `azure_rm_aks_version` module de ondersteunde versie te vinden.
- De `vnet_subnet_id` is van het subnet in de vorige sectie hebt gemaakt.
- De playbook wordt geladen `ssh_key` van `~/.ssh/id_rsa.pub`. Als u deze wijzigen, gebruikt u de indeling met één regel - beginnen met 'ssh-rsa' (zonder de aanhalingstekens).
- De `client_id` en `client_secret` waarden die worden geladen uit `~/.azure/credentials`, dit is de standaard-referentie-bestand. U kunt deze waarden ingesteld op uw service principal of deze waarden van omgevingsvariabelen laden:

    ```yml
    client_id: "{{ lookup('env', 'AZURE_CLIENT_ID') }}"
    client_secret: "{{ lookup('env', 'AZURE_SECRET') }}"
    ```

## <a name="run-the-sample-playbook"></a>De voorbeeld-playbook uitvoeren

De voorbeeldcode van de playbook in deze sectie wordt gebruikt voor het testen van verschillende functies die worden weergegeven in deze zelfstudie.

Sla het volgende playbook op als `aks-azure-cni.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: aksansibletest
      name: aksansibletest
      location: eastus
  tasks:
     - name: Ensure resource group exists
       azure_rm_resourcegroup:
           name: "{{ resource_group }}"
           location: "{{ location }}"

     - name: Create vnet
       include_tasks: vnet.yml

     - name: Create AKS
       vars:
           vnet_subnet_id: "{{ subnet.state.id }}"
       include_tasks: aks.yml

     - name: Show AKS cluster detail
       debug:
           var: aks
```

Hier volgen een aantal belangrijke opmerkingen bij het werken met de voorbeeld-playbook:

- Wijzig de `aksansibletest` waarde die u wilt de naam van uw resourcegroep.
- Wijzig de `aksansibletest` waarde naar uw AKS-naam.
- Wijzig de `eastus` waarde naar de locatie van uw resourcegroep.

De playbook die met de opdracht ansible-playbook uitvoeren:

```bash
ansible-playbook aks-azure-cni.yml
```

Nadat de playbook is uitgevoerd, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Ensure resource group exists] 
changed: [localhost]

TASK [Create vnet] 
included: /home/devops/aks-cni/vnet.yml for localhost

TASK [Create vnet] 
changed: [localhost]

TASK [Create subnet] 
changed: [localhost]

TASK [Create AKS] 
included: /home/devops/aks-cni/aks.yml for localhost

TASK [List supported kubernetes version from Azure] 
 [WARNING]: Azure API profile latest does not define an entry for
ContainerServiceClient

ok: [localhost]

TASK [Create AKS cluster with vnet] 
changed: [localhost]

TASK [Show AKS cluster detail] 
ok: [localhost] => {
    "aks": {
        "aad_profile": {},
        "addon": {},
        "agent_pool_profiles": [
            {
                "count": 3,
                "name": "nodepool1",
                "os_disk_size_gb": 100,
                "os_type": "Linux",
                "storage_profile": "ManagedDisks",
                "vm_size": "Standard_D2_v2",
                "vnet_subnet_id": "/subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourceGroups/aksansibletest/providers/Microsoft.Network/virtualNetworks/aksansibletest/subnets/aksansibletest"
            }
        ],
        "changed": true,
        "dns_prefix": "aksansibletest",
        "enable_rbac": false,
        "failed": false,
        "fqdn": "aksansibletest-0272707d.hcp.eastus.azmk8s.io",
        "id": "/subscriptions/BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB/resourcegroups/aksansibletest/providers/Microsoft.ContainerService/managedClusters/aksansibletest",
        "kube_config": "..."
        },
        "location": "eastus",
        "name": "aksansibletest",
        "network_profile": {
            "dns_service_ip": "10.2.0.10",
            "docker_bridge_cidr": "172.17.0.1/16",
            "network_plugin": "azure",
            "network_policy": null,
            "pod_cidr": null,
            "service_cidr": "10.2.0.0/24"
        },
        "node_resource_group": "MC_aksansibletest_aksansibletest_eastus",
        "provisioning_state": "Succeeded",
        "service_principal_profile": {
            "client_id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA"
        },
        "tags": null,
        "type": "Microsoft.ContainerService/ManagedClusters",
        "warnings": [
            "Azure API profile latest does not define an entry for ContainerServiceClient",
            "Azure API profile latest does not define an entry for ContainerServiceClient"
        ]
    }
}

PLAY RECAP 
localhost                  : ok=9    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer het niet meer nodig hebt, verwijdert u de resources die in dit artikel is gemaakt. 

De voorbeeldcode van de playbook in deze sectie wordt gebruikt om:

- Verwijderen een resourcegroep verwezen in de `vars` sectie.

Sla het volgende playbook op als `cleanup.yml`:

```yml
---
- hosts: localhost
  vars:
      resource_group: {{ resource_group_name }}
  tasks:
      - name: Clean up resource group
        azure_rm_resourcegroup:
            name: "{{ resource_group }}"
            state: absent
            force: yes
```

Hier volgen een aantal belangrijke opmerkingen bij het werken met de voorbeeld-playbook:

- Vervang de `{{ resource_group_name }}` tijdelijke aanduiding door de naam van uw resourcegroep.
- Alle resources binnen de opgegeven resourcegroep verwijderd.

De playbook die met de opdracht ansible-playbook uitvoeren:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Azure Active Directory configureren in AKS met Ansible](./ansible-aks-configure-rbac.md)