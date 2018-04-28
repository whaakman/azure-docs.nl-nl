---
title: Overzicht van Azure Automation DSC
description: Een overzicht van Azure Automation Desired State Configuration (DSC), de termen en bekende problemen
keywords: PowerShell dsc, de configuratie van de gewenste status, de powershell dsc-azure
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 4282333fc8c6f0835c451b781f70dfaed0697d96
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-automation-dsc-overview"></a>Overzicht van Azure Automation DSC

Azure Automation DSC is een Azure-service waarmee u kunt schrijven, beheren en PowerShell Desired State Configuration (DSC) worden gecompileerd [configuraties](https://msdn.microsoft.com/powershell/dsc/configurations), importeren [DSC-Resources](https://msdn.microsoft.com/powershell/dsc/resources), en configuraties toewijzen aan de doelknooppunten allemaal in de cloud.

## <a name="why-use-azure-automation-dsc"></a>Waarom Azure Automation DSC gebruiken

Azure Automation DSC biedt verschillende voordelen ten opzichte van DSC buiten Azure.

### <a name="built-in-pull-server"></a>Ingebouwde pull-server

Azure Automation biedt een [DSC-pull-server](https://msdn.microsoft.com/powershell/dsc/pullserver) zodat doelknooppunten automatisch configuraties ontvangen, voldoen aan de gewenste status en rapporteren over hun compliance.
De ingebouwde pull-server in Azure Automation hoeven instellen en onderhouden van uw eigen pull-server.
Azure Automation kunt richten virtuele of fysieke Windows of Linux machines, in de cloud of on-premises.

### <a name="management-of-all-your-dsc-artifacts"></a>Beheer van alle DSC-artefacten

Azure Automation DSC biedt dezelfde management laag bij [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) zoals Azure Automation voor het PowerShell-scripts biedt.

De Azure-portal, of PowerShell, kunt u alle uw DSC-configuraties, bronnen en doelknooppunten kunt beheren.

![Schermopname van het Azure Automation-blade](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Reporting gegevens importeren in Log Analytics

Gedetailleerde status rapportagegegevens verzenden knooppunten die worden beheerd met Azure Automation DSC naar de ingebouwde pull-server.
U kunt Azure Automation DSC voor het verzenden van deze gegevens naar de werkruimte voor logboekanalyse configureren.
Zie voor meer informatie over DSC statusgegevens verzenden naar de werkruimte voor logboekanalyse, [doorsturen Azure Automation-DSC rapportagegegevens met logboekanalyse](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Introductievideo

Wilt u liever kijken dan lezen? Bekijk de volgende video van mei 2015, wanneer Azure Automation DSC voor het eerst is aangekondigd hebben.

>[!NOTE]
>Terwijl de concepten en de levenscyclus van de in deze video worden besproken correct zijn, is Azure Automation DSC veel gevorderd sinds deze video is opgenomen.
>Het is nu algemeen beschikbaar, heeft een veel uitgebreidere gebruikersinterface in de Azure-portal en biedt ondersteuning voor veel meer mogelijkheden.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over hoe voor vrijgeven knooppunten moeten worden beheerd met Azure Automation DSC zien [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md)
* Om aan de slag met Azure Automation DSC, Zie [aan de slag met Azure Automation DSC](automation-dsc-getting-started.md)
* Zie voor meer informatie over het compileren van DSC-configuraties, zodat u deze aan de doelknooppunten toewijzen kunt, [compileren van configuraties in Azure Automation DSC](automation-dsc-compile.md)
* Zie voor referentiemateriaal voor PowerShell-cmdlet voor Azure Automation DSC, [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)
* Zie voor informatie over prijzen, [prijzen van Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)
* Zie voor een voorbeeld van het gebruik van Azure Automation DSC in een pijplijn continue implementatie [continue implementatie voor IaaS VM's met behulp van Azure Automation DSC en Chocolatey](automation-dsc-cd-chocolatey.md)
