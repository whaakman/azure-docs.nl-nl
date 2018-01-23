---
title: Serviceoverzicht configureren in Operations Management Suite | Microsoft Docs
description: Serviceoverzicht is een Operations Management Suite-oplossing die automatisch de onderdelen van toepassing op Windows- en Linux-systemen worden gedetecteerd en de communicatie tussen services wordt toegewezen. Dit artikel bevat informatie voor het Serviceoverzicht implementeren in uw omgeving en gebruiken in verschillende scenario's.
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/18/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: e23173fb6708104c39071145595e4eec3454ee76
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="configure-service-map-in-operations-management-suite"></a>Serviceoverzicht configureren in Operations Management Suite
Serviceoverzicht ontdekt automatisch toepassingsonderdelen op Windows- en Linux-systemen en wijst de communicatie tussen services toe. U kunt deze gebruiken om weer te geven van uw servers, zoals u ze--beschouwen als onderling verbonden systemen die essentiële services leveren. Service-kaart toont de verbindingen tussen servers, processen en poorten via een TCP-verbinding architectuur waarvoor geen configuratie vereist, behalve de installatie van een agent.

Dit artikel worden de details van het Serviceoverzicht en voorbereiding agents configureren. Zie voor meer informatie over het gebruik van Serviceoverzicht [het Serviceoverzicht-oplossing gebruiken in Operations Management Suite](operations-management-suite-service-map.md).

## <a name="dependency-agent-downloads"></a>Agent voor afhankelijkheden gedownload
| File | OS | Versie | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.3.0 | 1F5261CAAF6C8DF4E03E4927DA918B3461B40B41C6BF5845803878D7CF975693 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.3.0 | 7BADFF2411899114F0214766160E4E871A2462DC137141CEEDEFAF528F428ADD  |


## <a name="connected-sources"></a>Verbonden bronnen
Serviceoverzicht afkomstig zijn uit de Microsoft-Agent voor afhankelijkheden. De Agent voor afhankelijkheden, is afhankelijk van de OMS-Agent voor de verbindingen met Operations Management Suite. Dit betekent dat een server moet zijn geïnstalleerd en geconfigureerd eerst de OMS-Agent en vervolgens de Agent voor afhankelijkheden kunnen worden geïnstalleerd. De volgende tabel beschrijft de verbonden bronnen die ondersteuning biedt voor de oplossing Serviceoverzicht.

