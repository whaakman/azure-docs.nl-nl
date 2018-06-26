---
title: Serviceoverzicht configureren in Azure | Microsoft Docs
description: Serviceoverzicht is een oplossing in Azure die automatisch toepassingsonderdelen op Windows- en Linux-systemen detecteert en de communicatie tussen services toewijst. Dit artikel bevat informatie voor het Serviceoverzicht implementeren in uw omgeving en gebruiken in verschillende scenario's.
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
ms.date: 06/22/2018
ms.author: daseidma;bwren
ms.openlocfilehash: 872d5f05e4d607c9445d1af5cc9b9cb984c19e11
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752571"
---
# <a name="configure-service-map-in-azure"></a>Serviceoverzicht configureren in Azure
Serviceoverzicht ontdekt automatisch toepassingsonderdelen op Windows- en Linux-systemen en wijst de communicatie tussen services toe. U kunt deze gebruiken om weer te geven van uw servers, zoals u ze--beschouwen als onderling verbonden systemen die essentiële services leveren. Service-kaart toont de verbindingen tussen servers, processen en poorten via een TCP-verbinding architectuur waarvoor geen configuratie vereist, behalve de installatie van een agent.

Dit artikel worden de details van het Serviceoverzicht en voorbereiding agents configureren. Zie voor meer informatie over het gebruik van Serviceoverzicht [gebruik van de oplossing Serviceoverzicht in Azure]( monitoring-service-map.md).

## <a name="dependency-agent-downloads"></a>Downloads agent voor afhankelijkheden
| Bestand | Besturingssysteem | Versie | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.5.0 | 8B8FE0F6B0A9F589C4B7B52945C2C25DF008058EB4D4866DC45EE2485062C9D7 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.5.1 | 09D56EF43703A350FF586B774900E1F48E72FE3671144B5C99BB1A494C201E9E |


## <a name="connected-sources"></a>Verbonden bronnen
Serviceoverzicht afkomstig zijn uit de Microsoft-Agent voor afhankelijkheden. De agent voor afhankelijkheden is afhankelijk van de OMS-agent voor zijn verbindingen met Log Analytics. Dit betekent dat een server moet zijn geïnstalleerd en geconfigureerd eerst de OMS-Agent en vervolgens de Agent voor afhankelijkheden kunnen worden geïnstalleerd. De volgende tabel beschrijft de verbonden bronnen die ondersteuning biedt voor de oplossing Serviceoverzicht.

| Verbonden bron | Ondersteund | Beschrijving |
|:--|:--|:--|
| Windows-agents | Ja | Serviceoverzicht analyseert en verzamelt gegevens van agent-Windows-computers. <br><br>Naast de [OMS-Agent](../log-analytics/log-analytics-windows-agent.md) hebben Windows-agents de Microsoft-agent voor afhankelijkheden nodig. Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Linux-agents | Ja | Serviceoverzicht analyseert en verzamelt gegevens van Linux-agent-computers. <br><br>Naast de [OMS-agent](../log-analytics/log-analytics-linux-agents.md) hebben Linux-agents de Microsoft-agent voor afhankelijkheden nodig. Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Beheergroep System Center Operations Manager | Ja | Serviceoverzicht analyseert en verzamelt gegevens van Windows en Linux-agents in een verbonden [System Center Operations Manager-beheergroep](../log-analytics/log-analytics-om-agents.md). <br><br>Er is een directe verbinding van de System Center Operations Manager-agentcomputer naar Log Analytics vereist. Gegevens uit de beheergroep doorgestuurd naar de werkruimte voor logboekanalyse.|
| Azure Storage-account | Nee | Serviceoverzicht verzamelt gegevens van computers die door agents, dus er zijn geen gegevens te verzamelen van Azure Storage. |

Serviceoverzicht ondersteunt alleen 64-bits-platforms.

In Windows, Microsoft Monitoring Agent (MMA) wordt gebruikt door zowel System Center Operations Manager en logboekanalyse voor het verzamelen en verzenden bewakingsgegevens. (Deze agent heet de System Center Operations Manager-Agent, OMS-Agent, Log Analytics-Agent, MMA of Direct Agent, afhankelijk van de context.) System Center Operations Manager en Log Analytics bieden verschillende out van het vak versies van de MMA. Deze versies kunnen beide rapporteren aan System Center Operations Manager, aan Log Analytics of aan beide.  

Op Linux, de OMS-Agent voor Linux Hiermee en verzendt het bewaken van gegevens met logboekanalyse. U kunt Serviceoverzicht gebruiken op servers met OMS Direct Agents of op servers die zijn gekoppeld aan logboekanalyse via System Center Operations Manager-beheergroepen.  

