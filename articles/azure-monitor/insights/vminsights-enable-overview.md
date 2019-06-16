---
title: Azure Monitor inschakelen voor een overzicht van virtuele machines (preview) | Microsoft Docs
description: Informatie over het implementeren en configureren van Azure Monitor voor virtuele machines. Informatie over de systeemvereisten.
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
ms.date: 05/22/2019
ms.author: magoedte
ms.openlocfilehash: 3f93318dedb8a4667d32bcc97eb6a697ccebfcc4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122581"
---
# <a name="enable-azure-monitor-for-vms-preview-overview"></a>Azure Monitor inschakelen voor virtuele machines (preview)-overzicht

Dit artikel bevat een overzicht van de beschikbare opties voor Azure Monitor instellen voor VM's. Azure Monitor gebruiken voor virtuele machines voor het controleprogramma wordt de status en prestaties. Detecteren van afhankelijkheden voor toepassingen die worden uitgevoerd op Azure virtual machines (VM's) en virtuele-machineschaalset wordt ingesteld, on-premises virtuele machines of virtuele machines die worden gehost in een andere cloudomgeving.  

Azure Monitor instellen voor virtuele machines:

* Inschakelen van één virtuele Azure-machine of virtuele machine schaalset door te selecteren **inzichten (preview)** rechtstreeks vanuit de schaal van de virtuele machine of virtuele machine instellen.
* Inschakelen van twee of meer virtuele Azure-machines en virtuele-machineschaalsets ingesteld met behulp van Azure Policy. Deze methode zorgt ervoor dat op bestaande en nieuwe virtuele machines en schaalsets, de vereiste afhankelijkheden zijn geïnstalleerd en geconfigureerd. Niet-compatibele virtuele machines en schaalsets worden gemeld, zodat u kunt vervolgens beslissen of u ze wilt inschakelen en ze te herstellen.
* Inschakelen van twee of meer virtuele Azure-machines of virtuele machine-schaalsets voor een opgegeven abonnement of resourcegroep met behulp van PowerShell.
* Azure Monitor voor virtuele machines voor het bewaken van virtuele machines of fysieke computers die worden gehost in uw bedrijfsnetwerk of andere cloudomgeving inschakelen.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u weet dat de informatie in de volgende secties.

### <a name="log-analytics"></a>Log Analytics

Azure Monitor voor virtuele machines ondersteunt een Log Analytics-werkruimte in de volgende regio's:

- US - west-centraal
- VS-West 2<sup>1</sup>
- US - oost
- Canada centraal<sup>1</sup>
- UK-Zuid<sup>1</sup>
- Europa -west
- Zuidoost-Azië<sup>1</sup>

<sup>1</sup> deze regio momenteel de status-functie van Azure Monitor biedt geen ondersteuning voor virtuele machines.

>[!NOTE]
>U kunt Azure-VM's van andere regio's implementeren. Deze VM's zijn niet beperkt tot de regio's ondersteund door de Log Analytics-werkruimte.
>

