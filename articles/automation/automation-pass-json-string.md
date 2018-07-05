---
title: Een JSON-object doorgeven aan een Azure Automation-runbook
description: Hoe u kunt parameters doorgeven aan een runbook als een JSON-object
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
keywords: PowerShell runbook, json, azure automation
ms.openlocfilehash: 9fa60a56ecbff802e69e01e038bb45c7a6639873
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435755"
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a>Een JSON-object doorgeven aan een Azure Automation-runbook

Kan het nuttig zijn voor het opslaan van gegevens die u wilt doorgeven aan een runbook in een JSON-bestand.
U kunt bijvoorbeeld een JSON-bestand met alle van de parameters die u wilt doorgeven aan een runbook maken.
Om dit te doen, moet u de JSON converteren naar een tekenreeks en vervolgens de tekenreeks converteren naar een PowerShell-object voordat de inhoud ervan wordt doorgegeven aan het runbook.

In dit voorbeeld maken we een PowerShell-script aanroept [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) naar een PowerShell-runbook, de inhoud van de JSON wordt doorgegeven aan het runbook te starten.
De PowerShell-runbook start een Azure-VM ophalen van de parameters voor de virtuele machine uit de JSON die is doorgegeven.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of <a href="/pricing/free-account/" target="_blank">[u aanmelden voor een gratis account](https://azure.microsoft.com/free/).
* [Automation-account](automation-sec-configure-azure-runas-account.md) om het runbook te bevatten en te verifiëren voor Azure-resources.  Dit account moet machtigingen hebben om de virtuele machine te starten en stoppen.
* Een virtuele machine van Azure. We stoppen en starten deze machine, dus het mag geen productiemachine zijn.
* Azure Powershell is geïnstalleerd op een lokale computer. Zie [installeren en configureren van Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) voor informatie over het ophalen van Azure PowerShell.

## <a name="create-the-json-file"></a>Het JSON-bestand maken

Typ de volgende test in een tekstbestand en sla het bestand als `test.json` ergens op de lokale computer.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a>Het runbook te maken

Maak een nieuwe PowerShell-runbook met de naam 'Test-Json' in Azure Automation.
Zie voor meer informatie over het maken van een nieuwe PowerShell-runbook, [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md).

Voor het accepteren van de JSON-gegevens, moet het runbook uitvoeren van een object als invoerparameter.

Het runbook kan vervolgens gebruiken voor de eigenschappen die zijn gedefinieerd in de JSON.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Opslaan en publiceren van dit runbook in uw Automation-account.

## <a name="call-the-runbook-from-powershell"></a>Het runbook aanroepen vanuit PowerShell

U kunt nu het runbook aanroepen vanuit uw lokale computer met behulp van Azure PowerShell.
Voer de volgende PowerShell-opdrachten:

1. Meld u aan bij Azure:
   ```powershell
   Connect-AzureRmAccount
   ```
    U wordt gevraagd uw Azure-referenties in te voeren.

   > [!IMPORTANT]
   > **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Wanneer uw bibliotheek zoeken items, als u niet ziet **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt uw modules bijwerken in uw Automation-Account.

1. De inhoud van het JSON-bestand ophalen en deze converteren naar een tekenreeks:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath` is het pad waar u het JSON-bestand hebt opgeslagen.
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

Het runbook maakt gebruik van de waarden van het JSON-bestand om te starten van een virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het bewerken van PowerShell en PowerShell Workflow-runbooks met een teksteditor, [tekstrunbooks in Azure Automation bewerken](automation-edit-textual-runbook.md) 
* Zie voor meer informatie over het maken en importeren van runbooks [maken of importeren van een runbook in Azure Automation](automation-creating-importing-runbook.md)


