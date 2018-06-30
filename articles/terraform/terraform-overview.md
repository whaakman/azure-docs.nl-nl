---
title: Met behulp van Terraform met Azure
description: Inleiding tot het gebruik van Terraform voor versie en Azure-infrastructuur te implementeren.
ms.service: virtual-machines-linux
keywords: terraform, devops, overzicht, plannen, toepassen, automatiseren
author: binderjoe
ms.author: jbinder
ms.date: 10/19/2017
ms.topic: article
ms.openlocfilehash: 5d313bda6a1067e6d023f62fb26704f9aee5c7bf
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114469"
---
# <a name="terraform-with-azure"></a>Terraform met Azure

[Hashicorp Terraform](https://www.terraform.io/) is een open-source hulpprogramma voor het inrichten en beheren van cloudinfrastructuur. Deze codifies infrastructuur in configuratiebestanden die de topologie van cloudresources, zoals virtuele machines, opslagaccounts en netwerkinterfaces beschrijven. De Terraform-opdrachtregelinterface (CLI) biedt een eenvoudige methode voor het implementeren en de versie de configuratiebestanden in Azure of een ondersteunde cloud.

In dit artikel beschrijft de voordelen van het gebruik van Terraform voor het beheren van Azure-infrastructuur.

## <a name="automate-infrastructure-management"></a>Beheer van infrastructuur automatiseren.

Terraform van configuratiebestanden op basis van sjabloon kunnen u definiëren, inrichten en Azure-resources in een herhaalbare, voorspelbare wijze configureren. Automatiseren infrastructuur biedt verschillende voordelen:

- Wordt verlaagd eventuele menselijke fouten tijdens het implementeren en beheren van de infrastructuur.
- Implementeert dezelfde sjabloon meerdere keren identiek ontwikkeling, testen en productie-omgevingen maken.
- Beperkt de kosten van ontwikkeling en testomgevingen doordat ze op aanvraag worden gemaakt.

## <a name="understand-infrastructure-changes-before-they-are-applied"></a>Wijzigingen in de infrastructuur begrijpen voordat ze worden toegepast 

Als een resource topologie wordt complexe inzicht in de zin en de invloed van wijzigingen in de infrastructuur kan lastig zijn.

Terraform biedt een opdrachtregelinterface (CLI) op waarmee gebruikers om te valideren en wijzigingen in de infrastructuur te bekijken voordat ze worden geïmplementeerd. Een voorbeeldweergave wijzigingen in de infrastructuur in een kluis, heeft productief manier verschillende voordelen:
- Teamleden kunnen effectiever samenwerken door snel inzicht in de voorgestelde wijzigingen en het effect ervan.
- Onbedoelde wijzigingen kunnen worden onderschept vroeg in het ontwikkelingsproces


## <a name="deploy-infrastructure-to-multiple-clouds"></a>Infrastructuur implementeren op meerdere clouds

Terraform is de keuze van een populaire hulpprogramma voor meerdere cloud-scenario's waarbij vergelijkbare infrastructuur wordt geïmplementeerd op Azure en aanvullende cloudproviders of lokale datacenters. Deze kan ontwikkelaars de dezelfde hulpprogramma's en configuratiebestanden gebruiken voor het beheren van infrastructuur op meerdere cloudproviders.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van Terraform en de voordelen hebt, hier voorgestelde volgende stappen te volgen:

- Aan de slag door [Terraform installeren en configureren voor het gebruik van Azure](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure).
- [Maken van een virtuele machine van Azure met behulp van Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-create-complete-vm)
- Verken de [Azure Resource Manager-module voor Terraform](https://www.terraform.io/docs/providers/azurerm/) 