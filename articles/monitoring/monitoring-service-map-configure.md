---
title: Serviceoverzicht configureren in Azure | Microsoft Docs
description: Serviceoverzicht is een oplossing in Azure die automatisch toepassingsonderdelen op Windows- en Linux-systemen detecteert en de communicatie tussen services toewijst. Dit artikel bevat informatie voor het implementeren van Serviceoverzicht in uw omgeving en het gebruik hiervan in een verscheidenheid aan scenario's.
services: monitoring
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: daseidma;bwren
ms.openlocfilehash: a68c35ba2f740720e3d7940d6fafa2dcfe183589
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47064370"
---
# <a name="configure-service-map-in-azure"></a>Serviceoverzicht configureren in Azure
Serviceoverzicht ontdekt automatisch toepassingsonderdelen op Windows- en Linux-systemen en wijst de communicatie tussen services toe. U kunt deze gebruiken om weer te geven van uw servers beschouwen zoals u ze--onderling verbonden systemen die kritieke services verlenen. Servicetoewijzing toont verbindingen tussen servers, processen en poorten in alle via TCP verbonden architectuur zonder configuratie vereist, dan een agent geïnstalleerd.

Dit artikel beschrijft de details van het Serviceoverzicht en onboarding-agents configureren. Zie voor meer informatie over het gebruik van Serviceoverzicht [gebruik van de oplossing Serviceoverzicht in Azure]( monitoring-service-map.md).

## <a name="supported-azure-regions"></a>Ondersteunde Azure-regio 's
Serviceoverzicht is momenteel beschikbaar in de volgende Azure-regio's:
- US - oost
- Europa -west
- US - west-centraal
- Azië - zuidoost

## <a name="supported-windows-operating-systems"></a>Ondersteunde Windows-besturingssystemen
De volgende sectie worden de ondersteunde besturingssystemen voor de agent voor afhankelijkheden op Windows. 

>[!NOTE]
>Service Map ondersteunt alleen 64-bits platforms.
>

### <a name="windows-server"></a>Windows Server
- WindowsServer 2016 1803
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows-bureaublad
- Windows 10-1803
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

## <a name="supported-linux-operating-systems"></a>Ondersteunde Linux-besturingssystemen
De volgende sectie worden de ondersteunde besturingssystemen voor de agent voor afhankelijkheden op Red Hat Enterprise Linux, CentOS Linux en Oracle Linux (met RHEL Kernel).  

- Alleen standaard- en SMP Linux kernelversies worden ondersteund.
- Niet-standaard kernelversies, zoals PAE en Xen, worden voor geen enkele Linux-distributie ondersteund. Bijvoorbeeld, wordt een systeem met de tekenreeks voor de release van '2.6.16.21-0.8-xen' niet ondersteund.
- Aangepaste kernels, met inbegrip van hercompilaties van standaardkernels, worden niet ondersteund.
- CentOSPlus-kernel wordt niet ondersteund.
- Oracle Unbreakable Enterprise Kernel (UEK) wordt verderop in dit artikel beschreven.

### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |

### <a name="red-hat-linux-6"></a>Red Hat Linux 6

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

### <a name="ubuntu-server"></a>Ubuntu Server

| Versie van het besturingssysteem | Kernelversie |
|:--|:--|
| Ubuntu 18.04 | kernel 4.15. * |
| Ubuntu 16.04.3 | kernel 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

### <a name="oracle-enterprise-linux-6-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 6 met Unbreakable Enterprise Kernel
| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

### <a name="oracle-enterprise-linux-5-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 5 met Unbreakable Enterprise Kernel

| Versie van het besturingssysteem | Kernelversie
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

## <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Versie van het besturingssysteem | Kernelversie
|:--|:--|
|12 SP2 | 4.4. * |
|12 SP3 | 4.4. * |

## <a name="dependency-agent-downloads"></a>Agent voor afhankelijkheden downloaden

| File | OS | Versie | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.1 | 55030ABF553693D8B5112569FB2F97D7C54B66E9990014FC8CC43EFB70DE56C6 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.1 | 43C75EF0D34471A0CBCE5E396FFEEF4329C9B5517266108FA5D6131A353D29FE |

