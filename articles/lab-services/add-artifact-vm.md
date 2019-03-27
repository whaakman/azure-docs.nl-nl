---
title: Een artefact toevoegen aan een virtuele machine in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het toevoegen van een artefact aan een virtuele machine in een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 5e6a7cbc070d81de33fac07a89dabf2b469bd355
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449993"
---
# <a name="add-an-artifact-to-a-vm"></a>Een artefact toevoegen aan een virtuele machine
Tijdens het maken van een virtuele machine, kunt u bestaande artefacten toevoegen aan deze. Deze artefacten kunnen afkomstig zijn uit een de [openbare DevTest Labs Git-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) of vanuit uw eigen Git-opslagplaats. Dit artikel leest u hoe artefacten in Azure portal en met behulp van Azure PowerShell toevoegen. 

Azure DevTest Labs *artefacten* kunt u opgeven *acties* die worden uitgevoerd wanneer de virtuele machine is ingericht, zoals het uitvoeren van Windows PowerShell-scripts, Bash-opdrachten uitvoeren en installeren van software. Artefact *parameters* kunt u aanpassen van het artefact voor uw specifieke scenario.

Zie voor meer informatie over het maken van aangepaste artefacten, het artikel: [Aangepaste artefacten maken](devtest-lab-artifact-author.md).

