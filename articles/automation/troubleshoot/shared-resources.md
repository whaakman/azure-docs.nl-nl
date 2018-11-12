---
title: Problemen oplossen met Azure Automation gedeelde resources
description: Meer informatie over het oplossen van problemen met Azure Automation gedeelde resources
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 11/05/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 385d2969e65647ab0b5c5e21c07b127104587e7e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263559"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Problemen oplossen met gedeelde bronnen

Dit artikel worden oplossingen voor het oplossen van problemen die worden uitgevoerd op bij het gebruik van de gedeelde bronnen in Azure Automation.

## <a name="modules"></a>Modules

### <a name="module-stuck-importing"></a>Scenario: Een Module is vastgelopen importeren

#### <a name="issue"></a>Probleem

Wanneer u importeren of bijwerken van de modules in Azure automation, vindt u een module die is vastgelopen in de **importeren** staat.

#### <a name="error"></a>Fout

PowerShell-modules importeren, is een complex proces met meerdere stappen. Dit proces introduceert de mogelijkheid van een module importeren niet correct. Als dit gebeurt, kan de module die u wilt importeren in een tijdelijke situatie die zijn vastgelopen. Zie voor meer informatie over dit proces, [importeren van een PowerShell-Module]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Oplossing

U lost dit probleem, moet u de module die is vastgelopen in de **importeren** staat met behulp van de [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet. U kunt vervolgens opnieuw proberen door de module te importeren.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.