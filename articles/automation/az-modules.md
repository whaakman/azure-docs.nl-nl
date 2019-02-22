---
title: Met behulp van de Az-modules in Azure Automation
description: Dit artikel bevat informatie met Az-modules in Azure Automation
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 02/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c3f46e40dfaf0d1ba2ab393b593cdd479c48c45d
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56585059"
---
# <a name="az-module-support-in-azure-automation"></a>AZ-module-ondersteuning in Azure Automation

Azure automation biedt ondersteuning voor de mogelijkheid om met de [Az van Azure Powershell-module](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) in runbooks uitvoert. De Az-module is niet automatisch geïmporteerd in nieuwe of bestaande Automation-Accounts. In dit artikel wordt beschreven hoe Az-modules gebruiken met Azure Automation.

## <a name="considerations"></a>Overwegingen

Er zijn veel dingen die u moet rekening mee te houden bij het gebruik van de Az-module in Azure Automation. Runbooks en modules kunnen worden gebruikt door een hoger niveau oplossingen in uw Automation-Account. Bewerken van runbooks of upgraden van modules kunt u mogelijk problemen veroorzaken met uw runbooks. U moet alle runbooks en testen oplossingen zorgvuldig in een afzonderlijke Automation-Account voordat u importeert de nieuwe `Az` modules. Wijzigingen in modules, kunnen een negatieve invloed hebben op de oplossingen voor een hoger niveau, zoals updatebeheer en starten/stoppen van VM's buiten kantooruren. Het is raadzaam niet wijzigen-modules en runbooks in Automation-Accounts die oplossingen bevatten. Dit gedrag is niet specifiek zijn voor de Az-modules. Dit gedrag moet in aanmerking worden genomen wanneer Maak kennis met eventuele wijzigingen aan uw Automation-Account.

Importeren van een `Az` module in uw Automation-Account niet automatisch importeren van de module in de PowerShell-sessie die gebruikmaken van de runbooks. Modules worden geïmporteerd in de PowerShell-sessie in de volgende situaties:

* Wanneer een cmdlet uit een module wordt aangeroepen vanuit een runbook
* Wanneer een runbook importeert deze expliciet door de `Import-Module` cmdlet
* Wanneer een andere module, afhankelijk van de module wordt geïmporteerd in een PowerShell-sessie

> [!IMPORTANT]
> Het is belangrijk om ervoor te zorgen dat runbooks in een Automation-Account dat u alleen importeren `Az` of `AzureRM` modules in de PowerShell-sessies die worden gebruikt door runbooks en niet voor beide. Als `Az` wordt geïmporteerd voordat `AzureRM` in een runbook, het runbook wordt voltooid, maar een [fout verwijst naar de methode get_SerializationSettings](troubleshoot/runbooks.md#get-serializationsettings) wordt weergegeven in de taakstromen en cmdlets mogelijk zijn niet correct uitgevoerd. Als u importeert `AzureRM` en vervolgens `Az` uw runbook nog steeds voltooid, wordt er een fout in de taakstromen met de mededeling dat beide `Az` en `AzureRM` kan niet worden geïmporteerd in dezelfde sessie of in hetzelfde runbook gebruikt.

## <a name="migrating-to-az-modules"></a>Migreren naar Az-modules

Het raadzaam dat u de migratie naar de Az-modules gebruiken in plaats van de AzureRM-modules in een test Automation-Account testen. Zodra dit Automation-Account is gemaakt, kunt u de volgende stappen uit om te controleren of dat uw migratie vlot:

### <a name="stop-and-unschedule-all-runbook-that-uses-azurerm-modules"></a>Stoppen en verwijderen uit planning alle runbook die gebruikmaakt van de AzureRM-modules

Om ervoor te zorgen dat u niet alle bestaande runbooks die gebruikmaken van uitvoert `AzureRM` cmdlets die u moet stoppen en verwijderen uit planning alle runbooks die gebruikmaken van `AzureRM` modules. U kunt zien welke schema's bestaan en welke schema's moeten worden verwijderd door het volgende voorbeeld uitvoert:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Het is belangrijk om te controleren van elke planning afzonderlijk om te controleren of dat u kunt opnieuw te plannen in de toekomst voor uw runbooks indien nodig.

### <a name="import-the-az-modules"></a>De Az-modules importeren

Importeer alleen de Az-modules die vereist voor uw runbooks zijn. Het pakket niet importeren `Az` -module, zoals deze bevat alle van de `Az.*` modules moeten worden geïmporteerd. Deze handleiding is hetzelfde voor alle modules.

De [Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) -module is een afhankelijkheid voor de andere `Az.*` modules. Deze module moet worden geïmporteerd in uw Automation-Account voordat u andere modules importeren om deze reden.

Selecteer in uw Automation-Account **Modules** onder **gedeelde bronnen**. Klik op **bladeren in galerie** openen de **bladeren in galerie** pagina.  Voer in de zoekbalk de naam van de module (zoals `Az.Accounts`). Klik op de pagina PowerShell-Module op **importeren** naar de module importeren in uw Automation-Account.

![Importeren van modules in Automation-Account](media/az-modules/import-module.png)

Het importproces kan ook worden gedaan via de [PowerShell Gallery](https://www.powershellgallery.com) door te zoeken naar de module. Als u de module hebt gevonden, selecteert u deze en klikt u onder de **Azure Automation** tabblad **implementeren in Azure Automation**.

![Importeren van modules rechtstreeks vanuit de galerie](media/az-modules/import-gallery.png)

## <a name="test-your-runbooks"></a>Test uw runbooks

Zodra de `Az` modules worden geïmporteerd in uw Automation-Account, kun u nu uw runbooks voor het gebruik van de Az-module in plaats daarvan bewerken. Het merendeel van de cmdlets hebben dezelfde naam, behalve voor `AzureRM` is gewijzigd in `Az`. Zie voor een lijst met modules die niet aan dit proces voldoen, [lijst met uitzonderingen](/powershell/azure/migrate-from-azurerm-to-az?view=azps-1.1.0#change-module-imports-and-cmdlet-names).

Eén manier om uw runbooks testen voordat uw runbook voor het gebruik van de nieuwe-cmdlets wijzigen met behulp van `Enable-AzureRMAlias -Scope Process` aan het begin van een runbook. Door toe te voegen aan uw runbook, uw runbook zonder wijzigingen worden uitgevoerd.

## <a name="after-migration-details"></a>Na de details van de migratie

Nadat de migratie voltooid is, niet starten met behulp van runbooks `AzureRM` modules op het account meer. Het verdient ook niet importeren of bijwerken `AzureRM` modules voor dit account. Vanaf dit moment kunt u dit account wordt gemigreerd naar `Az`, en werken met `Az` alleen modules. Wanneer een nieuw Automation-Account wordt gemaakt aan de bestaande `AzureRM` modules worden nog steeds geïnstalleerd en de zelfstudierunbooks wordt nog steeds worden gemaakt met `AzureRM` cmdlets. Deze runbooks moeten niet worden uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van de Az-modules, [aan de slag met Az module](/powershell/azure/get-started-azureps?view=azps-1.1.0).