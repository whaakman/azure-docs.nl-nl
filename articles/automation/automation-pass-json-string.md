---
title: Een JSON-object doorgeven aan een Azure Automation-runbook | Microsoft Docs
description: Hoe parameters doorgeven aan een runbook als een JSON-object
services: automation
documentationcenter: dev-center-name
author: georgewallace
manager: carmonm
keywords: PowerShell, runbook, json, azure automation
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: gwallace
ms.openlocfilehash: 5390ba34a25713aed84d6e778335e30f27c2b1f8
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a>Een JSON-object doorgeven aan een Azure Automation-runbook

Dit kan nuttig zijn voor het opslaan van gegevens die u wilt doorgeven aan een runbook in een JSON-bestand zijn.
U kunt bijvoorbeeld een JSON-bestand met alle van de parameters die u wilt doorgeven aan een runbook maken.
U doet dit door die u moet de JSON converteren naar een tekenreeks en vervolgens de tekenreeks niet converteren naar een PowerShell-object voordat de inhoud wordt doorgegeven aan het runbook.

In dit voorbeeld maakt u een PowerShell-script aanroept [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) een PowerShell-runbook, de inhoud van de JSON wordt doorgegeven aan het runbook starten.
Het PowerShell-runbook start een Azure VM, het ophalen van de parameters voor de virtuele machine uit de JSON die is doorgegeven.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of <a href="/pricing/free-account/" target="_blank">[u aanmelden voor een gratis account](https://azure.microsoft.com/free/).
* [Automation-account](automation-sec-configure-azure-runas-account.md) om het runbook te bevatten en te verifiëren voor Azure-resources.  Dit account moet machtigingen hebben om de virtuele machine te starten en stoppen.
* Een virtuele machine van Azure. We stoppen en starten deze machine, dus het mag geen productiemachine zijn.
* Azure Powershell installeren op een lokale machine. Zie [installeren en configureren van Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) voor informatie over het ophalen van Azure PowerShell.

## <a name="create-the-json-file"></a>Het JSON-bestand maken

Typ de volgende test in een tekstbestand en sla het bestand als `test.json` ergens op uw lokale computer.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a>Het runbook te maken

Maak een nieuw PowerShell-runbook met de naam 'Test-Json' in Azure Automation.
Zie voor meer informatie over het maken van een nieuwe PowerShell-runbook, [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md).

Het runbook moet een object nemen als invoerparameter voor het accepteren van de JSON-gegevens.

De eigenschappen die zijn gedefinieerd in de JSON kan vervolgens worden gebruikt in het runbook.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Opslaan en dit runbook publiceren in uw Automation-account.

## <a name="call-the-runbook-from-powershell"></a>Het runbook aanroepen vanuit PowerShell

U kunt nu het runbook aanroepen vanuit uw lokale computer met behulp van Azure PowerShell.
Voer de volgende PowerShell-opdrachten:

1. Aanmelden bij Azure:
   ```powershell
   Login-AzureRmAccount
   ```
    U wordt gevraagd uw Azure-referenties invoeren.
1. Ophalen van de inhoud van het JSON-bestand en converteren naar een tekenreeks:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath`is het pad waar u het JSON-bestand opgeslagen.
1. Converteren van de inhoud van de tekenreeks van `$json` naar een PowerShell-object:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Maken van een hashtabel voor de parameters voor `Start-AzureRmAutomationRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   U ziet dat u de waarde van instelt `Parameters` naar het PowerShell-object dat de waarden van het JSON-bestand bevat. 
1. Het runbook starten
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

Het runbook maakt gebruik van de waarden van het JSON-bestand naar een virtuele machine start.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het bewerken van PowerShell en PowerShell Workflow-runbooks met een teksteditor, [tekstuele runbooks in Azure Automation bewerken](automation-edit-textual-runbook.md) 
* Zie voor meer informatie over het maken en importeren van runbooks [maken of importeren van een runbook in Azure Automation](automation-creating-importing-runbook.md)


