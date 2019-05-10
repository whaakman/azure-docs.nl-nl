---
title: Ansible-module en versie matrix voor Azure | Microsoft Docs
description: Overzicht met Ansible-modules en -versies voor Azure
keywords: ansible, rollen, matrix, versie, azure, devops
ms.topic: reference
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: b3396b2f4639cc7298b77810dbaafadd308d6b24
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230991"
---
# <a name="ansible-module-and-version-matrix"></a>Ansible-module en versie matrix

Ansible bevat een reeks modules voor gebruik in de inrichting en het configureren van Azure-resources. Deze resources omvatten virtuele machines, schaalsets, services en containerservices netwerken. In dit artikel worden de verschillende Ansible-modules voor Azure en de Ansible-versies waar ze af te leveren.

## <a name="ansible-modules-for-azure"></a>Ansible-modules voor Azure

De volgende modules kunnen worden uitgevoerd op externe hosts rechtstreeks of via playbooks.

Deze modules zijn beschikbaar in de officiële release van Ansible en uit de volgende Microsoft-playbook-rollen.

| Ansible-module voor Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | Ansible 2.8 | Ansible-rol | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|--------------|--------------| 
| **Compute**                    |           |                          |                          |                            |           |           |
| azure_rm_availabilityset                    | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_availabilityset_facts              | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_deployment                         | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_deployment_facts                   | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_functionapp                        | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_functionapp_facts                  | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_image                              | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_image_facts                        | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_resource                           | -            | -                           | Ja          | Ja          | Ja          | Ja          |
| azure_rm_resource_facts                     | -            | -                           | Ja          | Ja          | Ja          | Ja          |
| azure_rm_resourcegroup                      | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_resourcegroup_facts                | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_virtualmachine                     | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_virtualmachine_facts               | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_virtualmachineextension           | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_virtualmachineextension_facts      | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_virtualmachineimage_facts          | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_virtualmachinescaleset            | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_virtualmachinescaleset_facts      | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_virtualmachinescalesetextension    | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_virtualmachinescalesetextension_facts | -            | -                        | -            | -            | Ja          | Ja          |
| azure_rm_virtualmachinescalesetinstance     | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_virtualmachinescalesetinstance_facts | -            | -                         | -            | -            | Ja          | Ja          |
| **Netwerken**                              |              |                             |              |              |              |              |
| azure_rm_appgateway                         | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_appgwroute                         | -            | -                           | -            | -            | -          | Ja          |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | -          | Ja          |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | -          | Ja          |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | -          | Ja          |
| azure_rm_applicationsecuritygroup           | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_applicationsecuritygroup_facts     | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_cdnendpoint                        | -            | -                         | -          | -            | Ja          | Ja          |
| azure_rm_cdnendpoint_facts                  | -            | -                         | -          | -            | Ja          | Ja          |
| azure_rm_cdnprofile                         | -            | -                         | -          | -            | Ja          | Ja          |
| azure_rm_cdnprofile_facts                   | -            | -                         | -          | -            | Ja          | Ja          |
| azure_rm_dnsrecordset                       | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_dnsrecordset_facts                 | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_dnszone                            | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_dnszone_facts                      | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_loadbalancer                       | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_loadbalancer_facts                 | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_networkinterface                   | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_networkinterface_facts             | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_publicipaddress                    | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_publicipaddress_facts              | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_route                              | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_routetable                         | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_routetable_facts                   | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_securitygroup                      | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_subnet                             | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_subnet_facts                       | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_trafficmanagerendpoint             | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_trafficmanagerendpoint_facts       | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_trafficmanagerprofile              | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_trafficmanagerprofile_facts        | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_virtualnetwork                     | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_virtualnetwork_facts               | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_virtualnetworkpeering              | -            | -                         | -          | -            | Ja          | Ja          |
| **Storage**                    |           |                          |                          |                            |           |           |
| azure_rm_manageddisk                        | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_manageddisk_facts                  | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_storageaccount                     | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_storageaccount_facts               | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_storageblob                        | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| **Web**                    |           |                          |                          |                             |           |           |
| azure_rm_appserviceplan                     | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_appserviceplan_facts               | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_webapp                             | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_webapp_facts                       | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_webappslot                         | -            | -                         | -          | -            | Ja          | Ja          |
| **Containers**                    |           |                          |                          |                            |           |           |
| azure_rm_acs                                | Ja          | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_aks                                | -            | -                           | Ja          | Ja          | Ja          | Ja          |
| azure_rm_aks_facts                          | -            | -                           | Ja          | Ja          | Ja          | Ja          |
| azure_rm_aksversion_facts                   | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_containerinstance                  | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_containerinstance_facts            | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_containerregistry                  | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_containerregistry_facts            | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | Ja          | Ja          |
| **Databases**                    |           |                          |                          |                             |           |           |
| azure_rm_cosmosdbaccount                    | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_cosmosdbaccount_facts              | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mariadbconfiguration               | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mariadbconfiguration_facts         | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mariadbdatabase                    | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mariadbdatabase_facts              | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mariadbfirewallrule                | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mariadbfirewallrule_facts          | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mariadbserver                      | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mariadbserver_facts                | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mysqldatabase                      | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_mysqlserver                        | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_postgresqldatabase                 | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_postgresqlserver                   | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_rediscache                         | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_rediscache_facts                   | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_rediscachefirewallrule             | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_sqldatabase                        | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Ja          | Ja          | Ja          |
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_sqlserver                          | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_sqlserver_facts                    | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| **Analytische gegevens**                    |           |                          |                          |                             |           |           |
| azure_rm_hdinsightcluster                   | -            | -                           | -            | -            | Ja          | Ja          |
| **Integratie**                    |           |                          |                          |                             |           |           |
| azure_rm_servicebus                         | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_servicebus_facts                   | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_servicebusqueue                    | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_servicebussaspolicy                | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_servicebustopic                    | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_servicebustopicsubscription        | -            | -                           | -            | -            | Ja          | Ja          |
| **Beveiliging**                    |           |                          |                          |                             |           |           |
| azure_rm_keyvault                           | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -          | Ja          | Ja          |
| azure_rm_keyvaultkey                        | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_keyvaultsecret                     | -            | Ja                         | Ja          | Ja          | Ja          | Ja          |
| azure_rm_roleassignment                     | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_roleassignment_facts               | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_roledefinition                     | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_roledefinition_facts               | -            | -                           | -            | -            | Ja          | Ja          |
| **DevOps**               |           |                          |                          |                             |           |           |
| azure_rm_devtestlab                         | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlab_facts                   | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabarmtemplate_facts        | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabartifact_facts           | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabartifactsource           | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabartifactsource_facts     | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabcustomimage              | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabenvironment              | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabpolicy                   | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabschedule                 | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabvirtualmachine           | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabvirtualmachine_facts | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabvirtualnetwork           | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_devtestlabvirtualnetwork_facts     | -            | -                           | -            | -            | Ja          | Ja          |
| **Azure Monitor**          |           |                          |                          |                             |           |           |
| azure_rm_autoscale                  | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_autoscale_facts            | -            | -                         | -          | Ja          | Ja          | Ja          |
| azure_rm_loganalyticsworkspace              | -            | -                           | -            | -            | Ja          | Ja          |
| azure_rm_loganalyticsworkspace_facts        | -            | -                           | -            | -            | Ja          | Ja          |

## <a name="introduction-to-playbook-role-for-azure"></a>Inleiding tot de rol van de playbook voor Azure

De [azure_preview_module playbook rol](https://galaxy.ansible.com/Azure/azure_preview_modules/) bevat alle de meest recente Azure-modules. De updates en oplossingen voor problemen bent tijdig meer dan de officiële release van Ansible klaar. Als u Ansible voor Azure-resource doeleinden wordt ingericht gebruikt, bent u aangeraden voor het installeren van de `azure_preview_module` playbook-rol.

De `azure_preview_module` playbook rol elke drie weken wordt uitgebracht.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over playbook-rollen, [herbruikbare playbooks maken](https://docs.ansible.com/ansible/latest/playbooks_reuse.html). 
