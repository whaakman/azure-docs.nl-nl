---
title: Azure Monitor inschakelen voor een overzicht van virtuele machines (preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u implementeren en configureren van Azure Monitor voor VM's en de vereiste systeemvereisten.
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
ms.openlocfilehash: 4eb07b29f68c5f495bdbe5e23f5b226480b66661
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524068"
---
# <a name="enable-azure-monitor-for-vms-preview-overview"></a>Azure Monitor inschakelen voor virtuele machines (preview)-overzicht

In dit artikel biedt een overzicht van de beschikbare opties voor het instellen van Azure Monitor voor virtuele machines de status en prestaties, bewaken en detecteren van afhankelijkheden voor toepassingen die worden uitgevoerd op virtuele Azure-machines en virtuele-machineschaalsets, on-premises virtuele machines of virtuele machines die worden gehost in een andere cloudomgeving.  

U kunt Azure Monitor inschakelen voor virtuele machines met behulp van een van de volgende methoden:

* Inschakelen van een enkele virtuele Azure-machine of virtuele-machineschaalset door te selecteren **inzichten (preview)** rechtstreeks vanuit de schaal van de virtuele machine of virtuele machine instellen.
* Inschakelen van twee of meer virtuele Azure-machines en virtuele-machineschaalsets ingesteld met behulp van Azure Policy. Via deze methode worden de vereiste afhankelijkheden van de bestaande en nieuwe virtuele machines en schaalsets geïnstalleerd en geconfigureerd. Niet-compatibele virtuele machines en schaalsets worden gemeld, zodat u beslissen kunt of u ze wilt inschakelen en hoe u wilt dat ze te herstellen.
* Inschakelen van twee of meer virtuele Azure-machines of virtuele machine-schaalsets voor een opgegeven abonnement of resourcegroep met behulp van PowerShell.
* Inschakelen voor het bewaken van virtuele machines of fysieke computers die worden gehost in uw bedrijfsnetwerk of andere cloudomgeving.

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

Als u bewaking voor een enkele Azure-virtuele machine of virtuele-machineschaalset in Azure portal inschakelen bent, kunt u een werkruimte maken tijdens dit proces.

Voor het scenario voor op schaal met Azure Policy, Azure PowerShell of Azure Resource Manager-sjablonen, moet uw werkruimte voor logboekanalyse eerst het volgende geconfigureerd:

* Installeer de ServiceMap en InfrastructureInsights oplossingen. U kunt deze installatie voltooien met behulp van een Azure Resource Manager-sjabloon die opgegeven of met behulp van de **werkruimte configureren** optie gevonden op de **aan de slag** tabblad.
* Configureer de werkruimte voor logboekanalyse voor het verzamelen van prestatiemeteritems.

Voor het configureren van uw werkruimte voor het scenario op schaal, kunt u configureren met behulp van een van de volgende methoden:

* [Azure PowerShell](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace)
* Uit de **werkruimte configureren** optie op de Azure-Monitor voor virtuele machines [Beleidsdekking](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) pagina

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende tabel bevat de Windows- en Linux-besturingssystemen die worden ondersteund met Azure Monitor voor virtuele machines. Een volledige lijst die details van de primaire en secundaire versie voor Linux-besturingssysteem en de ondersteunde kernelversies wordt verderop in deze sectie.

|Versie van het besturingssysteem |Prestaties |Kaarten |Gezondheidszorg |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X | |
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

In een hybride omgeving, kunt u downloaden en installeren van de agent voor afhankelijkheden op twee manieren: handmatig of via een methode geautomatiseerde implementatie voor virtuele machines die worden gehost buiten Azure.

De volgende tabel beschrijft de verbonden bronnen die ondersteuning biedt voor de kaart-functie in een hybride omgeving.

| Verbonden bron | Ondersteund | Description |
|:--|:--|:--|
| Windows-agents | Ja | Naast de [Log Analytics-agent voor Windows](../../azure-monitor/platform/log-analytics-agent.md), Windows-agents moeten de agent voor Microsoft Dependency. Zie voor een volledige lijst van de versies van besturingssystemen, [ondersteunde besturingssystemen](#supported-operating-systems). |
| Linux-agents | Ja | Naast de [Log Analytics-agent voor Linux](../../azure-monitor/platform/log-analytics-agent.md), Linux-agents moeten de agent voor Microsoft Dependency. Zie voor een volledige lijst van de versies van besturingssystemen, [ondersteunde besturingssystemen](#supported-operating-systems). |
| Beheergroep System Center Operations Manager | Nee | |

De agent voor afhankelijkheden kan worden gedownload van de volgende locaties:

| File | OS | Versie | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.8.1 | 622C99924385CBF539988D759BCFDC9146BB157E7D577C997CDD2674E27E08DD |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.8.1 | 3037934A5D3FB7911D5840A9744AE9F980F87F620A7F7B407F05E276FE7AE4A8 |

## <a name="role-based-access-control"></a>Role-based access control

Als u wilt inschakelen en toegang tot de functies in Azure Monitor voor virtuele machines, moet u de volgende rollen voor toegang worden toegewezen:

- Als u wilt inschakelen, moet u de *Inzender van Log Analytics* rol.

- Als u wilt weergeven van prestaties, status, status en gegevens, hebt u de *Monitoring Reader* rol voor de virtuele machine van Azure. De Log Analytics-werkruimte moet worden geconfigureerd voor Azure Monitor voor virtuele machines.

Zie voor meer informatie over het beheren van toegang tot een Log Analytics-werkruimte [werkruimten beheren](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms-preview"></a>Azure Monitor inschakelen voor virtuele machines (preview)

U kunt Azure Monitor inschakelen voor virtuele machines met behulp van een van de volgende methoden die worden beschreven in de volgende tabel.

| Implementatiestatus | Methode | Description |
|------------------|--------|-------------|
| Één schaalset van virtuele Azure-machine of virtuele machine | [Rechtstreeks vanuit de virtuele machine](vminsights-enable-single-vm.md) | U kunt een enkel Azure-machine inschakelen door te selecteren **inzichten (preview)** rechtstreeks vanuit de schaal van de virtuele machine of virtuele machine instellen. |
| Meerdere virtuele Azure-machines of virtuele-machineschaalsets | [Azure Policy](vminsights-enable-at-scale-policy.md) | U kunt meerdere Azure-VM's met behulp van Azure Policy en beschikbare beleidsdefinities inschakelen. |
| Meerdere virtuele Azure-machines of virtuele-machineschaalsets | [Azure PowerShell of Azure Resource Manager-sjablonen](vminsights-enable-at-scale-powershell.md) | U kunt meerdere schaalsets voor virtuele Azure-machines of virtuele machine inschakelen voor een opgegeven abonnement of resourcegroep groep met behulp van Azure PowerShell of Azure Resource Manager-sjablonen. |
| Hybride cloud | [Inschakelen voor een hybride omgeving](vminsights-enable-hybrid-cloud.md) | U kunt implementeren op virtuele machines of fysieke computers die worden gehost in uw datacenter of andere cloudomgevingen. |

## <a name="performance-counters-enabled"></a>Prestatiemeteritems ingeschakeld

Azure Monitor voor virtuele machines configureert u een werkruimte voor logboekanalyse voor het verzamelen van de prestatiemeteritems die worden gebruikt. De volgende tabel bevat de objecten en prestatiemeteritems die elke 60 seconden worden verzameld.

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