Als u een werkruimte hebt, kunt u er een maken met behulp van een van deze resources:
* [De Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

U kunt ook een werkruimte maken terwijl u bent inschakelen van de controle voor één virtuele Azure-machine of virtuele machine schaalset in Azure portal.

Als u een scenario op schaal wilt instellen die gebruikmaakt van Azure Policy, Azure PowerShell of Azure Resource Manager-sjablonen, in uw Log Analytics-werkruimte:

* Installeer de ServiceMap en InfrastructureInsights oplossingen. U kunt deze installatie uitvoeren met behulp van een opgegeven Azure Resource Manager-sjabloon. Of op de **aan de slag** tabblad **werkruimte configureren**.
* Configureer de werkruimte voor logboekanalyse voor het verzamelen van prestatiemeteritems.

Voor het configureren van uw werkruimte voor het scenario op schaal, moet u een van de volgende methoden gebruiken:

* Gebruik [Azure PowerShell](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace).
* Op de Azure-Monitor voor virtuele machines [ **Beleidsdekking** ](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) weergeeft, schakelt **werkruimte configureren**. 

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende tabel bevat de besturingssystemen Windows en Linux die ondersteuning biedt voor Azure Monitor voor virtuele machines. Later in deze sectie vindt u een volledige lijst met gegevens over de primaire en secundaire Linux-besturingssysteem release en versies van de kernel wordt ondersteund.

|Versie van het besturingssysteem |Prestaties |Kaarten |Status |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X | X |
|WindowsServer 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | X |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| X |
|Ubuntu 14.04, 16.04, 18.04 | X | X | X |
|CentOS Linux, 6, 7 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 11, 12 | X | X | X |
|Debian 8, 9.4 | X<sup>1</sup> | | X |

<sup>1</sup> prestaties van de functie van Azure Monitor voor virtuele machines is alleen beschikbaar via Azure Monitor. Het is niet beschikbaar rechtstreeks vanuit het linkerdeelvenster van de Azure-VM.

>[!NOTE]
>In het Linux-besturingssysteem:
> - Alleen standaard- en SMP Linux kernelversies worden ondersteund.
> - Niet-standaard kernel versies, zoals fysieke Address Extension (PAE) en Xen, worden niet ondersteund voor een Linux-distributie. Bijvoorbeeld, een systeem met de tekenreeks voor de release van *2.6.16.21-0.8-xen* wordt niet ondersteund.
> - Aangepaste kernels, met inbegrip van hercompileringen van standard kernels, worden niet ondersteund.
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

#### <a name="centosplus"></a>CentOSPlus
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

De kaart-functie in Azure Monitor voor virtuele machines worden de gegevens uit de agent voor Microsoft Dependency opgehaald. De agent voor afhankelijkheden, is afhankelijk van de Log Analytics-agent voor de verbinding met Log Analytics. Uw systeem moet daarom de Log Analytics-agent geïnstalleerd en geconfigureerd met de agent voor afhankelijkheden hebben.

Of u Azure Monitor voor virtuele machines voor een enkele Azure-VM inschakelen of u de methode van de implementatie op schaal gebruiken, gebruikt u de afhankelijkheid van Azure VM agent-extensie voor het installeren van de agent als onderdeel van de ervaring.

U kunt in een hybride omgeving, downloaden en handmatig installeren van de agent voor afhankelijkheden. Als uw VM's buiten Azure worden gehost, moet u een methode geautomatiseerde implementatie gebruiken.

De volgende tabel beschrijft de verbonden bronnen die ondersteuning biedt voor de kaart-functie in een hybride omgeving.

| Verbonden bron | Ondersteund | Description |
|:--|:--|:--|
| Windows-agents | Ja | Samen met de [Log Analytics-agent voor Windows](../../azure-monitor/platform/log-analytics-agent.md), Windows-agents moeten de agent voor afhankelijkheden. Zie voor meer informatie, [ondersteunde besturingssystemen](#supported-operating-systems). |
| Linux-agents | Ja | Samen met de [Log Analytics-agent voor Linux](../../azure-monitor/platform/log-analytics-agent.md), Linux-agents moeten de agent voor afhankelijkheden. Zie voor meer informatie, [ondersteunde besturingssystemen](#supported-operating-systems). |
| Beheergroep System Center Operations Manager | Nee | |

U kunt de agent voor afhankelijkheden downloaden van deze locaties:

| File | OS | Versie | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.8.1 | 622C99924385CBF539988D759BCFDC9146BB157E7D577C997CDD2674E27E08DD |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.8.1 | 3037934A5D3FB7911D5840A9744AE9F980F87F620A7F7B407F05E276FE7AE4A8 |

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Als u wilt inschakelen en toegang tot de functies in Azure Monitor voor virtuele machines, moet u de *Inzender van Log Analytics* rol. Als u wilt weergeven van prestaties, status, status en gegevens, hebt u de *controlelezer* rol voor de virtuele machine van Azure. De Log Analytics-werkruimte moet worden geconfigureerd voor Azure Monitor voor virtuele machines.

Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms-preview"></a>Azure Monitor inschakelen voor virtuele machines (preview)

Azure Monitor voor VM's inschakelen met behulp van een van de methoden die worden beschreven in deze tabel:

| Implementatiestatus | Methode | Description |
|------------------|--------|-------------|
| Één schaalset van virtuele Azure-machine of virtuele machine | [Inschakelen van de virtuele machine](vminsights-enable-single-vm.md) | U kunt een enkele Azure-virtuele machine inschakelen door te selecteren **inzichten (preview)** rechtstreeks vanuit de schaal van de virtuele machine of virtuele machine instellen. |
| Meerdere virtuele Azure-machines of virtuele-machineschaalsets | [Schakel via Azure Policy](vminsights-enable-at-scale-policy.md) | U kunt meerdere Azure-VM's inschakelen met behulp van Azure Policy en beschikbare beleidsdefinities. |
| Meerdere virtuele Azure-machines of virtuele-machineschaalsets | [Schakel via Azure PowerShell of Azure Resource Manager-sjablonen](vminsights-enable-at-scale-powershell.md) | U kunt meerdere schaalsets voor virtuele Azure-machines of virtuele machine inschakelen voor een opgegeven abonnement of resourcegroep met behulp van Azure PowerShell of Azure Resource Manager-sjablonen. |
| Hybride cloud | [Inschakelen voor de hybride omgeving](vminsights-enable-hybrid-cloud.md) | U kunt implementeren op virtuele machines of fysieke computers die worden gehost in uw datacenter of andere cloudomgevingen. |

## <a name="performance-counters-enabled"></a>Prestatiemeteritems ingeschakeld 

Azure Monitor voor virtuele machines configureert u een werkruimte voor logboekanalyse voor het verzamelen van de prestatiemeteritems die worden gebruikt. De volgende tabellen worden de prestatiemeteritems die elke 60 seconden worden verzameld en objecten.

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

Microsoft verzamelt automatisch gebruiks- en -gegevens door uw gebruik van de Azure Monitor-service. Microsoft gebruikt deze gegevens voor het verbeteren van de kwaliteit, beveiliging en integriteit van de service. 

Om nauwkeurige en efficiënte mogelijkheden voor probleemoplossing, bevat de functie voor toewijzing gegevens over de configuratie van uw software. De gegevens bevat informatie zoals het besturingssysteem en versie, IP-adres, DNS-naam en de Werkstationnaam van het. Microsoft biedt geen namen, adressen of andere contactgegevens verzameld.

Zie voor meer informatie over het verzamelen van gegevens en het gebruik, de [privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

Nu dat u de bewaking voor uw virtuele machine hebt ingeschakeld, is controlegegevens beschikbaar voor analyse in Azure Monitor voor virtuele machines.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van de Health-functie, [weergave Azure Monitor voor virtuele machines Health](vminsights-health.md). Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).
