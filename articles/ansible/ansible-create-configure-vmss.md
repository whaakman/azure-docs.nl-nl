---
title: Maken van virtuele-machineschaalsets in Azure met Ansible
description: Leer hoe u Ansible gebruikt om te maken en configureren van een virtuele-machineschaalset in Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, virtuele machines, virtuele-machineschaalset, vmss
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: 5f915f7b1b425a3bd6e5d62eb70bb3f633b7eda8
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009005"
---
# <a name="create-virtual-machine-scale-sets-in-azure-using-ansible"></a>Maken van virtuele-machineschaalsets in Azure met Ansible
Ansible kunt u de implementatie en configuratie van bronnen in uw omgeving automatiseren. U kunt Ansible gebruiken voor het beheren van uw virtuele-machineschaalset (VMSS) in Azure, hetzelfde als een andere Azure-resource beheren. Dit artikel leest u hoe u Ansible gebruikt om te maken en uitbreiden van een virtuele-machineschaalset. 

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement** : als u geen Azure-abonnement, maak een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.
- **Ansible configureren** - [maken-Azure-referenties en Ansible configureren](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **Ansible en de Azure Python SDK-modules** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)

> [!Note]
> Ansible 2.6 is vereist voor het uitvoeren van de volgende de playbooks voorbeeld in deze zelfstudie. 

## <a name="create-a-vmss"></a>Maken van een VMSS
In deze sectie geeft een voorbeeld Ansible-playbook waarmee wordt gedefinieerd in de volgende bronnen:
- **Resourcegroep** waarin al uw resources worden geïmplementeerd
- **Virtueel netwerk** in de adresruimte 10.0.0.0/16
- **Subnet** binnen het virtuele netwerk
- **Openbare IP-adres** die wllows hebt u toegang tot resources via Internet
- **Netwerkbeveiligingsgroep** die Hiermee bepaalt u de stroom van het netwerkverkeer in-en uitchecken van uw virtuele-machineschaalset
- **Netwerktaakverdeler** die verkeer verdeeld over een reeks gedefinieerde virtuele machines met behulp van de load balancer-regels
- **Virtuele-machineschaalset** die gebruikmaakt van de gemaakte resources