In dit artikel, verwijzen we naar alle agents--of Linux- of Windows, of verbonden met een beheergroep van System Center Operations Manager of rechtstreeks met logboekanalyse--als de *OMS-Agent*. We gebruiken de specifieke implementatienaam van de agent alleen als dat nodig is binnen de context.

De agent Serviceoverzicht stuurt de gegevens zelf geen en vereist geen wijzigingen in de firewalls en poorten. De gegevens in het Serviceoverzicht worden altijd verzonden door de OMS-Agent met Log Analytics, rechtstreeks of via de OMS-Gateway.

![Serviceoverzicht agents](media/monitoring-service-map/agents.png)

Als u een klant System Center Operations Manager met een beheergroep die is verbonden met logboekanalyse:

- Als uw System Center Operations Manager-agents toegang krijgen het Internet verbinding maken met logboekanalyse tot kunnen, is geen aanvullende configuratie vereist.  
- Als de System Center Operations Manager-agents geen logboekanalyse via het Internet openen, moet u de OMS-Gateway werkt met System Center Operations Manager configureren.
  
Als u de directe OMS-Agent gebruikt, moet u de OMS-Agent zichzelf worden verbonden met logboekanalyse of met uw Gateway OMS configureren. De OMS-Gateway kan worden gedownload vanaf de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666). Zie voor meer informatie over het implementeren en configureren van de Gateway OMS [verbinding maken met computers zonder toegang tot het Internet met behulp van de Gateway OMS](../log-analytics/log-analytics-oms-gateway.md).  

### <a name="management-packs"></a>Management packs
Wanneer het Serviceoverzicht in een werkruimte voor logboekanalyse is geactiveerd, wordt een 300 KB management pack verzonden naar de Windows-servers in deze werkruimte. Als u System Center Operations Manager-agents in een [verbonden beheergroep](../log-analytics/log-analytics-om-agents.md), het Serviceoverzicht management pack van System Center Operations Manager is geïmplementeerd. Als de agents rechtstreeks verbonden zijn, levert Log Analytics het management pack.

De naam van het management pack is Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Deze worden naar %Programfiles%\Microsoft bewaking Agent\Agent\Health Service State\Management Packs\ geschreven. De gegevensbron die gebruikmaakt van het management pack is % Program files%\Microsoft bewaking Agent\Agent\Health Service State\Resources\<AutoGeneratedID > \ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="installation"></a>Installatie
### <a name="install-the-dependency-agent-on-microsoft-windows"></a>Installeer de Agent afhankelijkheid op Microsoft Windows
Er zijn beheerdersbevoegdheden vereist om de agent te installeren of verwijderen.

De afhankelijkheid-Agent is geïnstalleerd op Windows-computers via InstallDependencyAgent Windows.exe. Als u dit uitvoerbare bestand zonder opties uitvoert, wordt er een wizard gestart die u kunt volgen om interactief te installeren.  

Gebruik de volgende stappen voor het installeren van de Agent voor afhankelijkheden op elke Windows-computer:

1.  De volgende op een van de methoden van OMS-Agent installeren [verzamelen gegevens van computers in uw omgeving met logboekanalyse](../log-analytics/log-analytics-concept-hybrid.md).
2.  De Windows-agent downloaden en uitvoeren met behulp van de volgende opdracht: <br>`InstallDependencyAgent-Windows.exe`
3.  Volg de wizard om de agent te installeren.
4.  Als de agent voor afhankelijkheden niet start, controleert u de logboeken voor uitgebreide foutinformatie. Op Windows-agents is de logboekmap %Programfiles%\Microsoft Agent\logs afhankelijkheid. 

#### <a name="windows-command-line"></a>Windows-opdrachtregel
Gebruik opties uit de volgende tabel om de agent te installeren vanaf een opdrachtregel. Een overzicht van de vlaggen voor de installatie kunt u het installatieprogramma uitvoeren met behulp van de /? markering als volgt.

    InstallDependencyAgent-Windows.exe /?

| Vlag | Beschrijving |
|:--|:--|
| /? | Een lijst met de opdrachtregelopties ophalen. |
| /S | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |

Bestanden voor de Windows-agent voor afhankelijkheden worden standaard geplaatst in C:\Program Files\Microsoft Dependency Agent.

### <a name="install-the-dependency-agent-on-linux"></a>De agent voor afhankelijkheden installeren op Linux
Toegang tot de hoofdmap is vereist om de agent te installeren of configureren.

