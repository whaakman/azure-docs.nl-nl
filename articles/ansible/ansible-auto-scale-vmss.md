---
title: Zelfstudie - automatisch schalen virtuele-machineschaalsets in Azure met behulp van Ansible | Microsoft Docs
description: Leer hoe u Ansible gebruikt voor het schalen van virtuele-machineschaalsets met automatisch schalen in Azure
keywords: ansible, azure, devops, bash, playbook, schalen, automatische schaalaanpassing, virtuele machine, virtuele-machineschaalset, vmss
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 4f2cd66b7460fc6fe48cb55f45bf4bc309ae054c
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231280"
---
# <a name="tutorial-autoscale-virtual-machine-scale-sets-in-azure-using-ansible"></a>Zelfstudie: Automatisch schalen virtuele-machineschaalsets in Azure met behulp van Ansible

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-vmss.md](../../includes/open-source-devops-intro-vmss.md)]

De functie voor automatisch aanpassen van het aantal VM-exemplaren heet [voor automatisch schalen](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview). Het voordeel van automatisch schalen is dat de overhead vermindert voor het bewaken en optimaliseren van de prestaties van uw toepassing. Automatisch schalen kan worden geconfigureerd in reactie op aanvraag of volgens een ingesteld schema. Wanneer u Ansible gebruikt, kunt u de regels voor automatisch schalen waarmee het aanvaardbare prestaties voor een positieve gebruikerservaring opgeven.

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Een profiel voor automatisch schalen definiëren
> * Automatisch schalen op basis van een terugkerende planning
> * Automatisch schalen op basis van app-prestaties
> * Ophalen van informatie over de instellingen voor automatisch schalen 
> * Een instelling voor automatisch schalen uitschakelen

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)] 
[!INCLUDE [ansible-prereqs-vm-scale-set.md](../../includes/ansible-prereqs-vm-scale-set.md)]

## <a name="autoscale-based-on-a-schedule"></a>Automatisch schalen op basis van een planning

Als u automatisch schalen wilt inschakelen voor een schaalset, moet u eerst een profiel voor automatisch schalen definiëren. In dit profiel worden de minimum-, maximum- en standaardcapaciteit ingesteld voor de schaalset. Met behulp van deze limieten kunt u de kosten in de hand houden doordat er niet steeds VM-exemplaren hoeven te worden gemaakt, en kunt u aanvaardbare prestaties realiseren met een minimum aantal exemplaren die altijd kunnen worden ingeschaald. 

Ansible kunt u uw schaalsets op een specifieke datum of een terugkerende planning in te schalen.

De code playbook in deze sectie wordt het aantal VM-exemplaren drie om 10:00 elke maandag verhoogd.

Sla het volgende playbook op als `vmss-auto-scale.yml`:

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myScaleSet
    name: autoscalesetting
  tasks: 
    - name: Create autoscaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         target:
           namespace: "Microsoft.Compute"
           types: "virtualMachineScaleSets"
           name: "{{ vmss_name }}"
         enabled: true
         profiles:
         - count: '3'
           min_count: '3'
           max_count: '3'
           name: Auto created scale condition
           recurrence_timezone: Pacific Standard Time
           recurrence_frequency: Week
           recurrence_days:
              - Monday
           recurrence_mins:
              - '0'
           recurrence_hours:
              - '10'
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook vmss-auto-scale.yml
```

## <a name="autoscale-based-on-performance-data"></a>Automatisch schalen op basis van prestatiegegevens

Als de vraag voor een toepassing toeneemt, neemt de belasting van de VM-exemplaren in de schaalset ook toe. Als deze toegenomen belasting consistent is, en geen piekbelasting is, kunt u regels voor automatisch schalen configureren om het aantal VM-exemplaren in de schaalset te verhogen. Wanneer deze VM-exemplaren worden gemaakt en uw toepassingen worden geïmplementeerd, zorgt de schaalset ervoor dat er via de load balancer verkeer wordt gedistribueerd naar de exemplaren. Ansible kunt u bepalen welke metrische gegevens te bewaken, zoals CPU-gebruik, gebruik van de schijf en app-laadtijd. U kunt schalen en scale-out in de schaalset wordt ingesteld op basis van prestaties metrische drempels door een terugkerend schema, of door een bepaalde datum. 

De playbook-code in deze sectie wordt de CPU-belasting voor de vorige 10 minuten om 18:00 elke maandag gecontroleerd. 

De playbook wordt op basis van de CPU-percentage metrische gegevens, een van de volgende acties:

- Schalen van het aantal VM-instanties vier
- Schalen van het aantal VM-exemplaren een

Sla het volgende playbook op als `vmss-auto-scale-metrics.yml`:

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myScaleSet
    name: autoscalesetting
  tasks:
  - name: Get facts of the resource group
    azure_rm_resourcegroup_facts:
      name: "{{ resource_group }}"
    register: rg

  - name: Get scale set resource uri
    set_fact:
      vmss_id: "{{ rg.ansible_facts.azure_resourcegroups[0].id }}/providers/Microsoft.Compute/virtualMachineScaleSets/{{ vmss_name }}"
    
  - name: Create autoscaling
    azure_rm_autoscale:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      target: "{{ vmss_id }}"
      enabled: true
      profiles:
      - count: '1'
        max_count: '1'
        min_count: '1'
        name: 'This scale condition is executed when none of the other scale condition(s) match'
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
      - count: '1'
        min_count: '1'
        max_count: '4'
        name: Auto created scale condition
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
        rules:
          - cooldown: 5
            direction: Increase
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: GreaterThan
            statistic: Average
            threshold: 70
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
          - cooldown: 5
            direction: Decrease
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: LessThan
            statistic: Average
            threshold: 30
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook vmss-auto-scale-metrics.yml
```

## <a name="get-autoscale-settings-information"></a>Ophalen van informatie over de instellingen voor automatisch schalen 

De code van de playbook in deze sectie wordt gebruikgemaakt van de `azure_rm_autoscale_facts` module voor het ophalen van de gegevens van de instelling voor automatisch schalen.

Sla het volgende playbook op als `vmss-auto-scale-get-settings.yml`:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Retrieve autoscale settings information
      azure_rm_autoscale_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ name }}"
      register: autoscale_query
    
    - debug:
        var: autoscale_query.autoscales[0]
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook vmss-auto-scale-get-settings.yml
```

## <a name="disable-autoscale-settings"></a>Instellingen voor automatisch schalen uitschakelen

Er zijn twee manieren om uit te schakelen van instellingen voor automatisch schalen. Één manier is om te wijzigen de `enabled` sleutel van `true` naar `false`. De tweede manier is het verwijderen van de instelling.

De code playbook in deze sectie Hiermee verwijdert u de instelling voor automatisch schalen. 

Sla het volgende playbook op als `vmss-auto-scale-delete-setting.yml`:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Delete autoscaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         state: absent
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
vmss-auto-scale-delete-setting.yml
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Zelfstudie: Aangepaste installatiekopie van de update van de schaal van de virtuele machine van Azure wordt ingesteld wanneer u Ansible gebruikt](./ansible-vmss-update-image.md)