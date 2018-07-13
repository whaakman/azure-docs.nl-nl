---
title: Maken en configureren van de Service Azure Kubernetes-clusters in Azure met behulp van Ansible
description: Leer hoe u Ansible gebruikt om te maken en beheren van een Azure Kubernetes Service-cluster in Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, container, Kubernetes
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: 6d7c5f961256e0ae1831bd76353cadd761f4b8ac
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009201"
---
# <a name="create-and-configure-azure-kubernetes-service-clusters-in-azure-using-ansible"></a>Maken en configureren van de Service Azure Kubernetes-clusters in Azure met behulp van Ansible
Ansible kunt u de implementatie en configuratie van bronnen in uw omgeving automatiseren. U kunt Ansible gebruiken voor het beheren van uw Azure Kubernetes Service (AKS). Dit artikel leest u hoe u Ansible gebruikt om te maken en configureren van een Azure Kubernetes Service-cluster.

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement** : als u geen Azure-abonnement, maak een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
- **Ansible configureren** - [maken-Azure-referenties en Ansible configureren](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **Ansible en de Azure Python SDK-modules** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)
- **Azure service-principal** : wanneer [het maken van de service-principal](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal), houd er rekening mee de volgende waarden: **appId**, **displayName**, **wachtwoord** , en **tenant**.

> [!Note]
> Ansible 2.6 is vereist voor het uitvoeren van de volgende de playbooks voorbeeld in deze zelfstudie. 

## <a name="create-a-managed-aks-cluster"></a>Een beheerde AKS-cluster maken
Het volgende voorbeeld Ansible-playbook maakt u een resourcegroep en een AKS-cluster dat zich bevindt in de resourcegroep:

  ```yaml
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

De onderstaande lijst met opsommingstekens helpen om uit te leggen van de bovenstaande code van de Ansible-playbook:
- De eerste sectie binnen **taken** definieert een resourcegroep met de naam **myResourceGroup** binnen de **eastus** locatie. 
- De tweede sectie in **taken** definieert een AKS-cluster met de naam **myAKSCluster** binnen de **myResourceGroup** resourcegroep. 

Voor het maken van het AKS-cluster met Ansible, sla het voorgaande voorbeeld playbook als `azure_create_aks.yml`, en de playbook uitvoeren met de volgende opdracht:

  ```bash
  ansible-playbook azure_create_aks.yml
  ```

De uitvoer van de **ansible-playbook* opdracht lijkt op het volgende weergegeven dat het AKS-cluster is gemaakt:

  ```bash
  PLAY [Create AKS] ****************************************************************************************

  TASK [Gathering Facts] ********************************************************************************************
  ok: [localhost]

  TASK [Create resource group] **************************************************************************************
  changed: [localhost]

  TASK [Create a Azure Container Services (AKS) cluster] ***************************************************
  changed: [localhost]

  PLAY RECAP *********************************************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0
  ```

## <a name="scale-aks-nodes"></a>AKS-knooppunten schalen

De voorbeeld-playbook in de vorige sectie definieert twee knooppunten. Als u minder of meer containerwerkbelastingen in uw cluster, kunt u het aantal knooppunten gemakkelijk aanpassen. De voorbeeld-playbook in deze sectie wordt het aantal knooppunten uit twee knooppunten tot drie verhoogd. Wijzigen van het aantal knooppunten wordt uitgevoerd door het wijzigen van de **aantal** waarde in de **agent_pool_profiles** blokkeren. 

Voer uw eigen `ssh_key`, `client_id`, en `client_secret` in de **service_principal** blokkeren:

```yaml
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

Als u wilt schalen van de Azure Kubernetes Service-cluster met Ansible, sla de voorgaande playbook als *azure_configure_aks.yml*, en voer de playbook als volgt uit:

  ```bash
  ansible-playbook azure_configure_aks.yml
  ```

De volgende uitvoer ziet u dat het AKS-cluster is gemaakt:

  ```bash
  PLAY [Scale AKS cluster] ***************************************************************

  TASK [Gathering Facts] ******************************************************************
  ok: [localhost]

  TASK [Scaling an existed AKS cluster] **************************************************
  changed: [localhost]

  PLAY RECAP ******************************************************************************
  localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Zelfstudie: Toepassing schalen in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-scale)