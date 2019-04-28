---
title: Zelfstudie - Update de aangepaste installatiekopie van de schaal van de virtuele machine van Azure wordt ingesteld wanneer u Ansible gebruikt | Microsoft Docs
description: Leer hoe u Ansible gebruikt om bij te werken van virtuele-machineschaalsets in Azure met aangepaste installatiekopie
keywords: ansible, azure, devops, bash, playbook, virtuele machine, schaalset voor virtuele machines, vmss
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 8ef0c9c86562519fc3d41a72c022bf2531aa9451
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63767241"
---
# <a name="tutorial-update-the-custom-image-of-azure-virtual-machine-scale-sets-using-ansible"></a>Zelfstudie: Update de aangepaste installatiekopie van de schaal van de virtuele machine van Azure wordt ingesteld wanneer u Ansible gebruikt

[!INCLUDE [ansible-27-note.md](../../includes/ansible-28-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

Nadat een virtuele machine is geïmplementeerd, configureert u de virtuele machine met de software uw toepassingsbehoeften. In plaats van deze taak voor de configuratie voor elke virtuele machine uitvoert, kunt u een aangepaste installatiekopie maken. Een aangepaste installatiekopie is een momentopname van een bestaande virtuele machine met alle geïnstalleerde software. Wanneer u [configureren van een schaalset](./ansible-create-configure-vmss.md), u de afbeelding moet worden gebruikt voor deze schaalset-VM's opgeven. Met behulp van een aangepaste installatiekopie, is elk VM-exemplaar identiek geconfigureerd voor uw app. Soms moet u mogelijk aangepaste installatiekopie van uw schaalset bijwerken. Deze taak wordt de focus van deze zelfstudie.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Twee virtuele machines configureren met HTTPD
> * Een aangepaste installatiekopie van een bestaande virtuele machine maken
> * Een schaalset op basis van een installatiekopie maken
> * De aangepaste installatiekopie bijwerken

## <a name="prerequisites"></a>Vereisten

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="configure-two-vms"></a>Twee virtuele machines configureren

De playbook-code in deze sectie maakt u twee virtuele machines met HTTPD geïnstalleerd op beide. 

De `index.html` pagina voor elke virtuele machine een testtekenreeks wordt:

* Eerste VM geeft de waarde `Image A`
* Tweede virtuele machine wordt de waarde weergegeven `Image B`

Deze tekenreeks is bedoeld om na te bootsen elke virtuele machine configureren met andere software.

Er zijn twee manieren om op te halen van de voorbeeld-playbook:

* [Download de playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss_images/01-create-vms.yml) en sla deze op `create_vms.yml`.
* Maak een nieuw bestand met de naam `create_vms.yml` en kopieer naar het de volgende inhoud:

```yml
- name: Create two VMs (A and B) with HTTPS
  hosts: localhost
  connection: local
  vars:
    vm_name: vmforimage
    admin_username: testuser
    admin_password: Pass123$$$abx!
    location: eastus
  tasks:
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"

  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}"
      address_prefixes: "10.0.0.0/16"

  - name: Create subnets for VM A and B
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      virtual_network: "{{ vm_name }}"
      name: "{{ vm_name }}"
      address_prefix: "10.0.1.0/24"

  - name: Create Network Security Group that allows HTTP
    azure_rm_securitygroup:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}"
      rules:
        - name: HTTP
          protocol: Tcp
          destination_port_range: 80
          access: Allow
          priority: 1002
          direction: Inbound

  - name: Create public IP addresses for VM A and B
    azure_rm_publicipaddress:
      resource_group: "{{ resource_group }}"
      allocation_method: Static
      name: "{{ vm_name }}_{{ item }}"
    loop:
      - A
      - B
    register: pip_output

  - name: Create virtual network inteface cards for VM A and B
    azure_rm_networkinterface:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}_{{ item }}"
      virtual_network: "{{ vm_name }}"
      subnet: "{{ vm_name }}"
      public_ip_name: "{{ vm_name }}_{{ item }}"
      security_group: "{{ vm_name }}"
    loop:
      - A
      - B

  - name: Create VM A and B
    azure_rm_virtualmachine:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}{{ item }}"
      admin_username: "{{ admin_username }}"
      admin_password: "{{ admin_password }}"
      vm_size: Standard_B1ms
      network_interfaces: "{{ vm_name }}_{{ item }}"
      image:
        offer: UbuntuServer
        publisher: Canonical
        sku: 16.04-LTS
        version: latest
    loop:
      - A
      - B

  - name: Create VM Extension
    azure_rm_virtualmachineextension:
      resource_group: "{{ resource_group }}"
      name: testVMExtension
      virtual_machine_name: "{{ vm_name }}{{ item }}"
      publisher: Microsoft.Azure.Extensions
      virtual_machine_extension_type: CustomScript
      type_handler_version: 2.0
      auto_upgrade_minor_version: true
      settings: {"commandToExecute": "sudo apt-get -y install apache2"}
    loop:
      - A
      - B

  - name: Create VM Extension
    azure_rm_virtualmachineextension:
      resource_group: "{{ resource_group }}"
      name: testVMExtension
      virtual_machine_name: "{{ vm_name }}{{ item }}"
      publisher: Microsoft.Azure.Extensions
      virtual_machine_extension_type: CustomScript
      type_handler_version: 2.0
      auto_upgrade_minor_version: true
      settings: {"commandToExecute": "printf '<html><body><h1>Image {{ item }}</h1></body></html>' >> index.html; sudo cp index.html /var/www/html/"}
    loop:
      - A
      - B

  - debug:
      msg: "Public IP Address A: {{ pip_output.results[0].state.ip_address }}"

  - debug:
      msg: "Public IP Address B: {{ pip_output.results[1].state.ip_address }}"
```

Voer de playbook met behulp de `ansible-playbook` opdracht, vervangt `myrg` met de naam van resourcegroep:

```bash
ansible-playbook create-vms.yml --extra-vars "resource_group=myrg"
```

Vanwege de `debug` secties van de playbook de `ansible-playbook` opdracht wordt het IP-adres van elke VM afgedrukt. Kopieer deze IP-adressen voor later gebruik.

![IP-adressen voor virtuele machine](media/ansible-vmss-update-image/vmss-update-vms-ip-addresses.png)

## <a name="connect-to-the-two-vms"></a>Verbinding maken met de twee virtuele machines

In deze sectie maakt verbinding u met elke virtuele machine. Zoals vermeld in de vorige sectie, de tekenreeksen `Image A` en `Image B` nabootsen met twee afzonderlijke virtuele machines met verschillende configuraties.

Met behulp van de IP-adressen uit de vorige sectie, verbinding maken met beide VM's:

![Schermafdruk van de virtuele machine een](media/ansible-vmss-update-image/vmss-update-browser-vma.png)

![Schermafdruk van de virtuele machine B](media/ansible-vmss-update-image/vmss-update-browser-vmb.png)

## <a name="create-images-from-each-vm"></a>Installatiekopieën van elke virtuele machine maken

Op dit moment hebt u twee virtuele machines met enigszins verschillende configuraties (hun `index.html` bestanden).

De playbook-code in deze sectie maakt u een aangepaste installatiekopie voor elke virtuele machine:

* `image_vmforimageA` -Aangepaste installatiekopie die is gemaakt voor de virtuele machine die wordt weergegeven `Image A` op de startpagina.
* `image_vmforimageB` -Aangepaste installatiekopie die is gemaakt voor de virtuele machine die wordt weergegeven `Image B` op de startpagina.

Er zijn twee manieren om op te halen van de voorbeeld-playbook:

* [Download de playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss_images/02-capture-images.yml) en sla deze op `capture-images.yml`.
* Maak een nieuw bestand met de naam `capture-images.yml` en kopieer naar het de volgende inhoud:

```yml
- name: Capture VM Images
  hosts: localhost
  connection: local
  vars:
    vm_name: vmforimage
  tasks:

  - name: Stop and generalize VMs
    azure_rm_virtualmachine:
      resource_group: "{{ resource_group }}"
      name: "{{ vm_name }}{{ item }}"
      generalized: yes
    loop:
      - A
      - B

  - name: Create an images from a VMs
    azure_rm_image:
      resource_group: "{{ resource_group }}"
      name: "image_{{ vm_name }}{{ item }}"
      source: "{{ vm_name }}{{ item }}"
    loop:
      - A
      - B
```

Voer de playbook met behulp de `ansible-playbook` opdracht, vervangt `myrg` met de naam van resourcegroep:

```bash
ansible-playbook capture-images.yml --extra-vars "resource_group=myrg"
```

## <a name="create-scale-set-using-image-a"></a>Schaalset met behulp van een installatiekopie maken

In deze sectie wordt een playbook gebruikt om de volgende Azure-resources configureren:

* Openbaar IP-adres
* Load balancer
* Virtual Machine Scale sets die verwijzingen `image_vmforimageA`

Er zijn twee manieren om op te halen van de voorbeeld-playbook:

* [Download de playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss_images/03-create-vmss.yml) en sla deze op `create-vmss.yml`.
* Maak een nieuw bestand met de naam `create-vmss.yml` en kopieer naar het de volgende inhoud: "

```yml
---
- hosts: localhost
  vars:
    vmss_name: vmsstest
    location: eastus
    admin_username: vmssadmin
    admin_password: User123!!!abc
    vm_name: vmforimage
    image_name: "image_vmforimageA"

  tasks:

    - name: Create public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}"
        allocation_method: Static
        name: "{{ vmss_name }}"
      register: pip_output

    - name: Create a load balancer
      azure_rm_loadbalancer:
        name: "{{ vmss_name }}lb"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        public_ip: "{{ vmss_name }}"
        probe_protocol: Tcp
        probe_port: 80
        probe_interval: 10
        probe_fail_count: 3
        protocol: Tcp
        load_distribution: Default
        frontend_port: 80
        backend_port: 80
        idle_timeout: 4
        natpool_frontend_port_start: 50000
        natpool_frontend_port_end: 50040
        natpool_backend_port: 22
        natpool_protocol: Tcp

    - name: Create a scale set
      azure_rm_virtualmachinescaleset:
        resource_group: "{{ resource_group }}"
        name: "{{ vmss_name }}"
        vm_size: Standard_DS1_v2
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        ssh_password_enabled: true
        capacity: 2
        virtual_network_name: "{{ vm_name }}"
        subnet_name: "{{ vm_name }}"
        upgrade_policy: Manual
        tier: Standard
        managed_disk_type: Standard_LRS
        os_disk_caching: ReadWrite
        image:
          name: "{{ image_name }}"
          resource_group: "{{ resource_group }}"
        load_balancer: "{{ vmss_name }}lb"

    - debug:
        msg: "Scale set public IP address: {{ pip_output.state.ip_address }}"
```

Voer de playbook met behulp de `ansible-playbook` opdracht, vervangt `myrg` met de naam van resourcegroep:

```bash
ansible-playbook create-vmss.yml --extra-vars "resource_group=myrg"
```

Vanwege de `debug` sectie van de playbook de `ansible-playbook` opdracht het IP-adres van de schaalset wordt afgedrukt. Kopieer dit IP-adres voor later gebruik.

![Openbaar IP-adres](media/ansible-vmss-update-image/vmss-update-vmss-public-ip.png)

## <a name="connect-to-the-scale-set"></a>Verbinding maken met de schaalset

In deze sectie maakt verbinding u met de schaalset. 

Met behulp van het IP-adres uit de vorige sectie, verbinding maken met de schaalset.

Zoals vermeld in de vorige sectie, de tekenreeksen `Image A` en `Image B` nabootsen met twee afzonderlijke virtuele machines met verschillende configuraties.

De Virtual Machine scale sets verwijst naar de aangepaste installatiekopie met de naam `image_vmforimageA`. Aangepaste installatiekopie `image_vmforimageA` is gemaakt op basis van de virtuele machine waarvan startpagina geeft `Image A`.

Als gevolg hiervan, ziet u een startpagina weergeven `Image A`:

![De schaalset is gekoppeld aan de eerste virtuele machine.](media/ansible-vmss-update-image/vmss-update-browser-initial-vmss.png)

Laat uw browservenster geopend als u met de volgende sectie doorgaat.

## <a name="change-custom-image-in-scale-set-and-upgrade-instances"></a>Aangepaste afbeelding van wijzigen in de schaalset ingesteld en voer een upgrade instanties

De playbook-code in deze sectie van de afbeelding van de schaalset - verandert `image_vmforimageA` naar `image_vmforimageB`. Bovendien worden alle huidige virtuele machines die zijn geïmplementeerd door de schaalaanpassingsset worden bijgewerkt.

Er zijn twee manieren om op te halen van de voorbeeld-playbook:

* [Download de playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss_images/04-update-vmss-image.yml) en sla deze op `update-vmss-image.yml`.
* Maak een nieuw bestand met de naam `update-vmss-image.yml` en kopieer naar het de volgende inhoud:

```yml
- name: Update scale set image reference
  hosts: localhost
  connection: local
  vars:
    vmss_name: vmsstest
    image_name: image_vmforimageB
    admin_username: vmssadmin
    admin_password: User123!!!abc
  tasks:

  - name: Update scale set - second image
    azure_rm_virtualmachinescaleset:
      resource_group: "{{ resource_group }}"
      name: "{{ vmss_name }}"
      vm_size: Standard_DS1_v2
      admin_username: "{{ admin_username }}"
      admin_password: "{{ admin_password }}"
      ssh_password_enabled: true
      capacity: 3
      virtual_network_name: "{{ vmss_name }}"
      subnet_name: "{{ vmss_name }}"
      upgrade_policy: Manual
      tier: Standard
      managed_disk_type: Standard_LRS
      os_disk_caching: ReadWrite
      image:
        name: "{{ image_name }}"
        resource_group: "{{ resource_group }}"
      load_balancer: "{{ vmss_name }}lb"

  - name: List all of the instances
    azure_rm_virtualmachinescalesetinstance_facts:
      resource_group: "{{ resource_group }}"
      vmss_name: "{{ vmss_name }}"
    register: instances

  - debug:
      var: instances

  - name: manually upgrade all the instances 
    azure_rm_virtualmachinescalesetinstance:
      resource_group: "{{ resource_group }}"
      vmss_name: "{{ vmss_name }}"
      instance_id: "{{ item.instance_id }}"
      latest_model: yes
    with_items: "{{ instances.instances }}"
```

Voer de playbook met behulp de `ansible-playbook` opdracht, vervangt `myrg` met de naam van resourcegroep:

```bash
ansible-playbook update-vmss-image.yml --extra-vars "resource_group=myrg"
```

Ga terug naar de browser en vernieuw de pagina. 

U ziet dat de virtuele machine van de onderliggende aangepaste installatiekopie wordt bijgewerkt.

![De schaalset is gekoppeld aan de tweede virtuele machine](media/ansible-vmss-update-image/vmss-update-browser-updated-vmss.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer het niet meer nodig hebt, verwijdert u de resources die in dit artikel is gemaakt. 

Sla de volgende code als `cleanup.yml`:

```yml
- hosts: localhost
  vars:
    resource_group: myrg
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        force_delete_nonempty: yes
        state: absent
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook cleanup.yml
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Ansible in Azure](/azure/ansible)