---
title: Azure Monitor voor het voorbeeld van de VM's implementeren | Microsoft Docs
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
ms.date: 12/07/2018
ms.author: magoedte
ms.openlocfilehash: 741288bd1a927b12705b3b31c5a1c60d6b94db5b
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53194246"
---
# <a name="deploy-azure-monitor-for-vms-preview"></a>Azure Monitor voor het voorbeeld van de virtuele machines implementeren
In dit artikel wordt beschreven hoe u Azure Monitor instellen voor VM's. De service controleert de status van het besturingssysteem van uw Azure virtual machines (VM's) en virtuele-machineschaalsets en de virtuele machines in uw omgeving. Deze bewaking bevat de detectie en toewijzing van afhankelijkheden voor toepassingen die op deze kan worden gehost. 

U kunt Azure Monitor inschakelen voor virtuele machines met behulp van een van de volgende methoden:  

* Een enkel Azure-machine inschakelen door het selecteren van **inzichten (preview)** rechtstreeks vanuit de virtuele machine.
* Twee of meer Azure-VM's inschakelen met behulp van Azure Policy. Via deze methode worden de vereiste afhankelijkheden van bestaande en nieuwe VM's geïnstalleerd en geconfigureerd. Niet-compatibele VM's worden gerapporteerd, zodat u beslissen kunt of u ze wilt inschakelen en hoe u wilt dat ze te herstellen. 
* Inschakelen van twee of meer virtuele Azure-machines of virtuele machine-schaalsets voor een opgegeven abonnement of resourcegroep met behulp van PowerShell.

Als u meer informatie over elke methode vindt u verderop in het artikel.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg ervoor dat u weet dat de informatie in de volgende secties.

### <a name="log-analytics"></a>Log Analytics 

Een Log Analytics-werkruimte wordt momenteel ondersteund in de volgende regio's:

  - US - west-centraal  
  - US - oost  
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

