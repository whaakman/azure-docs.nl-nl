---
title: Web-apps van Azure App Service schalen met Ansible
description: Leer hoe u Ansible gebruikt om een web-app te maken met een runtime voor Java 8- en Tomcat-containers in App Service in Linux
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, Azure App Service, web-app, schalen, Java
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 4ef8320d3eba841ee64557e31e63b4e79ee3aa92
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159890"
---
# <a name="scale-azure-app-service-web-apps-by-using-ansible"></a>Web-apps van Azure App Service schalen met Ansible
Met [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/overview) (of kortweg Web Apps) worden webtoepassingen, REST API's en mobiele back-ends gehost. U kunt programmeren in uw favoriete taal&mdash;.NET, .NET Core, Java, Ruby, Node.js, PHP of Python.

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. Dit artikel laat zien hoe u Ansible gebruikt voor het schalen van uw app in Azure App Service.

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **Azure App Service Web Apps**: als u nog geen Azure App Service web-app hebt, kunt u [Azure web-apps maken met behulp van Ansible](ansible-create-configure-azure-web-apps.md).

## <a name="scale-up-an-app-in-app-service"></a>Een app omhoog schalen in App Service
U kunt omhoog schalen door de prijscategorie te wijzigen van het App Service-plan waartoe uw app behoort. Deze sectie bevat een voorbeeld-Ansible-playbook waarmee de volgende bewerking wordt gedefinieerd:
- Gegevens van een bestaand App Service-plan ophalen
- Het App service-plan bijwerken naar S2 met drie workers

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

Sla dit playbook op als *webapp_scaleup.yml*.

Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook webapp_scaleup.yml
```

Wanneer u het playbook hebt uitgevoerd, wordt in uitvoer die vergelijkbaar is met het volgende voorbeeld, aangegeven dat het App Service-plan is bijgewerkt naar S2 met drie workers:
```Output
PLAY [localhost] **************************************************************

TASK [Gathering Facts] ********************************************************
ok: [localhost]

TASK [Get facts of existing App service plan] **********************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] ******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] *******************************************
changed: [localhost]

TASK [Get facts] ***************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP **********************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Ansible in Azure](https://docs.microsoft.com/azure/ansible/)