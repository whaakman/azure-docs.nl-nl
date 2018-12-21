---
title: Een virtuele-machineschaalset automatisch schalen in Azure met Ansible
description: Meer informatie over hoe u een virtuele-machineschaalset in Azure maakt en configureert met behulp van Ansible
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, schalen, automatische schaalaanpassing, virtuele machine, virtuele-machineschaalset, vmss
author: tomarcher
manager: jeconnoc
ms.author: yuwzho, kyliel
ms.topic: tutorial
ms.date: 12/10/2018
ms.openlocfilehash: c6678d6df3a695d3a0471e5779bc3af4b6ba6c84
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53411118"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-azure-using-ansible"></a>Een virtuele-machineschaalset automatisch schalen in Azure met Ansible
U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. U kunt Ansible gebruiken voor het beheren van uw schaalsets voor virtuele machines in Azure, net zoals u andere Azure-resources zou beheren. 

Wanneer u een schaalset maakt, definieert u het aantal VM-exemplaren dat u wilt uitvoeren. Wanneer de vraag van de toepassing verandert, kunt u het aantal VM-exemplaren automatisch vergroten of verkleinen. De mogelijkheid van automatisch schalen stelt u in staat om altijd te voldoen aan de vraag van klanten houden of om gedurende de levenscyclus van uw app te reageren op wijzigingen in de prestaties van de toepassing. In dit artikel maakt u een instelling voor automatische schaalaanpassing en koppelt u deze aan een bestaande virtuele-machineschaalset. In de instelling voor automatische schaalaanpassing kunt u een regel configureren om naar wens in of uit te schalen.

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- Een bestaande virtuele-machineschaalset in Azure. - Als u deze nog niet hebt, kunt u [Virtuele-machineschaalsets maken in Azure met behulp van Ansible](https://docs.microsoft.com/azure/ansible/ansible-create-configure-vmss).

> [!Note]
> Ansible 2.7 is vereist om de volgende voorbeeld-playbooks in deze zelfstudie uit te voeren. 

## <a name="auto-scale-based-on-a-schedule"></a>Automatisch schalen op basis van een planning   
Als u automatisch schalen wilt inschakelen voor een schaalset, moet u eerst een profiel voor automatisch schalen definiëren. In dit profiel worden de minimum-, maximum- en standaardcapaciteit ingesteld voor de schaalset. Met behulp van deze limieten kunt u de kosten in de hand houden doordat er niet steeds VM-exemplaren hoeven te worden gemaakt en aanvaardbare prestaties realiseren met een minimum aantal exemplaren die altijd kunnen worden ingeschaald. 

U kunt virtuele-machineschaalsets in- en uitschalen op basis van een terugkerend schema of op een bepaalde datum. In deze sectie ziet u een voorbeeld van een Ansible-playbook waarmee een instelling voor automatische schaalaanpassing wordt gemaakt. Met deze instelling wordt het aantal VM-exemplaren in uw schaalsets elke maandag om 10:00 (Pacific-tijdzone) verhoogd naar drie. 

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myVMSS
    name: autoscalesetting
  tasks: 
    - name: Create auto scaling
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

Sla dit playbook op als *vmss-auto-scale.yml*. Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook vmss-auto-scale.yml
```

## <a name="auto-scale-based-on-performance-data"></a>Automatisch schalen op basis van prestatiegegevens
Als de vraag voor een toepassing toeneemt, neemt de belasting van de VM-exemplaren in de schaalset ook toe. Als deze toegenomen belasting consistent is, en geen piekbelasting is, kunt u regels voor automatisch schalen configureren om het aantal VM-exemplaren in de schaalset te verhogen. Wanneer deze VM-exemplaren worden gemaakt en uw toepassingen worden geïmplementeerd, zorgt de schaalset ervoor dat er via de load balancer verkeer wordt gedistribueerd naar de exemplaren. U bepaalt welke meetwaarden gegevens moeten worden bewaakt, zoals CPU of schijf, hoe lang de belasting van de toepassing overeen moet komen met een bepaalde drempelwaarde en hoeveel VM-exemplaren er moeten worden toegevoegd aan de schaalset.

U kunt virtuele-machineschaalsets in- en uitschalen op basis van drempelwaarden voor metrische prestatiegegevens, op basis van een terugkerend schema, of op een bepaalde datum. In deze sectie ziet u een voorbeeld van een Ansible-playbook waarmee elke maandag om 18:00 (Pacific-tijdzone) de workload van de afgelopen 10 minuten wordt gecontroleerd. Het aantal VM-exemplaren in uw schaalsets wordt uitgeschaald naar vier of ingeschaald naar één, afhankelijk van de metrische gegevens voor het CPU-percentage. 

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myVMSS
    name: autoscalesetting
  tasks:
  - name: Get facts of the resource group
    azure_rm_resourcegroup_facts:
      name: "{{ resource_group }}"
    register: rg

  - name: Get VMSS resource uri
    set_fact:
      vmss_id: "{{ rg.ansible_facts.azure_resourcegroups[0].id }}/providers/Microsoft.Compute/virtualMachineScaleSets/{{ vmss_name }}"
    
  - name: Create auto scaling
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

Sla dit playbook op als *vmss-auto-scale-metrics.yml*. Als u het Ansible-playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:

```bash
ansible-playbook vmss-auto-scale-metrics.yml
```

## <a name="get-information-for-existing-autoscale-settings"></a>Informatie krijgen over bestaande instellingen voor automatische schaalaanpassing
U kunt elke instelling voor automatische schaalaanpassing ophalen via de module *azure_rm_autoscale_facts* met het playbook. Dit doet u als volgt:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Retrieve auto scale settings information
      azure_rm_autoscale_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ name }}"
      register: autoscale_query
    
    - debug:
        var: autoscale_query.autoscales[0]
```

## <a name="disable-the-autoscale-settings"></a>Instellingen voor automatische schaalaanpassing uitschakelen
U kunt de instelling voor automatische schaalaanpassing uitschakelen door `enabled: true` te wijzigen in `enabled: false`, of door de instellingen voor automatische schaalaanpassing met het playbook te verwijderen. Dit doet u als volgt:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Delete auto scaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         state: absent
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Voorbeeld van Ansible-playbook voor virtuele-machineschaalset](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)