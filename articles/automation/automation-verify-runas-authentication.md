---
title: Azure Automation-accountconfiguratie valideren
description: In dit artikel wordt beschreven hoe u kunt controleren of uw Automation-account juist is geconfigureerd.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 32b73cf4570393ed24ee6d1121aef75aaf193134
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39713751"
---
# <a name="test-azure-automation-run-as-account-authentication"></a>De verificatie van een Azure Automation Uitvoeren als-account testen

Nadat een Automation-account is gemaakt, kunt u een eenvoudige test uitvoeren om te controleren of u zich met het zojuist gemaakte of bijgewerkte Automation Uitvoeren als-account kunt verifiëren in Azure Resource Manager of de klassieke Azure-implementatie.

## <a name="automation-run-as-authentication"></a>Automation Uitvoeren als-verificatie

Gebruik de voorbeeldcode hieronder om [een PowerShell-runbook te maken](automation-creating-importing-runbook.md) om verificatie met behulp van het Uitvoeren als-account te controleren. Gebruik deze voorbeeldcode ook in de aangepaste runbooks om Resource Manager-resources te beheren met behulp van het Automation-account.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

    $logonAttempt = 0
    $logonResult = $False

    while(!($connectionResult) -And ($logonAttempt -le 10))
    {
        $LogonAttempt++
    # Logging in to Azure...
    $connectionResult = Connect-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

     Start-Sleep -Seconds 30
    }
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Get all ARM resources from all resource groups
$ResourceGroups = Get-AzureRmResourceGroup

foreach ($ResourceGroup in $ResourceGroups)
{
    Write-Output ("Showing resources in resource group " + $ResourceGroup.ResourceGroupName)
    $Resources = Find-AzureRmResource -ResourceGroupNameContains $ResourceGroup.ResourceGroupName | Select ResourceName, ResourceType
    ForEach ($Resource in $Resources)
    {
        Write-Output ($Resource.ResourceName + " of type " +  $Resource.ResourceType)
    }
    Write-Output ("")
}
```

U ziet de cmdlet die wordt gebruikt voor het verifiëren van in het runbook - **Connect-AzureRmAccount**, maakt gebruik van de *ServicePrincipalCertificate* parameterset.  In plaats van referenties wordt voor verificatie het certificaat van de service-principal gebruikt.  

> [!IMPORTANT]
> **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Wanneer uw bibliotheek zoeken items, als u niet ziet **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt uw modules bijwerken in uw Automation-Account.

Wanneer u [het runbook uitvoeren](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) voor het valideren van uw uitvoeren als-account, een [runbooktaak](automation-runbook-execution.md) is gemaakt, de taak pagina wordt weergegeven en de taakstatus weergegeven in de **taaksamenvatting** tegel. In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.  Wanneer de runbooktaak is voltooid, is de status **Voltooid**.

Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.  Op de pagina **Uitvoer** ziet u dat de verificatie is geslaagd. Er wordt ook een lijst met alle resources in alle resourcegroepen weergegeven.  

Vergeet niet om het codeblok dat begint met de opmerking `#Get all ARM resources from all resource groups`, te verwijderen wanneer u de code voor de runbooks hergebruikt.

## <a name="classic-run-as-authentication"></a>Klassieke Uitvoeren als-verificatie

Gebruik de voorbeeldcode hieronder om [een PowerShell-runbook te maken](automation-creating-importing-runbook.md) om verificatie met behulp van het klassieke Uitvoeren als-account te controleren. Gebruik deze voorbeeldcode ook in de aangepaste runbooks om resources in het klassieke implementatiemodel te verifiëren en te beheren.  

```powershell
$ConnectionAssetName = "AzureClassicRunAsConnection"
# Get the connection
$connection = Get-AutomationConnection -Name $connectionAssetName

# Authenticate to Azure with certificate
Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
$Conn = Get-AutomationConnection -Name $ConnectionAssetName
if ($Conn -eq $null)
{
    throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
}

$CertificateAssetName = $Conn.CertificateAssetName
Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
$AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
if ($AzureCert -eq $null)
{
    throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
}

Write-Verbose "Authenticating to Azure with certificate." -Verbose
Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

#Get all VMs in the subscription and return list with name of each
Get-AzureVM | ft Name
```

Wanneer u [het runbook uitvoert](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) om het Uitvoeren als-account te valideren, wordt een [runbooktaak](automation-runbook-execution.md) gemaakt, de pagina Taak weergegeven en de taakstatus weergegeven in de tegel **Taaksamenvatting**. In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.  Wanneer de runbooktaak is voltooid, is de status **Voltooid**.

Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.  Op de pagina **Uitvoer** ziet u dat de verificatie is geslaagd. Er wordt ook een lijst met alle virtuele Azure-machines die zijn geïmplementeerd in het abonnement, weergegeven op VM-naam.  

Vergeet niet om de cmdlet **Get-AzureVM** te verwijderen wanneer u de code voor de runbooks hergebruikt.

## <a name="next-steps"></a>Volgende stappen

* Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md) om aan de slag te gaan met PowerShell-runbooks.
* Zie voor meer informatie over grafisch ontwerpen [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).
