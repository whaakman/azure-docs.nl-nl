---
title: Problemen oplossen met Azure Automation gedeelde resources
description: Meer informatie over het oplossen van problemen met Azure Automation gedeelde resources
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/12/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 35e39a070a4c976655296d2ea141478d13e43bbc
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57902821"
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

### <a name="update-azure-modules-importing"></a>Scenario: AzureRM-modules zijn vastgelopen na het bijwerken van deze importeren

#### <a name="issue"></a>Probleem

Een banner met het volgende bericht blijft in uw account na het bijwerken van de AzureRM-modules:

```
Azure modules are being updated
```

#### <a name="cause"></a>Oorzaak

Er is een bekend probleem met het bijwerken van de AzureRM-modules in een Automation-Account dat in een resourcegroep gemaakt met een numerieke naam die met 0 begint.

#### <a name="resolution"></a>Oplossing

Voor het bijwerken van uw Azure-modules in uw Automation-Account, moet deze zich in een resourcegroep met een alfanumerieke naam. Resourcegroepen met numerieke namen die beginnen met 0 zijn kan niet worden bijgewerkt AzureRM-modules op dit moment.

### <a name="module-fails-to-import"></a>Scenario: Module niet kan worden geïmporteerd of cmdlets kan niet worden uitgevoerd na het importeren

#### <a name="issue"></a>Probleem

Een module te importeren is mislukt of is geïmporteerd, maar geen cmdlets worden geëxtraheerd.

#### <a name="cause"></a>Oorzaak

Er zijn enkele veelvoorkomende redenen waarom dat een module niet met succes in Azure Automation importeren kan:

* De structuur komt niet overeen met de structuur die het Automation nodig heeft om te worden.
* De module is afhankelijk van een andere module die nog niet geïmplementeerd is naar uw Automation-account.
* De module ontbreekt de bijbehorende afhankelijkheden in de map.
* De `New-AzureRmAutomationModule` cmdlet voor het uploaden van de module wordt gebruikt en u niet de volledige opslagpad toegestaan of de module nog niet hebt geladen met behulp van een openbaar toegankelijke URL.

#### <a name="resolution"></a>Oplossing

Een van de volgende oplossingen het probleem wordt opgelost:

* Zorg ervoor dat de module de volgende indeling heeft: ModuleName.Zip **->** ModuleName of het versienummer **->** (ModuleName.psm1, ModuleName.psd1)
* Open het .psd1-bestand en zien of de module afhankelijkheden heeft. Als dit het geval is, moet u deze modules uploaden naar het Automation-account.
* Zorg ervoor dat eventuele waarnaar wordt verwezen, dll's zijn aanwezig in de modulemap.

### <a name="all-modules-suspended"></a>Scenario: Update-AzureModule.ps1 wordt onderbroken tijdens het bijwerken van modules

#### <a name="issue"></a>Probleem

Wanneer u de [Update AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook bijwerken van uw Azure-modules met de module-update het updateproces wordt onderbroken.

#### <a name="cause"></a>Oorzaak

De standaardinstelling om te bepalen hoeveel modules bijgewerkt tegelijk is 10 bij het gebruik van de `Update-AzureModule.ps1` script. Het updateproces is gevoelig voor fouten als er te veel modules worden bijgewerkt op hetzelfde moment.

#### <a name="resolution"></a>Oplossing

Het is niet gebruikelijk dat alle AzureRM-modules zijn vereist in het hetzelfde Automation-account. Het is raadzaam om te importeren alleen de AzureRM-modules die u nodig hebt.

> [!NOTE]
> Voorkomen dat u importeert de **AzureRM** module. Importeren van de **AzureRM** modules leidt ertoe dat alle **AzureRM.\***  modules moeten worden geïmporteerd, is dit niet aanbevolen.

Als het updateproces wordt onderbroken, moet u om toe te voegen de `SimultaneousModuleImportJobCount` parameter voor de `Update-AzureModules.ps1` script en een lagere waarde dan de standaardwaarde van 10 opgeven. Het wordt aanbevolen als u deze logica, om te beginnen met een waarde van 3 of 5 implementeert. `SimultaneousModuleImportJobCount` is een parameter van de `Update-AutomationAzureModulesForAccount` system-runbook dat wordt gebruikt voor het Azure-modules bijwerken. Deze wijziging maakt het proces meer uitvoeren, maar heeft een grotere kans om te voltooien. Het volgende voorbeeld ziet u de parameter en waar u deze in het runbook te plaatsen:

 ```powershell
         $Body = @"
            {
               "properties":{
               "runbook":{
                   "name":"Update-AutomationAzureModulesForAccount"
               },
               "parameters":{
                    ...
                    "SimultaneousModuleImportJobCount":"3",
                    ... 
               }
              }
           }
"@
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

Als het probleem vanwege een vergrendeling is, controleert u of de vergrendeling ok om deze te verwijderen. Vervolgens gaat u naar de resource is vergrendeld, met de rechtermuisknop op de vergrendeling en kies **verwijderen** de vergrendeling te verwijderen.

### <a name="iphelper"></a>Scenario: U ontvangt de fout 'Kan een ingangspunt met de naam 'GetPerAdapterInfo' in DLL-bestand 'iplpapi.dll' vinden' wanneer een runbook uitvoeren.

#### <a name="issue"></a>Probleem

Bij het uitvoeren van een runbook ontvangt u de volgende uitzondering:

```error
Unable to find an entry point named 'GetPerAdapterInfo' in DLL 'iplpapi.dll'
```

#### <a name="cause"></a>Oorzaak

Deze fout wordt waarschijnlijk veroorzaakt door een onjuist geconfigureerde [Run As-Account](../manage-runas-account.md).

#### <a name="resolution"></a>Oplossing

Zorg ervoor dat uw [Run As-Account](../manage-runas-account.md) correct is geconfigureerd. Als deze juist is geconfigureerd, zorg ervoor dat u hebt de juiste code in uw runbook om te verifiëren met Azure. Het volgende voorbeeld ziet een codefragment om te verifiëren bij Azure in een runbook met behulp van een Run As-Account.

```powershell
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint
```

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
