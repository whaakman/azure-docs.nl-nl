---
title: Een virtuele Linux-machine maken in Azure met Ansible
description: Lees hoe u een virtuele Linux-machine maakt in Azure met behulp van Ansible
ms.service: virtual-machines-linux
keywords: ansible, azure, devops, virtuele machine
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/22/2018
ms.openlocfilehash: 38cc6cd8f375fe7c60a706541bc74313e8ea2c4f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58090250"
---
# <a name="use-ansible-to-create-a-linux-virtual-machine-in-azure"></a>Een virtuele Linux-machine maken in Azure met Ansible
Ansible maakt gebruikt van een declaratieve taal om het maken, configureren en implementeren van Azure-resources te automatiseren via Ansible-*playbooks*. De verschillende gedeelten van dit artikel komen overeen met de verschillende secties van een Ansible-playbook voor het maken en configureren van verschillende aspecten van een virtuele Linux-machine. Het [volledige Ansible-playbook](#complete-sample-ansible-playbook) vindt u aan het einde van dit artikel.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan.

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)]

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Ansible heeft een resourcegroep nodig waarin uw resources worden geïmplementeerd. In het volgende voorbeeld van een sectie van een Ansible-playbook maakt u een resourcegroep met de naam `myResourceGroup` in de locatie `eastus`:

```yaml
- name: Create resource group
  azure_rm_resourcegroup:
    name: myResourceGroup
    location: eastus
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Wanneer u een virtuele Azure-machine maakt, moet u een [virtueel netwerk](/azure/virtual-network/virtual-networks-overview) maken of een bestaand virtueel netwerk gebruiken. U moet ook bepalen hoe uw virtuele machines kunnen worden benaderd via het virtuele netwerk. In het volgende voorbeeld van een sectie van een Ansible-playbook maakt u een virtueel netwerk met de naam `myVnet` in de adresruimte `10.0.0.0/16`:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.0.0.0/16"
```

Alle Azure-resources die worden geïmplementeerd in een virtueel netwerk worden geïmplementeerd in een [subnet](/azure/virtual-network/virtual-network-manage-subnet) binnen het virtuele netwerk. 

In het volgende voorbeeld van een sectie van een Ansible-playbook maakt u een subnet met de naam `mySubnet` in het virtuele netwerk `myVnet`:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken
Door [openbare IP-adressen](/azure/virtual-network/virtual-network-ip-addresses-overview-arm) te gebruiken, zijn internetbronnen in staat inkomende communicatie voor Azure-resources te verwerken. Openbare IP-adressen maken het ook mogelijk dat Azure-resources uitgaande communicatie naar internet en openbare Azure-services kunnen afhandelen via een IP-adres dat is toegewezen aan de resource. Het adres blijft toegewezen aan de resource totdat u deze toewijzing ongedaan maakt. Als een openbaar IP-adres niet aan een resource is toegewezen, kan de resource nog steeds de uitgaande communicatie met internet afhandelen, maar wijst Azure dynamisch een beschikbaar IP-adres toe dat niet aan de resource is toegewezen. 

In het volgende voorbeeld van een sectie van een Ansible-playbook maakt u een openbaar IP-adres met de naam `myPublicIP`:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken
Een [netwerkbeveiligingsgroep](/azure/virtual-network/security-overview) maakt het mogelijk om netwerkverkeer naar en van Azure-resources in een virtueel Azure-netwerk te filteren. Een netwerkbeveiligingsgroep bevat beveiligingsregels waarmee binnenkomend netwerkverkeer naar, of uitgaand netwerkverkeer van, diverse typen Azure-resources kan worden toegestaan of geweigerd. 

In het volgende voorbeeld van een sectie van een Ansible-playbook maakt u een netwerkbeveiligingsgroep met de naam `myNetworkSecurityGroup` en definieert u een regel om SSH-verkeer toe te staan op TCP-poort 22:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: Tcp
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-a-virtual-network-interface-card"></a>Een virtuele netwerkinterfacekaart maken
Een virtuele netwerkinterfacekaart verbindt uw virtuele machine met een bepaald virtueel netwerk, openbaar IP-adres en netwerkbeveiligingsgroep. 

