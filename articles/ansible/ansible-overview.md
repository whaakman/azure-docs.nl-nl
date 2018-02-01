---
title: Met behulp van Ansible met Azure
description: Inleiding tot het gebruik van Ansible naar automatiseert cloud inrichten, Configuratiebeheer en implementaties van toepassingen.
ms.service: ansible
keywords: ansible, azure, devops, overzicht, cloud inrichten, Configuratiebeheer, toepassingsimplementatie, ansible modules, ansible hulpmiddelen marketing en verkoop
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: a7ce3c239a50462a9af137eb958268f72dbf79d1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="ansible-with-azure"></a>Ansible met Azure

[Ansible](http://www.ansible.com) is een open source-product dat cloud inrichten, Configuratiebeheer en implementaties van toepassingen automatiseert. Met behulp van Ansible kunt u inrichten van virtuele machines en netwerk en containers en cloudinfrastructuren voltooien. Bovendien kunt Ansible u de implementatie en configuratie van resources in uw omgeving automatiseren.

In dit artikel biedt een basisoverzicht van enkele voordelen van het gebruik van Ansible met Azure.

## <a name="ansible-playbooks"></a>Ansible hulpmiddelen marketing en verkoop

[Ansible hulpmiddelen marketing en verkoop](http://docs.ansible.com/ansible/latest/playbooks.html) van Ansible configuratie, implementatie en orchestration-taal. Hiermee kunnen een beleid dat u wilt dat uw externe systemen om af te dwingen, of een reeks stappen in een algemene IT-proces beschreven. Wanneer u een playbook maakt u dat doet met YAML waarmee een model van een configuratie of een proces wordt gedefinieerd.

## <a name="ansible-modules"></a>Ansible modules

Ansible bevat een reeks [Ansible modules](http://docs.ansible.com/ansible/latest/modules_by_category.html) die kunnen worden uitgevoerd op externe hosts rechtstreeks of via [hulpmiddelen marketing en verkoop](http://docs.ansible.com/ansible/latest/playbooks.html). Gebruikers kunnen ook hun eigen modules maken. Modules kunnen worden gebruikt voor het beheren van systeembronnen -, zoals services, pakketten of bestanden - of systeemopdrachten uitvoeren.

Voor interactie met Azure-services, Ansible bevat een reeks [Ansible cloud modules](http://docs.ansible.com/ansible/list_of_cloud_modules.html#azure) die biedt de hulpprogramma's gemakkelijk maken en beheren van uw infrastructuur in Azure. 

## <a name="migrate-existing-workload-to-azure"></a>Migreren van bestaande werkbelasting naar Azure

Zodra u Ansible hebt gebruikt voor het definiëren van uw infrastructuur, kunt u uw toepassing playbook laten Azure automatisch schalen van uw omgeving naar behoefte kunt toepassen. 

## <a name="automate-cloud-native-application-in-azure"></a>Cloud-systeemeigen toepassing in Azure automatiseren

Ansible kunt u voor het automatiseren van cloud-systeemeigen toepassingen in Azure met Azure microservices zoals [Azure Functions](https://azure.microsoft.com//services/functions/) en [Kubernetes op Azure](https://azure.microsoft.com/services/container-service/kubernetes/)).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Beheren van implementaties met dynamische inventarisaties
Via de [dynamische inventaris](http://docs.ansible.com/ansible/intro_dynamic_inventory.html) functie, Ansible biedt de mogelijkheid pull-inventarisatie van Azure-resources. U kunt uw bestaande Azure implementaties tag en die met tags implementaties beheren via Ansible.

## <a name="additional-azure-marketplace-options"></a>Aanvullende opties voor Azure Marketplace
De [Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) Azure Marketplace-installatiekopie door Red Hat helpt organisaties bij IT automation schalen en complexe implementaties in fysieke, virtuele, beheren en cloudinfrastructuur. Ansible Tower bevat mogelijkheden waarmee u extra niveaus zichtbaarheid, beheer, beveiliging en efficiëntie voor de hedendaagse ondernemingen nodig. Ansible Tower versleutelt referenties zoals Azure en SSH-sleutels, zodat u taken aan minder ervaren werknemers zonder het risico overdragen kunt van het blootstellen van uw referenties.

## <a name="next-steps"></a>Volgende stappen
- [Ansible configureren](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Een virtuele Linux-machine maken](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)