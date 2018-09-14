---
title: Terraform gebruiken met Azure
description: Inleiding tot het gebruik van Terraform om Azure-infrastructuur te versioneren en te implementeren.
services: terraform
ms.service: terraform
keywords: terraform, devops, overzicht, plannen, toepassen, automatiseren
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/31/2018
ms.openlocfilehash: dd340238f8a70c1dd0cfc172976bef6b1ad282b1
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665626"
---
# <a name="terraform-with-azure"></a>Terraform met Azure

[Hashicorp Terraform](https://www.terraform.io/) is een open-source-hulpprogramma voor het inrichten en beheren van cloudinfrastructuur. Het codificeert de infrastructuur in configuratiebestanden die de topologie van cloudresources beschrijven, zoals virtuele machines, opslagaccounts en netwerkinterfaces. De opdrachtregelinterface (CLI) van Terraform biedt een eenvoudig mechanisme om de configuratiebestanden te implementeren en te versioneren naar Azure of een andere ondersteunde cloud.

In dit artikel worden de voordelen beschreven van het gebruik van Terraform voor het beheer van de Azure-infrastructuur.

## <a name="automate-infrastructure-management"></a>Automatiseer infrastructuurbeheer.

Met de op sjablonen gebaseerde configuratiebestanden van Terraform kunt u Azure-bronnen op een herhaalbare en voorspelbare manier definiëren, inrichten en configureren. Het automatiseren van infrastructuur heeft verschillende voordelen:

- Het verkleint de kans op menselijke fouten bij het inzetten en beheren van infrastructuur.
- Dezelfde sjabloon wordt meerdere keren geïmplementeerd om identieke ontwikkel-, test- en productieomgevingen te maken.
- Reduceert de kosten van ontwikkel- en testomgevingen door ze op aanvraag te maken.

## <a name="understand-infrastructure-changes-before-they-are-applied"></a>Infrastructuurwijzigingen begrijpen voordat ze worden toegepast 

Als een resourcetopologie complex wordt, kan het moeilijk zijn om de betekenis en impact van wijzigingen in de infrastructuur te begrijpen.

Terraform biedt een opdrachtregelinterface (CLI) waarmee gebruikers infrastructuurwijzigingen kunnen valideren en vooraf kunnen bekijken voordat ze worden geïmplementeerd. Het vooraf bekijken van wijzigingen in de infrastructuur op een veilige, productieve manier heeft verschillende voordelen:
- Teamleden kunnen effectiever samenwerken door voorgestelde wijzigingen en hun impact snel te begrijpen.
- Onbedoelde veranderingen kunnen vroeg in het ontwikkelingsproces worden opgevangen


## <a name="deploy-infrastructure-to-multiple-clouds"></a>Infrastructuur implementeren in meerdere clouds

Terraform is een populaire keuze als hulpmiddel voor multi-cloudscenario's, waarbij vergelijkbare infrastructuur wordt geïmplementeerd in Azure en aanvullende cloudproviders of on-premises datacenters. Hiermee kunnen ontwikkelaars dezelfde hulpprogramma's en configuratiebestanden gebruiken om de infrastructuur op meerdere cloudproviders te beheren.

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht van Terraform en de voordelen daarvan hebt, zijn dit mogelijke volgende stappen:

- Ga aan de slag door [Terraform te installeren en te configureren om Azure te gebruiken](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure).
- [Een virtuele machine maken in Azure met behulp van Terraform](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-create-complete-vm)
- De [Azure Resource Manager-module voor Terraform](https://www.terraform.io/docs/providers/azurerm/) verkennen 