## <a name="connected-sources"></a>Verbonden bronnen
Serviceoverzicht worden de gegevens uit de agent voor Microsoft Dependency opgehaald. De agent voor afhankelijkheden, is afhankelijk van de Log Analytics-agent voor de verbindingen met Log Analytics. Dit betekent dat een server de Log Analytics-agent geïnstalleerd en geconfigureerd met de agent voor afhankelijkheden moet hebben.  De volgende tabel beschrijft de verbonden bronnen die ondersteuning biedt voor de oplossing Serviceoverzicht.

| Verbonden bron | Ondersteund | Beschrijving |
|:--|:--|:--|
| Windows-agents | Ja | Serviceoverzicht analyseert en verzamelt gegevens van Windows-computers. <br><br>Naast de [Log Analytics-agent voor Windows](../log-analytics/log-analytics-concept-hybrid.md), Windows-agents moeten de agent voor Microsoft Dependency. Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Linux-agents | Ja | Serviceoverzicht analyseert en verzamelt gegevens van Linux-computers. <br><br>Naast de [Log Analytics-agent voor Linux](../log-analytics/log-analytics-concept-hybrid.md), Linux-agents moeten de agent voor Microsoft Dependency. Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Beheergroep System Center Operations Manager | Ja | Serviceoverzicht analyseert en verzamelt gegevens van Windows en Linux-agents in een verbonden [System Center Operations Manager-beheergroep](../log-analytics/log-analytics-om-agents.md). <br><br>Er is een directe verbinding van de System Center Operations Manager-agentcomputer naar Log Analytics vereist. |
| Azure Storage-account | Nee | Serviceoverzicht verzamelt gegevens van agentcomputers, dus er zijn geen gegevens te verzamelen over Azure Storage. |

Op Windows, Microsoft Monitoring Agent (MMA) wordt gebruikt door zowel de System Center Operations Manager en de Log Analytics om te verzamelen en verzenden door gegevens te controleren. (Deze agent heet de System Center Operations Manager-agent, OMS-Agent, Log Analytics-agent, MMA of Direct Agent, afhankelijk van de context.) System Center Operations Manager en de Log Analytics bieden verschillende kant-en-klare versies van de MMA. Deze versies kunnen beide rapporteren aan System Center Operations Manager, aan Log Analytics of aan beide.  

Op Linux, de Log Analytics-agent voor Linux verzamelt en verzendt gegevens naar Log Analytics te controleren. U kunt Service Map gebruiken op servers met Log Analytics-agents die rechtstreeks verbonden met de service of die rapporteren aan een Operations Manager-beheergroep is geïntegreerd met Log Analytics.  

In dit artikel, verwijzen we naar alle agents of Linux- of Windows met een beheergroep van System Center Operations Manager of rechtstreeks met Log Analytics, als verbonden de *Log Analytics-agent*. 

De Serviceoverzicht-agent wordt niet verzonden gegevens zelf, en hoeven er geen wijzigingen in de firewalls en poorten. De gegevens in het Serviceoverzicht worden altijd verzonden door de Log Analytics-agent naar de service Log Analytics, rechtstreeks of via de OMS-Gateway.

![Serviceoverzicht agents](media/monitoring-service-map/agents.png)

Als u een System Center Operations Manager-klant bent met een beheergroep die is verbonden met Log Analytics:

- Als de System Center Operations Manager-agents toegang heeft tot het Internet verbinding maken met Log Analytics, is geen aanvullende configuratie vereist.  
- Als de System Center Operations Manager-agents geen toegang Log Analytics via Internet tot, moet u de OMS-Gateway om te werken met System Center Operations Manager configureren.
  
Als uw Windows- of Linux-computers kunnen niet rechtstreeks verbinding met de service maken, moet u de Log Analytics-agent te koppelen aan Log Analytics met behulp van de OMS-Gateway configureren. Zie voor meer informatie over het implementeren en configureren van de OMS-Gateway [verbinding maken met computers zonder toegang tot het Internet met behulp van de OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md).  

