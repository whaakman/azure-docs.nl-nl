---
title: Ansible gebruiken met Azure
description: Leer Ansible gebruiken om cloudinrichting, configuratiebeheer en toepassingsimplementaties te automatiseren.
ms.service: ansible
keywords: ansible, azure, devops, overzicht, cloudinrichting, configuratiebeheer, implementatie van toepassing, ansible-modules, ansible-playbooks
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 09/02/2018
ms.topic: overview
ms.openlocfilehash: 55dcda953454ce3eb4e19dabbf198f886d028180
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54357927"
---
# <a name="ansible-with-azure"></a>Ansible met Azure

[Ansible](https://www.ansible.com) is een open-sourceproduct waarmee het inrichten van clouds, configuratiebeheer en toepassingsimplementatie kan worden geautomatiseerd. U kunt Ansible gebruiken om virtuele machines, containers, netwerken en volledige cloudinfrastructuren in te richten. Ook kunt u Ansible gebruiken om de implementatie en configuratie van resources in uw omgeving te automatiseren.

Dit artikel biedt een eenvoudig overzicht van de voordelen van het gebruik van Ansible in combinatie met Azure.

## <a name="ansible-playbooks"></a>Ansible-playbooks

[Ansible-playbooks](https://docs.ansible.com/ansible/latest/playbooks.html) zijn de taal die Ansible gebruikt voor configuratie, implementatie en indeling. Ze kunnen een beleid beschrijven dat u op uw externe systemen wilt afdwingen of een reeks stappen in een algemeen IT-proces. U maakt een playbook met behulp van YAML, waarmee u een model van een configuratie of een proces definieert.

## <a name="ansible-modules"></a>Ansible-modules

Ansible bevat een reeks [Ansible-modules](https://docs.ansible.com/ansible/latest/modules_by_category.html) die direct op externe hosts kunnen worden uitgevoerd of via [playbooks](https://docs.ansible.com/ansible/latest/playbooks.html). Gebruikers kunnen ook hun eigen modules maken. Modules kunnen worden gebruikt om systeembronnen te beheren, zoals services, pakketten of bestanden, of om systeemopdrachten uit te voeren.

Voor de interactie met Azure-services bevat Ansible een serie [Ansible-cloudmodules](https://docs.ansible.com/ansible/list_of_cloud_modules.html#azure) die de hulpmiddelen bieden om eenvoudig uw infrastructuur in Azure te maken en organiseren. 

## <a name="migrate-existing-workload-to-azure"></a>Bestaande workloads migreren naar Azure

Wanneer u Ansible gebruikt om uw infrastructuur te definiëren, kunt u het playbook van uw toepassing toepassen om te zorgen dat Azure automatisch de schaal van uw omgeving naar behoefte kan aanpassen. 

## <a name="automate-cloud-native-application-in-azure"></a>Cloudeigen toepassingen automatiseren in Azure

Met Ansible kunt u cloud-apps in Azure automatiseren met behulp van Azure-microservices zoals [Azure Functions](https://azure.microsoft.com//services/functions/) en [Kubernetes on Azure](https://azure.microsoft.com/services/container-service/kubernetes/).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Implementaties met een dynamische voorraad beheren
Via de functie voor [dynamische voorraad](https://docs.ansible.com/ansible/intro_dynamic_inventory.html) biedt Ansible de mogelijkheid om inventarisgegevens voor Azure-resources op te halen. Vervolgens kunt u uw bestaande Azure-implementaties labelen en deze gelabelde implementaties beheren via Ansible.

## <a name="additional-azure-marketplace-options"></a>Aanvullende opties voor Azure Marketplace
Met de Azure Marketplace-installatiekopie voor [Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) van Red Hat kunnen organisaties IT-automatisering schalen en complexe implementaties in fysieke, virtuele en cloudinfrastructuren beheren. Ansible Tower kan extra niveaus van zichtbaarheid, beheer, beveiliging en efficiëntie bieden die nodig zijn voor hedendaagse ondernemingen. Ansible Tower versleuteld referenties, zoals Azure- en SSH-sleutels, zodat u taken kunt delegeren aan minder ervaren werknemers zonder dat u het risico loopt uw referenties bloot te stellen.

## <a name="ansible-module-and-version-matrix-for-azure"></a>Overzicht met Ansible-modules en -versies voor Azure
Ansible wordt geleverd met een aantal modules dat direct op externe hosts of via playbooks kan worden uitgevoerd.
Het [overzicht met Ansible-modules en -versies](./ansible-matrix.md) biedt een lijst Ansible-modules voor Azure die Azure-cloudresources kunnen inrichten, zoals services voor virtuele machines, netwerken en containers. 

## <a name="next-steps"></a>Volgende stappen
- [Ansible configureren](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Een Linux-VM maken](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