In het volgende voorbeeld van een sectie van een Ansible-playbook maakt u een virtuele netwerkinterfacekaart met de naam `myNIC` die is verbonden met de virtuele netwerkenbronnen die u hebt gemaakt:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
De laatste stap is het maken van een virtuele machine die alle resources gebruikt die u in de voorgaande secties in dit artikel hebt gemaakt. 

In het volgende voorbeeld van een sectie van een Ansible-playbook maakt u een virtuele machine met de naam `myVM` en koppelt u de virtuele netwerkinterfacekaart met de naam `myNIC` aan de machine. Vervang de tijdelijke aanduiding &lt;your-key-data> door uw eigen volledige openbare sleutel.

```yaml
- name: Create VM
  azure_rm_virtualmachine:
    resource_group: myResourceGroup
    name: myVM
    vm_size: Standard_DS1_v2
    admin_username: azureuser
    ssh_password_enabled: false
    ssh_public_keys:
      - path: /home/azureuser/.ssh/authorized_keys
        key_data: <your-key-data>
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.5'
      version: latest
```

## <a name="complete-sample-ansible-playbook"></a>Volledige voorbeeld van Ansible-playbook

In deze sectie vindt u het volledige Ansible-playbook dat u hebt gemaakt met de verschillende stappen in dit artikel. 

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: myResourceGroup
      location: eastus
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
    register: output_ip_address
  - name: Dump public IP for VM which will be created
    debug:
      msg: "The public IP is {{ output_ip_address.state.ip_address }}."
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys:
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: <your-key-data>
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.5'
        version: latest
```

## <a name="run-the-sample-ansible-playbook"></a>Voorbeeld van Ansible-playbook uitvoeren

In deze sectie vindt u de stappen voor het uitvoeren van het voorbeeld van het Ansible-playbook uit dit artikel.

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Open [Cloud Shell](/azure/cloud-shell/overview).

1. Maak een bestand (voor het playbook) met de naam `azure_create_complete_vm.yml` en open dit in de vi-editor:

   ```azurecli-interactive
   vi azure_create_complete_vm.yml
   ```

1. Activeer de invoegmodus door op de toets **I** te drukken.

1. Plak het [volledige voorbeeld van het Ansible-playbook](#complete-sample-ansible-playbook) in de editor.

1. Verlaat de invoegmodus door op **Esc** te drukken.

1. Sla het bestand op en sluit vi Editor af met de volgende opdracht:

    ```bash
    :wq
    ```

1. Voer het voorbeeld van het Ansible-playbook uit.

   ```bash
   ansible-playbook azure_create_complete_vm.yml
   ```

1. De uitvoer lijkt op de onderstaande uitvoer, waar u kunt zien dat er een virtuele machine is gemaakt:

   ```bash
   PLAY [Create Azure VM] ****************************************************

   TASK [Gathering Facts] ****************************************************
   ok: [localhost]

   TASK [Create resource group] *********************************************
   changed: [localhost]

   TASK [Create virtual network] *********************************************
   changed: [localhost]

   TASK [Add subnet] *********************************************************
   changed: [localhost]

   TASK [Create public IP address] *******************************************
   changed: [localhost]

   TASK [Dump public IP for VM which will be created] ********************************************************************
   ok: [localhost] => {
      "msg": "The public IP is <ip-address>."
   }

   TASK [Create Network Security Group that allows SSH] **********************
   changed: [localhost]

   TASK [Create virtual network inteface card] *******************************
   changed: [localhost]

   TASK [Create VM] **********************************************************
   changed: [localhost]

   PLAY RECAP ****************************************************************
   localhost                  : ok=8    changed=7    unreachable=0    failed=0
   ```

1. De SSH-opdracht wordt gebruikt om toegang te krijgen tot uw Linux-VM. Vervang de tijdelijke aanduiding &lt;ip-address> door het IP-adres uit de vorige stap.

    ```bash
    ssh azureuser@<ip-address>
    ```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Een virtuele Linux-machine beheren in Azure met Ansible](./ansible-manage-linux-vm.md)