### <a name="management-packs"></a>Management packs
Als Service Map in een Log Analytics-werkruimte is geactiveerd, wordt een 300 KB managementpack wordt doorgestuurd naar de Windows-servers in deze werkruimte. Als u System Center Operations Manager-agents in een [verbonden beheergroep](../log-analytics/log-analytics-om-agents.md), het Serviceoverzicht managementpack van System Center Operations Manager is geïmplementeerd. 

De naam van het management pack is Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Ze worden geschreven naar %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\. De gegevensbron die gebruikmaakt van het managementpack is % Program files%\Microsoft bewaking Agent\Agent\Health Service State\Resources\<AutoGeneratedID > \ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="data-collection"></a>Gegevensverzameling
U kunt verwachten dat elke agent voor het verzenden van ongeveer 25 MB per dag, afhankelijk van hoe complex uw systeemafhankelijkheden zijn. Elke agent verzendt Serviceoverzicht afhankelijkheidsgegevens elke 15 seconden.  

De agent voor afhankelijkheden worden doorgaans 0,1 procent van het systeemgeheugen en 0.1 procent van de systeem-CPU verbruikt.

## <a name="diagnostic-and-usage-data"></a>Diagnostische en gebruiksgegevens
Microsoft verzamelt automatisch gebruiks- en prestatiegegevens gegevens via uw gebruik van de Service Map-service. Microsoft gebruikt deze gegevens te bieden en de kwaliteit, beveiliging en integriteit van de Service Map-service te verbeteren. Gegevens omvatten informatie over de configuratie van uw software, zoals het besturingssysteem en versie. Dit omvat ook IP-adres, de DNS-naam en de Werkstationnaam zodat de nauwkeurige en efficiënte mogelijkheden voor probleemoplossing. Er worden geen namen, adressen of andere contactgegevens verzameld.

Zie voor meer informatie over het verzamelen van gegevens en gebruik de [privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

## <a name="installation"></a>Installatie

## <a name="azure-vm-extension"></a>Azure VM-extensie
Er is een uitbreiding beschikbaar voor zowel Windows (DependencyAgentWindows)- als Linux (DependencyAgentLinux) en u kunt eenvoudig de agent voor afhankelijkheden implementeren naar uw Azure-VM's met een [Azure VM-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).  U kunt de agent voor afhankelijkheden op uw Windows en Linux-VM's met behulp van een PowerShell-script of rechtstreeks in de virtuele machine een Azure Resource Manager-sjabloon implementeren met de Azure VM-extensie.  Als u de agent met de Azure VM-extensie implementeert, worden de agents automatisch bijgewerkt naar de nieuwste versie.

Voor het implementeren van de Azure VM-extensie met PowerShell, kunt u het volgende voorbeeld:

```PowerShell
#
# Deploy the Dependency agent to every VM in a Resource Group
#

$version = "9.4"
$ExtPublisher = "Microsoft.Azure.Monitoring.DependencyAgent"
$OsExtensionMap = @{ "Windows" = "DependencyAgentWindows"; "Linux" = "DependencyAgentLinux" }
$rmgroup = "<Your Resource Group Here>"

Get-AzureRmVM -ResourceGroupName $rmgroup |
ForEach-Object {
    ""
    $name = $_.Name
    $os = $_.StorageProfile.OsDisk.OsType
    $location = $_.Location
    $vmRmGroup = $_.ResourceGroupName
    "${name}: ${os} (${location})"
    Date -Format o
    $ext = $OsExtensionMap.($os.ToString())
    $result = Set-AzureRmVMExtension -ResourceGroupName $vmRmGroup -VMName $name -Location $location `
    -Publisher $ExtPublisher -ExtensionType $ext -Name "DependencyAgent" -TypeHandlerVersion $version
    $result.IsSuccessStatusCode
}
```

Een nog eenvoudiger manier om te controleren of dat de agent voor afhankelijkheden is geïnstalleerd op uw virtuele machines is het opnemen van de agent in uw Azure Resource Manager-sjabloon.  Het volgende voorbeeld van de JSON-code kan worden toegevoegd aan de *resources* gedeelte van uw sjabloon.

```JSON
"type": "Microsoft.Compute/virtualMachines/extensions",
"name": "[concat(parameters('vmName'), '/DependencyAgent')]",
"apiVersion": "2017-03-30",
"location": "[resourceGroup().location]",
"dependsOn": [
"[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
],
"properties": {
    "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
    "type": "DependencyAgentWindows",
    "typeHandlerVersion": "9.4",
    "autoUpgradeMinorVersion": true
}

