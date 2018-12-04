---
title: Problemen oplossen met Azure Automation gedeelde resources
description: Meer informatie over het oplossen van problemen met Azure Automation gedeelde resources
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: ce78c86cdae9a06100fd17d00e0229805e42983b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848456"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Problemen oplossen met gedeelde bronnen

Dit artikel worden oplossingen voor het oplossen van problemen die worden uitgevoerd op bij het gebruik van de gedeelde bronnen in Azure Automation.

## <a name="modules"></a>Modules

### <a name="module-stuck-importing"></a>Scenario: Een Module is vastgelopen importeren

#### <a name="issue"></a>Probleem

Een module is vastgelopen de **importeren** status wanneer u importeren of uw in Azure automation-modules bijwerken.

#### <a name="cause"></a>Oorzaak

PowerShell-modules importeren, is een complex proces met meerdere stappen. Dit proces introduceert de mogelijkheid van een module importeren niet correct. Als dit probleem optreedt, kan de module die u wilt importeren in een tijdelijke situatie die zijn vastgelopen. Zie voor meer informatie over dit proces, [importeren van een PowerShell-Module]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Oplossing

U lost dit probleem, moet u de module die is vastgelopen in de **importeren** staat met behulp van de [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet. U kunt vervolgens opnieuw proberen door de module te importeren.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="run-as-accounts"></a>Run As-accounts

### <a name="unable-create-update"></a>Scenario: U bent maken of bijwerken van een uitvoeren als-account

#### <a name="issue"></a>Probleem

Als u probeert te maken of bijwerken van een uitvoeren als-account, krijgt u een fout die vergelijkbaar is met de volgende strekking weergegeven:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Oorzaak

U hebt de machtigingen die u wilt maken of bijwerken van uitvoeren als-account of de resource is vergrendeld op het niveau van een resource.

#### <a name="resolution"></a>Oplossing

Als u wilt maken of bijwerken van een uitvoeren als-account, moet u toegangsmachtigingen voor de verschillende bronnen die worden gebruikt door het uitvoeren als-account hebben. Zie voor meer informatie over de machtigingen die nodig zijn voor het maken of bijwerken van een uitvoeren als-account, [uitvoeren als-accountmachtigingen](../manage-runas-account.md#permissions).

Als het probleem vanwege een vergrendeling is, Controleer of de vergrendeling te verwijderen en gaat u naar de resource is vergrendeld, met de rechtermuisknop op de vergrendeling en kiest u **verwijderen** de vergrendeling te verwijderen.

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.