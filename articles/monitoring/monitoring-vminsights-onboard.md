---
title: Onboarding van Azure Monitor voor virtuele machines | Microsoft Docs
description: Dit artikel wordt beschreven hoe u met het voorbereiden en configureren van Azure Monitor voor virtuele machines, zodat u kunt inzicht krijgen in hoe uw gedistribueerde toepassing wordt uitgevoerd en welke statusproblemen zijn geïdentificeerd.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: 2f0568064eed556429675ffb34c84d588ac670d5
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47064353"
---
# <a name="how-to-onboard-the-azure-monitor-for-vms"></a>Hoe zorgen voor onboarding Azure controleren voor virtuele machines 
In dit artikel wordt beschreven hoe het instellen van Azure Monitor voor virtuele machines om te controleren van de status van het besturingssysteem van uw Azure virtual machines en detecteren en afhankelijkheden voor toepassingen die kunnen worden gehost op deze worden toegewezen.  

Azure Monitor inschakelen voor virtuele machines wordt gerealiseerd met behulp van een van de volgende methoden en informatie over het gebruik van elke methode vindt u verderop in het artikel.  

* Een enkel Azure-machine door het selecteren van **inzichten (preview)** rechtstreeks vanuit de virtuele machine.
* Meerdere virtuele Azure-machines met behulp van Azure-beleid om ervoor te zorgen bestaande en nieuwe VM's die zijn geëvalueerd hebben de vereiste afhankelijkheden geïnstalleerd en correct zijn geconfigureerd.  Niet-compatibele VM's worden gerapporteerd, zodat u op basis bepalen kunt van wat niet compatibel is, hoe u wilt herstellen.  
* Meerdere virtuele Azure-machines of virtuele machine-schaalsets voor een opgegeven abonnement of resourcegroep met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg ervoor dat u de volgende hebt zoals beschreven in de volgende subsecties.

### <a name="log-analytics"></a>Log Analytics 

Een Log Analytics-werkruimte in de volgende regio's worden momenteel ondersteund:

  - US - west-centraal  
  - US - oost  
  - Europa -west  
  - Zuidoost-Azië<sup>1</sup>  

<sup>1</sup> deze regio biedt momenteel geen ondersteuning de Health-functie van Azure Monitor voor virtuele machines   

Als u een werkruimte hebt, kunt u kunt maken via [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), tot en met [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), of in de [Azure-portal](../log-analytics/log-analytics-quick-create-workspace.md).  

Om in te schakelen van de oplossing, moet u lid zijn van de inzendersrol van Log Analytics. Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende tabel bevat de Windows- en Linux-besturingssystemen die worden ondersteund met Azure Monitor voor virtuele machines.  Een volledige lijst met gedetailleerde informatie over die de primaire en secundaire Linux-besturingssysteem vrijgeven en kernelversies die ondersteund worden verderop in deze sectie.

|Versie van het besturingssysteem |Prestaties |Kaarten |Status |  
|-----------|------------|-----|-------|  
|WindowsServer 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|RHEL 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |  
|Cent OS Linux 7, 6 | X | X | X |  
|SLES 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> prestaties van de functie van Azure Monitor voor virtuele machines is alleen beschikbaar is via Azure Monitor, is niet beschikbaar wanneer u rechtstreeks toegang hebben tot deze in het linkerdeelvenster van de virtuele machine van Azure.  

>[!NOTE]
>De volgende informatie is van toepassing op ondersteuning van het Linux-besturingssysteem:  
> - Alleen standaard- en SMP Linux kernelversies worden ondersteund.  
> - Niet-standaard kernelversies, zoals PAE en Xen, worden voor geen enkele Linux-distributie ondersteund. Bijvoorbeeld, wordt een systeem met de tekenreeks voor de release van '2.6.16.21-0.8-xen' niet ondersteund.  
> - Aangepaste kernels, met inbegrip van hercompilaties van standaardkernels, worden niet ondersteund.  
> - CentOSPlus-kernel wordt niet ondersteund.  


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

### <a name="hybrid-environment-connected-sources"></a>Hybride omgeving verbonden bronnen
De gegevens worden opgehaald van de agent voor Microsoft Dependency van Azure Monitor voor virtuele machines toewijzen. De agent voor afhankelijkheden, is afhankelijk van de Log Analytics-agent voor de verbinding met Log Analytics. Dit betekent dat een systeem de Log Analytics-agent geïnstalleerd en geconfigureerd met de agent voor afhankelijkheden moet hebben.  De volgende tabel beschrijft de verbonden bronnen die ondersteuning biedt voor de kaart-functie in een hybride omgeving.