```

### <a name="install-the-dependency-agent-on-microsoft-windows"></a>De agent voor afhankelijkheden op Microsoft Windows installeren
De agent voor afhankelijkheden kan handmatig worden geïnstalleerd op Windows-computers door uit te voeren `InstallDependencyAgent-Windows.exe`. Als u dit uitvoerbare bestand zonder opties uitvoert, begint deze een setup-wizard die u volgen kunt om interactief installeren.  

>[!NOTE]
>Er zijn beheerdersbevoegdheden vereist om de agent te installeren of verwijderen.

Gebruik de volgende stappen voor het installeren van de agent voor afhankelijkheden op elke Windows-computer:

1.  De Log Analytics-agent voor de volgende op een van de methoden die worden beschreven in Windows installeren [verzamelen van gegevens in een hybride omgeving met Log Analytics-agent](../log-analytics/log-analytics-concept-hybrid.md).
2.  De Windows-agent downloaden en uitvoeren met behulp van de volgende opdracht uit: 
    
    `InstallDependencyAgent-Windows.exe`

3.  Volg de installatiewizard om de agent te installeren.
4.  Als de agent voor afhankelijkheden niet kan worden gestart, controleert u de logboeken voor uitgebreide foutgegevens. Op Windows-agents is de logboekmap %Programfiles%\Microsoft afhankelijkheid Agent\logs. 

#### <a name="windows-command-line"></a>Windows-opdrachtregel
Gebruik opties uit de volgende tabel om de agent te installeren vanaf een opdrachtregel. Een lijst van vlaggen voor de installatie wilt bekijken, voert u het installatieprogramma met behulp van de /? markeren als volgt.

    InstallDependencyAgent-Windows.exe /?

| Vlag | Beschrijving |
|:--|:--|
| /? | Een lijst met de opdrachtregelopties ophalen. |
| /S | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |

Bestanden voor de Windows-afhankelijkheidsagent worden standaard in C:\Program Files\Microsoft-Agent voor afhankelijkheden geplaatst.

### <a name="install-the-dependency-agent-on-linux"></a>De afhankelijkheidsagent installeren in Linux
De agent voor afhankelijkheden is geïnstalleerd op Linux-doelcomputers uit `InstallDependencyAgent-Linux64.bin`, een shell-script met een zichzelf uitpakkend binair bestand. U kunt het bestand uitvoeren met behulp van `sh` of toe te voegen uitvoermachtigingen naar het bestand zelf.

>[!NOTE]
> Toegang tot de hoofdmap is vereist om de agent te installeren of configureren.

Gebruik de volgende stappen voor het installeren van de agent voor afhankelijkheden op elke Linux-computer:

1.  De volgende op een van de methoden die worden beschreven in Log Analytics-agent installeren [verzamelen van gegevens in een hybride omgeving met Log Analytics-agent](../log-analytics/log-analytics-concept-hybrid.md).
2.  De agent voor Linux-afhankelijkheden als root installeren met de volgende opdracht:
    
    `sh InstallDependencyAgent-Linux64.bin`

3.  Als de agent voor afhankelijkheden niet kan worden gestart, controleert u de logboeken voor uitgebreide foutgegevens. Op Linux-agents is de logboekmap /var/opt/microsoft/dependency-agent/log.

Voor een overzicht van de vlaggen voor installatie, voer de installatie programma met behulp - vlag als volgt.

    InstallDependencyAgent-Linux64.bin -help

| Vlag | Beschrijving |
|:--|:--|
| -help | Een lijst met de opdrachtregelopties ophalen. |
| -s | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |
| --controleren | Controleer machtigingen en het besturingssysteem, maar installeer niet de agent. |

Bestanden voor de agent voor afhankelijkheden worden geplaatst in de volgende mappen:

| Bestanden | Locatie |
|:--|:--|
| Kernbestanden | /opt/microsoft/dependency-agent |
| Logboekbestanden | /var/opt/microsoft/dependency-agent/log |
| Configuratiebestanden | /etc/opt/microsoft/dependency-agent/config |
| Uitvoerbare bestanden van de service | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binaire opslagbestanden | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Voorbeelden van installatiescript
Voor het implementeren van eenvoudig in één keer de agent voor afhankelijkheden op meerdere servers, krijgt u het volgende scriptvoorbeeld downloaden en installeren van de agent voor afhankelijkheden op Windows of Linux.

### <a name="powershell-script-for-windows"></a>PowerShell-script voor Windows
```PowerShell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Shell-script voor Linux
```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```
## <a name="desired-state-configuration"></a>Desired State Configuration
Voor het implementeren van de agent voor afhankelijkheden met behulp van Desired State Configuration (DSC), kunt u de module xPSDesiredStateConfiguration met de volgende voorbeeldcode:

```
configuration ServiceMap {

Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

Node localhost
{ 
    # Download and install the Dependency agent
    xRemoteFile DAPackage 
    {
        Uri = "https://aka.ms/dependencyagentwindows"
        DestinationPath = $DAPackageLocalPath
    }

    xPackage DA
    {
        Ensure="Present"
        Name = "Dependency Agent"
        Path = $DAPackageLocalPath
        Arguments = '/S'
        ProductId = ""
        InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
        InstalledCheckRegValueName = "DisplayName"
        InstalledCheckRegValueData = "Dependency Agent"
        DependsOn = "[xRemoteFile]DAPackage"
    }
  }
}
```

## <a name="remove-the-dependency-agent"></a>Verwijderen van de agent voor afhankelijkheden
### <a name="uinstall-agent-on-windows"></a>De agent verwijderen op Windows
Een beheerder kan de agent voor afhankelijkheden voor Windows via het Configuratiescherm kunt verwijderen.

Een beheerder kan ook uitvoeren %Programfiles%\Microsoft afhankelijkheid Agent\Uninstall.exe verwijderen van de agent voor afhankelijkheden.

### <a name="uninstall-agent-on-linux"></a>Verwijder de agent op Linux
U kunt de agent voor afhankelijkheden van Linux verwijderen met de volgende opdracht.

RHEL, CentOs of Oracle:

```
sudo rpm -e dependency-agent
```

Ubuntu:

```
sudo apt -y purge dependency-agent
```

## <a name="troubleshooting"></a>Problemen oplossen
Als er geen problemen installeren of uitvoeren van de Service Map, kunt in deze sectie u. Als u uw probleem nog steeds niet kunt oplossen, neem contact op met Microsoft Support.

### <a name="dependency-agent-installation-problems"></a>Problemen met de installatie van de afhankelijkheid agents
#### <a name="installer-prompts-for-a-reboot"></a>Installatieprogramma wordt u gevraagd om een opnieuw opstarten
De agent voor afhankelijkheden *algemeen* hoeft niet opnieuw worden opgestart bij het installeren of verwijderen. In sommige zeldzame gevallen vereist Windows Server echter opnieuw worden opgestart om door te gaan met een installatie. Dit gebeurt wanneer een afhankelijkheid, meestal het herdistribueerbare Microsoft Visual C++ pakket, worden opgestart vanwege een vergrendeld bestand moet.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--codenumber-appears"></a>Bericht ' kan niet voor het installeren van de agent voor afhankelijkheden: Visual Studio-Runtime-bibliotheken kunnen niet installeren (code = [codenummer]) ' wordt weergegeven

De agent voor Microsoft Dependency is gebouwd op de runtime-bibliotheken voor Microsoft Visual Studio. U krijgt een bericht als er een probleem tijdens de installatie van de bibliotheken. 