De agent voor afhankelijkheden wordt geïnstalleerd op Linux-computers via InstallDependencyAgent-Linux64.bin, een shell-script met een zelfuitpakkend binair bestand. U kunt het bestand uit te voeren met behulp van servicel of toevoegen schrijfrechten hebben voor het bestand zelf.
 
Gebruik de volgende stappen voor het installeren van de agent voor afhankelijkheden op elke Linux-computer:

1.  De volgende op een van de methoden van OMS-Agent installeren [verzamelen gegevens van computers in uw omgeving met logboekanalyse](../log-analytics/log-analytics-concept-hybrid.md).
2.  De afhankelijkheid van Linux-agent als hoofdmap installeren met behulp van de volgende opdracht:<br>`sh InstallDependencyAgent-Linux64.bin`
3.  Als de agent voor afhankelijkheden niet start, controleert u de logboeken voor uitgebreide foutinformatie. Op Linux-agents is de logboekmap /var/opt/microsoft/dependency-agent/log.

Voor een overzicht van de vlaggen voor installatie, voer de installatie programma dankzij - vlag als volgt.

    InstallDependencyAgent-Linux64.bin -help

| Vlag | Beschrijving |
|:--|:--|
| -help | Een lijst met de opdrachtregelopties ophalen. |
| -s | Een installatie op de achtergrond uitvoeren zonder gebruikersvragen. |
| --controleren | Controleer machtigingen en het besturingssysteem, maar installeer niet de agent. |

Bestanden voor de agent voor afhankelijkheden worden in de volgende mappen geplaatst:

| Bestanden | Locatie |
|:--|:--|
| Kernbestanden | /opt/microsoft/dependency-agent |
| Logboekbestanden | /var/opt/microsoft/dependency-agent/log |
| Configuratiebestanden | /etc/opt/microsoft/dependency-agent/config |
| Uitvoerbare bestanden van de service | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binaire opslagbestanden | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Voorbeelden van installatiescript
Als u de agent voor afhankelijkheden op een groot aantal servers tegelijk wilt implementeren, is het handig om een script gebruiken. U kunt de volgende scriptvoorbeelden gebruiken om de agent voor afhankelijkheden op Windows of Linux te downloaden en installeren.

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

## <a name="azure-vm-extension"></a>Azure VM-extensie
U kunt de Agent voor afhankelijkheden eenvoudig implementeren naar uw Azure VM's met een [Azure VM-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).  Met de Azure VM-extensie kunt u de Agent voor afhankelijkheden implementeren voor uw virtuele machines via een PowerShell-script of rechtstreeks in Azure Resource Manager-sjabloon voor de VM.  Er is een uitbreiding beschikbaar voor zowel Windows (DependencyAgentWindows)- als Linux (DependencyAgentLinux).  Als u via de Azure VM-extensie implementeert, kunnen de agents automatisch worden bijgewerkt naar de meest recente versies.

Voor het implementeren van de Azure VM-extensie via PowerShell, kunt u het volgende voorbeeld:

