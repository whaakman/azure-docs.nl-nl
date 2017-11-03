---
title: Wire-oplossing in Log Analytics | Microsoft Docs
description: Draadgegevens worden geleverd zijn geconsolideerde netwerk en de prestaties gegevens van computers met OMS-agent, met inbegrip van Operations Manager en verbonden met een Windows-agents. Gegevens van het netwerk wordt gecombineerd met uw gegevens aan het logboek voor hulp bij het correleren van gegevens.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: magoedte;banders
ms.openlocfilehash: 3bb4c82268fe7805227c213000dc803307876fe7
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a>Kabel gegevens 2.0 (Preview)-oplossing in Log Analytics

![Kabel gegevens symbool](./media/log-analytics-wire-data/wire-data2-symbol.png)

Kabel gegevens zijn geconsolideerde netwerk en de prestaties gegevens verzameld van verbonden met een Windows en Linux-verbonden computers met de OMS-agent, met inbegrip van die wordt bewaakt door Operations Manager in uw omgeving. Gegevens van het netwerk wordt gecombineerd met de andere logboekgegevens voor hulp bij het correleren van gegevens.

Naast de OMS-agent maakt gebruik van de oplossing Draadgegevens worden Microsoft afhankelijkheid Agents die u op computers in uw IT-infrastructuur installeert. Afhankelijkheid Agents bewaken van gegevens van het netwerk verzonden naar en van uw computers voor netwerk niveaus 2-3 in de [OSI-model](https://en.wikipedia.org/wiki/OSI_model), met inbegrip van de verschillende protocollen en poorten die worden gebruikt. Gegevens worden vervolgens verzonden met Log Analytics gebruikt agents.  

> [!NOTE]
> U kunt de vorige versie van de oplossing Draadgegevens worden geleverd niet toevoegen aan nieuwe werkruimten. Als u de oorspronkelijke Draadgegevens worden geleverd oplossing ingeschakeld hebt, kunt u blijven gebruiken. Echter, als u wilt gebruiken kabel gegevens 2.0, moet u eerst verwijderen de oorspronkelijke versie.

Standaard wordt met logboekanalyse gegevens geregistreerd voor CPU, geheugen, schijf en netwerk-prestatiegegevens van de items die zijn ingebouwd in Windows en Linux, evenals andere prestatiemeteritems die u kunt opgeven. Netwerk- en andere gegevensverzameling wordt uitgevoerd realtime voor elke agent, met inbegrip van subnetten en op toepassingsniveau-protocollen die worden gebruikt door de computer.  Draadgegevens worden gekeken netwerkgegevens op toepassingsniveau, niet omlaag op de TCP-transport-laag.  De oplossing ziet er niet op afzonderlijke bevestigingen en SYN.  Zodra de handshake is voltooid, wordt beschouwd als een live-verbinding en gemarkeerd als verbonden. Verbinding actief blijft, zolang beide zijden overeenstemming worden bereikt over de socket is geopend en gegevens heen en weer kunt doorgeven.  Wanneer beide zijden de verbinding is gesloten, wordt deze gemarkeerd als verbroken.  Daarom deze omvat alleen de bandbreedte van pakketten is voltooid, er niet gerapporteerd op opnieuw of pakketten is mislukt.

Als u hebt gebruikt [sFlow](http://www.sflow.org/) of andere software met [Cisco NetFlow protocol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), wordt de statistieken en de gegevens die u in de kabel gegevens zien u bekend zijn.

Enkele van de soorten ingebouwde logboek zoekquery's:

- Agents waarmee draadgegevens worden
- IP-adres van de agents waarmee draadgegevens worden geleverd
- Uitgaande communicatie per IP-adressen
- Aantal verzonden bytes per toepassingsprotocollen
- Aantal bytes dat door een toepassingsservice is verzonden
- Bytes ontvangen door verschillende protocollen
- Totaal aantal bytes verzonden en ontvangen door de IP-versie
- Gemiddelde latentie voor verbindingen die betrouwbaar zijn gemeten
- Computerprocessen die gestart of ontvangen van netwerkverkeer
- Hoeveelheid netwerkverkeer voor een proces

Wanneer u het zoeken met draadgegevens worden geleverd, kunt u filteren en groepsgegevens informatie bekijken over de bovenste agents en de bovenste protocollen. Of u kunt bekijken wanneer bepaalde computers (IP-adressen/MAC-adressen) gecommuniceerd met elkaar, hoe lang en hoeveel gegevens zijn verzonden, in principe u metagegevens over het netwerkverkeer, op basis van een zoekopdracht bekijken.

Aangezien u metagegevens bekijkt, is het echter niet per se nuttig is voor de diepgaande probleemoplossing. Draadgegevens worden geleverd in logboekanalyse is niet een volledige vastleggen van gegevens van het netwerk.  Het is niet bedoeld voor het oplossen van diep pakketniveau. Het voordeel van het gebruik van de agent, in vergelijking met andere methoden verzameling is dat er geen apparaten installeren, configureren van uw netwerkswitches of ingewikkeld configuraties uitvoeren. Draadgegevens worden geleverd is gewoon op agent gebaseerde — u de agent installeren op een computer en wordt het controleren van een eigen netwerkverkeer. Een ander voordeel is wanneer u bewaken van de werkbelastingen in de cloudproviders of hostserviceprovider of Microsoft Azure wilt, waarbij de gebruiker niet de eigenaar van de fabric-laag.

## <a name="connected-sources"></a>Verbonden bronnen

Draadgegevens worden afkomstig de gegevens uit de Microsoft-Agent voor afhankelijkheden. De Agent voor afhankelijkheden, is afhankelijk van de OMS-Agent voor de verbindingen met logboekanalyse. Dit betekent dat een server moet zijn geïnstalleerd en geconfigureerd eerst de OMS-Agent en installeer de Agent voor afhankelijkheden. De volgende tabel beschrijft de verbonden bronnen die ondersteuning biedt voor de oplossing Draadgegevens worden geleverd.

| **Verbonden bron** | **Ondersteund** | **Beschrijving** |
| --- | --- | --- |
| Windows-agents | Ja | Draadgegevens worden geleverd, analyseert en verzamelt gegevens van agent-Windows-computers. <br><br> Naast de [OMS-Agent](log-analytics-windows-agents.md), Windows-agents Microsoft afhankelijkheid Agent vereist. Zie de [ondersteunde besturingssystemen](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Linux-agents | Ja | Draadgegevens worden geleverd, analyseert en verzamelt gegevens van Linux-agent-computers.<br><br> Naast de [OMS-Agent](log-analytics-quick-collect-linux-computer.md), Linux-agents Microsoft afhankelijkheid Agent vereist. Zie de [ondersteunde besturingssystemen](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) voor een volledige lijst met versies van besturingssystemen. |
| Beheergroep System Center Operations Manager | Ja | Kabel gegevens analyseert en verzamelt gegevens van Windows en Linux-agents in een verbonden [System Center Operations Manager-beheergroep](log-analytics-om-agents.md). <br><br> Een directe verbinding van de System Center Operations Manager agent-computer met logboekanalyse is vereist. Gegevens uit de beheergroep doorgestuurd met logboekanalyse. |
| Azure Storage-account | Nee | Draadgegevens worden geleverd verzamelt gegevens van computers die door agents, dus er zijn geen gegevens te verzamelen van Azure Storage. |

In Windows, wordt Microsoft Monitoring Agent (MMA) door System Center Operations Manager en Log Analytics gebruikt voor het verzamelen en verzenden van gegevens. Afhankelijk van de context wordt de agent de System Center Operations Manager-Agent, OMS-Agent, Log Analytics-Agent, MMA of directe Agent genoemd. System Center Operations Manager en Log Analytics bieden enigszins verschillende versies van de MMA. Deze versies kunnen elk rapport naar System Center Operations Manager, met logboekanalyse of naar beide.

Op Linux, de OMS-Agent voor Linux worden verzameld en worden gegevens verzonden met logboekanalyse. U kunt Draadgegevens worden geleverd op servers met OMS Direct Agents of op servers die zijn gekoppeld aan logboekanalyse via System Center Operations Manager-beheergroepen.

In dit artikel verwijzingen naar alle agents of Linux- of Windows, of met een beheergroep van System Center Operations Manager verbonden of rechtstreeks met logboekanalyse worden aangeduid als de _OMS-agent_. We gebruiken de naam van de specifieke implementatie van de agent alleen indien nodig voor context.

Alle gegevens zelf wordt niet verzonden door de Agent voor afhankelijkheden en vereist geen wijzigingen in de firewalls en poorten. De gegevens in kabel worden altijd verzonden door de OMS-agent met Log Analytics, ofwel rechtstreeks of via de OMS-Gateway.

![diagram van agent](./media/log-analytics-wire-data/agents.png)

Als u een System Center Operations Manager-gebruiker met een beheergroep die is verbonden met logboekanalyse:

- Er is geen aanvullende configuratie vereist wanneer de System Center Operations Manager-agents kunnen toegang tot het Internet verbinding maken met logboekanalyse.
- U moet de OMS-Gateway met System Center Operations Manager te werken wanneer de System Center Operations Manager-agents geen toegang de logboekanalyse via Internet tot configureren.

Als u de directe Agent gebruikt, moet u de OMS-agent zichzelf worden verbonden met logboekanalyse of met uw Gateway OMS configureren. U kunt downloaden via de OMS-Gateway van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

## <a name="prerequisites"></a>Vereisten

- Vereist de [inzicht en analyse](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) oplossing aanbieding.
- Als u de vorige versie van de oplossing Draadgegevens worden geleverd, moet u deze eerst verwijderen. Alle gegevens die zijn vastgelegd via de oorspronkelijke Draadgegevens worden geleverd-oplossing is echter nog steeds beschikbaar in de kabel gegevens 2.0 en logboek zoeken.
- Er zijn beheerdersbevoegdheden vereist om te installeren of verwijderen van de Agent afhankelijkheid.
- De Agent voor afhankelijkheden moet worden geïnstalleerd op een computer met een 64-bits besturingssysteem.

### <a name="operating-systems"></a>Besturingssystemen

De volgende secties worden de ondersteunde besturingssystemen voor de Agent afhankelijkheid. 32-bits architecturen biedt geen voor elk besturingssysteem ondersteuning voor draadgegevens worden geleverd.

#### <a name="windows-server"></a>Windows Server

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Windows-bureaublad

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux-, CentOS Linux- en Oracle Linux (met RHEL Kernel)

- Alleen standaard en SMP Linux kernel-versies worden ondersteund.
- Niet-standaard kernel versies, zoals PAE en Xen, worden niet ondersteund voor een Linux-distributie. Bijvoorbeeld, een systeem met de release tekenreeks _2.6.16.21-0.8-xen_ wordt niet ondersteund.
- Aangepaste kernels, met inbegrip van hercompilaties van standaard kernels worden niet ondersteund.
- CentOSPlus kernel wordt niet ondersteund.
- Oracle Unbreakable Enterprise Kernel (UEK) wordt in verderop in dit artikel beschreven.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| **De versie van besturingssysteem** | **Kernelversie** |
| --- | --- |
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| **De versie van besturingssysteem** | **Kernelversie** |
| --- | --- |
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5

| **De versie van besturingssysteem** | **Kernelversie** |
| --- | --- |
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398 <br> 2.6.18-400 <br>2.6.18-402 <br>2.6.18-404 <br>2.6.18-406 <br> 2.6.18-407 <br> 2.6.18-408 <br> 2.6.18-409 <br> 2.6.18-410 <br> 2.6.18-411 <br> 2.6.18-412 <br> 2.6.18-416 <br> 2.6.18-417 <br> 2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux met Unbreakable Enterprise Kernel

#### <a name="oracle-linux-6"></a>Oracle Linux 6

| **De versie van besturingssysteem** | **Kernelversie** |
| --- | --- |
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| **De versie van besturingssysteem** | **Kernelversie** |
| --- | --- |
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11

| **De versie van besturingssysteem** | **Kernelversie** |
| --- | --- |
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10

| **De versie van besturingssysteem** | **Kernelversie** |
| --- | --- |
| 10 SP4 | 2.6.16.60 |

#### <a name="dependency-agent-downloads"></a>Agent voor afhankelijkheden gedownload

| **File** | **BESTURINGSSYSTEEM** | **Versie** | **SHA-256** |
| --- | --- | --- | --- |
| [InstallDependencyAgent Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |



## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit voor het configureren van de oplossing Draadgegevens worden geleverd voor uw werkruimten.

1. Inschakelen van de activiteit Log Analytics-oplossing van de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).
2. Installeer de Agent afhankelijkheid op elke computer waarop u gegevens wilt ophalen. De Agent voor afhankelijkheden kunnen verbindingen met onmiddellijke neighbors bewaken, dus u niet een agent op elke computer moet wellicht.

### <a name="install-the-dependency-agent-on-windows"></a>De afhankelijkheid-Agent installeren op Windows

Er zijn beheerdersbevoegdheden vereist om te installeren of verwijderen van de agent.

De afhankelijkheid-Agent is geïnstalleerd op computers waarop Windows wordt uitgevoerd via InstallDependencyAgent Windows.exe. Als u dit uitvoerbare bestand zonder opties uitvoert, wordt er een wizard waarmee u volgen kunt om u interactief installeren gestart.

Gebruik de volgende stappen voor het installeren van de Agent voor afhankelijkheden op elke computer waarop Windows wordt uitgevoerd:

1. De stappen in de OMS-Agent installeren [verzamelen van gegevens van Windows-computers in uw omgeving gehoste](log-analytics-windows-agents.md).
2. Download de Windows-afhankelijkheid-Agent op de koppeling in de vorige sectie en vervolgens uitvoeren met behulp van de volgende opdracht:`InstallDependencyAgent-Windows.exe`
3. Volg de wizard om de agent te installeren.
4. Als de Agent voor afhankelijkheden niet start, controleert u de logboeken voor uitgebreide foutinformatie. Voor Windows-agents is de logboekmap %Programfiles%\Microsoft Agent\logs afhankelijkheid.

#### <a name="windows-command-line"></a>Windows-opdrachtregel

Opties van de volgende tabel gebruiken om te installeren vanaf een opdrachtregel. Een overzicht van de vlaggen voor de installatie kunt u het installatieprogramma uitvoeren met behulp van de /? markering als volgt.

InstallDependencyAgent Windows.exe /?

| **Vlag** | **Beschrijving** |
| --- | --- |
| <code>/?</code> | Een lijst van de opdrachtregelopties ophalen. |
| <code>/S</code> | Een installatie zonder vragen van de gebruiker op de achtergrond uitvoeren. |

Bestanden voor de Windows-Agent voor afhankelijkheden worden standaard in C:\Program Files\Microsoft afhankelijkheid Agent geplaatst.

### <a name="install-the-dependency-agent-on-linux"></a>De afhankelijkheid-Agent installeren op Linux

Toegang tot de hoofdmap is vereist voor het installeren of configureren van de agent.

De afhankelijkheid-Agent is geïnstalleerd op Linux-computers via InstallDependencyAgent-Linux64.bin, een shell-script met een zichzelf uitpakkend binaire waarde. U kunt het bestand uitvoeren met behulp van _servicel_ of Voeg schrijfrechten hebben voor het bestand zelf.

Gebruik de volgende stappen voor het installeren van de Agent voor afhankelijkheden op elke Linux-computer:

1. De stappen in de OMS-Agent installeren [verzamelen van gegevens van Linux-computers in uw omgeving gehoste](log-analytics-quick-collect-linux-computer.md#obtain-workspace-id-and-key).
2. De Linux-afhankelijkheid-Agent op de koppeling in de vorige sectie Download en installeer deze vervolgens als hoofdmap met behulp van de volgende opdracht: servicel InstallDependencyAgent Linux64.bin
3. Als de Agent voor afhankelijkheden niet start, controleert u de logboeken voor uitgebreide foutinformatie. Op Linux-agents de logboekmap is: /var/opt/microsoft/dependency-agent/log.

Een overzicht van de vlaggen voor de installatie uitvoert het installatieprogramma met de `-help` markeren als volgt.

```
InstallDependencyAgent-Linux64.bin -help
```

| **Vlag** | **Beschrijving** |
| --- | --- |
| <code>-help</code> | Een lijst van de opdrachtregelopties ophalen. |
| <code>-s</code> | Een installatie zonder vragen van de gebruiker op de achtergrond uitvoeren. |
| <code>--check</code> | Controleer de machtigingen en het besturingssysteem, maar moet u de agent niet installeren. |

Bestanden voor de Agent voor afhankelijkheden worden geplaatst in de volgende mappen:

| **Bestanden** | **Locatie** |
| --- | --- |
| Core-bestanden | /Opt/Microsoft/Dependency-agent |
| Logboekbestanden | /var/opt/Microsoft/Dependency-agent/log |
| De config-bestanden | /etc/opt/Microsoft/Dependency-agent/config |
| Uitvoerbare bestanden voor service | /Opt/Microsoft/Dependency-agent/bIn/Microsoft-Dependency-agent<br><br>/Opt/Microsoft/Dependency-agent/bIn/Microsoft-Dependency-Agent-Manager |
| Binaire opslag-bestanden | /var/opt/Microsoft/Dependency-agent/Storage |

### <a name="installation-script-examples"></a>Voorbeelden van scripts voor installatie

Als u wilt eenvoudig in één keer de afhankelijkheid-Agent op veel servers implementeert, is het nuttig om een script gebruiken. U kunt de volgende scriptvoorbeelden downloaden en installeren van de Agent voor afhankelijkheden op Windows of Linux.

#### <a name="powershell-script-for-windows"></a>Windows PowerShell-script

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>Shell-script voor Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>Desired State Configuration

Voor het implementeren van de Agent voor afhankelijkheden via Desired State Configuration, kunt u de module xPSDesiredStateConfiguration en een deel van de code als volgt:

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install the Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-the-dependency-agent"></a>Verwijder de Agent afhankelijkheid

Gebruik de volgende secties voor hulp bij het verwijderen van de Agent voor afhankelijkheden.

#### <a name="uninstall-the-dependency-agent-on-windows"></a>Verwijder de Agent afhankelijkheid in Windows

Een beheerder kan de afhankelijkheid Agent voor Windows via het Configuratiescherm kunt verwijderen.

Een beheerder kan ook uitvoeren %Programfiles%\Microsoft afhankelijkheid Agent\Uninstall.exe de afhankelijkheid-Agent verwijderen.

#### <a name="uninstall-the-dependency-agent-on-linux"></a>Verwijder de Agent afhankelijkheid op Linux

De afhankelijkheid-Agent volledig verwijderen van Linux, moet u de agent zelf en de connector die wordt automatisch geïnstalleerd met de agent verwijderen. U kunt beide met behulp van de volgende opdracht uit één verwijderen:

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>Management packs

Wanneer Draadgegevens worden geleverd in een werkruimte voor logboekanalyse is geactiveerd, wordt een 300 KB management pack verzonden naar de Windows-servers in deze werkruimte. Als u System Center Operations Manager-agents in een [verbonden beheergroep](log-analytics-om-agents.md), het Afhankelijkheidsmonitor management pack van System Center Operations Manager is geïmplementeerd. Als de agents rechtstreeks verbonden zijn, levert logboekanalyse het management pack.

De naam van het management pack is Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Deze naar worden geschreven: %Programfiles%\Microsoft bewaking Agent\Agent\Health State\Management servicepacks. De gegevensbron die gebruikmaakt van het management pack: % Program files%\Microsoft bewaking Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="using-the-solution"></a>De oplossing gebruiken

**Installeren en configureren van de oplossing**

Gebruik de volgende informatie om te installeren en configureren van de oplossing.

- De oplossing Draadgegevens worden geleverd verkrijgt gegevens van computers met Windows Server 2012 R2, Windows 8.1 en latere besturingssystemen.
- Microsoft .NET Framework 4.0 of hoger is vereist op computers waarop u wilt verkrijgen kabel gegevens uit.
- De oplossing Draadgegevens worden geleverd aan met behulp van de procedure beschreven in werkruimte voor logboekanalyse toevoegen [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md). Er is geen verdere configuratie nodig.
- Als u weergeven van draadgegevens worden geleverd voor een specifieke oplossing wilt, moet u de oplossing die al is toegevoegd aan uw werkruimte hebben.

Nadat er agents zijn geïnstalleerd en u de oplossing hebt geïnstalleerd, wordt de kabel gegevens 2.0-tegel wordt weergegeven in de werkruimte.

> [!NOTE]
> Op dit moment moet u de OMS-portal om weer te geven draadgegevens worden geleverd. U niet de Azure portal gebruiken om weer te geven draadgegevens worden geleverd.

![Tegel draadgegevens worden geleverd](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-the-wire-data-20-solution"></a>Met behulp van de kabel gegevens 2.0-oplossing

Klik in de OMS-portal op de **kabel gegevens 2.0** tegel om de gegevens van de kabel dashboard te openen. Het dashboard bevat de blades in de volgende tabel. Elke blade bevat maximaal 10 items die overeenkomen met de criteria die blade voor het opgegeven bereik en tijdsbereik. U kunt een logboek-zoekquery waarmee alle records door te klikken op uitvoeren **alle** aan de onderkant van de blade of door te klikken op de blade-header.

| **Blade** | **Beschrijving** |
| --- | --- |
| Agents netwerkverkeer vastleggen | Toont het aantal agents die netwerkverkeer vastlegt en geeft een lijst van de eerste 10 computers die verkeer vastlegt. Klik op het aantal om uit te voeren een zoekopdracht logboek voor <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>. Klik op een computer in de lijst om te retourneren van het totale aantal bytes dat is vastgelegd logboek zoekopdracht uitvoert. |
| Lokale subnetten | Toont het aantal lokale subnetten die agents gedetecteerd.  Klik op het aantal om uit te voeren een zoekopdracht logboek voor <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> die een lijst met alle subnetten met het aantal bytes dat is verzonden via elke service. Klik op een subnet in de lijst om te retourneren van het totale aantal bytes dat is verzonden via het subnet logboek zoekopdracht uitvoert. |
| Protocollen op toepassingsniveau | Toont het aantal protocollen op toepassingsniveau in gebruik is, zoals gedetecteerd door agents. Klik op het aantal om uit te voeren een zoekopdracht logboek voor <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>. Klik op een protocol om uit te voeren een zoekopdracht logboek is het totale aantal bytes dat is verzonden via het protocol retourneren. |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Kabel gegevens dashboard](./media/log-analytics-wire-data/wire-data-dash.png)

U kunt de **Agents netwerkverkeer vastleggen** blade om te bepalen hoeveel netwerkbandbreedte wordt verbruikt door computers. Deze blade kunt u gemakkelijk terugvinden de _chattiest_ computer in uw omgeving. Deze computers kunnen worden overbelast abnormaal fungeert, of gebruik meer netwerkbronnen dan normaal.

![Voorbeeld van logboek zoeken](./media/log-analytics-wire-data/log-search-example01.png)

Op deze manier kunt u de **lokale subnetten** blade om te bepalen hoeveel netwerkverkeer via de subnetten verplaatsen. Gebruikers definiëren vaak subnetten rond de essentiële gebieden voor hun toepassingen. Deze blade biedt een overzicht van deze gebieden.

![Voorbeeld van logboek zoeken](./media/log-analytics-wire-data/log-search-example02.png)

De **protocollen op toepassingsniveau** blade is nuttig omdat het is nuttig weten welke protocollen worden gebruikt. U zou bijvoorbeeld verwachten SSH niet al in gebruik in uw netwerkomgeving. Weergeven van informatie die beschikbaar zijn in de blade snel wilt controleren of uw verwachting bewijzen.

![Voorbeeld van logboek zoeken](./media/log-analytics-wire-data/log-search-example03.png)

In dit voorbeeld kunt u inzoomen in SSH details om te zien welke computers zijn met SSH en veel andere communicatiegegevens.

![servicel zoekresultaten](./media/log-analytics-wire-data/ssh-details.png)

Het is ook handig om te weten als protocolverkeer is verhogen of gedurende een bepaalde periode verlagen. Bijvoorbeeld, als de hoeveelheid gegevens die worden verzonden door een toepassing stijgt, die mogelijk iets rekening houden met moet zijn of dat u wellicht opmerkelijk.

## <a name="input-data"></a>Invoergegevens

Kabel gegevens verzamelt metagegevens over het netwerkverkeer met behulp van de agents die u hebt ingeschakeld. Elke agent verzendt gegevens over elke 15 seconden.

## <a name="output-data"></a>uitvoergegevens

Een record met een type _WireData_ is gemaakt voor elk type invoergegevens. WireData-records hebben eigenschappen die in de volgende tabel worden weergegeven:

| Eigenschap | Beschrijving |
|---|---|
| Computer | De naam van de computer waar de gegevens zijn verzameld |
| TimeGenerated | Tijd van de record |
| LocalIP | IP-adres van de lokale computer |
| SessionState | Verbonden of verbroken |
| ReceivedBytes | Hoeveelheid ontvangen bytes |
| Protocolnaam | Naam van het netwerkprotocol gebruikt |
| IPVersion | IP-versie |
| Richting | Binnenkomend of uitgaand |
| MaliciousIP | IP-adres van een bekende schadelijke bron |
| Ernst | Ernst van de mogelijke malware |
| RemoteIPCountry | Land van het externe IP-adres |
| ManagementGroupName | Naam van de Operations Manager-beheergroep |
| SourceSystem | Bron waar de gegevens zijn verzameld |
| SessionStartTime | Begintijd van sessie |
| SessionEndTime | Eindtijd van sessie |
| LocalSubnet | Subnet waar de gegevens zijn verzameld |
| LocalPortNumber | Lokale poortnummer |
| RemoteIP | Extern IP-adres gebruikt door de externe computer |
| RemotePortNumber | Poortnummer van het externe IP-adres |
| Sessie-id | Een unieke waarde die sessie van de communicatie tussen twee IP-adressen aangeeft |
| SentBytes | Aantal verzonden bytes |
| TotalBytes | Totaal aantal bytes dat tijdens de sessie is verzonden |
| ApplicationProtocol | Type netwerkprotocol gebruikt   |
| Proces-id | Windows-proces-ID |
| Procesnaam | Pad en de naam van het proces |
| RemoteIPLongitude | IP-Lengtegraadwaarde |
| RemoteIPLatitude | IP-Breedtegraadwaarde |


## <a name="next-steps"></a>Volgende stappen

- [Zoeken in een logboek](log-analytics-log-searches.md) om gedetailleerde kabel gegevens zoeken records te bekijken.
