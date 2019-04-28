---
title: Zelfstudie - apps in Azure App Service met behulp van Ansible schaal | Microsoft Docs
description: Meer informatie over het schalen van een app in Azure App Service
keywords: ansible, azure, devops, bash, playbook, Azure App Service, web-app, schalen, Java
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 213c4e086db8b40fdec26ce9fb3e0be5ad055cbc
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63764304"
---
# <a name="tutorial-scale-apps-in-azure-app-service-using-ansible"></a>Zelfstudie: Apps schalen in Azure App Service met Ansible

[!INCLUDE [ansible-27-note.md](../../includes/ansible-27-note.md)]

[!INCLUDE [open-source-devops-intro-app-service.md](../../includes/open-source-devops-intro-app-service.md)]

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Gegevens van een bestaand App Service-plan ophalen
> * De App Service-plan naar S2 met drie workers opschalen

## <a name="prerequisites"></a>Vereisten

- [!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
- **Azure App Service-app** : als u een Azure App Service-app niet hebt [configureren van een app in Azure App Service met behulp van Ansible](ansible-create-configure-azure-web-apps.md).

## <a name="scale-up-an-app"></a>Een app omhoog schalen

Er zijn twee werkstromen voor het schalen van: *omhoog schalen* en *uitschalen*.

**Omhoog schalen:** Betekent om omhoog te schalen om te verkrijgen van andere bronnen. Deze bronnen omvatten CPU, geheugen, schijfruimte, VM's en meer. U Schalen een app omhoog door de prijscategorie van de App Service-plan waartoe de app behoort te wijzigen. 
**Uitschalen:** Als u wilt schalen betekent dat het aantal VM-exemplaren waarop uw app wordt uitgevoerd te verhogen. Afhankelijk van uw App Service-plan prijscategorie, kunt u uitschalen naar maximaal 20 exemplaren. [Automatisch schalen](/azure/azure-monitor/platform/autoscale-get-started) kunt u schalen exemplaren automatisch op basis van vooraf gedefinieerde regels en schema's.

De playbook-code in deze sectie definieert de volgende bewerking:

* Gegevens van een bestaand App Service-plan ophalen
* Het App service-plan bijwerken naar S2 met drie workers

Sla het volgende playbook op als `webapp_scaleup.yml`:

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App service plan
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku

  - name: Scale up the App service plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      is_linux: true
      sku: S2
      number_of_workers: 3
      
  - name: Get facts
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku
```

Voer de playbook met behulp de `ansible-playbook` opdracht:

```bash
ansible-playbook webapp_scaleup.yml
```

Nadat de playbook is uitgevoerd, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Get facts of existing App service plan] 
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] 
changed: [localhost]

TASK [Get facts] 
ok: [localhost]

TASK [debug] 
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP 
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Ansible in Azure](/azure/ansible/)