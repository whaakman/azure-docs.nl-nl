---
title: Azure-modules in Azure Automation bijwerken
description: Dit artikel wordt beschreven hoe u kunt algemene Azure PowerShell-modules standaardvoorziening in Azure Automation nu bijwerken.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 11/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 31d794aa1dccddc8018e9413c9743a0abe13f4ac
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160192"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Het bijwerken van Azure PowerShell-modules in Azure Automation

De meest voorkomende Azure PowerShell-modules worden geboden door standaard in elk Automation-account. Het Azure-team de Azure-modules regelmatig bijgewerkt, zodat in het Automation-account u een manier om bij te werken van de modules in het account vindt als nieuwe versies beschikbaar via de portal zijn.

Omdat modules regelmatig door de productgroep bijgewerkt worden, kunnen wijzigingen optreden met de cmdlets opgenomen, die mogelijk negatieve invloed hebben op uw runbooks afhankelijk van het type wijziging, zoals de naam van een parameter of volledig beëindigde van een cmdlet. Om te voorkomen dat dit gevolgen heeft voor uw runbooks en de processen die ze automatiseren, is het raadzaam dat u testen en te voordat u doorgaat valideren. Als u een speciale voor dit doel bestemde Automation-account hebt, kunt u het maken van een zodat u testen veel verschillende scenario's en permutaties tijdens de ontwikkeling van uw runbooks, Daarnaast iteratieve wijzigingen zoals het bijwerken van kunt de PowerShell-modules. Nadat de resultaten worden gevalideerd en u alle vereiste wijzigingen hebt aangebracht, gaat u verder met het coördineren van de migratie van alle runbooks die wijziging vereist en de volgende update uitvoeren, zoals beschreven in de productieomgeving.

> [!NOTE]
> Een nieuw Automation-account kan niet de meest recente modules bevatten.

## <a name="updating-azure-modules"></a>Azure-Modules bijwerken

1. Op de pagina Modules van uw Automation-account, is een optie met de naam **Update Azure-Modules**. Het is altijd ingeschakeld.<br><br> ![De optie Azure-Modules in Modules pagina bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Voordat u bijwerkt van uw Azure-modules die het wordt aanbevolen dat u werkt ze bij in een test Automation-Account om ervoor te zorgen dat werken uw bestaande scripts zoals verwacht voordat het bijwerken van uw Azure-modules.
  >
  > De **Update Azure-Modules** knop is alleen beschikbaar in de openbare cloud. Het is niet beschikbaar in de [soevereine regio's](https://azure.microsoft.com/global-infrastructure/). Raadpleeg [alternatieve manieren om bij te werken uw modules](#alternative-ways-to-update-your-modules) sectie voor meer informatie.


2. Klik op **Update Azure-Modules**, ter bevestiging een melding wordt weergegeven dat wordt gevraagd of u wilt doorgaan.<br><br> ![Melding van Azure-Modules bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Klik op **Ja** en begint met het updateproces voor de module. Het updateproces duurt ongeveer 15-20 minuten om bij te werken van de volgende modules:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Als de modules al bijgewerkt zijn, klikt u vervolgens het proces is voltooid binnen een paar seconden. Wanneer de update is voltooid, wordt u gewaarschuwd.<br><br> ![Updatestatus van de Azure-Modules bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

    De .NET core AzureRm-modules (AzureRm.*. Core) worden niet ondersteund in Azure Automation en kan niet worden geïmporteerd.

> [!NOTE]
> Azure Automation maakt gebruik van de meest recente modules in uw Automation-account wanneer een nieuwe geplande taak wordt uitgevoerd.  

Als u van deze modules Azure PowerShell-cmdlets in uw runbooks gebruiken, wilt u dit updateproces iedere maand uitvoeren of dus om ervoor te zorgen dat u de meest recente modules hebt. Azure Automation maakt gebruik van de verbinding AzureRunAsConnection om te verifiëren bij het bijwerken van de modules, als de service-principal is verlopen of niet meer op het abonnementsniveau, de module-update bestaat mislukt.

## <a name="alternative-ways-to-update-your-modules"></a>Alternatieve manieren om uw modules bijwerken

Zoals gezegd, de **Update Azure-Modules** knop is niet beschikbaar in soevereine clouds, dit is alleen beschikbaar in de globale Azure-cloud. Dit is vanwege het feit dat de nieuwste versie van de Azure PowerShell-modules uit de galerie met PowerShell niet met het Resource Manager-services in deze clouds die momenteel zijn geïmplementeerd werkt mogelijk.

Bijwerken van de modules kan nog steeds worden gedaan door het importeren van de [Update AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook ingecheckt in uw Automation-Account en uitvoeren. Met dit runbook kunt u Azure-modules bijwerken, maar ze nog steeds mogelijk niet compatibel is met wat wordt momenteel geïmplementeerd in de Azure-omgeving u gebruikt. U moet om te controleren of de juiste versies die nodig zijn voor elke module om ervoor te zorgen dat Azure Automation nog steeds uw resources kunt beheren. Azure Automation niet beheren of bijhouden van de versies van de services die zijn geïmplementeerd.

Gebruik de `AzureRmEnvironment` parameter om door te geven van de juiste omgeving aan het runbook.  Acceptabele waarden zijn **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, en **AzureUSGovernmentCloud**. Deze waarden kunnen worden verkregen via `Get-AzureRmEnvironment | select Name`. Als u een waarde voor deze parameter niet doorgegeven, het runbook de openbare cloud van Azure wordt standaard **AzureCloud**

Als u een specifieke versie van de Azure PowerShell-module gebruiken in plaats van de meest recente beschikbare op de PowerShell Gallery wilt, deze versies doorgeven aan de optionele `ModuleVersionOverrides` parameter van de **Update AzureModule** runbook. Zie voor voorbeelden van de [Update AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook. Azure PowerShell-modules die niet worden vermeld in de `ModuleVersionOverrides` parameter worden bijgewerkt met de meest recente moduleversies op de PowerShell Gallery. Als er niets wordt doorgegeven aan de `ModuleVersionOverrides` parameter, alle modules worden bijgewerkt met de meest recente moduleversies op de PowerShell Gallery, dit is het gedrag van de **Update Azure-Modules** knop.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over integratiemodules en over het maken van aangepaste modules voor het verder Automation integreren met andere systemen, services of oplossingen, [integratiemodules](automation-integration-modules.md).

* Overweeg het gebruik van bron besturingselement integratie [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) of [Azure DevOps](automation-scenario-source-control-integration-with-vsts.md) centraal beheren en beheren van versies van uw Automation-runbook en de configuratie-portfolio.  
