---
title: Schaalsets voor virtuele machines in Azure maken met Ansible
description: Meer informatie over hoe u een schaalset voor virtuele machines in Azure maakt en configureert met Ansible
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, virtuele machine, schaalset voor virtuele machines, vmss
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/24/2018
ms.openlocfilehash: f3b08c41d3bf083c7cca5897cee11a1a4b9c9092
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918572"
---
# <a name="create-virtual-machine-scale-sets-in-azure-using-ansible"></a>Schaalsets voor virtuele machines in Azure maken met Ansible
U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. U kunt Ansible gebruiken voor het beheren van uw schaalsets voor virtuele machines in Azure, net zoals u andere Azure-resources zou beheren. In dit artikel leest u hoe u Ansible gebruikt om een schaalset voor virtuele machines te maken en uit te breiden. 

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.6 is vereist voor het uitvoeren van de volgende playbooks-voorbeelden in deze zelfstudie. 

## <a name="create-a-vmss"></a>Een VMSS maken
Deze sectie bevat een voorbeeld-Ansible-playbook waarmee de volgende resources worden gedefinieerd:
- **Resourcegroep** waarin al uw resources worden geïmplementeerd
- **Virtueel netwerk** in de adresruimte 10.0.0.0/16
- **Subnet** binnen het virtuele netwerk
- **Openbaar IP-adres** waarmee u toegang krijgt tot resources via internet
- **Netwerkbeveiligingsgroep** voor het beheren van de stroom netwerkverkeer in en uit uw schaalset voor virtuele machines
- **Load balancer** waarmee verkeer wordt verdeeld over een reeks gedefinieerde virtuele machines met behulp van load balancer-regels
- **Schaalset voor virtuele machines** waarin alle gemaakte resources worden gebruikt

Geef uw eigen wachtwoord op bij de waarde *admin_password*.

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

Als u de schaalsetomgeving voor virtuele machines wilt maken met Ansible, slaat u het voorafgaande playbook op als `vmss-create.yml` of [downloadt u het voorbeeld-Ansible-playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-create.yml).

Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

  ```bash
  ansible-playbook vmss-create.yml
  ```

Wanneer u het playbook hebt uitgevoerd, wordt in uitvoer die vergelijkbaar is met het volgende voorbeeld, aangegeven dat de schaalset voor virtuele machines is gemaakt:

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
De gemaakte schaalset voor virtuele machines heeft twee exemplaren. Als u naar de schaalset voor virtuele machines in de Azure-portal navigeert, ziet u **Standard_DS1_v2 (2 exemplaren)**. U kunt ook de [Azure Cloud Shell](https://shell.azure.com/) gebruiken door de volgende opdracht uit te voeren in de Cloud Shell:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

U ziet resultaten die vergelijkbaar zijn met de volgende uitvoer:

  ```bash
  {
    "capacity": 2,
  }
  ```

We schalen nu van twee exemplaren naar drie exemplaren. Met de volgende Ansible-playbookcode wordt informatie opgehaald over de schaalset voor virtuele machines en wordt de capaciteit gewijzigd van twee in drie. 

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

Als u de gemaakte schaalset voor virtuele machines wilt uitschalen, slaat u het voorafgaande playbook op als `vmss-scale-out.yml` of [downloadt u het voorbeeld-Ansible-playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-scale-out.yml). 

Met de volgende opdracht wordt het playbook uitgevoerd:

  ```bash
  ansible-playbook vmss-scale-out.yml
  ```

In de uitvoer van het uitvoeren van het Ansible-playbook ziet u dat de schaalset voor virtuele machines is uitgeschaald:

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

Als u navigeert naar de schaalset voor virtuele machines die u hebt geconfigureerd in de Azure-portal, ziet u **Standard_DS1_v2 (3 exemplaren)**. U kunt de wijziging ook verifiëren in [Azure Cloud Shell](https://shell.azure.com/) door de volgende opdracht uit te voeren:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

In het resultaat van het uitvoeren van de opdracht in Cloud Shell kunt u zien dat er nu drie exemplaren bestaan. 

  ```bash
  {
    "capacity": 3,
  }
  ```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Ansible-voorbeeldplaybook voor VMSS](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)