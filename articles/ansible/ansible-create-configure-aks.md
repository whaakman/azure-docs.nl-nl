---
title: Zelfstudie - Azure Kubernetes Service (AKS)-clusters configureren in Azure met behulp van Ansible | Microsoft Docs
description: Leer hoe u Ansible gebruikt om een Azure Kubernetes Service-cluster in Azure te maken en beheren
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, container, aks, kubernetes
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 1467afce60038e086daace72947c1ab21569865a
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231325"
---
# <a name="tutorial-configure-azure-kubernetes-service-aks-clusters-in-azure-using-ansible"></a>Zelfstudie: Azure Kubernetes Service (AKS)-clusters in Azure met behulp van Ansible configureren

[!INCLUDE [ansible-28-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-aks.md](../../includes/open-source-devops-intro-aks.md)]

AKS kan worden geconfigureerd voor het gebruik van [Azure Active Directory (AD)](/azure/active-directory/) voor gebruikersverificatie. Nadat geconfigureerd, gebruikt u uw Azure AD-verificatietoken aan te melden bij het AKS-cluster. De RBAC kan worden gebaseerd op de identiteit van een gebruiker of groepslidmaatschap van een directory.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Een AKS-cluster maken
> * Een AKS-cluster configureren

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-a-managed-aks-cluster"></a>Een beheerd AKS-cluster maken

De playbook voorbeeld maakt u een resourcegroep en een AKS-cluster in de resourcegroep.

Sla het volgende playbook op als `azure_create_aks.yml`:

```yml
- name: Create Azure Kubernetes Service
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create a managed Azure Container Services (AKS) cluster
    azure_rm_aks:
      name: "{{ aks_name }}"
      location: "{{ location }}"
      resource_group: "{{ resource_group }}"
      dns_prefix: "{{ aks_name }}"
      linux_profile:
        admin_username: "{{ username }}"
        ssh_key: "{{ ssh_key }}"
      service_principal:
        client_id: "{{ client_id }}"
        client_secret: "{{ client_secret }}"
      agent_pool_profiles:
        - name: default
          count: 2
          vm_size: Standard_D2_v2
      tags:
        Environment: Production
```

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:

- De eerste sectie binnen `tasks` definieert een resourcegroep met de naam `myResourceGroup` binnen de `eastus` locatie.
- De tweede sectie in `tasks` definieert een AKS-cluster met de naam `myAKSCluster` binnen de `myResourceGroup` resourcegroep.
- Voor de tijdelijke aanduiding `your_ssh_key` voert u uw openbare RSA-sleutel in in de indeling met één regel, beginnend met 'ssh-rsa' (zonder aanhalingstekens).

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook azure_create_aks.yml
```

Uitvoeren van de playbook toont resultaten die lijkt op de volgende uitvoer:

```Output
PLAY [Create AKS] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create resource group] 
changed: [localhost]

TASK [Create an Azure Container Services (AKS) cluster] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="scale-aks-nodes"></a>AKS-knooppunten schalen

Het voorbeeld-playbook in de vorige sectie definieert twee knooppunten. U het aantal knooppunten aanpassen door het wijzigen van de `count` waarde in de `agent_pool_profiles` blokkeren.

Sla het volgende playbook op als `azure_configure_aks.yml`:

```yml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

Voordat u de playbook uitvoert, Zie de volgende opmerkingen:

- Voor de tijdelijke aanduiding `your_ssh_key` voert u uw openbare RSA-sleutel in in de indeling met één regel, beginnend met 'ssh-rsa' (zonder aanhalingstekens).

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook azure_configure_aks.yml
```

Uitvoeren van de playbook toont resultaten die lijkt op de volgende uitvoer:

```Output
PLAY [Scale AKS cluster] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Scaling an existed AKS cluster] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

## <a name="delete-a-managed-aks-cluster"></a>Wis een beheerd AKS-cluster

De voorbeeld-playbook Hiermee verwijdert u een AKS-cluster.

Sla het volgende playbook op als `azure_delete_aks.yml`:


```yml
- name: Delete a managed Azure Container Services (AKS) cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    aks_name: myAKSCluster
  tasks:
  - name:
    azure_rm_aks:
      name: "{{ aks_name }}"
      resource_group: "{{ resource_group }}"
      state: absent
  ```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook azure_delete_aks.yml
```

Uitvoeren van de playbook toont resultaten die lijkt op de volgende uitvoer:

```Output
PLAY [Delete a managed Azure Container Services (AKS) cluster] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [azure_rm_aks] 

PLAY RECAP 
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: een toepassing schalen in AKS (Azure Kubernetes Service)](/azure/aks/tutorial-kubernetes-scale)