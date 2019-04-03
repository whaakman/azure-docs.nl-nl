---
title: Met Automation-runbooks in Azure DevTest Labs-machines starten | Microsoft Docs
description: Leer hoe u virtuele machines in een lab in Azure DevTest Labs starten met behulp van Azure Automation-runbooks.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: spelluru
ms.openlocfilehash: d80328943ae818b3bad9c0a275b74968ee33d4b7
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887211"
---
# <a name="start-virtual-machines-in-a-lab-in-order-by-using-azure-automation-runbooks"></a>Virtuele machines in een lab in volgorde met behulp van Azure Automation-runbooks starten
De [autostart](devtest-lab-set-lab-policy.md#set-autostart) functie van DevTest Labs kunt u virtuele machines automatisch gestart op een bepaald tijdstip configureren. Deze functie biedt echter geen ondersteuning voor machines om te starten in een specifieke volgorde. Er zijn verschillende scenario's waarbij dit type automatisering handig kan zijn.  Een scenario is wanneer een Jumpbox-VM in een lab moet eerst worden gestart voordat de andere VM's, zoals de Jumpbox wordt gebruikt als het toegangspunt aan de andere virtuele machines.  Dit artikel leest u hoe het instellen van een Azure Automation-account met een PowerShell-runbook dat een script wordt uitgevoerd. Het script maakt gebruik van tags op virtuele machines in de testomgeving kunt u voor het beheren van de opstartvolgorde te wijzigen zonder dat het script wijzigen.

## <a name="setup"></a>Instellen
In dit voorbeeld VM's in het lab wilt hebben van de tag **StartupOrder** toegevoegd met de juiste waarde (0,1,2, enz.). Wijs elke machine die niet hoeven te worden gestart als de -1.

## <a name="create-an-azure-automation-account"></a>Een Azure Automation-account maken
Een Azure Automation-account maken met de instructies in [in dit artikel](../automation/automation-create-standalone-account.md). Kies de **Run As-Accounts** optie bij het maken van het account. Nadat het automation-account is gemaakt, opent u de **Modules** pagina en selecteer **Update Azure-Modules** op de menubalk. De standaardmodules zijn dat verschillende versies oude en zonder de update van het script kunnen niet worden gebruikt.

## <a name="add-a-runbook"></a>Een runbook toevoegen
Selecteer nu een runbook toevoegen aan het automation-account, **Runbooks** in het menu links. Selecteer **toevoegen van een runbook** in het menu en de instructies volgen voor het [maken van een PowerShell-runbook](../automation/automation-first-runbook-textual-powershell.md).

## <a name="powershell-script"></a>PowerShell-script
Het volgende script neemt de naam van het abonnement, de labnaam als parameters. De stroom van het script is ontvangen van alle virtuele machines in de testomgeving en parseren en vervolgens de tag-informatie voor het maken van een lijst met de namen van de virtuele machine en de opstartvolgorde te wijzigen. Het script begeleid bij de virtuele machines in volgorde en start de virtuele machines. Als er meerdere virtuele machines in een aantal specifieke volgorde, kunt ze asynchroon met behulp van PowerShell-taken worden gestart. Voor deze virtuele machines waarvoor geen een label, opstartwaarde instellen op de laatste (10), worden ze gestart laatste, standaard.  Als de testomgeving niet automatisch worden gestart als de virtuele machine wilt, stel de tagwaarde op 11 en wordt dit genegeerd.

```powershell
#Requires -Version 3.0
#Requires -Module AzureRM.Resources

param
(
    [Parameter(Mandatory=$false, HelpMessage="Name of the subscription that has the lab")]
    [string] $SubscriptionName,

    [Parameter(Mandatory=$false, HelpMessage="Lab name")]
    [string] $LabName
)

# Connect and add the appropriate subscription
$Conn = Get-AutomationConnection -Name AzureRunAsConnection

Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationId -Subscription $SubscriptionName -CertificateThumbprint $Conn.CertificateThumbprint

# Find the lab
$dtLab = Find-AzureRmResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceNameEquals $LabName

# Get the VMs
$dtlAllVms = New-Object System.Collections.ArrayList
$AllVMs = Get-AzureRmResource -ResourceId "$($dtLab.ResourceId)/virtualmachines" -ApiVersion 2016-05-15

# Get the StartupOrder tag, if missing set to be run last (10)
ForEach ($vm in $AllVMs) {
    if ($vm.Tags) {
        if ($vm.Tags['StartupOrder']) {
            $startupValue = $vm.Tags['StartupOrder']
        } else {
            $startupValue = 10
        }
        } else {
            $startupValue = 10
        }
        $dtlAllVms.Add(@{$vm.Name = $startupValue}) > $null
}

# Setup for the async multiple vm start

# Save profile
$profilePath = Join-Path $env:Temp "profile.json"
If (Test-Path $profilePath){
    Remove-Item $profilePath
}
Save-AzureRmContext -Path $profilePath

# Job to start VMs asynch
$startVMBlock = {
    Param($devTestLab,$vmToStart,$profilePath)
    Import-AzureRmContext -Path ($profilePath)
    Invoke-AzureRmResourceAction `
        -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vmToStart" `
        -Action Start `
        -Force
    Write-Output "Started: $vmToStart"
}

$current = 0
# Start in order from 0 to 10

While ($current -le 10) {
# Get the VMs in the current stage
    $tobeStarted = $null
    $tobeStarted = $dtlAllVms | Where-Object { $_.Values -eq $current}
    if ($tobeStarted.Count -eq 1) {
        # Run sync – jobs not necessary for a single VM
        $returnStatus = Invoke-AzureRmResourceAction `
                -ResourceId "$($dtLab.ResourceId)/virtualmachines/$($tobeStarted.Keys)" `
                -Action Start `
                -Force
        Write-Output "$($tobeStarted.Keys) status: $($returnStatus.status)"
    } elseif ($tobeStarted.Count -gt 1) {
        # Start multiple VMs async
        $jobs = @()
        Write-Output "Start Jobs start: $(Get-Date)"
        
        # Jobs
        $jobs += Start-Job -ScriptBlock $startVMBlock -ArgumentList $dtLab, $($singlevm.Keys), $profilePath
        Write-Output "Start Jobs end: $(Get-Date)"
    }

    # Get results from all jobs
    if($jobs.Count -ne 0) {
        Write-Output "Receive Jobs start: $(Get-Date)"
        foreach ($job in $jobs){
            $jobResult = Receive-Job -Job $job -Wait | Write-Output
        }
        Remove-Job -Job $jobs -Force
    }
    else
    {
        Write-Output "Information: No jobs available"
    }
}
```

## <a name="create-a-schedule"></a>Een planning maken
Om dit script uitvoeren dagelijks, [maakt u een planning](../automation/shared-resources/schedules.md#creating-a-schedule) in het automation-account. Zodra de planning is gemaakt, [koppelen aan het runbook](../automation/shared-resources/schedules.md#linking-a-schedule-to-a-runbook). 

In een grootschalige situatie waarbij er meerdere abonnementen met meerdere labs, de parametergegevens opslaan in een bestand voor verschillende labs en het bestand aan het script in plaats van de afzonderlijke parameters doorgeven. Het script moet worden gewijzigd, maar de core worden uitgevoerd op dezelfde wijze worden. In dit voorbeeld maakt gebruik van de Azure Automation voor het uitvoeren van het PowerShell-script, maar er zijn andere opties, zoals het gebruik van een taak in een Build en Release-pijplijn.

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel voor meer informatie over Azure Automation: [Een inleiding tot Azure Automation](../automation/automation-intro.md).
