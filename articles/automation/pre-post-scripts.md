---
title: Configureren vóór en na-scripts in de Update Management-implementatie in Azure (Preview)
description: In dit artikel wordt beschreven hoe u configureren en beheren van vóór en na-scripts voor update-implementaties
services: automation
ms.service: automation
ms.subservice: update-management
author: georgewallace
ms.author: gwallace
ms.date: 02/12/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 90616544b1fddb8b6def04c30202035bec04d599
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236002"
---
# <a name="manage-pre-and-post-scripts-preview"></a>Beheren van scripts voor vóór en na (Preview)

Scripts voor vóór en na kunnen u PowerShell-runbooks uitvoeren in uw Automation-Account voordat (vooraf taak) en na de (na taak) een update-implementatie. Scripts voor vóór en na uitvoeren in de context van Azure en niet lokaal. Pre-scripts uitgevoerd aan het begin van de update-implementatie. Post-scripts uitgevoerd aan het einde van de implementatie en na een opnieuw opstarten die zijn geconfigureerd.

## <a name="runbook-requirements"></a>Runbook-vereisten

Voor een runbook moet worden gebruikt als een script voor vóór of na, moet u het runbook worden geïmporteerd in uw automation-account en gepubliceerd. Zie voor meer informatie over dit proces, [een runbook publiceren](automation-creating-importing-runbook.md#publishing-a-runbook).

## <a name="using-a-prepost-script"></a>Met behulp van een vooraf/post-script

Gebruik van een vooraf en boeken in een Update-implementatie een script, beginnen met het maken van een Update-implementatie. Selecteer **scripts die voorafgaan aan en bericht-Scripts (Preview)**. Deze actie opent u de **Selecteer scripts die voorafgaan aan en scripts die volgen op** pagina.  

![Selecteer de optie scripts](./media/pre-post-scripts/select-scripts.png)

Het script dat u gebruiken wilt, in dit voorbeeld te selecteren, wordt u gebruikt de **UpdateManagement TurnOnVms** runbook. Wanneer u het runbook selecteert de **Script configureren** pagina wordt geopend, geef waarden op voor de parameters en kies **Pre-script**. Klik op **OK** wanneer u klaar bent.

![Script configureren](./media/pre-post-scripts/configure-script.png)

Herhaal dit proces voor de **UpdateManagement TurnOffVms** script. Maar bij het kiezen van de **scripttype**, kiest u **post-script**.

De **geselecteerde items** sectie ziet u nu zowel uw scripts die zijn geselecteerd en is een Pre-script en de andere is een script dat volgt.

![Geselecteerde items](./media/pre-post-scripts/selected-items.png)

Configuratie van de Update-implementatie te voltooien.

Wanneer de Update-implementatie voltooid is, gaat u naar **Update-implementaties** om de resultaten weer te geven. Zoals u ziet, wordt de status van de Pre-script en post-script worden geleverd.

![Updateresultaten](./media/pre-post-scripts/update-results.png)

Door te klikken in de update-implementatie uitvoert, krijgt u meer informatie naar de scripts voor vóór en na de. Een koppeling naar de scriptbron op het moment van de uitvoering is opgegeven.

![Implementatie-uitvoering van resultaten](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>Doorgeven van parameters

Wanneer u vooraf configureren en boeken scripts, u kunt parameters doorgeven net als een runbook plannen. Parameters worden gedefinieerd op het moment van de update-implementatie maken. Scripts voor vóór en na vereisen parameters van het type `String`. Als u een ander objecttype nodig hebt, kunt u dit casten naar het andere type met `[System.Convert]` of verwerkt; het met uw eigen logica.

Naast de standaard runbookparameters, is een extra parameter opgegeven. Deze parameter is **SoftwareUpdateConfigurationRunContext**. Deze parameter is een JSON-tekenreeks en als u de parameter in het script voor vóór of na definieert, wordt automatisch doorgegeven door de update-implementatie. De parameter bevat informatie over de update-implementatie, die een subset van gegevens die zijn geretourneerd door de [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration) de volgende tabel ziet u de eigenschappen die zijn opgegeven in de variabele:

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext eigenschappen

|Eigenschap  |Description  |
|---------|---------|
|SoftwareUpdateConfigurationName     | De naam van de configuratie van de Software-updates        |
|SoftwareUpdateConfigurationRunId     | De unieke id voor de uitvoering.        |
|SoftwareUpdateConfigurationSettings     | Een verzameling eigenschappen met betrekking tot de Software-Updateconfiguratie         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | De besturingssystemen die bedoeld zijn voor de update-implementatie         |
|SoftwareUpdateConfigurationSettings.duration     | De maximale duur van de update-implementatie uitvoeren als `PT[n]H[n]M[n]S` aan de hand van ISO8601, moet u ook het venster' Onderhoud' genoemd          |
|SoftwareUpdateConfigurationSettings.Windows     | Een verzameling van eigenschappen die betrekking hebben op Windows-computers         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | Een lijst met kB's die zijn uitgesloten van de update-implementatie        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | Updateclassificaties geselecteerd voor de update-implementatie        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | Instellingen voor de implementatie van de update opnieuw opstarten        |
|azureVirtualMachines     | Een lijst met resourceIds voor de Azure VM's in de update-implementatie        |
|nonAzureComputerNames|Een lijst van de niet-Azure-computers FQDN-namen in de update-implementatie|

Het volgende voorbeeld wordt een JSON-tekenreeks doorgegeven aan de **SoftwareUpdateConfigurationRunContext** parameter:

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ], 
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

Een compleet voorbeeld met alle eigenschappen kan worden gevonden op: [Configuraties van Software-Update - ophalen met de naam](/rest/api/automation/softwareupdateconfigurations/getbyname#examples)

## <a name="samples"></a>Voorbeelden

Voorbeelden voor vóór en na-scripts vindt u de [Scriptcentrumgalerie](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell), of via de Azure-portal geïmporteerd. Deze te importeren via de portal, in uw Automation-Account onder **procesautomatisering**, selecteer **Runbookgalerie**. Gebruik **updatebeheer** voor het filter.

![Galerielijst](./media/pre-post-scripts/runbook-gallery.png)

Of u kunt zoeken naar deze de naam van de script zoals te zien is in de volgende lijst:

* Updatebeheer - VM's inschakelen
* Updatebeheer - VM's uitschakelen
* Updatebeheer - Script lokaal uitvoeren
* Update Management - sjabloon voor het vooraf/Post-Scripts
* Updatebeheer - Script uitvoeren met de opdracht uitvoeren

> [!IMPORTANT]
> Nadat u de runbooks importeert, moet u **publiceren** ze voordat ze kunnen worden gebruikt. Om te doen die het runbook niet vinden in uw Automation-Account selecteren **bewerken**, en klikt u op **publiceren**.

De voorbeelden zijn gebaseerd op de basic-sjabloon die is gedefinieerd in het volgende voorbeeld. Deze sjabloon kan worden gebruikt om uw eigen runbook te gebruiken met behulp van scripts voor vóór en na te maken. De benodigde logica voor de verificatie met Azure en verwerking van de `SoftwareUpdateConfigurationRunContext` parameter zijn opgenomen.

```powershell
<# 
.SYNOPSIS 
 Barebones script for Update Management Pre/Post 
 
.DESCRIPTION 
  This script is intended to be run as a part of Update Management Pre/Post scripts.  
  It requires a RunAs account. 
 
.PARAMETER SoftwareUpdateConfigurationRunContext 
  This is a system variable which is automatically passed in by Update Management during a deployment. 
#> 
 
param( 
    [string]$SoftwareUpdateConfigurationRunContext 
) 
#region BoilerplateAuthentication 
#This requires a RunAs account 
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection' 
 
Add-AzureRmAccount ` 
    -ServicePrincipal ` 
    -TenantId $ServicePrincipalConnection.TenantId ` 
    -ApplicationId $ServicePrincipalConnection.ApplicationId ` 
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint 
 
$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID 
#endregion BoilerplateAuthentication 
 
#If you wish to use the run context, it must be converted from JSON 
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext 
#Access the properties of the SoftwareUpdateConfigurationRunContext 
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines 
$runId = $context.SoftwareUpdateConfigurationRunId 
 
Write-Output $context 
 
#Example: How to create and write to a variable using the pre-script: 
<# 
#Create variable named after this run so it can be retrieved 
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false 
#Set value of variable  
Set-AutomationVariable –Name $runId -Value $vmIds 
#> 
 
#Example: How to retrieve information from a variable set during the pre-script 
<# 
$variable = Get-AutomationVariable -Name $runId 
#>      
```

## <a name="interacting-with-non-azure-machines"></a>Interactie met niet-Azure-machines

Vóór en na-taken uitvoeren in de context van Azure en geen toegang hebt tot niet-Azure-machines. Om te communiceren met de niet-Azure-machines, hebt u de volgende items:

* Een uitvoeren als-account
* Hybrid Runbook Worker is geïnstalleerd op de computer
* Een runbook dat u wilt lokaal uitvoeren
* Bovenliggend runbook

Om te communiceren met niet-Azure-machines, wordt een bovenliggend runbook uitvoeren in de context van Azure. Dit runbook roept een onderliggend runbook met de [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet. Moet u de `-RunOn` parameter en geef de naam van de Hybrid Runbook Worker voor het script uit te voeren op.

```powershell
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$resourceGroup = "AzureAutomationResourceGroup"
$aaName = "AzureAutomationAccountName"

$output = Start-AzureRmAutomationRunbook -Name "StartService" -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName -RunOn "hybridWorker"

$status = Get-AzureRmAutomationJob -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName
while ($status.status -ne "Completed")
{ 
    Start-Sleep -Seconds 5
    $status = Get-AzureRmAutomationJob -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName
}

$summary = Get-AzureRmAutomationJobOutput -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName

if ($summary.Type -eq "Error")
{
    Write-Error -Message $summary.Summary
}
```

## <a name="known-issues"></a>Bekende problemen

* U toevoegen geen objecten of -matrices aan de parameters bij het gebruik van scripts voor vóór en na. Het runbook zal mislukken.

## <a name="next-steps"></a>Volgende stappen

Doorgaan naar de zelfstudie voor informatie over het beheren van updates voor uw virtuele machines van Windows.

> [!div class="nextstepaction"]
> [Updates en patches voor uw Azure Windows VM's beheren](automation-tutorial-update-management.md)

