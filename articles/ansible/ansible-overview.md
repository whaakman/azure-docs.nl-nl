---
title: Wanneer u Ansible met Azure | Microsoft Docs
description: Leer Ansible gebruiken om cloudinrichting, configuratiebeheer en toepassingsimplementaties te automatiseren.
keywords: ansible, azure, devops, overzicht, cloudinrichting, configuratiebeheer, implementatie van toepassing, ansible-modules, ansible-playbooks
ms.topic: overview
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/22/2019
ms.openlocfilehash: 54d66a0ac425a9a0a63c91317ead0e02ecf9452c
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63764150"
---
# <a name="using-ansible-with-azure"></a>Ansible gebruiken met Azure

[Ansible](https://www.ansible.com) is een open-sourceproduct waarmee het inrichten van clouds, configuratiebeheer en toepassingsimplementatie kan worden geautomatiseerd. U kunt Ansible gebruiken om virtuele machines, containers, netwerken en volledige cloudinfrastructuren in te richten. Ansible ook, kunt u de implementatie en configuratie van bronnen in uw omgeving automatiseren.

Dit artikel biedt een eenvoudig overzicht van de voordelen van het gebruik van Ansible in combinatie met Azure.

## <a name="ansible-playbooks"></a>Ansible-playbooks

[Ansible-playbooks](https://docs.ansible.com/ansible/latest/playbooks.html) kunt u direct Ansible om uw omgeving te configureren. Playbooks zijn gecodeerd met behulp van YAML zodat deze mensen leesbaar. De sectie zelfstudies bevat veel voorbeelden van het gebruik van playbooks installeren en configureren van Azure-resources. 

## <a name="ansible-modules"></a>Ansible-modules

Ansible bevat een reeks [Ansible modules](https://docs.ansible.com/ansible/latest/modules_by_category.html) die worden uitgevoerd op externe hosts rechtstreeks of via [playbooks](https://docs.ansible.com/ansible/latest/playbooks.html). Gebruikers kunnen hun eigen modules maken. Modules worden gebruikt voor het beheren van systeembronnen -, zoals services, pakketten of -bestanden - of systeemopdrachten uit te voeren.

Voor interactie met Azure-services, Ansible bevat een suite van [Ansible cloud modules](https://docs.ansible.com/ansible/list_of_cloud_modules.html#azure). Deze modules kunt u maken en organiseren van uw infrastructuur in Azure. 

## <a name="migrate-existing-workload-to-azure"></a>Bestaande workloads migreren naar Azure

Wanneer u Ansible gebruikt voor het definiëren van uw infrastructuur, kunt u van uw toepassing playbook laat Azure uw omgeving automatisch schalen naar behoefte kunt toepassen. 

## <a name="automate-cloud-native-application-in-azure"></a>Cloudeigen toepassingen automatiseren in Azure

Met Ansible kunt u cloud-apps in Azure automatiseren met behulp van Azure-microservices zoals [Azure Functions](https://azure.microsoft.com//services/functions/) en [Kubernetes on Azure](https://azure.microsoft.com/services/container-service/kubernetes/).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Implementaties met een dynamische voorraad beheren

Via de functie voor [dynamische voorraad](https://docs.ansible.com/ansible/intro_dynamic_inventory.html) biedt Ansible de mogelijkheid om inventarisgegevens voor Azure-resources op te halen. Vervolgens kunt u uw bestaande Azure-implementaties labelen en deze gelabelde implementaties beheren via Ansible.

## <a name="additional-azure-marketplace-options"></a>Aanvullende opties voor Azure Marketplace

De [Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) is een Azure Marketplace-installatiekopie door Red Hat. 

Ansible Tower is een webgebaseerde gebruikersinterface en een dashboard voor Ansible met de volgende functies:

* Hiermee kunt u voor het definiëren van op rollen gebaseerd toegangsbeheer, taakplanning en grafische inventarisbeheer. 
* Bevat een REST-API en de CLI, zodat u Tower in bestaande hulpprogramma's en processen invoegen kunt. 
* Biedt ondersteuning voor realtime uitvoer van de playbook-uitvoerbewerkingen. 
* Versleutelt de referenties - zoals Azure en SSH-sleutels -, zodat u taken delegeren kunt zonder referenties bloot te stellen.

## <a name="ansible-module-and-version-matrix-for-azure"></a>Overzicht met Ansible-modules en -versies voor Azure

Ansible bevat een reeks modules voor gebruik in de inrichting en het configureren van Azure-resources. Deze resources omvatten virtuele machines, schaalsets, services en containerservices netwerken. De [Ansible matrix](./ansible-matrix.md) geeft een lijst van de Ansible-modules voor Azure en de Ansible-versie waarin ze af te leveren.

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: De sjabloon van de Ansible-oplossing implementeren voor Azure op CentOS](./ansible-deploy-solution-template.md)
- [Snelstart: Virtuele Linux-machines in Azure met behulp van Ansible configureren](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fazure%2Fansible%2Ftoc.json&bc=%2Fazure%2Fbread%2Ftoc.json)