---
title: Azure-web-apps maken met Ansible (preview)
description: Leer hoe u Ansible gebruikt om een web-app te maken met een runtime voor Java 8- en Tomcat-containers in App Service onder Linux
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, Azure App Service, web-app, Java
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: 1899b1fc1e0a38d859fb3a7ce2153585579650f3
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586670"
---
# <a name="create-azure-app-service-web-apps-using-ansible-preview"></a>Web-apps van Azure App Service maken met Ansible (preview)
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview) (of kortweg Web Apps) is een service voor het hosten van webtoepassingen, REST-API's en mobiele back-ends. U kunt er in uw favoriete taal programmeren, of het nu .NET, .NET Core, Java, Ruby, Node.js, PHP of Python is.

U kunt Ansible ook gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren. In dit artikel leest u hoe u Ansible gebruikt om een web-app te maken met de Java-runtime. 

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.7 is vereist om de volgende voorbeeld-playbooks in deze zelfstudie uit te voeren. U kunt de RC-versie van Ansible 2.7 installeren door `sudo pip install ansible[azure]==2.7.0rc2` uit te voeren. Ansible 2.7 wordt uitgebracht in oktober 2018. Daarna hoeft u hier geen versie op te geven omdat 2.7 de standaardversie is. 

## <a name="create-a-simple-app-service"></a>Een eenvoudige app-service maken
Deze sectie bevat een voorbeeld-Ansible-playbook waarmee de volgende resources worden gedefinieerd:
- Resourcegroep, waarin uw App Service-plan en web-app worden geïmplementeerd
- Web-app, een web-app met een runtime voor Java 8- en Tomcat-containers in App Service onder Linux

```
- hosts: localhost
  connection: local
  vars:
    resource_group: myfirstResourceGroup
    webapp_name: myfirstWebApp
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
          name: myappplan
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
Sla het bovenstaande playbook op als firstwebapp.yml.

Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook firstwebapp.yml
```

In de uitvoer van het uitvoeren van het Ansible-playbook ziet u dat de web-app is gemaakt:

```
TASK [Create a resource group] *************************************************
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ******************************
 [WARNING]: Azure API profile latest does not define an entry for
WebSiteManagementClient
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0   
```

## <a name="create-app-service-with-traffic-manager"></a>App Service maken met Traffic Manager
U kunt [Azure Traffic Manager](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager) gebruiken om te bepalen hoe aanvragen van web-clients worden gedistribueerd naar apps in Azure App Service. Wanneer App Service-eindpunten worden toegevoegd aan een Azure Traffic Manager-profiel, houdt Azure Traffic Manager de status van uw App Service-apps bij (actief, gestopt of verwijderd) zodat de service kan bepalen welke van deze eindpunten verkeer moeten ontvangen.

In App Service wordt een app uitgevoerd in een [App Service-plan](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview
). Een App Service-plan definieert een set rekenresources waarmee een web-app kan worden uitgevoerd. U kunt uw App Service-plan en de web-app in verschillende groepen beheren.

Deze sectie bevat een voorbeeld-Ansible-playbook waarmee de volgende resources worden gedefinieerd:
- Resourcegroep, waarin uw App Service-plan wordt geïmplementeerd
- App Service-plan
- Resourcegroep, waarin u uw App Service-plan en web-app worden geïmplementeerd
- Web-app, een web-app met een runtime voor Java 8- en Tomcat-containers in App Service onder Linux
- Traffic Manager-profiel
- Traffic Manager-eindpunt, dat gemaakte website gebruikt

```
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_resource_group: planResourceGroup
    app_name: myLinuxWebApp
    location: eastus
    linux_plan_name: myAppServicePlan
    traffic_manager_profile_name: myTrafficManagerProfile
    traffic_manager_endpoint_name: myTrafficManagerEndpoint

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

  - name: Create secondary resource group
    azure_rm_resourcegroup:
        name: "{{ plan_resource_group }}"
        location: "{{ location }}"

  - name: Create App Service Plan
    azure_rm_appserviceplan:
      resource_group: "{{ plan_resource_group }}"
      name: "{{ linux_plan_name }}"
      location: "{{ location }}"
      is_linux: true
      sku: S1
      number_of_workers: 1

  - name: Create App Service on Linux with Java Runtime
    azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ app_name }}"
        plan:
          resource_group: "{{ plan_resource_group }}"
          name: "{{ linux_plan_name }}"
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
      resource_group: "{{ resource_group }}"
      name: "{{ app_name }}"
    register: webapp
    
  - name: Create Traffic Manager Profile
    azure_rm_trafficmanagerprofile:
      resource_group: "{{ resource_group }}"
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
      resource_group: "{{ resource_group }}"
      profile_name: "{{ traffic_manager_profile_name }}"
      name: "{{ traffic_manager_endpoint_name }}"
      type: azure_endpoints
      location: "{{ location }}"
      target_resource_id: "{{ webapp.webapps[0].id }}"

```
Sla het playbook hierboven op als webapp.yml of [download het playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/webapp.yml).

Als u het playbook wilt uitvoeren, gebruikt u de opdracht **ansible-playbook** als volgt:
```bash
ansible-playbook webapp.yml
```

In de uitvoer van het Ansible-playbook ziet u dat het App Service-plan, de web-app, het Traffic Manager-profiel en het eindpunt zijn gemaakt:
```
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
> [Ansible in Azure](https://docs.microsoft.com/azure/ansible/)