| Verbonden bron | Ondersteund | Beschrijving |
|:--|:--|:--|
| Windows-agents | Ja | Naast de [Log Analytics-agent voor Windows](../log-analytics/log-analytics-concept-hybrid.md), Windows-agents moeten de agent voor Microsoft Dependency. Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Linux-agents | Ja | Naast de [Log Analytics-agent voor Linux](../log-analytics/log-analytics-concept-hybrid.md), Linux-agents moeten de agent voor Microsoft Dependency. Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Beheergroep System Center Operations Manager | Nee | |  

Op Windows, Microsoft Monitoring Agent (MMA) wordt gebruikt door zowel de System Center Operations Manager en de Log Analytics om te verzamelen en verzenden door gegevens te controleren. System Center Operations Manager en de Log Analytics bieden verschillende kant-en-klare versies van de agent. Deze versies kunnen beide rapporteren aan System Center Operations Manager, aan Log Analytics of aan beide.  

Op Linux, de Log Analytics-agent voor Linux verzamelt en verzendt gegevens naar Log Analytics te controleren.   

Als uw Windows- of Linux-computers kunnen niet rechtstreeks verbinding met de service maken, moet u de Log Analytics-agent te koppelen aan Log Analytics met behulp van de OMS-Gateway configureren. Zie voor meer informatie over het implementeren en configureren van de OMS-Gateway [verbinding maken met computers zonder toegang tot het Internet met behulp van de OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md).  

De agent voor afhankelijkheden kan worden gedownload vanaf de volgende locatie.

| File | OS | Versie | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.1 | 55030ABF553693D8B5112569FB2F97D7C54B66E9990014FC8CC43EFB70DE56C6 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.1 | 43C75EF0D34471A0CBCE5E396FFEEF4329C9B5517266108FA5D6131A353D29FE |

## <a name="diagnostic-and-usage-data"></a>Diagnostische en gebruiksgegevens
Microsoft verzamelt automatisch gebruiks- en -gegevens door uw gebruik van de Azure Monitor-service. Microsoft gebruikt deze gegevens te bieden en de kwaliteit, beveiliging en integriteit van de service te verbeteren. Voor nauwkeurige en efficiënte mogelijkheden voor probleemoplossing, bevatten gegevens van de functie voor toewijzing informatie over de configuratie van uw software, zoals het besturingssysteem en versie, IP-adres, DNS-naam en de Werkstationnaam van het. Microsoft verzamelt geen namen, adressen of andere contactgegevens.

