---
title: Wanneer u Ansible gebruikt met Azure
description: Inleiding tot het gebruik van Ansible voor het automatisch inrichten van clouds, Configuratiebeheer en toepassingsimplementaties.
ms.service: ansible
keywords: ansible, azure, devops, overzicht, cloud inrichten, Configuratiebeheer, de implementatie van toepassing, ansible-modules, ansible-playbooks
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: e710770131c844598762feebe09ba50dc120de0c
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43106897"
---
# <a name="ansible-with-azure"></a>Ansible met Azure

[Ansible](http://www.ansible.com) is een open-source-product waarmee het inrichten van clouds, Configuratiebeheer en toepassingsimplementaties worden geautomatiseerd. Wanneer u Ansible gebruikt kunt u virtuele machines, containers en netwerk inrichten en cloudinfrastructuren voltooien. Ansible Bovendien kunt u de implementatie en configuratie van bronnen in uw omgeving automatiseren.

In dit artikel biedt een eenvoudig overzicht van de voordelen van het gebruik van Ansible met Azure.

## <a name="ansible-playbooks"></a>Ansible-playbooks

[Ansible-playbooks](http://docs.ansible.com/ansible/latest/playbooks.html) van Ansible-configuratie, implementatie en orchestration-taal. Ze kunnen een beleid dat u wilt dat uw externe systemen om af te dwingen, of een reeks stappen in een algemene IT-proces wordt beschreven. Bij het maken van een playbook u dat doet met YAML, waarmee een model van een configuratie of een proces wordt gedefinieerd.

## <a name="ansible-modules"></a>Ansible-modules

Ansible bevat een reeks [Ansible modules](http://docs.ansible.com/ansible/latest/modules_by_category.html) die kunnen worden uitgevoerd op externe hosts rechtstreeks of via [playbooks](http://docs.ansible.com/ansible/latest/playbooks.html). Gebruikers kunnen ook hun eigen modules maken. Modules kunnen worden gebruikt voor het beheren van systeembronnen -, zoals services, pakketten of -bestanden - of systeemopdrachten uit te voeren.

Voor interactie met Azure-services, Ansible bevat een suite van [Ansible cloud modules](http://docs.ansible.com/ansible/list_of_cloud_modules.html#azure) waarmee de hulpmiddelen voor het eenvoudig maken en organiseren van uw infrastructuur in Azure. 

## <a name="migrate-existing-workload-to-azure"></a>Bestaande workloads migreren naar Azure

Wanneer u Ansible gebruikt voor het definiëren van uw infrastructuur, kunt u van uw toepassing playbook laat Azure uw omgeving automatisch schalen naar behoefte kunt toepassen. 

## <a name="automate-cloud-native-application-in-azure"></a>Cloud-native toepassing in Azure automatiseren

Ansible kunt u voor het automatiseren van cloud-eigen apps in Azure met behulp van Azure-microservices zoals [Azure Functions](https://azure.microsoft.com//services/functions/) en [Kubernetes op Azure](https://azure.microsoft.com/services/container-service/kubernetes/)).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Beheren van implementaties met een dynamische voorraad
Via de [dynamische voorraad](http://docs.ansible.com/ansible/intro_dynamic_inventory.html) functie, Ansible biedt de mogelijkheid voor pull-inventarisatie van Azure-resources. Vervolgens kunt u code van uw bestaande Azure-implementaties en deze gelabelde implementaties via Ansible beheren.

## <a name="additional-azure-marketplace-options"></a>Aanvullende opties voor Azure Marketplace
De [Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) Azure Marketplace-installatiekopie door Red Hat helpt organisaties bij het schalen van IT-automatisering en beheer complexe implementaties in fysieke, virtuele en cloudinfrastructuren. Ansible Tower bevat mogelijkheden die meer besturingsmogelijkheden van zichtbaarheid, controle, beveiliging en efficiëntie die nodig zijn voor de hedendaagse ondernemingen bieden. Ansible Tower gecodeerd referenties zoals Azure en SSH-sleutels, zodat u taken aan minder ervaren werknemers zonder het risico delegeren kunt van het blootstellen van uw referenties.

## <a name="next-steps"></a>Volgende stappen
- [Ansible configureren](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Een Linux-VM maken](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)