---
title: Azure-web-apps maken met Ansible
description: Leer hoe u Ansible gebruikt om een web-app te maken met een runtime voor Java 8- en Tomcat-containers in App Service in Linux
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, Azure App Service, web-app, Java
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 5f67a9f7d629eec9ab1462a25940355869c1cd28
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791219"
---
# <a name="create-azure-app-service-web-apps-by-using-ansible"></a>Web-apps van Azure App Service maken met Ansible
Met [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/overview) (of kortweg Web Apps) worden webtoepassingen, REST API's en mobiele back-ends gehost. U kunt programmeren in uw favoriete taal&mdash;.NET, .NET Core, Java, Ruby, Node.js, PHP of Python.

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. In dit artikel leest u hoe u Ansible gebruikt om een web-app te maken met de Java-runtime. 

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7 is vereist om de volgende voorbeeld-playbooks in deze zelfstudie uit te voeren.

## <a name="create-a-simple-app-service"></a>Een eenvoudige app-service maken
Deze sectie bevat een voorbeeld-Ansible-playbook waarmee de volgende resources worden gedefinieerd:
- Resourcegroep, waarin uw App Service-plan en web-app worden geïmplementeerd
- Web-app met een runtime voor Java 8- en Tomcat-containers in App Service in Linux

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    webapp_name: myfirstWebApp
    plan_name: myAppServicePlan
    location: eastus
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

    - name: Create App Service on Linux with Java Runtime
      azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: "{{ plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        frameworks:
          - name: "java"
            version: "8"
            settings:
              java_container: tomcat
              java_container_version: 8.5
```
Sla het voorgaande playbook op als **firstwebapp.yml**.

Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook firstwebapp.yml
```

In de uitvoer van het uitvoeren van het Ansible-playbook ziet u dat het maken van de web-app is geslaagd:

```Output
PLAY [localhost] *************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [Create a resource group] *************************************************
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] *************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

PLAY RECAP *************************************************
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="create-an-app-service-by-using-traffic-manager"></a>Een app-service maken met behulp van Traffic Manager
U kunt [Azure Traffic Manager](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager) gebruiken om te bepalen hoe aanvragen van web-clients worden gedistribueerd naar apps in Azure App Service. Wanneer App Service-eindpunten worden toegevoegd aan een Azure Traffic Manager-profiel, wordt met Traffic Manager de status van de App Service-apps bijgehouden. Statussen omvatten: actief, gestopt en verwijderd. In Traffic Manager wordt vervolgens bepaald of deze eindpunten verkeer kunnen ontvangen.

In App Service wordt een app uitgevoerd in een [App Service-plan](https://docs.microsoft.com/azure/app-service/overview-hosting-plans
). Een App Service-plan definieert een set rekenresources waarmee een web-app kan worden uitgevoerd. U kunt het App Service-plan en de web-app in verschillende groepen beheren.

Deze sectie bevat een voorbeeld-Ansible-playbook waarmee de volgende resources worden gedefinieerd:
- Resourcegroep, waarin uw App Service-plan wordt geïmplementeerd
- App Service-plan
- Secundaire resourcegroep, waarin de web-app wordt geïmplementeerd
- Web-app met een runtime voor Java 8- en Tomcat-containers in App Service in Linux
- Traffic Manager-profiel
- Traffic Manager-eindpunt, dat gebruikmaakt van de gemaakte website

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group_webapp: myResourceGroupWebapp
    resource_group: myResourceGroup
    webapp_name: myLinuxWebApp
    plan_name: myAppServicePlan
    location: eastus
    traffic_manager_profile_name: myTrafficManagerProfile
    traffic_manager_endpoint_name: myTrafficManagerEndpoint

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group_webapp }}"
        location: "{{ location }}"

  - name: Create secondary resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

  - name: Create App Service Plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      location: "{{ location }}"
      is_linux: true
      sku: S1
      number_of_workers: 1

  - name: Create App Service on Linux with Java Runtime
    azure_rm_webapp:
        resource_group: "{{ resource_group_webapp }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: "{{ plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        app_settings:
          testkey: "testvalue"
        frameworks:
          - name: java
            version: 8
            settings:
              java_container: "Tomcat"
              java_container_version: "8.5"

  - name: Get web app facts
    azure_rm_webapp_facts:
      resource_group: "{{ resource_group_webapp }}"
      name: "{{ webapp_name }}"
    register: webapp
    
  - name: Create Traffic Manager Profile
    azure_rm_trafficmanagerprofile:
      resource_group: "{{ resource_group_webapp }}"
      name: "{{ traffic_manager_profile_name }}"
      location: global
      routing_method: performance
      dns_config:
        relative_name: "{{ traffic_manager_profile_name }}"
        ttl:  60
      monitor_config:
        protocol: HTTPS
        port: 80
        path: '/'

  - name: Add endpoint to traffic manager profile, using created web site
    azure_rm_trafficmanagerendpoint:
      resource_group: "{{ resource_group_webapp }}"
      profile_name: "{{ traffic_manager_profile_name }}"
      name: "{{ traffic_manager_endpoint_name }}"
      type: azure_endpoints
      location: "{{ location }}"
      target_resource_id: "{{ webapp.webapps[0].id }}"
```
Sla het voorgaande playbook op als **webapp.yml** of [download het playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/webapp.yml).

Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook webapp.yml
```

In de uitvoer van het Ansible-playbook ziet u dat het maken van het App Service-plan, de web-app, het Traffic Manager-profiel en het eindpunt is geslaagd:
```Output
PLAY [localhost] *************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [Create resource group] ****************************************************************************
changed: [localhost]

TASK [Create resource group for app service plan] ****************************************************************************
changed: [localhost]

TASK [Create App Service Plan] ****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ****************************************************************************
changed: [localhost]

TASK [Get web app facts] *****************************************************************************
ok: [localhost]

TASK [Create Traffic Manager Profile] *****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for TrafficManagerManagementClient

changed: [localhost]

TASK [Add endpoint to traffic manager profile, using the web site created above] *****************************************************************************
changed: [localhost]

TASK [Get Traffic Manager Profile facts] ******************************************************************************
ok: [localhost]

PLAY RECAP ******************************************************************************
localhost                  : ok=9    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Web-apps van Azure App Service schalen met Ansible](https://docs.microsoft.com/azure/ansible/ansible-scale-azure-web-apps)