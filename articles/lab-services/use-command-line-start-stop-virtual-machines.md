---
title: Opdrachtregelprogramma's gebruiken om te starten en stoppen van virtuele machines Azure DevTest Labs | Microsoft Docs
description: Informatie over het gebruik van opdrachtregelprogramma's starten en stoppen van virtuele machines in Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 9b7df83b710bac0b37ac28c432f63a47ddda21d1
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439828"
---
# <a name="use-command-line-tools-to-start-and-stop-azure-devtest-labs-virtual-machines"></a>Opdrachtregelprogramma's gebruiken om te starten en stoppen van virtuele machines van Azure DevTest Labs
In dit artikel wordt beschreven hoe u Azure PowerShell of Azure CLI gebruiken om te starten of stoppen van virtuele machines in een lab in Azure DevTest Labs. U kunt PowerShell/CLI-scripts voor het automatiseren van deze bewerkingen kunt maken. 

## <a name="overview"></a>Overzicht
Azure DevTest Labs is een manier om snelle, eenvoudige en slanke ontwikkel-en testomgevingen maken. Hiermee kunt u kosten beheren, snel inrichten van virtuele machines, en minimaliseren afval.  Er zijn ingebouwde functies in Azure portal waarmee u kunt virtuele machines te configureren in een testomgeving automatisch starten en stoppen op specifieke tijdstippen. 

In sommige scenario's kunt u echter voor het automatiseren van starten en stoppen van VM's van PowerShell/CLI-scripts. Dit biedt u enige flexibiliteit bij het starten en stoppen van afzonderlijke computers op elk gewenst moment in plaats van op specifieke tijdstippen. Hier volgen enkele van de gevallen in welke uitgevoerd deze taken met behulp van scripts handig zou zijn.

- Wanneer u een 3-laagse toepassing gebruikt als onderdeel van een testomgeving, moeten de lagen in een volgorde worden gestart. 
- Een virtuele machine uitschakelen als een aangepaste criteria wordt voldaan om geld te besparen. 
- Gebruik dit als een taak in een CI/CD-werkstroom voor aan het begin van de stroom gestart, gebruikt u de virtuele machines als machines opzetten, testen machines, of een infrastructuur en vervolgens de virtuele machines stoppen wanneer het proces voltooid is. Een voorbeeld hiervan is de aangepaste installatiekopie factory met Azure DevTest Labs.  

## <a name="azure-powershell"></a>Azure PowerShell
De volgende PowerShell-script wordt een virtuele machine gestart in een testomgeving. [Aanroepen AzureRmResourceAction](/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-6.13.0) is de primaire focus voor dit script. De **ResourceId** parameter is de volledig gekwalificeerde resource-ID voor de virtuele machine in het lab. De **actie** parameter is de locatie waar de **Start** of **stoppen** opties zijn afhankelijk van wat u nodig hebt ingesteld.

```powershell
# The id of the subscription
$subscriptionId = "111111-11111-11111-1111111"

# The name of the lab
$devTestLabName = "yourlabname"

# The name of the virtual machine to be started
$vMToStart = "vmname"

# The action on the virtual machine (Start or Stop)
$vmAction = "Start"

# Select the Azure subscription
Select-AzureRMSubscription -SubscriptionId $subscriptionId

# Get the lab information
if ($(Get-Module -Name AzureRM).Version.Major -eq 6) {
    $devTestLab = Get-AzureRmResource -ResourceType 'Microsoft.DevTestLab/labs' -Name $devTestLabName
} else {
    $devTestLab = Find-AzureRmResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceNameEquals $devTestLabName
}

# Start the VM and return a succeeded or failed status
$returnStatus = Invoke-AzureRmResourceAction `
                    -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vMToStart" `
                    -Action $vmAction `
                    -Force

if ($returnStatus.Status -eq 'Succeeded') {
    Write-Output "##[section] Successfully updated DTL machine: $vMToStart, Action: $vmAction"
}
else {
    Write-Error "##[error]Failed to update DTL machine: $vMToStart, Action: $vmAction"
}
```


## <a name="azure-cli"></a>Azure-CLI
De [Azure CLI](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) is een andere manier voor het automatiseren van het starten en stoppen van DevTest Labs-VM's. Azure CLI kan worden [geïnstalleerd](/cli/azure/install-azure-cli?view=azure-cli-latest) op verschillende besturingssystemen. Het volgende script geeft u opdrachten voor het starten en stoppen van een virtuele machine in een testomgeving. 

```azurecli
# Sign in to Azure
az login 

## Get the name of the resource group that contains the lab
az resource list --resource-type "Microsoft.DevTestLab/labs" --name "yourlabname" --query "[0].resourceGroup" 

## Start the VM
az lab vm start --lab-name yourlabname --name vmname --resource-group labResourceGroupName

## Stop the VM
az lab vm stop --lab-name yourlabname --name vmname --resource-group labResourceGroupName
```


## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel voor het gebruik van de Azure-portal voor deze bewerkingen: [Een VM opnieuw opstarten](devtest-lab-restart-vm.md).