| Verbonden bron | Ondersteund | Beschrijving |
|:--|:--|:--|
| Windows-agents | Ja | Serviceoverzicht analyseert en verzamelt gegevens van agent-Windows-computers. <br><br>Naast de [OMS-Agent](../log-analytics/log-analytics-windows-agent.md), Windows-agents Microsoft afhankelijkheid Agent vereist. Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Linux-agents | Ja | Serviceoverzicht analyseert en verzamelt gegevens van Linux-agent-computers. <br><br>Naast de [OMS-Agent](../log-analytics/log-analytics-linux-agents.md), Linux-agents Microsoft afhankelijkheid Agent vereist. Zie de [ondersteunde besturingssystemen](#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Beheergroep System Center Operations Manager | Ja | Serviceoverzicht analyseert en verzamelt gegevens van Windows en Linux-agents in een verbonden [System Center Operations Manager-beheergroep](../log-analytics/log-analytics-om-agents.md). <br><br>Er is een directe verbinding van de System Center Operations Manager agent-computer met Operations Management Suite vereist. Gegevens uit de beheergroep doorgestuurd naar de Operations Management Suite-opslagplaats.|
| Azure Storage-account | Nee | Serviceoverzicht verzamelt gegevens van computers die door agents, dus er zijn geen gegevens te verzamelen van Azure Storage. |

Serviceoverzicht ondersteunt alleen 64-bits-platforms.

In Windows, Microsoft Monitoring Agent (MMA) wordt gebruikt door zowel System Center Operations Manager en Operations Management Suite verzamelen en verzenden van het bewaken van gegevens. (Deze agent heet de System Center Operations Manager-Agent, OMS-Agent, Log Analytics-Agent, MMA of Direct Agent, afhankelijk van de context.) System Center Operations Manager en Operations Management Suite bieden verschillende out van het vak versies van de MMA. Deze versies kunnen elk rapport naar System Center Operations Manager, Operations Management Suite of naar beide.  

Op Linux, de OMS-Agent voor Linux Hiermee en verzendt het bewaken van gegevens met Operations Management Suite. U kunt Serviceoverzicht gebruiken op servers met OMS Direct Agents of op servers die zijn gekoppeld aan Operations Management Suite via System Center Operations Manager-beheergroepen.  

In dit artikel, verwijzen we naar alle agents--of Linux- of Windows, of verbonden met een beheergroep van System Center Operations Manager of rechtstreeks met Operations Management Suite--als de "OMS-Agent." We gebruiken de naam van de specifieke implementatie van de agent alleen indien nodig voor context.

De agent Serviceoverzicht stuurt de gegevens zelf geen en vereist geen wijzigingen in de firewalls en poorten. De gegevens in het Serviceoverzicht worden altijd verzonden door de OMS-Agent met Operations Management Suite rechtstreeks of via de OMS-Gateway.

![Serviceoverzicht agents](media/oms-service-map/agents.png)

Als u een klant System Center Operations Manager met een beheergroep die is verbonden met Operations Management Suite:

- Als de System Center Operations Manager-agents toegang krijgen het Internet verbinding maken met Operations Management Suite tot kunnen, is geen aanvullende configuratie vereist.  
- Als de System Center Operations Manager-agents geen toegang Operations Management Suite via Internet tot, moet u de OMS-Gateway werkt met System Center Operations Manager configureren.
  
Als u de directe OMS-Agent gebruikt, moet u de OMS-Agent zelf verbinding maken met Operations Management Suite of met uw Gateway OMS configureren. De OMS-Gateway kan worden gedownload vanaf de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

### <a name="management-packs"></a>Management packs
Wanneer het Serviceoverzicht in een Operations Management Suite-werkruimte is geactiveerd, wordt een 300 KB management pack verzonden naar de Windows-servers in deze werkruimte. Als u System Center Operations Manager-agents in een [verbonden beheergroep](../log-analytics/log-analytics-om-agents.md), het Serviceoverzicht management pack van System Center Operations Manager is geïmplementeerd. Als de agents rechtstreeks verbonden zijn, levert Operations Management Suite het management pack.

De naam van het management pack is Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Deze worden naar %Programfiles%\Microsoft bewaking Agent\Agent\Health Service State\Management Packs\ geschreven. De gegevensbron die gebruikmaakt van het management pack is % Program files%\Microsoft bewaking Agent\Agent\Health Service State\Resources\<AutoGeneratedID > \ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="installation"></a>Installatie
### <a name="install-the-dependency-agent-on-microsoft-windows"></a>Installeer de Agent afhankelijkheid op Microsoft Windows
Er zijn beheerdersbevoegdheden vereist om te installeren of verwijderen van de agent.

De afhankelijkheid-Agent is geïnstalleerd op Windows-computers via InstallDependencyAgent Windows.exe. Als u dit uitvoerbare bestand zonder opties uitvoert, wordt er een wizard waarmee u volgen kunt om u interactief installeren gestart.  

Gebruik de volgende stappen voor het installeren van de Agent voor afhankelijkheden op elke Windows-computer:

1.  De OMS-Agent installeren met behulp van de instructies op de [verbinding maken met Windows-computers naar de Log Analytics-service in Azure](../log-analytics/log-analytics-windows-agent.md).
2.  De Windows-agent downloaden en uitvoeren met behulp van de volgende opdracht: <br>`InstallDependencyAgent-Windows.exe`
3.  Volg de wizard om de agent te installeren.
4.  Als de Agent voor afhankelijkheden niet start, controleert u de logboeken voor uitgebreide foutinformatie. Op Windows-agents is de logboekmap %Programfiles%\Microsoft Agent\logs afhankelijkheid. 

#### <a name="windows-command-line"></a>Windows-opdrachtregel
Opties van de volgende tabel gebruiken om te installeren vanaf een opdrachtregel. Een overzicht van de vlaggen voor de installatie kunt u het installatieprogramma uitvoeren met behulp van de /? markering als volgt.

    InstallDependencyAgent-Windows.exe /?

| Vlag | Beschrijving |
|:--|:--|
| /? | Een lijst van de opdrachtregelopties ophalen. |
| /S | Een installatie zonder vragen van de gebruiker op de achtergrond uitvoeren. |

Bestanden voor de Windows-Agent voor afhankelijkheden worden standaard in C:\Program Files\Microsoft afhankelijkheid Agent geplaatst.

### <a name="install-the-dependency-agent-on-linux"></a>De afhankelijkheid-Agent installeren op Linux
Toegang tot de hoofdmap is vereist voor het installeren of configureren van de agent.

De afhankelijkheid-Agent is geïnstalleerd op Linux-computers via InstallDependencyAgent-Linux64.bin, een shell-script met een zichzelf uitpakkend binaire waarde. U kunt het bestand uit te voeren met behulp van servicel of toevoegen schrijfrechten hebben voor het bestand zelf.
 
Gebruik de volgende stappen voor het installeren van de Agent voor afhankelijkheden op elke Linux-computer:

1.  De OMS-Agent installeren met behulp van de instructies op de [verzamelen en beheren van gegevens van Linux-computers](https://technet.microsoft.com/library/mt622052.aspx).
2.  De afhankelijkheid van Linux-agent als hoofdmap installeren met behulp van de volgende opdracht:<br>`sh InstallDependencyAgent-Linux64.bin`
3.  Als de Agent voor afhankelijkheden niet start, controleert u de logboeken voor uitgebreide foutinformatie. Op Linux-agents is de logboekmap /var/opt/microsoft/dependency-agent/log.

Voor een overzicht van de vlaggen voor installatie, voer de installatie programma dankzij - vlag als volgt.

    InstallDependencyAgent-Linux64.bin -help

| Vlag | Beschrijving |
|:--|:--|
| -help | Een lijst van de opdrachtregelopties ophalen. |
| -s | Een installatie zonder vragen van de gebruiker op de achtergrond uitvoeren. |
| --controleren | Controleer de machtigingen en het besturingssysteem, maar moet u de agent niet installeren. |

Bestanden voor de Agent voor afhankelijkheden worden geplaatst in de volgende mappen:

| Bestanden | Locatie |
|:--|:--|
| Core-bestanden | /Opt/Microsoft/Dependency-agent |
| Logboekbestanden | /var/opt/Microsoft/Dependency-agent/log |
| De config-bestanden | /etc/opt/microsoft/dependency-agent/config |
| Uitvoerbare bestanden voor service | /Opt/Microsoft/Dependency-agent/bIn/Microsoft-Dependency-agent<br>/Opt/Microsoft/Dependency-agent/bIn/Microsoft-Dependency-Agent-Manager |
| Binaire opslag-bestanden | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Voorbeelden van scripts voor installatie
Als u wilt eenvoudig in één keer de afhankelijkheid-Agent op veel servers implementeert, is het nuttig om een script gebruiken. U kunt de volgende scriptvoorbeelden downloaden en installeren van de Agent voor afhankelijkheden op Windows of Linux.

### <a name="powershell-script-for-windows"></a>Windows PowerShell-script
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

$version = "9.1"
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

Een zeer eenvoudige wijze om te controleren of de de afhankelijkheid-Agent wordt op elk van uw virtuele machines is om op te nemen van de agent in uw Azure Resource Manager-sjabloon.  Opmerking dat de Agent voor afhankelijkheden nog steeds gebruik van de OMS-Agent, zodat de [OMS-Agent VM-extensie](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-vm-extension) eerst moet worden geïmplementeerd.  Het volgende JSON-fragment kan worden toegevoegd aan de *resources* gedeelte van uw sjabloon.
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
    "typeHandlerVersion": "9.1",
    "autoUpgradeMinorVersion": true
}

```


## <a name="desired-state-configuration"></a>Desired State Configuration
Voor het implementeren van de Agent voor afhankelijkheden via Desired State Configuration, kunt u de module xPSDesiredStateConfiguration en een deel van de code als volgt:
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
### <a name="uninstall-the-dependency-agent-on-windows"></a>Verwijder de Agent afhankelijkheid in Windows
Een beheerder kan de afhankelijkheid Agent voor Windows via het Configuratiescherm kunt verwijderen.

Een beheerder kan ook uitvoeren %Programfiles%\Microsoft afhankelijkheid Agent\Uninstall.exe de afhankelijkheid-Agent verwijderen.

### <a name="uninstall-the-dependency-agent-on-linux"></a>Verwijder de Agent afhankelijkheid op Linux
U kunt de Agent voor afhankelijkheden van Linux verwijderen met de volgende opdracht.
<br>RHEL, CentOs of Oracle:
```
sudo rpm -e dependency-agent
```
Ubuntu:
```
sudo dpkg --purge dependency-agent
```
## <a name="troubleshooting"></a>Problemen oplossen
Als er problemen installeren of uitvoeren Serviceoverzicht, kunt in deze sectie u. Als u uw probleem niet kunt oplossen, neem contact op met Microsoft Support.

### <a name="dependency-agent-installation-problems"></a>Problemen tijdens de installatie van de afhankelijkheid Agent
#### <a name="installer-asks-for-a-reboot"></a>Installatieprogramma vraagt om de computer opnieuw is opgestart
De Agent voor afhankelijkheden *doorgaans* hoeft niet opnieuw worden opgestart na het installeren of verwijderen. In bepaalde zeldzame gevallen vereist Windows Server echter opnieuw opstarten om door te gaan met een installatie. Dit gebeurt wanneer een afhankelijkheid, meestal het herdistribueerbare Microsoft Visual C++ pakket, opnieuw worden opgestart vanwege een vergrendeld bestand moet.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--codenumber-appears"></a>Bericht ' kan niet voor het installeren van de Agent voor afhankelijkheden: installatie van Visual Studio-Runtime-bibliotheken is mislukt (code = [codenummer]) ' wordt weergegeven

De Microsoft-Agent voor afhankelijkheid is gebaseerd op de runtimebibliotheken van Microsoft Visual Studio. U krijgt een bericht als er een probleem opgetreden tijdens de installatie van de tapewisselaars. 

Installatieprogramma's voor de bibliotheek runtime maken Logboeken in de map %LOCALAPPDATA%\temp. Het bestand is dd_vcredist_arch_yyyymmddhhmmss.log, waarbij *arch* 'x86' of 'amd64' en *jjjjmmdduummss* is de datum en tijd (24-uurs klok) wanneer het logboek is gemaakt. Het logboek bevat informatie over het probleem dat door de installatie wordt geblokkeerd.

Mogelijk handig voor het installeren van de [nieuwste runtimebibliotheken](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) zelf eerste.

De volgende tabel bevat de code cijfers en voorgestelde oplossingen.

| Code | Beschrijving | Oplossing |
|:--|:--|:--|
| 0x17 | Het installatieprogramma bibliotheek vereist een Windows-update is niet geïnstalleerd. | Zoek in de meest recente bibliotheek installer-logboekbestand.<br><br>Als een verwijzing naar 'Windows8.1-KB2999226-x64.msu' wordt gevolgd door een regel ' fout 0x80240017: uitvoeren van MSU-pakket is mislukt "u hebt de vereisten voor het installeren van KB2999226. Volg de instructies in de sectie vereisten in [universeel C Runtime in Windows](https://support.microsoft.com/kb/2999226). Mogelijk moet u Windows Update uit te voeren en meerdere keren opstarten om de vereiste software installeren.<br><br>Voer het installatieprogramma van Microsoft afhankelijkheid Agent opnieuw uit. |

### <a name="post-installation-issues"></a>Na de installatie problemen
#### <a name="server-doesnt-appear-in-service-map"></a>Server niet wordt weergegeven in het Serviceoverzicht
Als uw afhankelijkheid Agent-installatie is voltooid, maar uw server in de oplossing Serviceoverzicht niet wordt weergegeven:
* Is de Agent voor afhankelijkheden geïnstalleerd? U kunt dit controleren door te controleren of de service is geïnstalleerd en uitgevoerd.<br><br>
**Windows**: zoek naar de service met de naam 'Microsoft Dependency Agent'.<br>
**Linux**: Kijk naar de uitgevoerd verwerken 'microsoft-afhankelijkheid-agent'.

* Weet u op de [gratis prijscategorie van Operations Management Suite/Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers)? Het gratis-plan kunt u maximaal vijf unieke Serviceoverzicht-servers. Alle volgende servers zijn weergegeven niet in het Serviceoverzicht, zelfs als de voorafgaande vijf niet meer gegevens verzendt.

* Is uw server verzenden logboek- en prestatiegegevens met Operations Management Suite? Ga naar het logboek zoeken en voer de volgende query voor uw computer: 

        * Computer="<your computer name here>" | measure count() by Type
        
  Krijgt u een aantal gebeurtenissen in de resultaten? Zijn de gegevens recente? Als dit het geval is, wordt de OMS-Agent naar behoren werkt en communicatie met de service Operations Management Suite. Als dit niet het geval is, controleert u de OMS-Agent op uw server: [OMS-Agent voor Windows probleemoplossing](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) of [OMS-Agent voor het oplossen van Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Server wordt weergegeven in het Serviceoverzicht, maar heeft geen processen
Als u uw server in het Serviceoverzicht zien, maar geen gegevens verwerken of verbinding heeft, die aangeeft dat de afhankelijkheid-Agent geïnstalleerd en wordt uitgevoerd is, maar het kernelstuurprogramma niet geladen. 

Controleer de C:\Program Files\Microsoft afhankelijkheid Agent\logs\wrapper.log-bestand (Windows) of /var/opt/microsoft/dependency-agent/log/service.log-bestand (Linux). De laatste regels van het bestand zou moeten aangeven waarom de kernel niet laden. Bijvoorbeeld, de kernel mogelijk niet ondersteund op Linux als u uw kernel bijgewerkt.

## <a name="data-collection"></a>Gegevensverzameling
Elke agent voor het verzenden van ongeveer 25 MB per dag, afhankelijk van hoe complexe uw systeem-afhankelijkheden zijn, kunt u verwachten. Elke agent verzendt Serviceoverzicht afhankelijkheid elke 15 seconden.  

De Agent voor afhankelijkheden verbruikt doorgaans 0,1 procent van het systeemgeheugen en 0,1 procent van CPU-systeem.

## <a name="supported-azure-regions"></a>Ondersteunde Azure-regio 's
Serviceoverzicht is momenteel beschikbaar in de volgende Azure-regio's:
- VS - oost
- West-Europa
- West-centraal VS
- Zuidoost-Azië


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De volgende secties worden de ondersteunde besturingssystemen voor de Agent afhankelijkheid. Serviceoverzicht biedt geen ondersteuning voor 32-bits architecturen voor elk besturingssysteem.

### <a name="windows-server"></a>Windows Server
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows-bureaublad
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux-, CentOS Linux- en Oracle Linux (met RHEL Kernel)
- Alleen standaard en SMP Linux kernel-versies worden ondersteund.
- Niet-standaard kernel versies, zoals PAE en Xen, worden niet ondersteund voor een Linux-distributie. Bijvoorbeeld, wordt een systeem met de release tekenreeks '2.6.16.21-0.8-xen' niet ondersteund.
- Aangepaste kernels, met inbegrip van hercompilaties van standaard kernels worden niet ondersteund.
- CentOSPlus kernel wordt niet ondersteund.
- Oracle Unbreakable Enterprise Kernel (UEK) wordt in verderop in dit artikel beschreven.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Besturingssysteemversie | Kernelversie |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |

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

### <a name="ubuntu-server"></a>Ubuntu Server
- Aangepaste kernels, met inbegrip van hercompilaties van standaard kernels worden niet ondersteund.

| Besturingssysteemversie | Kernelversie |
|:--|:--|
| 16.04 | 4.4.0-103<br>4.11.0-1016 |
| 14.04 | 3.13.0-137<br>4.4.0-103 |

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


## <a name="diagnostic-and-usage-data"></a>Diagnostische gegevens en gebruiksgegevens
Microsoft verzamelt automatisch gebruiks- en via uw gebruik van de service Serviceoverzicht. Microsoft gebruikt deze gegevens te bieden en de kwaliteit, beveiliging en integriteit van de service Serviceoverzicht te verbeteren. Gegevens omvatten informatie over de configuratie van uw software, zoals het besturingssysteem en versie. Dit omvat ook IP-adres, de DNS-naam en de Werkstationnaam zodat nauwkeurige en efficiënte mogelijkheden voor probleemoplossing. Er worden geen namen, adressen of andere contactgegevens verzameld.

Zie voor meer informatie over het verzamelen van gegevens en gebruiksgegevens het [privacyverklaring van Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Volgende stappen
- Meer informatie over hoe [Serviceoverzicht gebruiken](operations-management-suite-service-map.md) nadat deze is geïmplementeerd en geconfigureerd.