Zie configureren van uw werkruimte voor het scenario op schaal [instellen van Log Analytics-werkruimte voor een implementatie op schaal](#setup-log-analytics-workspace).

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende tabel bevat de Windows- en Linux-besturingssystemen die worden ondersteund met Azure Monitor voor virtuele machines. Een volledige lijst die details van de primaire en secundaire versie voor Linux-besturingssysteem en de ondersteunde kernelversies wordt verderop in deze sectie.

|Versie van het besturingssysteem |Prestaties |Kaarten |Status |  
|-----------|------------|-----|-------|  
|WindowsServer 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|Red Hat Enterprise Linux (RHEL) 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |  
|CentOS Linux 7, 6 | X | X | X |  
|SUSE Linux Enterprise Server (SLES) 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> prestaties van de functie van Azure Monitor voor virtuele machines is alleen beschikbaar via Azure Monitor. Het is niet beschikbaar wanneer u deze rechtstreeks vanuit het linkerdeelvenster van de Azure-VM openen. 

>[!NOTE]
>De volgende informatie is van toepassing op ondersteuning van het Linux-besturingssysteem:  
> - Alleen standaard- en SMP Linux kernelversies worden ondersteund. 
> - Niet-standaard kernel versies, zoals fysieke Address Extension (PAE) en Xen, worden niet ondersteund voor een Linux-distributie. Bijvoorbeeld, een systeem met de tekenreeks voor de release van *2.6.16.21-0.8-xen* wordt niet ondersteund. 
> - Aangepaste kernels, met inbegrip van hercompilaties van standard kernels, worden niet ondersteund. 
> - CentOSPlus kernel wordt niet ondersteund. 


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |
| 6.9 | 2.6.32-696 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| Ubuntu 18.04 | kernel 4.15. * |
| Ubuntu 16.04.3 | kernel 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="oracle-enterprise-linux-6-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 6 met Unbreakable Enterprise Kernel
| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-enterprise-linux-5-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 5 met Unbreakable Enterprise Kernel

| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

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
    ```

1. Sla dit bestand als *installsolutionsforvminsights.json* naar een lokale map.
1. Bewerk de waarden voor *WorkspaceName*, *ResourceGroupName*, en *WorkspaceLocation*. De waarde voor *WorkspaceName* is de volledige resource-ID van uw Log Analytics-werkruimte, waaronder de naam van de werkruimte. De waarde voor *WorkspaceLocation* is de regio in de werkruimte is gedefinieerd.
1. Bent u klaar voor het implementeren van deze sjabloon met behulp van de volgende PowerShell-opdracht:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid is, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

    ```powershell
    provisioningState       : Succeeded
    ```

### <a name="enable-by-using-azure-policy"></a>Inschakelen met behulp van Azure Policy
Om in te schakelen Azure Monitor voor virtuele machines op schaal op een manier dat helpt ervoor te zorgen consistent naleving en het automatisch inschakelen van de nieuw ingerichte virtuele machines, wordt aangeraden [Azure Policy](../../azure-policy/azure-policy-introduction.md). Deze beleidsregels:

* Implementeer de Log Analytics-agent en de agent voor afhankelijkheden. 
* Rapport over nalevingsresultaten. 
* Herstel voor niet-compatibele virtuele machines.

Azure Monitor inschakelen voor virtuele machines met behulp van Azure Policy in uw tenant: 

- Het initiatief toewijzen aan een bereik: beheergroep, abonnement of resourcegroep 
- Controleer en nalevingsresultaten herstellen  

Zie voor meer informatie over het toewijzen van Azure Policy [overzicht van Azure Policy](../../governance/policy/overview.md#policy-assignment) en bekijk de [overzicht van beheergroepen](../../governance/management-groups/index.md) voordat u doorgaat. 

De beleidsdefinities worden vermeld in de volgende tabel: 

|Name |Description |Type |  
|-----|------------|-----|  
|[Preview]: Azure Monitor voor virtuele machines inschakelen |Azure Monitor inschakelen voor de virtuele Machines (VM's) in het opgegeven bereik (beheergroep, abonnement of resourcegroep). Log Analytics-werkruimte wordt gebruikt als parameter. |Initiatief |  
|[Preview]: Implementatie van de afhankelijkheid Agent-VM-installatiekopie (OS) niet-vermelde controleren |Rapporten VM's als niet-compatibel als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |  
|[Preview]: Implementatie van Log Analytics-Agent: VM-installatiekopie (OS) niet-vermelde controleren |Rapporten VM's als niet-compatibel als de VM-installatiekopie (OS) is niet gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |  
|[Preview]: Agent voor afhankelijkheden voor virtuele Linux-machines implementeren |Agent voor afhankelijkheden implementeren voor Linux-VM's als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |  
|[Preview]: Agent voor afhankelijkheden voor Windows-VM's implementeren |Afhankelijkheid Agent voor de Windows VM's implementeren als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |  
|[Preview]: Log Analytics-Agent voor Linux-VM's implementeren |Implementeer Log Analytics-Agent voor Linux-VM's als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |  
|[Preview]: Log Analytics-Agent voor Windows-VM's implementeren |Log Analytics-Agent voor Windows-VM's implementeren als de VM-installatiekopie (OS) is gedefinieerd in de lijst en de agent is niet geïnstalleerd. |Beleid |  

Beleid voor zelfstandige (niet opgenomen in het initiatief) wordt hier beschreven: 

|Name |Description |Type |  
|-----|------------|-----|  
|[Preview]: Audit Log Analytics-werkruimte voor VM - rapport verschil |Rapporteren als niet-compatibele VM's als ze worden niet van de Log Analytics-werkruimte die is opgegeven in de toewijzing van beleid/initiatief registreren. |Beleid |

#### <a name="assign-the-azure-monitor-initiative"></a>De Azure Monitor-initiatief toewijzen
Met deze eerste release kunt u de beleidstoewijzing maken alleen in de Azure-portal. Zie voor meer informatie over hoe u deze stappen hebt voltooid, [een beleidstoewijzing maken vanuit Azure portal](../../governance/policy/assign-policy-portal.md). 

1. Om te starten in de Azure Policy-service in Azure portal, selecteert u **alle services**, en zoek en selecteer **beleid**. 
1. Selecteer in het linkerdeelvenster van de pagina Azure Policy, **toewijzingen**.  
    Een toewijzing is een beleid dat is toegewezen om te worden toegepast binnen een bepaald bereik.
1. Aan de bovenkant van de **beleid - toewijzingen** weergeeft, schakelt **initiatief toewijzen**.
1. Op de **initiatief toewijzen** weergeeft, schakelt de **bereik** door te klikken op het weglatingsteken (...) en selecteer een beheergroep of het abonnement.  
    In ons voorbeeld wordt een scope beperkt de beleidstoewijzing naar een groepering van virtuele machines voor afdwinging.
1. Aan de onderkant van de **bereik** pagina, sla uw wijzigingen door te selecteren **Selecteer**.
1. (Optioneel) Selecteer een of meer als resources wilt verwijderen uit het bereik, **uitsluitingen**. 
1. Selecteer de **initiatiefdefinitie** selecteert u het weglatingsteken (...) om weer te geven van de lijst met beschikbare definities  **[Preview] Azure Monitor inschakelen voor virtuele machines**, en selecteer vervolgens  **Selecteer**.  
    De **opdrachtnaam** vak wordt automatisch gevuld met de naam van de initiatief die u hebt geselecteerd, maar u kunt deze wijzigen. U kunt ook een optionele beschrijving toevoegen. De **toegewezen door** vakje wordt automatisch ingevuld op basis van die zich heeft aangemeld, en deze waarde is optioneel.
1. In de **Log Analytics-werkruimte** vervolgkeuzelijst lijst voor de ondersteunde regio, selecteer een werkruimte.

    >[!NOTE]
    >Als de werkruimte buiten het bereik van de toewijzing valt, verleent *Inzender van Log Analytics* machtigingen voor de beleidstoewijzing Principal-ID. Als u dit niet doet, ziet u mogelijk een implementatiefout zoals: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ... ` Om toegang te verlenen, Bekijk [het handmatig configureren van de beheerde identiteit](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
    >  
    De **beheerde identiteit** selectievakje is ingeschakeld, omdat het initiatief wordt toegewezen een beleid met omvat de *deployIfNotExists* effect. 
1. In de **identiteit beheren locatie** vervolgkeuzelijst, selecteert u de juiste regio. 
1. Selecteer **Toewijzen**.

#### <a name="review-and-remediate-the-compliance-results"></a>Controleren en herstellen van de compliantieresultaten 

U kunt leren hoe u nalevingsresultaten controleren door te lezen [identificeren van niet-naleving resultaten](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). Selecteer in het linkerdeelvenster **naleving**, en ga vervolgens naar de  **[Preview] Azure Monitor inschakelen voor virtuele machines** initiatief voor virtuele machines die niet compatibel zijn op basis van de toewijzing u hebt gemaakt.

![Naleving van het beleid voor virtuele Azure-machines](./media/vminsights-onboard/policy-view-compliance-01.png)

Op basis van de resultaten van de beleidsregels die zijn opgenomen in het initiatief, worden VM's gerapporteerd als niet-compatibel in de volgende scenario's:  
  
* Log Analytics of de agent voor afhankelijkheden is niet geïmplementeerd. 
   Dit scenario is gebruikelijk om een bereik met bestaande virtuele machines. Om te beperken het, implementeert u de vereiste agents per [het maken van herstel taken](../../governance/policy/how-to/remediate-resources.md) op een beleid voor niet-compatibel.   
 
    - [Preview]: Deploy Dependency Agent for Linux VMs   
    - [Preview]: Deploy Dependency Agent for Windows VMs  
    - [Preview]: Deploy Log Analytics Agent for Linux VMs  
    - [Preview]: Deploy Log Analytics Agent for Windows VMs  

* VM-installatiekopie (OS) wordt niet aangegeven in de beleidsdefinitie. 
   De criteria van het implementatiebeleid voor zijn alleen de VM's die zijn geïmplementeerd vanuit een bekende Azure VM-installatiekopieën. Raadpleeg de documentatie om te zien of het besturingssysteem van de virtuele machine wordt ondersteund. Als dit wordt niet ondersteund, dupliceren van de update en van implementatiebeleid of te wijzigen zodat de installatiekopie van het voldoen aan het beleid. 
  
    - [Preview]: Implementatie van de afhankelijkheid Agent-VM-installatiekopie (OS) niet-vermelde controleren  
    - [Preview]: Implementatie van Log Analytics-Agent: VM-installatiekopie (OS) niet-vermelde controleren

* Virtuele machines zijn niet aangemeld bij de opgegeven Log Analytics-werkruimte.  
    Het is mogelijk dat sommige virtuele machines binnen het initiatief bereik zijn aangemeld bij een Log Analytics-werkruimte dan de naam die opgegeven in de beleidstoewijzing. Dit beleid is een hulpprogramma om te bepalen uit welke VM's rapporteren aan een niet-compatibele werkruimte. 
 
    - [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch  

### <a name="enable-with-powershell"></a>Inschakelen met PowerShell
Om in te schakelen Azure Monitor voor VM's voor meerdere virtuele machines of virtuele-machineschaalsets, kunt u het PowerShell-script [installeren VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), beschikbaar is via de Azure PowerShell Gallery. Met dit script doorloopt elke virtuele machine en de virtuele machine schaalset in uw abonnement, in de groep binnen het bereik resource die opgegeven door *ResourceGroup*, of aan een enkele virtuele machine of VM-schaalset die opgegeven door *Naam*. Voor elke virtuele machine of VM-schaalset het script wordt gecontroleerd of de VM-extensie al is geïnstalleerd. Als de VM-extensie is niet geïnstalleerd, wordt het script probeert te installeren. Als de VM-extensie is geïnstalleerd, installeert het script de Log Analytics- en Afhankelijkheidsmonitors agent VM-extensies.  

Dit script moet Azure PowerShell-moduleversie 5.7.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

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
    ```

1. Sla dit bestand als *installsolutionsforvminsights.json* naar een lokale map.
1. Bewerk de waarden voor *WorkspaceName*, *ResourceGroupName*, en *WorkspaceLocation*. De waarde voor *WorkspaceName* is de volledige resource-ID van uw Log Analytics-werkruimte, waaronder de naam van de werkruimte. De waarde voor *WorkspaceLocation* is de regio in de werkruimte is gedefinieerd.
1. Bent u klaar voor het implementeren van deze sjabloon met behulp van de volgende PowerShell-opdracht:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
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
