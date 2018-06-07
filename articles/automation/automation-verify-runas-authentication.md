---
title: Configuratie van Azure Automation-account valideren
description: In dit artikel wordt beschreven hoe u kunt controleren of uw Automation-account juist is geconfigureerd.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c1ac698a1774a7a6242506ec65193434bb81cc25
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598766"
---
# <a name="test-azure-automation-run-as-account-authentication"></a>De verificatie van een Azure Automation Uitvoeren als-account testen
Nadat een Automation-account is gemaakt, kunt u een eenvoudige test uitvoeren om te controleren of u zich met het zojuist gemaakte of bijgewerkte Automation Uitvoeren als-account kunt verifiëren in Azure Resource Manager of de klassieke Azure-implementatie.    

## <a name="automation-run-as-authentication"></a>Automation Uitvoeren als-verificatie
Gebruik de voorbeeldcode hieronder om [een PowerShell-runbook te maken](automation-creating-importing-runbook.md) om verificatie met behulp van het Uitvoeren als-account te controleren. Gebruik deze voorbeeldcode ook in de aangepaste runbooks om Resource Manager-resources te beheren met behulp van het Automation-account.

    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

        "Logging in to Azure..."
        Connect-AzureRmAccount `
           -ServicePrincipal `
           -TenantId $servicePrincipalConnection.TenantId `
           -ApplicationId $servicePrincipalConnection.ApplicationId `
           -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
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

U ziet de cmdlet gebruikt voor de verificatie in het runbook - **Connect-AzureRmAccount**, gebruikt de *ServicePrincipalCertificate* parameterset.  In plaats van referenties wordt voor verificatie het certificaat van de service-principal gebruikt.  

> [!IMPORTANT]
> **Add-AzureRmAccount** is nu een alias voor **Connect-AzureRMAccount**. Als er geen **Connect-AzureRMAccount**, kunt u **Add-AzureRmAccount**, of u kunt uw modules op uw Automation-Account bijwerken.

Wanneer u [uitvoeren van het runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) uw Run As-account te valideren een [runbooktaak](automation-runbook-execution.md) is gemaakt, de taak pagina wordt weergegeven en de taakstatus wordt weergegeven in de **taakoverzicht** tegel. In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.  Wanneer de runbooktaak is voltooid, is de status **Voltooid**.

Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.  Op de pagina **Uitvoer** ziet u dat de verificatie is geslaagd. Er wordt ook een lijst met alle resources in alle resourcegroepen weergegeven.  

Vergeet niet om het codeblok dat begint met de opmerking `#Get all ARM resources from all resource groups`, te verwijderen wanneer u de code voor de runbooks hergebruikt.

## <a name="classic-run-as-authentication"></a>Klassieke Uitvoeren als-verificatie
Gebruik de voorbeeldcode hieronder om [een PowerShell-runbook te maken](automation-creating-importing-runbook.md) om verificatie met behulp van het klassieke Uitvoeren als-account te controleren. Gebruik deze voorbeeldcode ook in de aangepaste runbooks om resources in het klassieke implementatiemodel te verifiëren en te beheren.  

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

Wanneer u [het runbook uitvoert](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) om het Uitvoeren als-account te valideren, wordt een [runbooktaak](automation-runbook-execution.md) gemaakt, de pagina Taak weergegeven en de taakstatus weergegeven in de tegel **Taaksamenvatting**. In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.  Wanneer de runbooktaak is voltooid, is de status **Voltooid**.

Klik op de tegel **Uitvoer** als u de gedetailleerde resultaten van het runbook wilt bekijken.  Op de pagina **Uitvoer** ziet u dat de verificatie is geslaagd. Er wordt ook een lijst met alle virtuele Azure-machines die zijn geïmplementeerd in het abonnement, weergegeven op VM-naam.  

Vergeet niet om de cmdlet **Get-AzureVM** te verwijderen wanneer u de code voor de runbooks hergebruikt.

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md) om aan de slag te gaan met PowerShell-runbooks.
* Zie voor meer informatie over grafisch ontwerpen [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md).
