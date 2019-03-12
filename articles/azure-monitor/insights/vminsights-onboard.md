---
title: Azure Monitor implementeren voor virtuele machines (preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u implementeren en configureren van Azure Monitor voor virtuele machines, zodat u kunt informatie over hoe de gedistribueerde toepassing wordt uitgevoerd en welke status problemen zijn geïdentificeerd.
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
ms.date: 03/11/2019
ms.author: magoedte
ms.openlocfilehash: 08cbff04a1755aec807862d373d4c10e423c1b3a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57781745"
---
# <a name="deploy-azure-monitor-for-vms-preview"></a>Azure Monitor implementeren voor virtuele machines (preview)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

In dit artikel wordt beschreven hoe u Azure Monitor instellen voor VM's. De service controleert de status van het besturingssysteem van uw Azure virtual machines (VM's) en virtuele-machineschaalsets en de virtuele machines in uw omgeving. Deze bewaking bevat de detectie en toewijzing van afhankelijkheden voor toepassingen die op deze kan worden gehost. 

U kunt Azure Monitor inschakelen voor virtuele machines met behulp van een van de volgende methoden:

* Een enkel Azure-machine inschakelen door het selecteren van **inzichten (preview)** rechtstreeks vanuit de virtuele machine.
* Twee of meer Azure-VM's inschakelen met behulp van Azure Policy. Via deze methode worden de vereiste afhankelijkheden van bestaande en nieuwe VM's geïnstalleerd en geconfigureerd. Niet-compatibele VM's worden gerapporteerd, zodat u beslissen kunt of u ze wilt inschakelen en hoe u wilt dat ze te herstellen.
* Inschakelen van twee of meer virtuele Azure-machines of virtuele machine-schaalsets voor een opgegeven abonnement of resourcegroep met behulp van PowerShell.

Als u meer informatie over elke methode vindt u verderop in het artikel.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg ervoor dat u weet dat de informatie in de volgende secties.

### <a name="log-analytics"></a>Log Analytics

Azure Monitor voor virtuele machines ondersteunt een Log Analytics-werkruimte in de volgende regio's:

- US - west-centraal
- US - oost
- Canada centraal<sup>1</sup>
- UK-Zuid<sup>1</sup>
- Europa -west
- Zuidoost-Azië<sup>1</sup>

<sup>1</sup> deze regio momenteel de status-functie van Azure Monitor biedt geen ondersteuning voor virtuele machines.

>[!NOTE]
>Virtuele machines van Azure van andere regio's kunnen worden geïmplementeerd en worden niet beperkt tot de ondersteunde regio's voor de Log Analytics-werkruimte.
>

