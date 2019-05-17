---
title: Azure Monitor inschakelen voor virtuele machines (preview) met behulp van Azure PowerShell of Resource Manager-sjabloon | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Monitor inschakelen voor virtuele machines voor een of meer virtuele Azure-machines of virtuele-machineschaalsets met behulp van Azure PowerShell of Azure Resource Manager-sjablonen.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: a22bc88fb066d9b845f7fdf1592e2194a03915bc
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524128"
---
# <a name="enable-azure-monitor-for-vms-preview-using-azure-powershell-or-resource-manager-template"></a>Azure Monitor inschakelen voor virtuele machines (preview) met behulp van Azure PowerShell of Resource Manager-sjabloon

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

In dit artikel wordt uitgelegd hoe u Azure Monitor voor virtuele machines (preview) inschakelen voor Azure virtual machines of virtuele-machineschaalsets met behulp van Azure PowerShell of Azure Resource Manager-sjablonen. Aan het einde van dit proces dat u wordt is begonnen met het bewaken van al uw virtuele machines en meer als een prestatie- of beschikbaarheidsproblemen optreedt. 

## <a name="set-up-a-log-analytics-workspace"></a>Stel een Log Analytics-werkruimte 

Als u een Log Analytics-werkruimte hebt, maakt u een aan de hand van de methoden die worden voorgesteld in de [vereisten](vminsights-enable-overview.md#log-analytics) sectie voordat u doorgaat met de stappen voor het configureren van het voltooien van de implementatie van Azure Monitor voor Virtuele machines met behulp van de Azure Resource Manager-sjabloon-methode.

### <a name="enable-performance-counters"></a>Inschakelen van prestatiemeteritems

Als de Log Analytics-werkruimte waarnaar wordt verwezen door de oplossing niet is al geconfigureerd voor het verzamelen van de prestatiemeteritems die is vereist voor de oplossing, moet u ze inschakelen. U kunt dit doen op twee manieren:
* Handmatig, zoals beschreven in [Windows en Linux-gegevensbronnen van de prestaties die u in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Door te downloaden en uitvoeren van een PowerShell-script die beschikbaar is in [Azure PowerShell Gallery](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>Installeer de ServiceMap en InfrastructureInsights oplossingen
Deze methode bevat een JSON-sjabloon waarmee de configuratie voor het inschakelen van de oplossingsonderdelen in uw Log Analytics-werkruimte.

Als u niet bekend bent met het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.27 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                            "promotionCode": ""
                        }
                    },
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },
                        "plan": {
                            "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Sla dit bestand als *installsolutionsforvminsights.json* naar een lokale map.

1. Vastleggen van de waarden voor *WorkspaceName*, *ResourceGroupName*, en *WorkspaceLocation*. De waarde voor *WorkspaceName* is de naam van uw Log Analytics-werkruimte. De waarde voor *WorkspaceLocation* is de regio in de werkruimte is gedefinieerd.

1. U kunt deze sjabloon nu implementeren.
 
    * Gebruik de volgende PowerShell-opdrachten in de map met de sjabloon:

        ```powershell
        New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName <ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
        ```

        Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid is, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

        ```powershell
        provisioningState       : Succeeded
        ```

    * De volgende opdracht uitvoeren met behulp van de Azure CLI:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --name DeploySolutions --resource-group <ResourceGroupName> --template-file InstallSolutionsForVMInsights.json --parameters WorkspaceName=<workspaceName> WorkspaceLocation=<WorkspaceLocation - example: eastus>
        ```

Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

```azurecli
provisioningState       : Succeeded
```

## <a name="enable-with-azure-resource-manager-template"></a>Met Azure Resource Manager-sjabloon inschakelen
Voorbeeld van Azure Resource Manager-sjablonen voor onboarding hebben we uw virtuele machines en schaalsets voor virtuele machines gemaakt. Deze sjablonen zijn scenario's voor het inschakelen van bewaking op een bestaande resource en voor het maken van een nieuwe resource waarvoor bewaking ingeschakeld.

>[!NOTE]
>De sjabloon opnieuw moet worden geïmplementeerd in dezelfde resourcegroep bevinden als de resource waarvoor onboarding moet worden.

Als u niet bekend met het concept bent van het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.27 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="download-templates"></a>Downloaden van sjablonen

De Azure Resource Manager-sjablonen vindt u in een archiefbestand (.zip) die u kunt [downloaden](https://aka.ms/VmInsightsARMTemplates) van onze GitHub-opslagplaats. Inhoud van het bestand bevatten mappen voor elk implementatiescenario met een bestand sjabloon en de parameterbestanden. Voordat u die wordt uitgevoerd, wijzigt u de parameterbestand en geef de waarden die zijn vereist. Het sjabloonbestand mag niet worden gewijzigd, tenzij u aanpassen wilt ter ondersteuning van uw specifieke vereisten. Nadat u de parameter-bestand hebt gewijzigd, kunt u met behulp van de volgende methoden beschreven verderop in dit artikel kunt implementeren. 

Het gedownloade bestand bevat de volgende sjablonen voor verschillende scenario's:

- **ExistingVmOnboarding** sjabloon kunt voor virtuele machines van Azure Monitor als de virtuele machines al bestaat.
- **NewVmOnboarding** sjabloon wordt gemaakt van een virtuele machine en Azure Monitor voor virtuele machines om te controleren, kunt.
- **ExistingVmssOnboarding** sjabloon kunt voor virtuele machines van Azure Monitor als de virtuele-machineschaalset al bestaat.
- **NewVmssOnboarding** sjabloon maakt u een virtuele-machineschaalset sets en kunt u Azure-Monitor voor virtuele machines bewaken.
- **ConfigureWorksapce*** sjabloon configureert uw Log Analytics-werkruimte ter ondersteuning van Azure Monitor voor virtuele machines te maken met de oplossingen en het verzamelen van prestatiemeteritems voor Linux en Windows-besturingssysteem.

>[!NOTE]
>Als virtuele-machineschaalsets al aanwezig zijn zijn en het Upgradebeleid is ingesteld op **handmatig**, Azure-Monitor voor virtuele machines wordt niet ingeschakeld voor exemplaren standaard nadat de **ExistingVmssOnboarding** Azure Resource Manager-sjabloon. U moet handmatig een upgrade van de exemplaren.

### <a name="deploy-using-azure-powershell"></a>Implementeren met Azure PowerShell

De volgende stap kunt controleren met behulp van Azure PowerShell.

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```
Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

```powershell
provisioningState       : Succeeded
```
### <a name="deploy-using-azure-cli"></a>Implementeren met behulp van Azure CLI

De volgende stap kunt controleren met behulp van Azure CLI.   

```azurecli
az login
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```

De uitvoer ziet eruit als in het volgende:

```azurecli
provisioningState       : Succeeded
```

## <a name="enable-with-powershell"></a>Inschakelen met PowerShell

Om in te schakelen Azure Monitor voor VM's voor meerdere virtuele machines of virtuele-machineschaalsets, kunt u het PowerShell-script [installeren VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), beschikbaar is via de Azure PowerShell Gallery. Met dit script doorloopt elke virtuele machine en de virtuele machine schaalset in uw abonnement, in de groep binnen het bereik resource die opgegeven door *ResourceGroup*, of aan een enkele virtuele machine of VM-schaalset die opgegeven door *Naam*. Voor elke virtuele machine of VM-schaalset het script wordt gecontroleerd of de VM-extensie al is geïnstalleerd. Als de VM-extensie is niet geïnstalleerd, wordt het script probeert te installeren. Als de VM-extensie is geïnstalleerd, installeert het script de Log Analytics- en Afhankelijkheidsmonitors agent VM-extensies.

Dit script moet Azure PowerShell-module Az versie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

Uitvoeren als u een lijst van het script argument details en een voorbeeld, `Get-Help`.

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VMs and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VMs/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VMs in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to reinstall (move to a new workspace)
```

Het volgende voorbeeld ziet u met behulp van de PowerShell-opdrachten in de map Azure Monitor inschakelen voor virtuele machines en begrijpen van de verwachte uitvoer:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or VM ScaleSets matching criteria specified

VMs or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on above 2 VMs or VM Scale Sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="next-steps"></a>Volgende stappen

Nu dat de controle is ingeschakeld voor uw virtuele machines, is deze informatie is beschikbaar voor analyse met Azure Monitor voor virtuele machines. Zie voor meer informatie over het gebruik van de Health-functie, [weergave Azure Monitor voor virtuele machines Health](vminsights-health.md). Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md). Voor het identificeren van knelpunten en het algehele gebruik met de prestaties van uw VM's, Zie [weergave Azure VM-prestaties](vminsights-performance.md), of als u afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).