Zie voor meer informatie over het verzamelen van gegevens en het gebruik, de [privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="performance-counters-enabled"></a>Prestatiemeteritems ingeschakeld
Azure Monitor voor virtuele machines configureert u een Log Analytics-werkruimte voor het verzamelen van prestatiemeteritems die worden gebruikt door de oplossing.  De volgende tabel bevat de objecten en geconfigureerd door de oplossing voor prestatiemeteritems die elke 60 seconden worden verzameld.

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

## <a name="enable-from-the-azure-portal"></a>Inschakelen van de Azure-portal
Voor bewaking van uw Azure-VM in Azure portal, het volgende doen:

1. Selecteer in de Azure portal, **virtuele Machines**. 
2. Selecteer een VM in de lijst. 
3. Op de pagina virtuele machine in de **bewaking** sectie, selecteer **inzichten (preview)**.
4. Op de **inzichten (preview)** weergeeft, schakelt **Probeer nu**.

    ![Azure Monitor voor virtuele machines voor een virtuele machine inschakelen](./media/monitoring-vminsights-onboard/enable-vminsights-vm-portal-01.png)

5. Op de **Azure Monitor Insights voorbereiden** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement, selecteert u deze in de vervolgkeuzelijst.  De lijst worden er de standaardwerkruimte en de locatie die de virtuele machine is geïmplementeerd op in het abonnement. 

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de bewakingsgegevens van de virtuele machine, volgt u de instructies in [een Log Analytics-werkruimte maken](../log-analytics/log-analytics-quick-create-workspace.md) in een van de ondersteunde regio's eerder vermelde.   

Wanneer u bewaking inschakelt, is het duurt ongeveer 10 minuten voordat u de gezondheid van metrische gegevens voor de virtuele machine kunt weergeven. 

![Azure Monitor inschakelen voor virtuele machines verwerking van de implementatie controleren](./media/monitoring-vminsights-onboard/onboard-vminsights-vm-portal-status.png)

## <a name="enable-using-azure-policy"></a>Inschakelen met behulp van Azure Policy
Voor het inschakelen van de oplossing voor meerdere Azure-VM's die ervoor zorgt dat consistent naleving en automatische activering voor nieuwe virtuele machines die zijn ingericht, [Azure Policy](../azure-policy/azure-policy-introduction.md) wordt aanbevolen.  Met behulp van Azure Policy aan het beleid dat is opgegeven, biedt de volgende voordelen voor nieuwe virtuele machines:

* Azure Monitor inschakelen voor virtuele machines voor elke virtuele machine in het bereik zijn gedefinieerd
* Log Analytics-Agent implementeren 
* Agent voor afhankelijkheden voor toepassingsafhankelijkheden detecteren en weergeven in de kaart implementeren
* Als uw Azure VM-installatiekopie in een vooraf gedefinieerde lijst in de beleidsdefinitie controleren  
* Als uw Azure-VM is logboekregistratie naar werkruimte dan een opgegeven controleren
* Een rapport over nalevingsresultaten 
* Ondersteuning voor herstel voor niet-compatibele VM 's

Om deze te activeren voor uw tenant, is dit proces vereist:

- Een Log Analytics-werkruimte met behulp van de hier vermelde stappen hier configureren
- De initiatiefdefinitie importeren in uw tenant (op het niveau van de beheergroep of een nieuw abonnement)
- Wijs het beleid toe aan het gewenste bereik
- Bekijk de nalevingsresultaten van de

### <a name="add-the-policies-and-initiative-to-your-subscription"></a>Het beleid en de initiatief aan uw abonnement toevoegen
Voor het gebruik van het beleid, kunt u een opgegeven PowerShell-script - [toevoegen VMInsightsPolicy.ps1](https://www.powershellgallery.com/packages/Add-VMInsightsPolicy/1.2) beschikbaar is via de galerie met Azure PowerShell om deze taak te voltooien. Het script wordt het beleid en een initiatief toegevoegd aan uw abonnement.  Voer de volgende stappen uit om Azure-beleid configureren in uw abonnement. 

1. De PowerShell-script downloaden naar uw lokale bestandssysteem.

2. Gebruik de volgende PowerShell-opdracht in de map om toe te voegen, beleidsregels. Het script ondersteunt de volgende optionele parameters: 

    ```powershell
    -UseLocalPolicies [<SwitchParameter>]
      <Optional> Load the policies from a local folder instead of https://raw.githubusercontent.com/dougbrad/OnBoardVMInsights/Policy/Policy/

    -SubscriptionId <String>
      <Optional> SubscriptionId to add the Policies/Initiatives to
    -ManagementGroupId <String>
      <Optional> Management Group Id to add the Policies/Initiatives to

    -Approve [<SwitchParameter>]
      <Optional> Gives the approval to add the Policies/Initiatives without any prompt
    ```  

    >[!NOTE]
    >Opmerking: Als u van plan bent het initiatief/beleid toewijzen aan meerdere abonnementen, de definities moeten worden opgeslagen in de beheergroep met de abonnementen die u het beleid toewijst. Daarom moet u de parameter - ManagementGroupID gebruiken.
    >
   
    Voorbeeld zonder parameters:  `.\Add-VMInsightsPolicy.ps1`

### <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken
Nadat u de `Add-VMInsightsPolicy.ps1` PowerShell-script, de volgende initiatief en het beleid worden toegevoegd:

* **Implementeren van Log Analytics-Agent voor Windows-VM's - Preview**
* **Implementeren van Log Analytics-Agent voor Linux-VM's - Preview**
* **Agent voor afhankelijkheden voor virtuele machines van Windows - voorbeeld implementeren**
* **Agent voor afhankelijkheden voor virtuele machines van Linux - voorbeeld implementeren**
* **Audit Log Analytics-Agent implementatie - VM Image (OS) niet-vermelde - Preview**
* **Audit afhankelijkheid Agent implementatie - VM Image (OS) niet-vermelde - Preview**

De volgende initiatiefparameter is toegevoegd:

- **Meld u Analytice werkruimte** (u moet de ResourceID van de werkruimte opgeven als het toepassen van een toewijzing met PowerShell of CLI)

    Voor virtuele machines als niet-compatibel van het controlebeleid gevonden **VM's niet binnen het bereik van de OS...**  de criteria van het implementatiebeleid voor de bevat alleen virtuele machines die zijn geïmplementeerd vanuit een bekende Azure VM-installatiekopieën. Raadpleeg de documentatie als het besturingssysteem van de virtuele machine of niet wordt ondersteund.  Als dat niet het geval is, vervolgens moet u de implementatie dupliceerbeleid en update/wijzigen, zodat de afbeelding in het bereik.

De volgende zelfstandige optioneel beleid is toegevoegd:

- **Virtuele machine is geconfigureerd voor niet-overeenkomende Log Analytics-werkruimte - Preview**

    Dit kan worden gebruikt voor het identificeren van virtuele machines die al zijn geconfigureerd met de [Log Analytics VM-extensie](../virtual-machines/extensions/oms-windows.md), maar zijn geconfigureerd met een andere werkruimte dan bedoeld (zoals aangegeven door de beleidstoewijzing). Dit heeft een parameter voor de werkruimte-id.

Met deze eerste release kunt u alleen de beleidstoewijzing maken vanuit Azure portal. Zie voor meer informatie over hoe u deze stappen hebt voltooid, [een beleidstoewijzing maken vanuit Azure portal](../azure-policy/assign-policy-definition.md).

## <a name="enable-with-powershell"></a>Inschakelen met PowerShell
Als u wilt inschakelen voor virtuele machines Azure Monitor voor meerdere VM's of VM-schaalsets, kunt u een opgegeven PowerShell-script - [installeren VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0) beschikbaar is via de galerie met Azure PowerShell om deze taak te voltooien.  Met dit script wordt herhaald voor elke virtuele machine en de VM-schaalset in uw abonnement, in de scope die is opgegeven door *ResourceGroup*, of op een enkele virtuele machine of schaalset opgegeven door *naam*.  Voor elke virtuele machine of virtuele machine controleert schaalset het script of als de VM-extensie is al geïnstalleerd en niet geprobeerd deze opnieuw installeren.  Anders wordt de voortgezet voor het installeren van de Log Analytics en afhankelijkheid Agent VM-extensies.   

Dit script moet Azure PowerShell-moduleversie 5.7.0 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

Als u help-informatie over het script, kunt u uitvoeren `Get-Help` voor een lijst van argument details en een voorbeeld.   

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and Dependency Agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VM's and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VM's/VM Scale Sets that will apply to and let you confirm to continue.
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
        If using PolicyAssignmentName parameter, subscription that VM's are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong to

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VM's to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VM's/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Suported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
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

    Install for all VM's in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to ReInstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to ReInstall (move to a new workspace)
```

Het volgende voorbeeld ziet u met behulp van de PowerShell-opdrachten in de map Azure Monitor inschakelen voor virtuele machines en begrijpen van de verwachte uitvoer:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VM's or VM ScaleSets matching criteria specified

VM's or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency Agent extensions on above 2 VM's or VM Scale Sets.
VM's in a non-running state will be skipped.
Extension will not be re-installed if already installed. Use -ReInstall if desired, for example to update workspace

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

## <a name="enable-for-hybrid-environment"></a>Inschakelen voor een hybride omgeving
Deze sectie wordt uitgelegd hoe virtuele machines activeren of fysieke computers die worden gehost in uw datacenter of andere cloudomgeving voor het bewaken van Azure monitor voor virtuele machines.  

De gegevens zelf wordt niet verzonden door de Azure-Monitor voor agent voor afhankelijkheden van virtuele machines toewijzen, en hoeven er geen wijzigingen in de firewalls en poorten. De gegevens in kaart worden altijd verzonden door de Log Analytics-agent naar de service Azure Monitor, hetzij rechtstreeks of via de [OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md) als uw IT-beveiligingsbeleid niet toestaat computers op het netwerk verbinding maken met Internet.

Bekijk de vereisten en methoden voor het implementeren van de [Log Analytics Linux en Windows-agent](../log-analytics/log-analytics-concept-hybrid.md).

Samengevatte stappen:

1. Log Analytics-Agent voor Windows of Linux installeren
2. Azure Monitor voor virtuele machines kaart afhankelijkheidsagent installeren
3. Inschakelen van verzamelen van prestatiemeteritems
4. Onboarding van Azure Monitor voor virtuele machines

### <a name="install-the-dependency-agent-on-windows"></a>De afhankelijkheidsagent installeren op Windows 
De agent voor afhankelijkheden kan handmatig worden geïnstalleerd op Windows-computers door uit te voeren `InstallDependencyAgent-Windows.exe`. Als u dit uitvoerbare bestand zonder opties uitvoert, begint deze een setup-wizard die u volgen kunt om interactief installeren.  

>[!NOTE]
>Er zijn beheerdersbevoegdheden vereist om de agent te installeren of verwijderen.

De volgende tabel ziet u de specifieke parameters die worden ondersteund door het installatieprogramma voor de agent vanaf de opdrachtregel.  

| Parameter | Beschrijving |
|:--|:--|
| /? | Retourneert een lijst van de opdrachtregelopties. |
| /S | Voer een installatie op de achtergrond zonder tussenkomst van de gebruiker. |

Bijvoorbeeld, om uit te voeren van het installatieprogramma met de `/?` parameter, type `InstallDependencyAgent-Windows.exe /?`

Bestanden voor de agent voor Windows-afhankelijkheden zijn geïnstalleerd in `C:\Program Files\Microsoft Dependency Agent` standaard.  Als de agent voor afhankelijkheden niet kunt starten nadat setup voltooid is, controleert u de logboeken voor uitgebreide foutgegevens. De logboekmap is `%Programfiles%\Microsoft Dependency Agent\logs`. 

### <a name="install-the-dependency-agent-on-linux"></a>De afhankelijkheidsagent installeren in Linux
De agent voor afhankelijkheden is geïnstalleerd op Linux-servers van `InstallDependencyAgent-Linux64.bin`, een shell-script met een zichzelf uitpakkend binair bestand. U kunt het bestand uitvoeren met behulp van `sh` of toe te voegen uitvoermachtigingen naar het bestand zelf.

>[!NOTE]
> Toegang tot de hoofdmap is vereist om de agent te installeren of configureren.
> 

| Parameter | Beschrijving |
|:--|:--|
| -help | Een lijst met de opdrachtregelopties ophalen. |
| -s | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |
| --controleren | Controleer machtigingen en het besturingssysteem, maar installeer niet de agent. |

Bijvoorbeeld, om uit te voeren van het installatieprogramma met de `-help` parameter, type `InstallDependencyAgent-Linux64.bin -help`.

De agent voor Linux-afhankelijkheden als root installeren met de volgende opdracht `sh InstallDependencyAgent-Linux64.bin`
    
Als de agent voor afhankelijkheden niet kan worden gestart, controleert u de logboeken voor uitgebreide foutgegevens. Op Linux-agents, de logboekmap is `/var/opt/microsoft/dependency-agent/log`.

Bestanden voor de agent voor afhankelijkheden worden geplaatst in de volgende mappen:

| Bestanden | Locatie |
|:--|:--|
| Kernbestanden | /opt/microsoft/dependency-agent |
| Logboekbestanden | /var/opt/microsoft/dependency-agent/log |
| Configuratiebestanden | /etc/opt/microsoft/dependency-agent/config |
| Uitvoerbare bestanden van de service | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binaire opslagbestanden | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Inschakelen van prestatiemeteritems
Als de Log Analytics-werkruimte waarnaar wordt verwezen door de oplossing niet is geconfigureerd voor het verzamelen van al de prestatiemeteritems die is vereist voor de oplossing, moet deze worden ingeschakeld. Dit zo beschreven handmatig kan worden bewerkstelligd [hier](../log-analytics/log-analytics-data-sources-performance-counters.md), of door te downloaden en uitvoeren van een PowerShell-script beschikbaar is via [Azure Powershell Gallery](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1).
 
### <a name="onboard-azure-monitor-for-vms"></a>Onboarding van Azure Monitor voor virtuele machines
Deze methode bevat een JSON-sjabloon waarmee de configuratie voor het inschakelen van de onderdelen van de oplossing aan uw Log Analytics-werkruimte.  

Als u niet bekend met het concept bent van het implementeren van resources met behulp van een sjabloon, Zie:
* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) 

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

2. Sla dit bestand als **installsolutionsforvminsights.json** naar een lokale map.
3. Bewerk de waarden voor **WorkspaceName**, **ResourceGroupName**, en **WorkspaceLocation**.  De waarde voor **WorkspaceName** is de is de volledige resource-ID van uw Log Analytics-werkruimte, met inbegrip van de naam van de werkruimte en de waarde voor **WorkspaceLocation** de regio is de werkruimte is gedefinieerd in.
4. U bent klaar om te implementeren met deze sjabloon met de volgende PowerShell-opdracht:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    Wijzigen van de configuratie kan een paar minuten duren. Wanneer deze voltooid, wordt er een bericht weergegeven dat vergelijkbaar is met het volgende en het resultaat bevat:

    ```powershell
    provisioningState       : Succeeded
    ```
Wanneer u bewaking inschakelt, is het duurt ongeveer 10 minuten voordat u de status en metrische gegevens voor de hybride-computer kunt weergeven. 

## <a name="next-steps"></a>Volgende stappen

Met de bewaking ingeschakeld voor uw virtuele machine, is deze informatie beschikbaar voor analyse met Azure Monitor voor virtuele machines.  Zie voor meer informatie over het gebruik van de Health-functie, [weergave Azure Monitor voor de gezondheid van virtuele machines](monitoring-vminsights-health.md), of als u afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](monitoring-vminsights-maps.md).  