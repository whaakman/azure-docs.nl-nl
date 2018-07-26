---
title: Overzicht van Azure Automation DSC
description: Een overzicht van Azure Automation Desired State Configuration (DSC), de voorwaarden en bekende problemen
keywords: PowerShell dsc, configuratie van gewenste status, powershell dsc azure
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f6d49ffa59ed53c0a1966a4132fd5fe1689a13ce
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247346"
---
# <a name="azure-automation-dsc-overview"></a>Overzicht van Azure Automation DSC

Azure Automation DSC is een Azure-service waarmee u te schrijven, beheren en compileren PowerShell Desired State Configuration (DSC) [configuraties](https://msdn.microsoft.com/powershell/dsc/configurations), importeren [DSC-Resources](https://msdn.microsoft.com/powershell/dsc/resources), en configuraties toe te wijzen aan de doelknooppunten, allemaal in de cloud.

## <a name="why-use-azure-automation-dsc"></a>Waarom Azure Automation DSC gebruiken

Azure Automation DSC biedt verschillende voordelen boven het gebruik van DSC buiten Azure.

### <a name="built-in-pull-server"></a>Ingebouwde pull-server

Azure Automation biedt een DSC-pull-server die vergelijkbaar is met de [Windows functie DSC-Service](/powershell/dsc/pullserver) zodat doelknooppunten automatisch configuraties ontvangen, voldoen aan de gewenste status en rapporteren over hun compliance.
De ingebouwde pull-server in Azure Automation elimineert de noodzaak te instellen en onderhouden van uw eigen pull-server.
Azure Automation kunt zich richten op virtuele of fysieke Windows- of Linux-systemen, in de cloud of on-premises.

### <a name="management-of-all-your-dsc-artifacts"></a>Beheer van alle DSC-artefacten

Azure Automation DSC onderweg beschikt over dezelfde beheerlaag [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) als Azure Automation voor PowerShell-scripts biedt.

Vanuit Azure portal of PowerShell, kunt u alle uw DSC-configuraties, resources en doelknooppunten kunt beheren.

![Schermopname van de Azure Automation-blade](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Rapportage gegevens importeren in Log Analytics

Gedetailleerde status rapportagegegevens verzenden knooppunten die worden beheerd met Azure Automation DSC naar de ingebouwde pull-server.
U kunt Azure Automation DSC voor het verzenden van deze gegevens aan uw Log Analytics-werkruimte configureren.
Zie voor meer informatie over het verzenden van DSC-statusgegevens naar uw Log Analytics-werkruimte, [doorsturen Azure Automation-DSC reporting gegevens naar Log Analytics](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Introductievideo

Wilt u liever kijken dan lezen? Bekijk de volgende video van mei 2015 wanneer Azure Automation DSC voor het eerst werd aangekondigd hebben.

>[!NOTE]
>Hoewel de concepten en de levenscyclus van die in deze video worden besproken correct zijn, Azure Automation DSC is er veel vooruitgang geboekt sinds deze video is opgenomen.
>Het is nu algemeen beschikbaar, heeft een veel uitgebreidere gebruikersinterface in Azure portal en biedt ondersteuning voor veel extra mogelijkheden.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over hoe om te onboarden knooppunten die moeten worden beheerd met Azure Automation DSC, Zie [Onboarding van machines voor beheer met Azure Automation DSC](automation-dsc-onboarding.md)
* Als u wilt aan de slag met Azure Automation DSC, Zie [aan de slag met Azure Automation DSC](automation-dsc-getting-started.md)
* Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [in Azure Automation DSC-configuraties compileren](automation-dsc-compile.md)
* Zie voor PowerShell-cmdlet-naslaginformatie voor Azure Automation DSC, [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)
* Zie voor informatie over de prijzen [prijzen voor Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)
* Zie voor een voorbeeld van het gebruik van Azure Automation DSC in een pijplijn voor continue implementatie [continue implementatie voor IaaS VM's met behulp van Azure Automation DSC en Chocolatey](automation-dsc-cd-chocolatey.md)
