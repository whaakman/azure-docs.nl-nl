---
title: Ansible-module en versie matrix voor Azure
description: Ansible-module en versie matrix voor Azure
ms.service: ansible
keywords: ansible, rollen, matrix, versie, azure, devops
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 07/02/2018
ms.topic: article
ms.openlocfilehash: c9be94d1ea77b3609f146a373574e10b7f4d4355
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37859915"
---
# <a name="ansible-module-and-version-matrix"></a>Ansible-module en versie matrix

## <a name="ansible-modules-for-azure"></a>Ansible-modules voor Azure
Ansible wordt geleverd met een aantal modules die kunnen worden uitgevoerd op externe hosts rechtstreeks of via playbooks.
In dit artikel geeft een lijst van de Ansible-modules voor Azure die Azure-cloud-resources zoals VM-, netwerk- en containerservices kunt inrichten. U kunt deze modules ophalen van de officiële release van Ansible of van de volgende playbook-rollen dat is uitgegeven door Microsoft.

| Ansible-module voor Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 |  Playbook Role [azure_preview_module](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------| 
| **Compute**                    |           |                          |                          |                                  | 
| azure_rm_availabilityset                    | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_availabilityset_facts              | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_deployment                         | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_resource                           | -            | -                           | Ja          | Ja                                 | 
| azure_rm_resource_facts                     | -            | -                           | Ja          | Ja                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_virtualmachineimage_facts          | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_resourcegroup                      | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_resourcegroup_facts                | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_virtualmachine                     | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_virtualmachine_extension           | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_virtualmachine_scaleset            | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_image                              |              | Ja                         | Ja          | Ja                                 | 
| **Netwerken**                    |           |                          |                          |                                  | 
| azure_rm_virtualnetwork                     | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_virtualnetwork_facts               | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_subnet                             | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_networkinterface                   | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_networkinterface_facts             | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_publicipaddress                    | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_publicipaddress_facts              | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_dnsrecordset                       | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_dnsrecordset_facts                 | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_dnszone                            | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_dnszone_facts                      | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_loadbalancer                       | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_loadbalancer_facts                 | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_appgw                              | -            | -                           | -            | Ja                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | Ja                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | Ja                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | Ja                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | Ja                                 |
| azure_rm_securitygroup                      | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | Ja                                 | 
| **Storage**                    |           |                          |                          |                                  | 
| azure_rm_storageaccount                     | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_storageaccount_facts               | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_storageblob                        | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_managed_disk                       | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_managed_disk_facts                 | Ja          | Ja                         | Ja          | Ja                                 | 
| **Containers**                    |           |                          |                          |                                  | 
| azure_rm_aks                                | -            | -                           | Ja          | Ja                                 | 
| azure_rm_aks_facts                          | -            | -                           | Ja          | Ja                                 | 
| azure_rm_acs                                | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_containerinstance                  | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -            | Ja                                 | 
| azure_rm_containerregistry                  | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | Ja                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | Ja                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | Ja                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | Ja                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | Ja                                 | 
| **Azure Functions**                    |           |                          |                          |                                  | 
| azure_rm_functionapp                        | Ja          | Ja                         | Ja          | Ja                                 | 
| azure_rm_functionapp_facts                  | Ja          | Ja                         | Ja          | Ja                                 | 
| **Databases**                    |           |                          |                          |                                  | 
| azure_rm_sqlserver                          | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_sqlserver_facts                    | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_sqldatabase                        | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | Ja                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | Ja                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | Ja                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Ja                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | Ja                                 | 
| azure_rm_mysqlserver                        | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | Ja                                 | 
| azure_rm_mysqldatabase                      | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | Ja                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | Ja                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | Ja                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | Ja                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | Ja                                 | 
| azure_rm_postgresqlserver                   | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | Ja                                 | 
| azure_rm_postgresqldatabase                 | -            | Ja                         | Ja          | Ja                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | Ja                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | Ja                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | Ja                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | Ja                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | Ja                                 | 
| **Key Vault**                    |           |                          |                          |                                  | 
| azure_rm_keyvault                           | -            | Ja                         | Ja          | Ja                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -            | Ja                                 |
| azure_rm_keyvaultkey                        | -            | Ja                         | Ja          | Ja                                 |
| azure_rm_keyvaultsecret                     | -            | Ja                         | Ja          | Ja                                 |


## <a name="introduction-to-playbook-role-for-azure"></a>Inleiding tot de rol van de playbook voor Azure
De [azure_preview_module playbook rol](https://galaxy.ansible.com/Azure/azure_preview_modules/) is de meest volledige rol en bevat alle de meest recente Azure-modules. De updates en oplossingen voor problemen bent tijdig meer dan de officiële release van Ansible klaar. Als u Ansible voor Azure-resource doeleinden wordt ingericht gebruikt, bent u aangeraden de functie azure_preview_module installeren.

De rol van de playbook azure_preview_module wordt elke drie weken vrijgegeven.

## <a name="next-steps"></a>Volgende stappen
Meer informatie met betrekking tot playbook-rollen, verwijzen naar [Playbooks voor het maken van herbruikbare](http://docs.ansible.com/ansible/latest/playbooks_reuse.html). 