De runtime-bibliotheek installatieprogramma's maken Logboeken in de map %LOCALAPPDATA%\temp. Het bestand is dd_vcredist_arch_yyyymmddhhmmss.log, waar *arch* 'x86' of 'amd64' en *jjjjmmdduummss* is de datum en tijd (24-uurs notatie) wanneer het logboek is gemaakt. Het logboek bevat informatie over het probleem dat wordt geblokkeerd door de installatie.

Kan het zijn nuttig voor het installeren van de [nieuwste runtimebibliotheken](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) zelf eerste.

De volgende tabel bevat de codenummers en voorgestelde oplossingen.

| Code | Beschrijving | Oplossing |
|:--|:--|:--|
| 0x17 | Het installatieprogramma bibliotheek vereist een Windows-update die is niet geïnstalleerd. | Zoek in de meest recente bibliotheek installer-logboekbestand.<br><br>Als een verwijzing naar "Windows8.1-KB2999226-x64.msu" wordt gevolgd door een regel "fout 0x80240017: kan niet worden uitgevoerd MSU-pakket," u hebt de vereisten voor het installeren van KB2999226. Volg de instructies in de sectie vereisten van [universeel C Runtime in Windows](https://support.microsoft.com/kb/2999226). Mogelijk moet u Windows Update uitvoeren en meerdere keren opnieuw om de vereiste onderdelen installeren.<br><br>Voer het installatieprogramma van Microsoft Dependency agent opnieuw uit. |

### <a name="post-installation-issues"></a>Na de installatie problemen
#### <a name="server-doesnt-appear-in-service-map"></a>Server niet wordt weergegeven in het Serviceoverzicht
Als uw agent-installatie van afhankelijkheid is voltooid, maar u kunt uw server in de oplossing Serviceoverzicht niet ziet:
* Is de agent voor afhankelijkheden zijn geïnstalleerd? U kunt dit controleren door te controleren of de service is geïnstalleerd en uitgevoerd.<br><br>
**Windows**: zoek naar de service met de naam 'Microsoft Dependency agent'.<br>
**Linux**: zoeken naar de actieve verwerken "microsoft--agent voor afhankelijkheden."

* Weet u op de [gratis prijscategorie van Operations Management Suite/Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers)? Het gratis abonnement kunt u maximaal vijf unieke Service Map-servers. Alle volgende servers zijn weergegeven niet in het Serviceoverzicht, zelfs als de voorafgaande vijf niet meer gegevens worden verzonden.

* Is uw server verzenden logboek- en prestatiegegevens naar Log Analytics? Ga naar zoeken in Logboeken en voer de volgende query uit voor uw computer: 

        Usage | where Computer == "admdemo-appsvr" | summarize sum(Quantity), any(QuantityUnit) by DataType

Krijgt u een aantal gebeurtenissen in de resultaten? Zijn de gegevens recente? Als dit het geval is, wordt uw Log Analytics-Agent naar behoren werkt en communiceren met Log Analytics. Als dit niet het geval is, controleert u de agent op uw server: [Log Analytics-agent voor het oplossen van Windows](https://support.microsoft.com/help/3126513/how-to-troubleshoot-monitoring-onboarding-issues) of [Log Analytics-agent voor het oplossen van Linux](../log-analytics/log-analytics-agent-linux-support.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Server wordt weergegeven in het Serviceoverzicht, maar er zijn geen processen is
Als u uw server in het Serviceoverzicht ziet, maar er geen gegevens verwerken of verbinding, die aangeeft dat de agent voor afhankelijkheden geïnstalleerd en actief is, maar het kernelstuurprogramma zijn niet geladen. 

Controleer de C:\Program Files\Microsoft afhankelijkheid Agent\logs\wrapper.log-bestand (Windows) of /var/opt/microsoft/dependency-agent/log/service.log-bestand (Linux). De laatste regels van het bestand aangeven waarom de kernel niet laden. De kernel kan bijvoorbeeld niet worden ondersteund op Linux als u de kernel wordt bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [gebruik van Serviceoverzicht]( monitoring-service-map.md) nadat deze is geïmplementeerd en geconfigureerd.
