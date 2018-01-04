---
title: Een Azure Resource Manager-sjabloon in een Azure Automation-runbook implementeren | Microsoft Docs
description: Het implementeren van een Azure Resource Manager-sjabloon die is opgeslagen in Azure Storage vanuit een runbook
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
ms.date: 07/09/2017
ms.author: gwallace
ms.openlocfilehash: 63c8f1b1190e19e1f1d2a7871bffee44ef5c7877
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a>Een Azure Resource Manager-sjabloon implementeren in een Azure Automation-PowerShell-runbook

U kunt schrijven een [PowerShell voor Azure Automation-runbook](automation-first-runbook-textual-powershell.md) die een Azure-resource implementeert met behulp van een [Azure Resource Management-sjabloon](../azure-resource-manager/resource-manager-create-first-template.md).

Op deze manier kunt u de implementatie van Azure-resources te automatiseren. U kunt uw Resource Manager-sjablonen in een beveiligde centrale locatie zoals Azure Storage onderhouden.

In dit onderwerp, maken we een PowerShell-runbook die gebruikmaakt van een Resource Manager-sjabloon die is opgeslagen in [Azure Storage](../storage/common/storage-introduction.md) voor het implementeren van een nieuw Azure-opslagaccount.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of <a href="/pricing/free-account/" target="_blank">[u aanmelden voor een gratis account](https://azure.microsoft.com/free/).
* [Automation-account](automation-sec-configure-azure-runas-account.md) om het runbook te bevatten en te verifiëren voor Azure-resources.  Dit account moet machtigingen hebben om de virtuele machine te starten en stoppen.
* [Azure Storage-account](../storage/common/storage-create-storage-account.md) waarin u voor het opslaan van de Resource Manager-sjabloon
* Azure Powershell installeren op een lokale machine. Zie [installeren en configureren van Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) voor informatie over het ophalen van Azure PowerShell.

## <a name="create-the-resource-manager-template"></a>Het Resource Manager-sjabloon maken

In dit voorbeeld gebruiken we een Resource Manager-sjabloon die u een nieuwe Azure Storage-account implementeert.

In een teksteditor, kopieert u de volgende tekst:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

Sla het bestand lokaal als `TemplateTest.json`.

## <a name="save-the-resource-manager-template-in-azure-storage"></a>De Resource Manager-sjabloon opslaan in Azure Storage

Nu we PowerShell gebruiken voor het maken van een Azure Storage-bestandsshare en upload de `TemplateTest.json` bestand.
Zie voor instructies over het maken van een bestand delen en een bestand in de Azure portal uploaden, [aan de slag met Azure File storage in Windows](../storage/files/storage-dotnet-how-to-use-files.md).

Start PowerShell op uw lokale computer en voer de volgende opdrachten een bestandsshare maken en uploaden van de Resource Manager-sjabloon naar of de bestandsshare.

```powershell
# Login to Azure
Login-AzureRmAccount

# Get the access key for your storage account
$key = Get-AzureRmStorageAccountKey -ResourceGroupName 'MyAzureAccount' -Name 'MyStorageAccount'

# Create an Azure Storage context using the first access key
$context = New-AzureStorageContext -StorageAccountName 'MyStorageAccount' -StorageAccountKey $key[0].value

# Create a file share named 'resource-templates' in your Azure Storage account
$fileShare = New-AzureStorageShare -Name 'resource-templates' -Context $context

# Add the TemplateTest.json file to the new file share
# "TemplatePath" is the path where you saved the TemplateTest.json file
$templateFile = 'C:\TemplatePath'
Set-AzureStorageFileContent -ShareName $fileShare.Name -Context $context -Source $templateFile
```

## <a name="create-the-powershell-runbook-script"></a>Het PowerShell-runbookscript maken

Nu we maken een PowerShell-script wordt de `TemplateTest.json` bestand van Azure Storage en implementeert u de sjabloon voor het maken van een nieuw Azure-opslagaccount.

In een teksteditor en plak de volgende tekst:

```powershell
param (
    [Parameter(Mandatory=$true)]
    [string]
    $ResourceGroupName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountKey,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageFileName
)



# Authenticate to Azure if running from Azure Automation
$ServicePrincipalConnection = Get-AutomationConnection -Name "AzureRunAsConnection"
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint | Write-Verbose

#Set the parameter values for the Resource Manager template
$Parameters = @{
    "storageAccountType"="Standard_LRS"
    }

# Create a new context
$Context = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

Get-AzureStorageFileContent -ShareName 'resource-templates' -Context $Context -path 'TemplateTest.json' -Destination 'C:\Temp'

$TemplateFile = Join-Path -Path 'C:\Temp' -ChildPath $StorageFileName

# Deploy the storage account
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterObject $Parameters 
``` 

Sla het bestand lokaal als `DeployTemplate.ps1`.

## <a name="import-and-publish-the-runbook-into-your-azure-automation-account"></a>Importeren en het runbook publiceren in Azure Automation-account

Nu we PowerShell gebruiken voor het importeren van het runbook in Azure Automation-account en vervolgens het runbook te publiceren.
Zie voor meer informatie over het importeren en publiceren van een runbook in de Azure portal [maken of importeren van een runbook in Azure Automation](automation-creating-importing-runbook.md).

Voor het importeren van `DeployTemplate.ps1` in uw Automation-account als een PowerShell-runbook, voer de volgende PowerShell-opdrachten:

```powershell
# MyPath is the path where you saved DeployTemplate.ps1
# MyResourceGroup is the name of the Azure ResourceGroup that contains your Azure Automation account
# MyAutomationAccount is the name of your Automation account
$importParams = @{
    Path = 'C:\MyPath\DeployTemplate.ps1'
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Type = 'PowerShell'
}
Import-AzureRmAutomationRunbook @importParams

# Publish the runbook
$publishParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
}
Publish-AzureRmAutomationRunbook @publishParams
```

## <a name="start-the-runbook"></a>Het runbook starten

Nu we het runbook door het aanroepen van starten de [Start AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) cmdlet.

Zie voor meer informatie over het starten van een runbook in de Azure portal [een runbook starten in Azure Automation](automation-starting-a-runbook.md).

Voer de volgende opdrachten in de PowerShell-console:

```powershell
# Set up the parameters for the runbook
$runbookParams = @{
    ResourceGroupName = 'MyResourceGroup'
    StorageAccountName = 'MyStorageAccount'
    StorageAccountKey = $key[0].Value # We got this key earlier
    StorageFileName = 'TemplateTest.json' 
}

# Set up parameters for the Start-AzureRmAutomationRunbook cmdlet
$startParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
    Parameters = $runbookParams
}

# Start the runbook
$job = Start-AzureRmAutomationRunbook @startParams
```

Het runbook wordt uitgevoerd, en u kunt de status ervan controleren door te voeren `$job.Status`.

Het runbook opgehaald van de Resource Manager-sjabloon en wordt gebruikt voor het implementeren van een nieuw Azure-opslagaccount.
U kunt zien dat het nieuwe opslagaccount is gemaakt door de volgende opdracht uit te voeren:
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a>Samenvatting

Dat is alles. Nu kunt u Azure Automation en Azure Storage en Resource Manager-sjablonen voor het implementeren van alle Azure-resources.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het Resource Manager-sjablonen, [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* Aan de slag met Azure Storage Zie [Inleiding tot Azure Storage](../storage/common/storage-introduction.md).
* Andere nuttige Azure Automation-runbooks vindt [galerieën Runbook en de module voor Azure Automation](automation-runbook-gallery.md).
* Andere nuttige Resource Manager-sjablonen vindt [Azure Quick Start-sjablonen](https://azure.microsoft.com/resources/templates/)