## <a name="use-azure-portal"></a>Azure Portal gebruiken 
1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer in de lijst met labs, in het lab met de virtuele machine die u wilt werken.  
1. Selecteer **mijn virtuele machines**.
1. Selecteer de gewenste virtuele machine.
1. Selecteer **artefacten beheren**. 
1. Selecteer **toepassen artefacten**.
1. Op de **toepassen artefacten** deelvenster, selecteert u het artefact dat u wilt toevoegen aan de virtuele machine.
1. Op de **toevoegen artefact** deelvenster, voert u de vereiste parameterwaarden en eventuele optionele parameters die u nodig hebt.  
1. Selecteer **toevoegen** het artefact toevoegen en terugkeren naar de **toepassen artefacten** deelvenster.
1. Doorgaan met het toevoegen van artefacten, zoals die nodig zijn voor uw virtuele machine.
1. Als u uw artefacten hebt toegevoegd, kunt u [wijzigt de volgorde waarin de artefacten zijn uitgevoerd](#change-the-order-in-which-artifacts-are-run). U kunt ook teruggaan naar [weergeven of wijzigen van een artefact](#view-or-modify-an-artifact).
1. Wanneer u klaar bent met het artefacten toe te voegen, selecteert u **toepassen**

### <a name="change-the-order-in-which-artifacts-are-run"></a>Wijzig de volgorde waarin artefacten worden uitgevoerd
De acties van de artefacten worden standaard uitgevoerd in de volgorde waarin ze worden toegevoegd aan de virtuele machine. De volgende stappen laten zien hoe u de volgorde waarin de artefacten worden uitgevoerd.

1. Aan de bovenkant van de **toepassen artefacten** deelvenster, selecteert u de koppeling met het nummer van de artefacten die zijn toegevoegd aan de virtuele machine.
   
    ![Aantal artefacten toegevoegd aan de virtuele machine](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Op de **geselecteerd artefacten** deelvenster slepen en neerzetten van de artefacten in de gewenste volgorde. Als u problemen ondervindt met het slepen van het artefact, zorg ervoor dat u vanaf de linkerkant van het artefact sleept. 
1. Selecteer **OK** wanneer u gereed bent.  

### <a name="view-or-modify-an-artifact"></a>Weergeven of wijzigen van een artefact
De volgende stappen laten zien hoe u weergeven of wijzigen van de parameters van een artefact:

1. Aan de bovenkant van de **toepassen artefacten** deelvenster, selecteert u de koppeling met het nummer van de artefacten die zijn toegevoegd aan de virtuele machine.
   
    ![Aantal artefacten toegevoegd aan de virtuele machine](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Op de **geselecteerd artefacten** deelvenster, selecteert u de artefacten die u wilt weergeven of bewerken.  
1. Op de **toevoegen artefact** deelvenster, controleert alle wijzigingen die nodig zijn, en selecteer **OK** sluiten de **toevoegen artefact** deelvenster.
1. Selecteer **OK** sluiten de **geselecteerd artefacten** deelvenster.

## <a name="use-powershell"></a>PowerShell gebruiken
Het opgegeven artefact geldt in het volgende script voor de opgegeven virtuele machine. De [Invoke-AzureRmResourceAction](/powershell/module/azurerm.resources/invoke-azurermresourceaction?view=azurermps-6.13.0) opdracht is degene die de bewerking wordt uitgevoerd.  

```powershell
#Requires -Version 3.0
#Requires -Module AzureRM.Resources

param
(
[Parameter(Mandatory=$true, HelpMessage="The ID of the subscription that contains the lab")]
   [string] $SubscriptionId,
[Parameter(Mandatory=$true, HelpMessage="The name of the lab containing the virtual machine")]
   [string] $DevTestLabName,
[Parameter(Mandatory=$true, HelpMessage="The name of the virtual machine")]
   [string] $VirtualMachineName,
[Parameter(Mandatory=$true, HelpMessage="The repository where the artifact is stored")]
   [string] $RepositoryName,
[Parameter(Mandatory=$true, HelpMessage="The artifact to apply to the virtual machine")]
   [string] $ArtifactName,
[Parameter(ValueFromRemainingArguments=$true)]
   $Params
)

# Set the appropriate subscription
Set-AzureRmContext -SubscriptionId $SubscriptionId | Out-Null
 
# Get the lab resource group name
$resourceGroupName = (Find-AzureRmResource -ResourceType 'Microsoft.DevTestLab/labs' | Where-Object { $_.Name -eq $DevTestLabName}).ResourceGroupName
if ($resourceGroupName -eq $null) { throw "Unable to find lab $DevTestLabName in subscription $SubscriptionId." }

# Get the internal repo name
$repository = Get-AzureRmResource -ResourceGroupName $resourceGroupName `
                    -ResourceType 'Microsoft.DevTestLab/labs/artifactsources' `
                    -ResourceName $DevTestLabName `
                    -ApiVersion 2016-05-15 `
                    | Where-Object { $RepositoryName -in ($_.Name, $_.Properties.displayName) } `
                    | Select-Object -First 1

if ($repository -eq $null) { "Unable to find repository $RepositoryName in lab $DevTestLabName." }

# Get the internal artifact name
$template = Get-AzureRmResource -ResourceGroupName $resourceGroupName `
                -ResourceType "Microsoft.DevTestLab/labs/artifactSources/artifacts" `
                -ResourceName "$DevTestLabName/$($repository.Name)" `
                -ApiVersion 2016-05-15 `
                | Where-Object { $ArtifactName -in ($_.Name, $_.Properties.title) } `
                | Select-Object -First 1

if ($template -eq $null) { throw "Unable to find template $ArtifactName in lab $DevTestLabName." }

# Find the virtual machine in Azure
$FullVMId = "/subscriptions/$SubscriptionId/resourceGroups/$resourceGroupName`
                /providers/Microsoft.DevTestLab/labs/$DevTestLabName/virtualmachines/$virtualMachineName"

$virtualMachine = Get-AzureRmResource -ResourceId $FullVMId

# Generate the artifact id
$FullArtifactId = "/subscriptions/$SubscriptionId/resourceGroups/$resourceGroupName`
                        /providers/Microsoft.DevTestLab/labs/$DevTestLabName/artifactSources/$($repository.Name)`
                        /artifacts/$($template.Name)"

# Handle the inputted parameters to pass through
$artifactParameters = @()

# Fill artifact parameter with the additional -param_ data and strip off the -param_
$Params | ForEach-Object {
   if ($_ -match '^-param_(.*)') {
      $name = $_.TrimStart('^-param_')
   } elseif ( $name ) {
      $artifactParameters += @{ "name" = "$name"; "value" = "$_" }
      $name = $null #reset name variable
   }
}

# Create structure for the artifact data to be passed to the action

$prop = @{
artifacts = @(
    @{
        artifactId = $FullArtifactId
        parameters = $artifactParameters
    }
    )
}

# Check the VM
if ($virtualMachine -ne $null) {
   # Apply the artifact by name to the virtual machine
   $status = Invoke-AzureRmResourceAction -Parameters $prop -ResourceId $virtualMachine.ResourceId -Action "applyArtifacts" -ApiVersion 2016-05-15 -Force
   if ($status.Status -eq 'Succeeded') {
      Write-Output "##[section] Successfully applied artifact: $ArtifactName to $VirtualMachineName"
   } else {
      Write-Error "##[error]Failed to apply artifact: $ArtifactName to $VirtualMachineName"
   }
} else {
   Write-Error "##[error]$VirtualMachine was not found in the DevTest Lab, unable to apply the artifact"
}

```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen op artefacten:

- [Verplichte artefacten voor uw lab opgeven](devtest-lab-mandatory-artifacts.md)
- [Aangepaste artefacten maken](devtest-lab-artifact-author.md)
- [Een artefactopslagplaats toevoegen aan een lab](devtest-lab-artifact-author.md)
- [Problemen met artefacten vaststellen](devtest-lab-troubleshoot-artifact-failure.md)