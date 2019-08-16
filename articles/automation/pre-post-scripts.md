---
title: Pre-en post-scripts configureren in uw Updatebeheer-implementatie in azure
description: In dit artikel wordt beschreven hoe u vooraf en post scripts voor update-implementaties configureert en beheert
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f13851dd43c80a63ec628e04b98271894c15afc0
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69542855"
---
# <a name="manage-pre-and-post-scripts"></a>Pre-en post-scripts beheren

Met scripts vooraf en post kunt u Power shell-runbooks uitvoeren in uw Automation-account vóór (vóór taak) en na (na taak) een update-implementatie. Scripts die worden uitgevoerd in de Azure-context en niet op de andere plaats, worden niet lokaal geïnstalleerd. Pre-scripts worden uitgevoerd aan het begin van de update-implementatie. Post scripts worden aan het einde van de implementatie uitgevoerd en nadat opnieuw opstarten is geconfigureerd.

## <a name="runbook-requirements"></a>Runbook-vereisten

Het runbook moet worden geïmporteerd in uw Automation-account en gepubliceerd om een runbook te kunnen gebruiken als pre-of post script. Zie [een Runbook publiceren](manage-runbooks.md#publish-a-runbook)voor meer informatie over dit proces.

## <a name="using-a-prepost-script"></a>Een pre/post-script gebruiken

Als u een pre-en of post-script wilt gebruiken in een update-implementatie, moet u beginnen met het maken van een update-implementatie. Selecteer **pre-scripts + post scripts**. Met deze actie opent **u de pagina pre-scripts + post-scripts selecteren** .  

![Scripts selecteren](./media/pre-post-scripts/select-scripts.png)

Selecteer het script dat u wilt gebruiken. in dit voor beeld hebt u het **UpdateManagement-TurnOnVms-** runbook gebruikt. Wanneer u het runbook selecteert de pagina **script configureren** wordt geopend, kiest u **vooraf script**. Klik op **OK** wanneer u klaar bent.

Herhaal dit proces voor het script **UpdateManagement-TurnOffVms** . Klik, als u het **script type**kiest, vervolgens **post-script**.

In het gedeelte **geselecteerde items** ziet u nu dat uw geselecteerde scripts en op een pre-script zijn en de andere een post script.

![Geselecteerde items](./media/pre-post-scripts/selected-items.png)

De configuratie van de update-implementatie volt ooien.

Wanneer de update-implementatie is voltooid, kunt u naar **Update** -implementaties gaan om de resultaten weer te geven. Zoals u ziet, wordt de status van het pre-script en het post script weer gegeven.

![Updateresultaten](./media/pre-post-scripts/update-results.png)

Als u op de update-implementatie wilt klikken, krijgt u aanvullende informatie over de pre-en post-scripts. Er wordt een koppeling naar de script bron op het moment van de uitvoering gegeven.

![Resultaten van implementatie-uitvoering](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>Para meters door geven

Wanneer u vooraf en post-scripts configureert, kunt u de para meters door geven, net zoals bij het plannen van een runbook. Para meters worden gedefinieerd op het moment dat de update-implementatie wordt gemaakt. Voor-en post-scripts ondersteunen de volgende typen:

* char
* DBCS
* integer
* omvang
* komma
* afzonderlijke
* Dubbelklik
* DateTime
* tekenreeksexpressie

Als u een ander object type nodig hebt, kunt u dit naar een ander type converteren met uw eigen logica in het runbook.

Naast uw standaard runbook-para meters wordt een extra para meter opgegeven. Deze para meter is **SoftwareUpdateConfigurationRunContext**. Deze para meter is een JSON-teken reeks en als u de para meter definieert in uw pre-of post-script, wordt deze automatisch door gegeven door de update-implementatie. De para meter bevat informatie over de update-implementatie. Dit is een subset van informatie die wordt geretourneerd door de [SoftwareUpdateconfigurations-API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration) . in de volgende tabel ziet u de eigenschappen die in de variabele zijn opgegeven:


### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext-eigenschappen

|Eigenschap  |Description  |
|---------|---------|
|SoftwareUpdateConfigurationName     | De naam van de configuratie van de software-update        |
|SoftwareUpdateConfigurationRunId     | De unieke id voor de uitvoering.        |
|SoftwareUpdateConfigurationSettings     | Een verzameling eigenschappen die betrekking hebben op de software-update configuratie         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | De besturings systemen die gericht zijn op de update-implementatie         |
|SoftwareUpdateConfigurationSettings.duration     | De maximale duur van de update-implementatie zoals `PT[n]H[n]M[n]S` iso8601, ook wel het onderhouds venster genoemd          |
|SoftwareUpdateConfigurationSettings.Windows     | Een verzameling eigenschappen die betrekking hebben op Windows-computers         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | Een lijst met Kb's die zijn uitgesloten van de update-implementatie        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | Update classificaties geselecteerd voor de update-implementatie        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | Instellingen voor de update-implementatie opnieuw opstarten        |
|azureVirtualMachines     | Een lijst met resourceIds voor de virtuele Azure-machines in de update-implementatie        |
|nonAzureComputerNames|Een lijst met de FQDN-namen van niet-Azure-computers in de update-implementatie|

Het volgende voor beeld is een JSON-teken reeks die is door gegeven aan de para meter **SoftwareUpdateConfigurationRunContext** :

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

Een volledig voor beeld van alle eigenschappen vindt u op: [Software-update configuraties-ophalen op naam](/rest/api/automation/softwareupdateconfigurations/getbyname#examples)

> [!NOTE]
> Het `SoftwareUpdateConfigurationRunContext` object kan dubbele vermeldingen voor machines bevatten. Dit kan ertoe leiden dat scripts vooraf en post meerdere keren op dezelfde computer worden uitgevoerd. U kunt dit probleem omzeilen `Sort-Object -Unique` door alleen unieke VM-namen in uw script te selecteren.


## <a name="stopping-a-deployment"></a>Een implementatie stoppen

Als u een implementatie op basis van een vooraf script wilt stoppen, moet u een uitzonde ring [genereren](automation-runbook-execution.md#throw) . Als u geen uitzonde ring genereert, worden de implementatie en het post script nog steeds uitgevoerd. In het [voor beeld-runbook](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44?redir=0) in de galerie ziet u hoe u dit kunt doen. Hier volgt een fragment van dat runbook.

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.  
        throw $summary.Summary
    }
}
```


## <a name="samples"></a>Voorbeelden

Voor beelden voor voorafgaande en post scripts vindt u in de [Script Center-galerie](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell)of worden geïmporteerd via de Azure Portal. Als u deze wilt importeren via de portal, selecteert u in uw Automation-account, onder **proces automatisering**, **Runbooks galerie**. Gebruik **updatebeheer** voor het filter.

![Galerie lijst](./media/pre-post-scripts/runbook-gallery.png)

U kunt ook zoeken naar de naam van het script, zoals wordt weer gegeven in de volgende lijst:

* Updatebeheer-Vm's inschakelen
* Updatebeheer-Vm's uitschakelen
* Updatebeheer script lokaal uitvoeren
* Updatebeheer-sjabloon voor pre/post-scripts
* Updatebeheer script uitvoeren met de opdracht uitvoeren

> [!IMPORTANT]
> Nadat u de runbooks hebt geïmporteerd, moet u deze **publiceren** voordat ze kunnen worden gebruikt. Als u het runbook wilt zoeken in uw Automation-account, selecteert u **bewerken**en klikt u op **publiceren**.

De voor beelden zijn gebaseerd op de basis sjabloon die in het volgende voor beeld is gedefinieerd. Deze sjabloon kan worden gebruikt om uw eigen runbook te maken voor gebruik met pre-en post-scripts. De benodigde logica voor verificatie met Azure en het verwerken `SoftwareUpdateConfigurationRunContext` van de para meter zijn opgenomen.

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
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
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

## <a name="interacting-with-machines"></a>Interactie met computers

Taken worden uitgevoerd als een runbook in uw Automation-account en niet rechtstreeks op de computers in uw implementatie. Taken voorafgaand aan en post worden ook uitgevoerd in de Azure-context en hebben geen toegang tot niet-Azure-machines. In de volgende secties ziet u hoe u direct met de computers kunt werken, of dit nu een Azure-VM of een niet-Azure-machine is:

### <a name="interacting-with-azure-machines"></a>Interactie met Azure-machines

Voorafgaande en post taken worden uitgevoerd als runbooks en kunnen niet systeem eigen worden uitgevoerd op uw virtuele Azure-machines in uw implementatie. Als u met uw Azure-Vm's wilt communiceren, hebt u de volgende items nodig:

* Een uitvoeren als-account
* Een runbook dat u wilt uitvoeren

Als u wilt communiceren met Azure-machines, moet u de cmdlet [invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) gebruiken om te communiceren met uw Azure-vm's. Zie voor een voor beeld van hoe u dit doet, het runbook [-voor beeld updatebeheer script uitvoeren met de opdracht Run](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc).

### <a name="interacting-with-non-azure-machines"></a>Interactie met niet-Azure-machines

Taken die worden uitgevoerd in de Azure-context en die u uitvoert, hebben geen toegang tot niet-Azure-machines. Als u wilt communiceren met de niet-Azure-computers, hebt u de volgende items nodig:

* Een uitvoeren als-account
* Hybrid Runbook Worker geïnstalleerd op de computer
* Een runbook dat u lokaal wilt uitvoeren
* Bovenliggend runbook

Een bovenliggend runbook wordt uitgevoerd in de Azure-context om te communiceren met niet-Azure-machines. Met dit runbook wordt een onderliggend runbook aangeroepen met de cmdlet [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) . U moet de `-RunOn` para meter opgeven en de naam opgeven van de Hybrid Runbook worker waarmee het script moet worden uitgevoerd. Zie voor een voor beeld van hoe u dit doet het runbook-voor beeld [updatebeheer script lokaal uitvoeren](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44).

## <a name="abort-patch-deployment"></a>Patch implementatie afbreken

Als uw pre-script een fout retourneert, kunt u de implementatie afbreken. Als u dit wilt doen, moet u een fout in het script [genereren](/powershell/module/microsoft.powershell.core/about/about_throw) voor eventuele logica die een fout vormt.

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.  
    throw "There was an error, abort deployment"
}
```

## <a name="known-issues"></a>Bekende problemen

* U kunt geen Booleaanse, objecten of matrices door geven aan para meters bij het gebruik van pre-en post-scripts. Het runbook kan niet worden uitgevoerd. Zie [para meters](#passing-parameters)voor een volledige lijst met ondersteunde typen.

## <a name="next-steps"></a>Volgende stappen

Ga verder met de zelf studie voor meer informatie over het beheren van updates voor uw virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Updates en patches voor uw Azure Windows-Vm's beheren](automation-tutorial-update-management.md)

