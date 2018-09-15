---
title: Overzicht van Azure Automation-status
description: Een overzicht van Azure Automation State Configuration (DSC), de voorwaarden en bekende problemen
keywords: PowerShell dsc, configuratie van gewenste status, powershell dsc azure
services: automation
ms.service: automation
ms.component: dsc
author: bobbytreed
ms.author: robreed
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ef55e6ca6fc913710bae68a7423369b33f26c009
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45628992"
---
# <a name="azure-automation-state-configuration-overview"></a>Overzicht van Azure Automation-status

Configuratie van de status van Azure Automation is een Azure-service waarmee u te schrijven, beheren en compileren PowerShell Desired State Configuration (DSC) [configuraties](/powershell/dsc/configurations), importeren [DSC-Resources](/powershell/dsc/resources), en configuraties toewijzen aan doelknooppunten, allemaal in de cloud.

## <a name="why-use-azure-automation-state-configuration"></a>Waarom Azure Automation State Configuration gebruiken

Statusconfiguratie van Azure Automation biedt verschillende voordelen boven het gebruik van DSC buiten Azure.

### <a name="built-in-pull-server"></a>Ingebouwde pull-server

Configuratie van de status van Azure Automation biedt een DSC-pull-server die vergelijkbaar is met de [Windows functie DSC-Service](/powershell/dsc/pullserver) zodat doelknooppunten automatisch configuraties ontvangen, voldoen aan de gewenste status en rapporteren over hun naleving van beleid. De ingebouwde pull-server in Azure Automation elimineert de noodzaak te instellen en onderhouden van uw eigen pull-server. Azure Automation kunt zich richten op virtuele of fysieke Windows- of Linux-systemen, in de cloud of on-premises.

### <a name="management-of-all-your-dsc-artifacts"></a>Beheer van alle DSC-artefacten

Statusconfiguratie van Azure Automation biedt dezelfde beheerlaag [PowerShell Desired State Configuration](/powershell/dsc/overview) als Azure Automation voor PowerShell-scripts biedt.

Vanuit Azure portal of PowerShell, kunt u alle uw DSC-configuraties, resources en doelknooppunten kunt beheren.

![Schermopname van de Azure Automation-blade](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Rapportage gegevens importeren in Log Analytics

Gedetailleerde status rapportagegegevens verzenden knooppunten die worden beheerd met Azure Automation State Configuration naar de ingebouwde pull-server. U kunt Azure Automation State Configuration voor het verzenden van deze gegevens aan uw Log Analytics-werkruimte configureren. Zie voor meer informatie over het verzenden van gegevens over de toepassingsstatus State Configuration naar uw Log Analytics-werkruimte, [doorsturen Azure Automation State Configuration gegevens rapporteren aan Log nalytics](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Introductievideo

Wilt u liever kijken dan lezen? Bekijk de volgende video van mei 2015 wanneer Azure Automation State Configuration voor het eerst werd aangekondigd hebben.

> [!NOTE]
> Hoewel de concepten en de levenscyclus van die in deze video worden besproken correct zijn, Azure Automation State Configuration heeft veel vooruitgang geboekt met sinds deze video is opgenomen. Het is nu algemeen beschikbaar, heeft een veel uitgebreidere gebruikersinterface in Azure portal en biedt ondersteuning voor veel extra mogelijkheden.

[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen, Zie [aan de slag met Azure Automation State Configuration](automation-dsc-getting-started.md)
- Voor meer informatie over hoe voor onboarding knooppunten, Zie [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md)
- Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md)
- Zie voor naslagdocumentatie voor PowerShell-cmdlet, [Statusconfiguratie van Azure Automation-cmdlets](/powershell/module/azurerm.automation/#automation)
- Zie voor informatie over de prijzen [Statusconfiguratie van Azure Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/)
- Zie voor een voorbeeld van het gebruik van Azure Automation State Configuration in een pijplijn voor continue implementatie [continue implementatie met behulp van Azure Automation Statusconfiguratie- en Chocolatey](automation-dsc-cd-chocolatey.md)