Voer uw eigen wachtwoord in voor de *admin_password* waarde.

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      vmss_name: myVMSS
      vmss_lb_name: myVMSSlb
      location: eastus
      admin_username: azureuser
      admin_password: "your_password"
    tasks:
      - name: Create a resource group
        azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
      - name: Create virtual network
        azure_rm_virtualnetwork:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          address_prefixes: "10.0.0.0/16"
      - name: Add subnet
        azure_rm_subnet:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          address_prefix: "10.0.1.0/24"
          virtual_network: "{{ vmss_name }}"
      - name: Create public IP address
        azure_rm_publicipaddress:
          resource_group: "{{ resource_group }}"
          allocation_method: Static
          name: "{{ vmss_name }}"
      - name: Create Network Security Group that allows SSH
        azure_rm_securitygroup:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          rules:
            - name: SSH
              protocol: Tcp
              destination_port_range: 22
              access: Allow
              priority: 1001
              direction: Inbound

      - name: Create a load balancer
        azure_rm_loadbalancer:
          name: "{{ vmss_lb_name }}"
          location: "{{ location }}"
          resource_group: "{{ resource_group }}"
          public_ip: "{{ vmss_name }}"
          probe_protocol: Tcp
          probe_port: 8080
          probe_interval: 10
          probe_fail_count: 3
          protocol: Tcp
          load_distribution: Default
          frontend_port: 80
          backend_port: 8080
          idle_timeout: 4
          natpool_frontend_port_start: 50000
          natpool_frontend_port_end: 50040
          natpool_backend_port: 22
          natpool_protocol: Tcp

      - name: Create VMSS
        azure_rm_virtualmachine_scaleset:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          vm_size: Standard_DS1_v2
          admin_username: "{{ admin_username }}"
          admin_password: "{{ admin_password }}"
          ssh_password_enabled: true
          capacity: 2
          virtual_network_name: "{{ vmss_name }}"
          subnet_name: "{{ vmss_name }}"
          upgrade_policy: Manual
          tier: Standard
          managed_disk_type: Standard_LRS
          os_disk_caching: ReadWrite
          image:
            offer: UbuntuServer
            publisher: Canonical
            sku: 16.04-LTS
            version: latest
          load_balancer: "{{ vmss_lb_name }}"
          data_disks:
            - lun: 0
              disk_size_gb: 20
              managed_disk_type: Standard_LRS
              caching: ReadOnly
            - lun: 1
              disk_size_gb: 30
              managed_disk_type: Standard_LRS
              caching: ReadOnly
  ```

Voor het maken van de virtuele machine scale set-omgeving met Ansible, sla de voorgaande playbook als `vmss-create.yml`, of [downloaden van de voorbeeld-Ansible-playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-create.yml).

Als u wilt de Ansible-playbook uitvoeren, gebruiken de **ansible-playbook** opdracht als volgt:

  ```bash
  ansible-playbook vmss-create.yml
  ```

Na het uitvoeren van de playbook uitvoer is vergelijkbaar met het volgende voorbeeld ziet u dat de virtuele-machineschaalset set is gemaakt:

  ```bash
  PLAY [localhost] ***********************************************************

  TASK [Gathering Facts] *****************************************************
  ok: [localhost]

  TASK [Create a resource group] ****************************************************************************
  changed: [localhost]

  TASK [Create virtual network] ****************************************************************************
  changed: [localhost]

  TASK [Add subnet] **********************************************************
  changed: [localhost]

  TASK [Create public IP address] ****************************************************************************
  changed: [localhost]

  TASK [Create Network Security Group that allows SSH] ****************************************************************************
  changed: [localhost]

  TASK [Create a load balancer] ****************************************************************************
  changed: [localhost]

  TASK [Create VMSS] *********************************************************
  changed: [localhost]

  PLAY RECAP *****************************************************************
  localhost                  : ok=8    changed=7    unreachable=0    failed=0

  ```

## <a name="scale-out-a-vmss"></a>Een VMSS uitschalen
De gemaakte virtuele-machineschaalset heeft twee exemplaren. Als u naar de virtuele-machineschaalset in Azure portal navigeert, ziet u **Standard_DS1_v2 (2 exemplaren)**. U kunt ook de [Azure Cloud Shell](https://shell.azure.com/) door het uitvoeren van de volgende opdracht in de Cloud Shell:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

De uitvoer moet er ongeveer als volgt uitzien:

  ```bash
  {
    "capacity": 2,
  }
  ```

Nu gaan we uitgebreid van twee exemplaren naar drie exemplaren. De volgende Ansible-playbook code haalt informatie op over de virtuele-machineschaalset en wijzigt de capaciteit van twee tot drie. 

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      vmss_name: myVMSS
    tasks: 
      - name: Get scaleset info
        azure_rm_virtualmachine_scaleset_facts:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          format: curated
        register: output_scaleset

      - name: Dump scaleset info
        debug:
          var: output_scaleset

      - name: Modify scaleset (change the capacity to 3)
        set_fact:
          body: "{{ output_scaleset.ansible_facts.azure_vmss[0] | combine({'capacity': 3}, recursive=True) }}"

      - name: Update something in that VMSS
        azure_rm_virtualmachine_scaleset: "{{ body }}"
  ```

Opslaan als wilt schalen van de virtuele-machineschaalset die u hebt gemaakt, de voorgaande playbook als `vmss-scale-out.yml` of [downloaden van de voorbeeld-Ansible-playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-scale-out.yml)). 

De volgende opdracht wordt de playbook uitvoeren:

  ```bash
  ansible-playbook vmss-scale-out.yml
  ```

De uitvoer van het uitvoeren van de Ansible-playbook ziet u dat is de virtuele-machineschaalset is geschaald van:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get scaleset info] ***************************************************************************
  ok: [localhost]

  TASK [Dump scaleset info] ***************************************************************************
  ok: [localhost] => {
      "output_scaleset": {
          "ansible_facts": {
              "azure_vmss": [
                  {
                      ......
                  }
              ]
          },
          "changed": false,
          "failed": false
      }
  }

  TASK [Modify scaleset (set upgradePolicy to Automatic and capacity to 3)] ***************************************************************************
  ok: [localhost]

  TASK [Update something in that VMSS] ***************************************************************************
  changed: [localhost]

  PLAY RECAP ****************************************************************
  localhost                  : ok=5    changed=1    unreachable=0    failed=0
  ```

Als gaat u naar de virtuele-machineschaalset die u hebt geconfigureerd in de Azure-portal ziet u **Standard_DS1_v2 (3 exemplaren)**. U kunt ook controleren of de wijziging met de [Azure Cloud Shell](https://shell.azure.com/) door het uitvoeren van de volgende opdracht uit:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

De resultaten van de opdracht in Cloud Shell bevat drie exemplaren nu bestaan. 

  ```bash
  {
    "capacity": 3,
  }
  ```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Voorbeeld-Ansible-playbook voor VMSS](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)