```PowerShell
#
# Deploy the Dependency Agent to every VM in a Resource Group
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

Een zeer eenvoudige wijze om te controleren of de Agent voor afhankelijkheden op elk van uw VM is om op te nemen van de agent in uw Azure Resource Manager-sjabloon.  Opmerking dat de Agent voor afhankelijkheden nog steeds gebruik van de OMS-Agent, zodat de [OMS-Agent VM-extensie](../virtual-machines/extensions/oms-linux.md) eerst moet worden geïmplementeerd.  Het volgende JSON-fragment kan worden toegevoegd aan de *resources* gedeelte van uw sjabloon.

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


## <a name="desired-state-configuration"></a>Desired State Configuration
Voor het implementeren van de agent voor afhankelijkheden via Desired State Configuration kunt u de module xPSDesiredStateConfiguration en een stukje code zoals dit gebruiken:

```
configuration ServiceMap {

Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

Node localhost
{ 
    # Download and install the Dependency Agent
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

## <a name="uninstallation"></a>Verwijderen
### <a name="uninstall-the-dependency-agent-on-windows"></a>De agent voor afhankelijkheden verwijderen op Windows
Een beheerder kan de Windows-agent voor afhankelijkheden verwijderen via het Configuratiescherm.

Een beheerder kan ook %Programfiles%\Microsoft Dependency Agent\Uninstall.exe uitvoeren om de agent te verwijderen.

### <a name="uninstall-the-dependency-agent-on-linux"></a>De agent voor afhankelijkheden verwijderen op Linux
U kunt de Agent voor afhankelijkheden van Linux verwijderen met de volgende opdracht.
<br>RHEL, CentOs of Oracle:

```
sudo rpm -e dependency-agent
```

Ubuntu:

```
sudo apt -y purge dependency-agent
```
## <a name="troubleshooting"></a>Problemen oplossen
Als er problemen installeren of uitvoeren Serviceoverzicht, kunt in deze sectie u. Als u uw probleem niet kunt oplossen, neem contact op met Microsoft Support.

### <a name="dependency-agent-installation-problems"></a>Problemen tijdens de installatie van de afhankelijkheid Agent
#### <a name="installer-prompts-for-a-reboot"></a>Installatieprogramma wordt u gevraagd om een opnieuw opstarten
De Agent voor afhankelijkheden *doorgaans* hoeft niet opnieuw worden opgestart na het installeren of verwijderen. In bepaalde zeldzame gevallen vereist Windows Server echter opnieuw opstarten om door te gaan met een installatie. Dit gebeurt wanneer een afhankelijkheid, meestal het herdistribueerbare Microsoft Visual C++ pakket, opnieuw worden opgestart vanwege een vergrendeld bestand moet.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--codenumber-appears"></a>Bericht ' kan niet voor het installeren van de Agent voor afhankelijkheden: installatie van Visual Studio-Runtime-bibliotheken is mislukt (code = [codenummer]) ' wordt weergegeven

De Microsoft-Agent voor afhankelijkheid is gebaseerd op de runtimebibliotheken van Microsoft Visual Studio. U krijgt een bericht als er een probleem opgetreden tijdens de installatie van de tapewisselaars. 

Installatieprogramma's voor de bibliotheek runtime maken Logboeken in de map %LOCALAPPDATA%\temp. Het bestand is dd_vcredist_arch_yyyymmddhhmmss.log, waarbij *arch* 'x86' of 'amd64' en *jjjjmmdduummss* is de datum en tijd (24-uurs klok) wanneer het logboek is gemaakt. Het logboek bevat informatie over het probleem dat door de installatie wordt geblokkeerd.

Mogelijk handig voor het installeren van de [nieuwste runtimebibliotheken](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) zelf eerste.

De volgende tabel bevat de code cijfers en voorgestelde oplossingen.

| Coderen | Beschrijving | Resolutie |
|:--|:--|:--|
| 0x17 | Het installatieprogramma bibliotheek vereist een Windows-update is niet geïnstalleerd. | Zoek in de meest recente bibliotheek installer-logboekbestand.<br><br>Als een verwijzing naar 'Windows8.1-KB2999226-x64.msu' wordt gevolgd door een regel ' fout 0x80240017: uitvoeren van MSU-pakket is mislukt "u hebt de vereisten voor het installeren van KB2999226. Volg de instructies in de sectie vereisten in [universeel C Runtime in Windows](https://support.microsoft.com/kb/2999226). Mogelijk moet u Windows Update uit te voeren en meerdere keren opstarten om de vereiste software installeren.<br><br>Voer het installatieprogramma van Microsoft afhankelijkheid Agent opnieuw uit. |

### <a name="post-installation-issues"></a>Na de installatie problemen
#### <a name="server-doesnt-appear-in-service-map"></a>Server niet wordt weergegeven in het Serviceoverzicht
Als uw afhankelijkheid Agent-installatie is voltooid, maar uw server in de oplossing Serviceoverzicht niet wordt weergegeven:
* Is de Agent voor afhankelijkheden geïnstalleerd? U kunt dit controleren door te controleren of de service is geïnstalleerd en uitgevoerd.<br><br>
**Windows**: zoek naar de service met de naam 'Microsoft Dependency Agent'.<br>
**Linux**: Kijk naar de uitgevoerd verwerken 'microsoft-afhankelijkheid-agent'.

* Weet u op de [gratis prijscategorie van Operations Management Suite/Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers)? Het gratis-plan kunt u maximaal vijf unieke Serviceoverzicht-servers. Alle volgende servers zijn weergegeven niet in het Serviceoverzicht, zelfs als de voorafgaande vijf niet meer gegevens verzendt.

* Is uw server verzenden logboek- en prestatiegegevens met Log Analytics? Ga naar het logboek zoeken en voer de volgende query voor uw computer: 

        * Computer="<your computer name here>" | measure count() by Type
        
  Krijgt u een aantal gebeurtenissen in de resultaten? Zijn de gegevens recente? Als dit het geval is, wordt de OMS-Agent naar behoren werkt en communiceren met logboekanalyse. Als dit niet het geval is, controleert u de OMS-Agent op uw server: [OMS-Agent voor Windows probleemoplossing](https://support.microsoft.com/help/3126513/how-to-troubleshoot-monitoring-onboarding-issues) of [OMS-Agent voor het oplossen van Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Server wordt weergegeven in het Serviceoverzicht, maar heeft geen processen
Als u uw server in het Serviceoverzicht zien, maar geen gegevens verwerken of verbinding heeft, die aangeeft dat de afhankelijkheid-Agent geïnstalleerd en wordt uitgevoerd is, maar het kernelstuurprogramma niet geladen. 

Controleer de C:\Program Files\Microsoft afhankelijkheid Agent\logs\wrapper.log-bestand (Windows) of /var/opt/microsoft/dependency-agent/log/service.log-bestand (Linux). De laatste regels van het bestand zou moeten aangeven waarom de kernel niet laden. Bijvoorbeeld, de kernel mogelijk niet ondersteund op Linux als u uw kernel bijgewerkt.

## <a name="data-collection"></a>Gegevens verzamelen
Elke agent voor het verzenden van ongeveer 25 MB per dag, afhankelijk van hoe complexe uw systeem-afhankelijkheden zijn, kunt u verwachten. Elke agent verzendt Serviceoverzicht afhankelijkheid elke 15 seconden.  

De Agent voor afhankelijkheden verbruikt doorgaans 0,1 procent van het systeemgeheugen en 0,1 procent van CPU-systeem.

## <a name="supported-azure-regions"></a>Ondersteunde Azure-regio 's
Serviceoverzicht is momenteel beschikbaar in de volgende Azure-regio's:
- VS - oost
- West-Europa
- VS - west/centraal
- Zuidoost-Azië


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
In de volgende secties worden de ondersteunde besturingssystemen voor de agent voor afhankelijkheden vermeld. Serviceoverzicht biedt geen ondersteuning voor 32-bits architecturen voor elk besturingssysteem.

### <a name="windows-server"></a>Windows Server
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows-desktop
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux en Oracle Linux (met RHEL Kernel)
- Alleen standaard- en SMP Linux kernelversies worden ondersteund.
- Niet-standaard kernelversies, zoals PAE en Xen, worden voor geen enkele Linux-distributie ondersteund. Bijvoorbeeld, wordt een systeem met de release tekenreeks '2.6.16.21-0.8-xen' niet ondersteund.
- Aangepaste kernels, met inbegrip van hercompilaties van standaardkernels, worden niet ondersteund.
- CentOSPlus-kernel wordt niet ondersteund.
- Oracle Unbreakable Enterprise Kernel (UEK) wordt verderop in dit artikel beschreven.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Besturingssysteemversie | Kernelversie |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| Besturingssysteemversie | Kernelversie |
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

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| Besturingssysteemversie | Kernelversie |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411<br>2.6.18-412<br>2.6.18-416<br>2.6.18-417<br>2.6.18-419<br>2.6.18-420 |

### <a name="ubuntu-server"></a>Ubuntu-server
- Aangepaste kernels, met inbegrip van hercompilaties van standaardkernels, worden niet ondersteund.

| Besturingssysteemversie | Kernelversie |
|:--|:--|
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux met Unbreakable Enterprise Kernel
#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Besturingssysteemversie | Kernelversie
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Besturingssysteemversie | Kernelversie
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Besturingssysteemversie | Kernelversie
|:--|:--|
| 11 SP2 | 3.0.101-0.7 |
| 11 SP3 | 3.0.101-0.47 |
| 11 SP4 | 3.0.101-65 |


## <a name="diagnostic-and-usage-data"></a>Diagnostische en gebruiksgegevens
Microsoft verzamelt automatisch gebruiks- en via uw gebruik van de service Serviceoverzicht. Microsoft gebruikt deze gegevens te bieden en de kwaliteit, beveiliging en integriteit van de service Serviceoverzicht te verbeteren. Gegevens omvatten informatie over de configuratie van uw software, zoals het besturingssysteem en versie. Dit omvat ook IP-adres, de DNS-naam en de Werkstationnaam zodat nauwkeurige en efficiënte mogelijkheden voor probleemoplossing. Er worden geen namen, adressen of andere contactgegevens verzameld.

Zie voor meer informatie over het verzamelen van gegevens en gebruiksgegevens het [privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [Serviceoverzicht gebruiken]( monitoring-service-map.md) nadat deze is geïmplementeerd en geconfigureerd.
