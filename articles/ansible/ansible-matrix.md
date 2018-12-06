---
title: Ansible-module en versie matrix voor Azure
description: Ansible-module en versie matrix voor Azure
ms.service: ansible
keywords: ansible, rollen, matrix, versie, azure, devops
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 09/22/2018
ms.topic: article
ms.openlocfilehash: accb7b84793b9fc98bc20f1637ec30f97a7f4a87
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52962077"
---
# <a name="ansible-module-and-version-matrix"></a>Ansible-module en versie matrix

## <a name="ansible-modules-for-azure"></a>Ansible-modules voor Azure
Ansible wordt geleverd met een aantal modules die kunnen worden uitgevoerd op externe hosts rechtstreeks of via playbooks.
In dit artikel geeft een lijst van de Ansible-modules voor Azure die Azure-cloud-resources zoals VM-, netwerk- en containerservices kunt inrichten. U kunt deze modules ophalen van de officiële release van Ansible of van de volgende playbook-rollen dat is uitgegeven door Microsoft.

| Ansible-module voor Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | [Ansible-rol](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|-------------------------------------| 
| **Compute**                    |           |                          |                          |                            |                                | 
| azure_rm_availabilityset                    | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_availabilityset_facts              | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_deployment                         | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_resource                           | -            | -                           | Ja          | Ja          | Ja                                 | 
| azure_rm_resource_facts                     | -            | -                           | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachineimage_facts          | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_resourcegroup                      | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_resourcegroup_facts                | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachine                     | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachine_facts               | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_virtualmachine_extension           | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualmachine_scaleset            | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_image                              |              | Ja                         | Ja          | Ja          | Ja                                 | 
| **Netwerken**                    |           |                          |                          |                             |                               | 
| azure_rm_virtualnetwork                     | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_virtualnetwork_facts               | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_subnet                             | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_networkinterface                   | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_networkinterface_facts             | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_publicipaddress                    | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_publicipaddress_facts              | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_dnsrecordset                       | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_dnsrecordset_facts                 | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_dnszone                            | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_dnszone_facts                      | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_loadbalancer                       | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_loadbalancer_facts                 | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_appgateway                         | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | Ja                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | Ja                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | Ja                                 |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_securitygroup                      | Ja          | Ja                         | Ja          | Ja          | Ja                                 |
| azure_rm_route                              | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_routetable                         | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_routetable_facts                   | -            | -                           | -            | Ja          | Ja                                 | 
| **Storage**                    |           |                          |                          |                             |                               | 
| azure_rm_storageaccount                     | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_storageaccount_facts               | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_storageblob                        | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_managed_disk                       | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_managed_disk_facts                 | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| **Containers**                    |           |                          |                          |                            |                                | 
| azure_rm_aks                                | -            | -                           | Ja          | Ja          | Ja                                 | 
| azure_rm_aks_facts                          | -            | -                           | Ja          | Ja          | Ja                                 | 
| azure_rm_acs                                | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_containerinstance                  | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -              | -            | Ja                                 | 
| azure_rm_containerregistry                  | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | Ja                                 | 
| **Azure Functions**                    |           |                          |                          |                            |                                | 
| azure_rm_functionapp                        | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_functionapp_facts                  | Ja          | Ja                         | Ja          | Ja          | Ja                                 | 
| **Databases**                    |           |                          |                          |                             |                               | 
| azure_rm_sqlserver                          | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_sqlserver_facts                    | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_sqldatabase                        | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_mysqlserver                        | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_mysqldatabase                      | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_postgresqlserver                   | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_postgresqldatabase                 | -            | Ja                         | Ja          | Ja          | Ja                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | Ja          | Ja                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Ja                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | Ja                                 | 
| **Key Vault**                    |           |                          |                          |                             |                               | 
| azure_rm_keyvault                           | -            | Ja                         | Ja          | Ja          | Ja                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -              | Ja                               |
| azure_rm_keyvaultkey                        | -            | Ja                         | Ja          | Ja          | Ja                                 |
| azure_rm_keyvaultsecret                     | -            | Ja                         | Ja          | Ja          | Ja                                 |
| **Web-apps**                    |           |                          |                          |                             |                               | 
| azure_rm_appserviceplan                          | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_appserviceplan_facts                    | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_webapp                                  | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_webapp_facts                            | -            | -                         | -          | Ja          | Ja                                 | 
| **Traffic Manager**                    |           |                          |                          |                             |                               | 
| azure_rm_trafficmanagerendpoint                  | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_trafficmanagerendpoint_facts            | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_trafficmanagerprofile                   | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_trafficmanagerprofile_facts             | -            | -                         | -          | Ja          | Ja                                 | 
| **Automatisch schalen**                    |           |                          |                          |                             |                               | 
| azure_rm_autoscale                  | -            | -                         | -          | Ja          | Ja                                 | 
| azure_rm_autoscale_facts            | -            | -                         | -          | Ja          | Ja                                 | 

## <a name="introduction-to-playbook-role-for-azure"></a>Inleiding tot de rol van de playbook voor Azure
De [azure_preview_module playbook rol](https://galaxy.ansible.com/Azure/azure_preview_modules/) is de meest volledige rol en bevat alle de meest recente Azure-modules. De updates en oplossingen voor problemen bent tijdig meer dan de officiële release van Ansible klaar. Als u Ansible voor Azure-resource doeleinden wordt ingericht gebruikt, bent u aangeraden de azure_preview_module playbook-functie installeren.

De rol van de playbook azure_preview_module wordt elke drie weken vrijgegeven.

## <a name="next-steps"></a>Volgende stappen
Meer informatie met betrekking tot playbook-rollen, verwijzen naar [Playbooks voor het maken van herbruikbare](https://docs.ansible.com/ansible/latest/playbooks_reuse.html). 