Als u een werkruimte hebt, kunt u er een maken met een van de volgende methoden:
* [De Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Als u bewaking voor één Azure-VM in Azure portal inschakelen bent, kunt u een werkruimte maken tijdens dit proces.

Om in te schakelen van de oplossing voor het scenario op schaal, moet u eerst het volgende configureren in uw Log Analytics-werkruimte:

* Installeer de ServiceMap en InfrastructureInsights oplossingen. U kunt deze installatie voltooien alleen met behulp van een Azure Resource Manager-sjabloon die opgegeven in dit artikel.
* Configureer de werkruimte voor logboekanalyse voor het verzamelen van prestatiemeteritems.

Zie voor het configureren van uw werkruimte voor het scenario op schaal, instellen van Log Analytics-werkruimte voor een implementatie op schaal.

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende tabel bevat de Windows- en Linux-besturingssystemen die worden ondersteund met Azure Monitor voor virtuele machines. Een volledige lijst die details van de primaire en secundaire versie voor Linux-besturingssysteem en de ondersteunde kernelversies wordt verderop in deze sectie.

|Versie van het besturingssysteem |Prestaties |Kaarten |Status |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X |  |
|WindowsServer 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| X |
|Ubuntu 14.04, 16.04, 18.04 | X | X | X |
|CentOS Linux, 6, 7 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 11, 12 | X | X | X |
|Debian 8, 9.4 | X<sup>1</sup> | | X |

<sup>1</sup> prestaties van de functie van Azure Monitor voor virtuele machines is alleen beschikbaar via Azure Monitor. Het is niet beschikbaar wanneer u deze rechtstreeks vanuit het linkerdeelvenster van de Azure-VM openen.

>[!NOTE]
>De volgende informatie is van toepassing op ondersteuning van het Linux-besturingssysteem:
> - Alleen standaard- en SMP Linux kernelversies worden ondersteund.
> - Niet-standaard kernel versies, zoals fysieke Address Extension (PAE) en Xen, worden niet ondersteund voor een Linux-distributie. Bijvoorbeeld, een systeem met de tekenreeks voor de release van *2.6.16.21-0.8-xen* wordt niet ondersteund.
> - Aangepaste kernels, met inbegrip van hercompilaties van standard kernels, worden niet ondersteund.
> - CentOSPlus kernel wordt ondersteund.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

### <a name="centosplus"></a>CentOSPlus
| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| Ubuntu 18.04 | kernel 4.15. * |
| Ubuntu 16.04.3 | kernel 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="suse-linux-11-enterprise-server"></a>SUSE Linux 11 Enterprise Server

| Versie van het besturingssysteem | Kernelversie
|:--|:--|
|11 SP4 | 3.0.* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Versie van het besturingssysteem | Kernelversie
|:--|:--|
|12 SP2 | 4.4. * |
|12 SP3 | 4.4. * |

### <a name="the-microsoft-dependency-agent"></a>De agent voor Microsoft Dependency
De Azure-Monitor voor de functie voor toewijzing van virtuele machines worden de gegevens uit de agent voor Microsoft Dependency opgehaald. De agent voor afhankelijkheden, is afhankelijk van de Log Analytics-agent voor de verbinding met Log Analytics. Uw systeem moet daarom de Log Analytics-agent geïnstalleerd en geconfigureerd met de agent voor afhankelijkheden hebben.

Of u Azure Monitor voor virtuele machines voor een enkele Azure-VM inschakelen of u de methode van de implementatie op schaal gebruiken, moet u de afhankelijkheid van Azure VM agent-extensie gebruiken voor het installeren van de agent als onderdeel van de ervaring.

U kunt in een hybride omgeving, downloaden en installeren van de agent voor afhankelijkheden op twee manieren: Handmatig of via een methode geautomatiseerde implementatie voor virtuele machines die worden gehost buiten Azure.

De volgende tabel beschrijft de verbonden bronnen die ondersteuning biedt voor de kaart-functie in een hybride omgeving.

| Verbonden bron | Ondersteund | Description |
|:--|:--|:--|
| Windows-agents | Ja | Naast de [Log Analytics-agent voor Windows](../../azure-monitor/platform/log-analytics-agent.md), Windows-agents moeten de agent voor Microsoft Dependency. Zie voor een volledige lijst van de versies van besturingssystemen, [ondersteunde besturingssystemen](#supported-operating-systems). |
| Linux-agents | Ja | Naast de [Log Analytics-agent voor Linux](../../azure-monitor/platform/log-analytics-agent.md), Linux-agents moeten de agent voor Microsoft Dependency. Zie voor een volledige lijst van de versies van besturingssystemen, [ondersteunde besturingssystemen](#supported-operating-systems). |
| Beheergroep System Center Operations Manager | Nee | |

De agent voor afhankelijkheden kan worden gedownload van de volgende locaties:

| File | OS | Versie | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Als u wilt inschakelen en toegang tot de functies in Azure Monitor voor virtuele machines, moet u de volgende rollen voor toegang worden toegewezen:

- Als u wilt inschakelen op de oplossing, hebt u de *Inzender van Log Analytics* rol.

- Als u wilt weergeven van prestaties, status, status en gegevens, hebt u de *Monitoring Reader* rol voor de virtuele machine van Azure. De Log Analytics-werkruimte moet worden geconfigureerd voor Azure Monitor voor virtuele machines.

Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../../azure-monitor/platform/manage-access.md).

## <a name="enable-monitoring-in-the-azure-portal"></a>Schakel bewaking in Azure portal
Voor bewaking van uw Azure-VM in Azure portal, het volgende doen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **virtuele Machines**.

1. Selecteer een VM in de lijst.

1. Op de pagina virtuele machine in de **bewaking** sectie, selecteer **inzichten (preview)**.

1. Op de **inzichten (preview)** weergeeft, schakelt **Probeer nu**.

    ![Azure Monitor voor virtuele machines voor een virtuele machine inschakelen](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)
1. Op de **Azure Monitor Insights voorbereiden** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement, selecteert u deze in de vervolgkeuzelijst.  
    De lijst worden er de standaardwerkruimte en de locatie die de virtuele machine is geïmplementeerd op in het abonnement. 

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de bewakingsgegevens van de virtuele machine, volgt u de instructies in [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md) in een van de ondersteunde regio's eerder vermelde.

Wanneer u bewaking inschakelt, is het duurt ongeveer 10 minuten voordat u de status van metrische gegevens voor de virtuele machine kunt weergeven.

![Azure Monitor inschakelen voor virtuele machines verwerking van de implementatie controleren](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="deploy-at-scale"></a>Implementeer op schaal
In deze sectie maakt implementeert u Azure Monitor voor virtuele machines op schaal met behulp van Azure Policy of Azure PowerShell.

Voordat u uw virtuele machines implementeert, vooraf configureren van uw Log Analytics-werkruimte door de volgende te doen:

1. Als u nog een werkruimte hebt, maakt u een dat kan Azure Monitor worden ondersteund voor virtuele machines.  
    Voordat u doorgaat, Zie [werkruimten beheren](../../log-analytics/log-analytics-manage-access.md?toc=/azure/azure-monitor/toc.json) om te begrijpen van de overwegingen kosten, beheer en naleving.

1. Maak een nieuwe werkruimte als deze niet al die bestaat kan worden gebruikt voor de ondersteuning van Azure Monitor voor virtuele machines. Beoordeling [werkruimten beheren](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json) voordat u een nieuwe werkruimte voor meer informatie over de kosten, beheer en naleving overwegingen voordat u doorgaat.

1. Inschakelen van prestatiemeteritems in de werkruimte voor de verzameling op Linux- en Windows-machines.

1. Installeren en inschakelen van de oplossing ServiceMap en InfrastructureInsights in uw werkruimte.

### <a name="set-up-a-log-analytics-workspace"></a>Stel een Log Analytics-werkruimte
Als u een Log Analytics-werkruimte hebt, maakt u een aan de hand van de methoden die worden voorgesteld in de ['Vereisten'](#log-analytics) sectie.

#### <a name="enable-performance-counters"></a>Inschakelen van prestatiemeteritems
Als de Log Analytics-werkruimte waarnaar wordt verwezen door de oplossing niet is al geconfigureerd voor het verzamelen van de prestatiemeteritems die is vereist voor de oplossing, moet u ze inschakelen. U kunt dit doen op twee manieren:
* Handmatig, zoals beschreven in [Windows en Linux-gegevensbronnen van de prestaties die u in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Door te downloaden en uitvoeren van een PowerShell-script die beschikbaar is in [Azure PowerShell Gallery](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>Installeer de ServiceMap en InfrastructureInsights oplossingen
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

### Enable by using Azure Policy
To enable Azure Monitor for VMs at scale in a way that helps ensure consistent compliance and the automatic enabling of the newly provisioned VMs, we recommend [Azure Policy](../../governance/policy/overview.md). These policies:

* Deploy the Log Analytics agent and the Dependency agent.
* Report on compliance results.
* Remediate for non-compliant VMs.

To enable Azure Monitor for VMs by using Azure Policy in your tenant:

- Assign the initiative to a scope: management group, subscription, or resource group
- Review and remediate compliance results

For more information about assigning Azure Policy, see [Azure Policy overview](../../governance/policy/overview.md#policy-assignment) and review the [overview of management groups](../../governance/management-groups/index.md) before you continue.

The policy definitions are listed in the following table:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Enable Azure Monitor for VMs |Enable Azure Monitor for the Virtual Machines (VMs) in the specified scope (management group, subscription, or resource group). Takes Log Analytics workspace as a parameter. |Initiative |
|[Preview]: Audit Dependency Agent Deployment – VM Image (OS) unlisted |Reports VMs as non-compliant if the VM Image (OS) isn't defined in the list and the agent isn't installed. |Policy |
|[Preview]: Audit Log Analytics Agent Deployment – VM Image (OS) unlisted |Reports VMs as non-compliant if the VM Image (OS) isn't defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Dependency Agent for Linux VMs |Deploy Dependency Agent for Linux VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Dependency Agent for Windows VMs |Deploy Dependency Agent for Windows VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Log Analytics Agent for Linux VMs |Deploy Log Analytics Agent for Linux VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Log Analytics Agent for Windows VMs |Deploy Log Analytics Agent for Windows VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |

Standalone policy (not included with the initiative) is described here:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Audit Log Analytics Workspace for VM - Report Mismatch |Report VMs as non-compliant if they aren't logging to the Log Analytics workspace specified in the policy/initiative assignment. |Policy |

#### Assign the Azure Monitor initiative
With this initial release, you can create the policy assignment only in the Azure portal. To understand how to complete these steps, see [Create a policy assignment from the Azure portal](../../governance/policy/assign-policy-portal.md).

1. To launch the Azure Policy service in the Azure portal, select **All services**, and then search for and select **Policy**.

1. In the left pane of the Azure Policy page, select **Assignments**.  
    An assignment is a policy that has been assigned to take place within a specific scope.
    
1. At the top of the **Policy - Assignments** page, select **Assign Initiative**.

1. On the **Assign Initiative** page, select the **Scope** by clicking the ellipsis (...), and select a management group or subscription.  
    In our example, a scope limits the policy assignment to a grouping of virtual machines for enforcement.
    
1. At the bottom of the **Scope** page, save your changes by selecting **Select**.

1. (Optional) To remove one or more resources from the scope, select **Exclusions**.

1. Select the **Initiative definition** ellipsis (...) to display the list of available definitions, select **[Preview] Enable Azure Monitor for VMs**, and then select **Select**.  
    The **Assignment name** box is automatically populated with the initiative name you selected, but you can change it. You can also add an optional description. The **Assigned by** box is automatically populated based on who is logged in, and this value is optional.
    
1. In the **Log Analytics workspace** drop-down list for the supported region, select a workspace.

    >[!NOTE]
    >If the workspace is beyond the scope of the assignment, grant *Log Analytics Contributor* permissions to the policy assignment's Principal ID. If you don't do this, you might see a deployment failure such as: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ... `
    >To grant access, review [how to manually configure the managed identity](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
    >  
    The **Managed Identity** check box is selected, because the initiative being assigned includes a policy with the *deployIfNotExists* effect.
    
1. In the **Manage Identity location** drop-down list, select the appropriate region.

1. Select **Assign**.

#### Review and remediate the compliance results

You can learn how to review compliance results by reading [identify non-compliance results](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). In the left pane, select **Compliance**, and then locate the **[Preview] Enable Azure Monitor for VMs** initiative for VMs that aren't compliant according to the assignment you created.

![Policy compliance for Azure VMs](./media/vminsights-onboard/policy-view-compliance-01.png)

Based on the results of the policies included with the initiative, VMs are reported as non-compliant in the following scenarios:

* Log Analytics or the Dependency agent isn't deployed.  
    This scenario is typical for a scope with existing VMs. To mitigate it, deploy the required agents by [creating remediation tasks](../../governance/policy/how-to/remediate-resources.md) on a non-compliant policy.  
    - [Preview]: Deploy Dependency Agent for Linux VMs
    - [Preview]: Deploy Dependency Agent for Windows VMs
    - [Preview]: Deploy Log Analytics Agent for Linux VMs
    - [Preview]: Deploy Log Analytics Agent for Windows VMs

* VM Image (OS) isn't identified in the policy definition.  
    The criteria of the deployment policy include only VMs that are deployed from well-known Azure VM images. Check the documentation to see whether the VM OS is supported. If it isn't supported, duplicate the deployment policy and update or modify it to make the image compliant.  
    - [Preview]: Audit Dependency Agent Deployment – VM Image (OS) unlisted
    - [Preview]: Audit Log Analytics Agent Deployment – VM Image (OS) unlisted

* VMs aren't logging in to the specified Log Analytics workspace.  
    It's possible that some VMs in the initiative scope are logging in to a Log Analytics workspace other than the one that's specified in the policy assignment. This policy is a tool to identify which VMs are reporting to a non-compliant workspace.  
    - [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch

### Enable with PowerShell
To enable Azure Monitor for VMs for multiple VMs or virtual machine scale sets, you can use the PowerShell script [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), available from the Azure PowerShell Gallery. This script iterates through every virtual machine and virtual machine scale set in your subscription, in the scoped resource group that's specified by *ResourceGroup*, or to a single VM or virtual machine scale set that's specified by *Name*. For each VM or virtual machine scale set, the script verifies whether the VM extension is already installed. If the VM extension is not installed, the script tries to reinstall it. If the VM extension is installed, the script installs the Log Analytics and Dependency agent VM extensions.

This script requires Azure PowerShell module Az version 1.0.0 or later. Run `Get-Module -ListAvailable Az` to find the version. If you need to upgrade, see [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps). If you're running PowerShell locally, you also need to run `Connect-AzAccount` to create a connection with Azure.

To get a list of the script's argument details and example usage, run `Get-Help`.

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

## <a name="enable-for-a-hybrid-environment"></a>Inschakelen voor een hybride omgeving
In deze sectie wordt uitgelegd hoe u virtuele machines of fysieke computers die worden gehost in uw datacenter of andere cloudomgevingen voor het bewaken van Azure monitor voor VM's implementeren.

De Azure-Monitor voor agent voor afhankelijkheden van virtuele machines toewijzen niet de gegevens zelf worden verzonden en er is geen wijzigingen in de firewalls en poorten vereist. De kaartgegevens worden altijd verzonden door de Log Analytics-agent naar de service Azure Monitor, hetzij rechtstreeks of via de [OMS-Gateway](../../azure-monitor/platform/gateway.md) als uw IT-beveiligingsbeleid niet toestaat dat computers in het netwerk verbinding maken met internet.

Bekijk de vereisten en methoden voor het implementeren van de [Log Analytics Linux en Windows-agent](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

De stappen zijn als volgt worden samengevat:

1. Log Analytics-Agent voor Windows of Linux installeren.

1. Download en installeer de Azure-Monitor voor agent voor virtuele machines kaart afhankelijkheden voor [Windows](https://aka.ms/dependencyagentwindows) of [Linux](https://aka.ms/dependencyagentlinux).

1. Schakel het verzamelen van prestatiemeteritems.

1. Implementeren met Azure Monitor voor virtuele machines.

### <a name="install-the-dependency-agent-on-windows"></a>De afhankelijkheidsagent installeren op Windows
U kunt de agent voor afhankelijkheden handmatig installeren op Windows-computers door uit te voeren `InstallDependencyAgent-Windows.exe`. Als u dit uitvoerbare bestand zonder opties uitvoert, wordt er een setup-wizard die u volgen kunt om de installatie interactief gestart.

>[!NOTE]
>*Beheerder* bevoegdheden zijn vereist om te installeren of verwijderen van de agent.

De volgende tabel ziet u de parameters die worden ondersteund door het installatieprogramma voor de agent vanaf de opdrachtregel.

| Parameter | Description |
|:--|:--|
| /? | Retourneert een lijst van de opdrachtregelopties. |
| /S | Voert een installatie op de achtergrond zonder tussenkomst van de gebruiker. |

Bijvoorbeeld, om uit te voeren van het installatieprogramma met de `/?` parameter, type **InstallDependencyAgent Windows.exe /?**.

Bestanden voor de agent voor Windows-afhankelijkheden zijn geïnstalleerd in *C:\Program Files\Microsoft Afhankelijkheidsagent* standaard. Als de agent voor afhankelijkheden niet kunt starten nadat setup voltooid is, controleert u de logboeken voor uitgebreide foutgegevens. De logboekmap is *%Programfiles%\Microsoft afhankelijkheid Agent\logs*.

### <a name="install-the-dependency-agent-on-linux"></a>De afhankelijkheidsagent installeren in Linux
De agent voor afhankelijkheden is geïnstalleerd op Linux-servers van *InstallDependencyAgent Linux64.bin*, een shell-script met een zichzelf uitpakkend binair bestand. U kunt het bestand uitvoeren met behulp van `sh` of toe te voegen uitvoermachtigingen naar het bestand zelf.

>[!NOTE]
> Toegang tot de hoofdmap is vereist om de agent te installeren of configureren.
>

| Parameter | Description |
|:--|:--|
| -help | Een lijst met de opdrachtregelopties ophalen. |
| -s | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |
| --controleren | Controleer machtigingen en het besturingssysteem, maar de agent niet installeren. |

Bijvoorbeeld, om uit te voeren van het installatieprogramma met de `-help` parameter, type **InstallDependencyAgent Linux64.bin-help**.

De agent voor Linux-afhankelijkheden als root installeren met de volgende opdracht `sh InstallDependencyAgent-Linux64.bin`.

Als de agent voor afhankelijkheden niet kan worden gestart, controleert u de logboeken voor uitgebreide foutgegevens. Op Linux-agents, de logboekmap is */var/opt/microsoft/dependency-agent/log*.

Bestanden voor de agent voor afhankelijkheden worden geplaatst in de volgende mappen:

| Bestanden | Locatie |
|:--|:--|
| Kernbestanden | /opt/microsoft/dependency-agent |
| Logboekbestanden | /var/opt/microsoft/dependency-agent/log |
| Configuratiebestanden | /etc/opt/microsoft/dependency-agent/config |
| Uitvoerbare bestanden van de service | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binaire opslagbestanden | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Inschakelen van prestatiemeteritems
Als de Log Analytics-werkruimte waarnaar wordt verwezen door de oplossing niet is al geconfigureerd voor het verzamelen van de prestatiemeteritems die is vereist voor de oplossing, moet u ze inschakelen. U kunt dit doen op twee manieren:
* Handmatig, zoals beschreven in [Windows en Linux-gegevensbronnen van de prestaties die u in Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Door te downloaden en uitvoeren van een PowerShell-script die beschikbaar is in [Azure PowerShell Gallery](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

### <a name="deploy-azure-monitor-for-vms"></a>Azure Monitor voor virtuele machines implementeren
Deze methode bevat een JSON-sjabloon waarmee de configuratie voor het inschakelen van de oplossingsonderdelen in uw Log Analytics-werkruimte.

Als u niet bekend bent met het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Als u ervoor de Azure CLI gebruiken kiest, moet u eerst installeren en de CLI lokaal gebruikt. U moet worden uitgevoerd van Azure CLI versie 2.0.27 of hoger. Voor het identificeren van uw versie uitvoeren `az --version`. Als u wilt installeren of upgraden van de Azure CLI, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

#### <a name="create-and-execute-a-template"></a>Maken en uitvoeren van een sjabloon

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

1. Bewerk de waarden voor *WorkspaceName*, *ResourceGroupName*, en *WorkspaceLocation*. De waarde voor *WorkspaceName* is de volledige resource-ID van uw Log Analytics-werkruimte, waaronder de naam van de werkruimte. De waarde voor *WorkspaceLocation* is de regio in de werkruimte is gedefinieerd.

1. Bent u klaar voor het implementeren van deze sjabloon met behulp van de volgende PowerShell-opdracht:

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid is, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

    ```powershell
    provisioningState       : Succeeded
    ```
Wanneer u bewaking inschakelt, is het duurt ongeveer 10 minuten voordat u de status en metrische gegevens voor de hybride-computer kunt weergeven.

## <a name="performance-counters-enabled"></a>Prestatiemeteritems ingeschakeld
Azure Monitor voor virtuele machines configureert u een werkruimte voor logboekanalyse voor het verzamelen van de prestatiemeteritems die worden gebruikt door de oplossing. De volgende tabel bevat de objecten en geconfigureerd door de oplossing voor prestatiemeteritems die elke 60 seconden worden verzameld.

### <a name="windows-performance-counters"></a>Windows-prestatiemeteritems

|Objectnaam |Naam van het prestatiemeteritem |
|------------|-------------|
|Logische schijf |Percentage vrije ruimte |
|Logische schijf |Gem. Schijf sec/lezen |
|Logische schijf |Gem. Schijfoverdrachten per seconde |
|Logische schijf |Gem. Schijf sec/schrijven |
|Logische schijf |Schijf Bytes per seconde |
|Logische schijf |Bytes gelezen op schijf/sec |
|Logische schijf |Schijf lezen per seconde |
|Logische schijf |Schijfoverdrachten per seconde |
|Logische schijf |Bytes geschreven naar schijf/sec |
|Logische schijf |Schijf schrijven per seconde |
|Logische schijf |Beschikbare Megabytes |
|Geheugen |Beschikbare megabytes (MB) |
|-Netwerkadapter |Ontvangen bytes per seconde |
|-Netwerkadapter |Verzonden bytes per seconde |
|Processor |% Processortijd |

### <a name="linux-performance-counters"></a>Linux-prestatiemeteritems

|Objectnaam |Naam van het prestatiemeteritem |
|------------|-------------|
|Logische schijf |Percentage gebruikte ruimte |
|Logische schijf |Bytes gelezen op schijf/sec |
|Logische schijf |Schijf lezen per seconde |
|Logische schijf |Schijfoverdrachten per seconde |
|Logische schijf |Bytes geschreven naar schijf/sec |
|Logische schijf |Schijf schrijven per seconde |
|Logische schijf |Beschikbare Megabytes |
|Logische schijf |Logische schijf Bytes per seconde |
|Geheugen |Beschikbaar geheugen in megabytes |
|Netwerk |Totaal aantal ontvangen Bytes |
|Netwerk |Totaal aantal verzonden Bytes |
|Processor |% Processortijd |

## <a name="diagnostic-and-usage-data"></a>Diagnostische en gebruiksgegevens
Microsoft verzamelt automatisch gebruiks- en -gegevens door uw gebruik van de Azure Monitor-service. Microsoft gebruikt deze gegevens te bieden en de kwaliteit, beveiliging en integriteit van de service te verbeteren. Voor nauwkeurige en efficiënte mogelijkheden voor probleemoplossing, bevatten gegevens van de functie voor toewijzing informatie over de configuratie van uw software, zoals het besturingssysteem en versie, IP-adres, DNS-naam en de Werkstationnaam van het. Microsoft biedt geen namen, adressen of andere contactgegevens verzameld.

Zie voor meer informatie over het verzamelen van gegevens en het gebruik, de [privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]
## <a name="next-steps"></a>Volgende stappen

Nu dat de controle is ingeschakeld voor uw virtuele machine, is deze informatie is beschikbaar voor analyse met Azure Monitor voor virtuele machines. Zie voor meer informatie over het gebruik van de Health-functie, [weergave Azure Monitor voor virtuele machines Health](vminsights-health.md). Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).
