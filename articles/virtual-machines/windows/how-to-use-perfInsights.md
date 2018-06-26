---
title: Het gebruik van PerfInsights in Microsoft Azure | Microsoft Docs
description: Leert PerfInsights gebruiken voor het oplossen van prestatieproblemen met virtuele machine van Windows.
services: virtual-machines-windows'
documentationcenter: ''
author: anandhms
manager: cshepard
editor: na
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 2f496f906eef416b35e2e59b2db93481ce65acb1
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36946487"
---
# <a name="how-to-use-perfinsights"></a>Het gebruik van PerfInsights

[PerfInsights](http://aka.ms/perfinsightsdownload) is een hulpprogramma van zelfhulp diagnostische gegevens die worden verzameld en analyseert de diagnostische gegevens en biedt een rapport voor het oplossen van problemen met Windows virtuele machine de prestaties in Azure. PerfInsights kan worden uitgevoerd op virtuele machines als een zelfstandig hulpprogramma of rechtstreeks vanuit de portal door installeren [Azure prestaties diagnostische VM-extensie](performance-diagnostics-vm-extension.md).

Als u prestatieproblemen met virtuele machines, voordat u contact op met ondersteuning ondervindt, kunt u dit hulpprogramma uitvoeren.

## <a name="supported-troubleshooting-scenarios"></a>Ondersteunde scenario's voor het oplossen van problemen

PerfInsights kunt verzamelen en analyseren van verschillende soorten gegevens. Algemene scenario's hebben betrekking op de volgende secties.

### <a name="quick-performance-analysis"></a>Snelle prestatieanalyse

Dit scenario verzamelt configuratie van de schijf en andere belangrijke informatie, waaronder:

-   Gebeurtenislogboeken

-   De status van het netwerk voor alle binnenkomende en uitgaande verbindingen

-   Netwerk- en firewallinstellingen configuratie-instellingen

-   Lijst met taken voor alle toepassingen die momenteel worden uitgevoerd op het systeem

-   Microsoft SQL Server-database configuratie-instellingen (als de virtuele machine wordt geïdentificeerd als een server waarop SQL Server)

-   Opslag betrouwbaarheid prestatiemeteritems

-   Belangrijke hotfixes voor Windows

-   Geïnstalleerde filterstuurprogramma 's

Dit is een passief verzamelen van informatie die mag niet van invloed zijn op het systeem. 

>[!Note]
>Dit scenario is automatisch opgenomen in elk van de volgende scenario's:

### <a name="benchmarking"></a>Benchmarking

Dit scenario wordt uitgevoerd de [Diskspd](https://github.com/Microsoft/diskspd) benchmarktest (IOPS en MBPS) voor alle stations die zijn gekoppeld aan de virtuele machine. 

> [!Note]
> Dit scenario kan invloed hebben op het systeem en mag niet worden uitgevoerd op een live productiesysteem. Voer indien nodig in dit scenario in een speciale onderhoudsvenster om eventuele problemen te voorkomen. Een grotere werkbelasting die wordt veroorzaakt door een tracering of benchmark-test kan een nadelige invloed heeft op de prestaties van uw virtuele machine.
>

### <a name="slow-vm-analysis"></a>Analyse van trage VM

Dit scenario wordt uitgevoerd een [prestatiemeteritem](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) traceren met behulp van de items die zijn opgegeven in het bestand RuleEngineConfig.json. Als de virtuele machine wordt geïdentificeerd als een server waarop SQL Server wordt uitgevoerd, wordt een teller prestatietracering wordt uitgevoerd. Dit gebeurt met behulp van de items die zijn gevonden in het bestand RuleEngineConfig.json. Dit scenario omvat ook de prestatiegegevens van de diagnostische gegevens.

### <a name="azure-files-analysis"></a>Azure Files analyse

Dit scenario voert een speciale prestaties teller vastleggen samen met een netwerktracering maken. Het vastleggen omvat alle Server Message Block (SMB) client shares prestatiemeteritems. Hier volgen enkele belangrijke SMB-share clientprestatietellers die deel van het vastleggen uitmaken:

| **Type**     | **SMB-client shares teller** |
|--------------|-------------------------------|
| IOPS         | Gegevens aanvragen per seconde             |
|              | Aanvragen per seconde gelezen             |
|              | Schrijven van aanvragen per seconde            |
| Latentie      | Gem. per seconde/gegevensaanvraag         |
|              | Gemiddelde leestijd                 |
|              | Gemiddelde schrijftijd                |
| I/o-grootte      | Gem. Bytes/gegevensaanvraag       |
|              | Gem. Bytes/Read               |
|              | Gem. Geschreven bytes              |
| Doorvoer   | Gegevens Bytes per seconde                |
|              | Gelezen Bytes per seconde                |
|              | Geschreven Bytes per seconde               |
| Wachtrijlengte | Gem. Wachtrijlengte voor lezen        |
|              | Gem. Wachtrijlengte voor schrijven       |
|              | Gem. Wachtrijlengte van gegevens        |

### <a name="advanced-slow-vm-analysis"></a>Geavanceerde analyse van trage VM

Als u een geavanceerde analyse van trage VM uitvoert, selecteert u traceringen tegelijkertijd worden uitgevoerd. Als u wilt, kunt u ze op alle (prestatiemeteritem, Xperf, netwerk en StorPort) uitvoeren.  

> [!Note]
> Dit scenario kan invloed hebben op het systeem en mag niet worden uitgevoerd op een live productiesysteem. Voer indien nodig in dit scenario in een speciale onderhoudsvenster om eventuele problemen te voorkomen. Een grotere werkbelasting die wordt veroorzaakt door een tracering of benchmark-test kan een nadelige invloed heeft op de prestaties van uw virtuele machine.
>

## <a name="what-kind-of-information-is-collected-by-perfinsights"></a>Wat voor soort informatie wordt verzameld door PerfInsights?

Registreert informatie over de virtuele machine van Windows, schijven of groepen opslagconfiguratie, prestatiemeteritems, en verschillende traceringen worden verzameld. Dit is afhankelijk van de prestaties scenario dat u gebruikt. De volgende tabel biedt details:

|Gegevens die worden verzameld                              |  |  | Scenario's voor prestaties |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                               | Snelle prestatieanalyse | Benchmarking | Analyse van trage VM | Azure Files analyse | Geavanceerde analyse van trage VM |
| Gegevens van gebeurtenislogboeken       | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Informatie over het bestandssysteem                | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Volume-kaart                        | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Schijf-kaart                          | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Actieve taken                     | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Opslag betrouwbaarheid prestatiemeteritems      | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Storage-gegevens               | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Fsutil uitvoer                     | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Filterinformatie stuurprogramma                | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Netstat-uitvoer                    | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Netwerkconfiguratie             | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Firewall-configuratie            | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Configuratie van SQL Server          | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Prestaties diagnostische traceringen *  | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Teller prestatietracering **      |                            |                                    | Ja                      |                      | Ja                  |
| SMB-prestatiemeteritems trace **              |                            |                                    |                          | Ja                  |                      |
| SQL Server teller trace **       |                            |                                    | Ja                      |                      | Ja                  |
| XPerf tracering                       |                            |                                    |                          |                      | Ja                  |
| StorPort-tracering                    |                            |                                    |                          |                      | Ja                  |
| Netwerktracering                     |                            |                                    |                          | Ja                  | Ja                  |
| Diskspd benchmark trace ***       |                            | Ja                                |                          |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Diagnostische traceerlogboeken voor prestaties (*)

Een op regels gebaseerde engine op de achtergrond voor het verzamelen van gegevens en prestatieproblemen analyseren voortdurend uitgevoerd. De volgende regels worden momenteel ondersteund:

- HighCpuUsage regel: hoog CPU-gebruik perioden detecteert en de bovenste consumenten van de CPU-gebruik tijdens deze perioden weergegeven.
- HighDiskUsage regel: hoge schijf gebruik punten op fysieke schijven detecteert en ziet u de eerste schijf gebruik consumenten tijdens deze perioden.
- HighResolutionDiskMetric regel: toont IOPS, doorvoer en i/o-latentie metrische gegevens per 50 milliseconden voor elke fysieke schijf. Het helpt snel schijf perioden beperking kan identificeren.
- HighMemoryUsage regel: veel geheugen gebruik perioden detecteert en het bovenste geheugen gebruik consumenten tijdens deze perioden weergegeven.

> [!NOTE] 
> Windows-versies met .NET Framework 3.5 of hoger worden momenteel ondersteund.

### <a name="performance-counter-trace-"></a>Teller prestatietracering (*)

Verzamelt de volgende prestatiemeteritems:

- \Process, \Processor \Memory, \Thread, \PhysicalDisk en \LogicalDisk
- \Cache\Dirty pagina's, \Cache\Lazy schrijfbewerkingen per seconde, \Server\Pool wisselbaar, storingen, en \Server\Pool wisselbaar geheugen:
- Geselecteerde items onder \Network Interface, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network Adapter, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \ QoS-Policy\Packets \Per Network Interface Card processoractiviteit en \Microsoft Winsock BSP-netwerk

#### <a name="for-sql-server-instances"></a>Voor SQL Server-exemplaren
- \Sql server: bufferbeheer \SQLServer:Resource groep statistieken en \SQLServer:SQL Statistics\
- \SQLServer:locks, \SQLServer:General, statistieken
- \SQLServer:Access methoden

#### <a name="for-azure-files"></a>Voor Azure-bestanden
\SMB Clientshares

### <a name="diskspd-benchmark-trace-"></a>Diskspd benchmark trace (*)
Diskspd i/o-werkbelasting tests (Besturingssysteemschijf [schrijven] en groep stations [lezen/schrijven])

## <a name="run-the-perfinsights-tool-on-your-vm"></a>Het hulpprogramma PerfInsights uitvoeren op uw virtuele machine

### <a name="what-do-i-have-to-know-before-i-run-the-tool"></a>Wat heb ik moet weten voordat ik het hulpprogramma uitvoeren? 

#### <a name="tool-requirements"></a>Vereisten voor hulpprogramma

-  Dit hulpprogramma moet worden uitgevoerd op de virtuele machine met het prestatieprobleem. 

-  De volgende besturingssystemen worden ondersteund: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016; Windows 8.1 en Windows 10.

#### <a name="possible-problems-when-you-run-the-tool-on-production-vms"></a>Mogelijke problemen wanneer u het hulpprogramma voor productie-virtuele machines uitvoeren

-  Voor het scenario benchmarking of het 'Advanced trage VM Analysis'-scenario dat is geconfigureerd voor gebruik van Xperf of Diskspd, het hulpprogramma mogelijk nadelige invloed heeft op de prestaties van de virtuele machine. Deze scenario's moeten niet worden uitgevoerd in een werkende productieomgeving.

-  Zorg ervoor dat er geen andere activiteit op de achtergrond de i/o-werkbelasting verstoort voor het scenario benchmarking of het 'Advanced trage VM Analysis'-scenario dat is geconfigureerd voor gebruik van Diskspd.

-  Het hulpprogramma maakt standaard gebruik van het station voor tijdelijke opslag om gegevens te verzamelen. Als u tracering blijft ingeschakeld gedurende een langere periode, kan de hoeveelheid gegevens die worden verzameld relevant zijn. Dit verkleint de beschikbaarheid van de tijdelijke schijfruimte en kan daarom van invloed op alle toepassingen die afhankelijk van dit station is.

### <a name="how-do-i-run-perfinsights"></a>Hoe kan ik PerfInsights uitvoeren? 

U kunt PerfInsights uitvoeren op een virtuele machine door het installeren van [Azure prestaties diagnostische VM-extensie](performance-diagnostics-vm-extension.md). U kunt deze ook uitvoeren als een zelfstandig hulpprogramma. 

**Installeren en PerfInsights uitvoeren vanuit de Azure-portal**

Zie voor meer informatie over deze optie [installeren Azure prestaties VM extensie voor diagnostische gegevens](performance-diagnostics-vm-extension.md#install-the-extension).  

**PerfInsights uitvoeren in de zelfstandige modus**

Voor het uitvoeren van het hulpprogramma PerfInsights, de volgende stappen uit:


1. Download [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Het bestand PerfInsights.zip deblokkeren. U doet dit door met de rechtermuisknop op het bestand PerfInsights.zip en selecteer **eigenschappen**. In de **algemene** tabblad **blokkering**, en selecteer vervolgens **OK**. Dit zorgt ervoor dat het hulpprogramma wordt uitgevoerd zonder extra beveiliging wordt gevraagd.  

    ![Schermopname van PerfInsights eigenschappen met het opheffen van blokkeringen gemarkeerd](media/how-to-use-perfInsights/unlock-file.png)

3.  Het gecomprimeerde PerfInsights.zip-bestand naar de tijdelijke schijf uitbreiden (standaard dit is meestal het D-station). 

4.  Open Windows-opdrachtprompt als beheerder en voer vervolgens PerfInsights.exe om de beschikbare commandline parameters weer te geven.

    ```
    cd <the path of PerfInsights folder>
    PerfInsights
    ```
    ![Schermopname van PerfInsights commandline uitvoer geproduceerd](media/how-to-use-perfInsights/PerfInsightsCommandline.png)
    
    De syntaxis van de basis voor het uitvoeren van PerfInsights scenario's is:
    
    ```
    PerfInsights /run <ScenarioName> [AdditionalOptions]
    ```

    U kunt het onderstaande voorbeeld van trage VM scenario voor 5 minuten worden uitgevoerd:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    Het volgende voorbeeld kunt u de geavanceerde scenario met Xperf en prestaties van de teller traceringen, 5 minuten worden uitgevoerd:
    
    ```
    PerfInsights /run advanced xp /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    U kunt het onderstaande voorbeeld trage VM scenario voor 5 minuten worden uitgevoerd en het resultaat zip-bestand uploaden naar het storage-account:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics /sa <StorageAccountName> /sk <StorageAccountKey>
    ```

    U de beschikbare scenario's en opties kunt opzoeken met behulp van de **/lijst** opdracht:
    
    ```
    PerfInsights /list
    ```

    >[!Note]
    >Voordat u een scenario uitvoert, vraagt PerfInsights de gebruiker voor het delen van diagnostische gegevens en ga akkoord om de gebruiksrechtovereenkomst accepteren. Gebruik **/AcceptDisclaimerAndShareDiagnostics** optie voor het overslaan van deze vragen. 
    >
    >Als u een actieve ondersteuningsticket met Microsoft en actieve PerfInsights per verzoek van de ondersteuningstechnicus u met werkt hebt, zorg voor de ondersteuning ticket nummer met behulp van de **/sr** optie.
    >
    >Standaard probeert PerfInsights zichzelf bijwerken naar de nieuwste versie, indien beschikbaar. Gebruik **/SkipAutoUpdate** of **/sau** -parameter voor het overslaan van automatische updates.  
    >
    >Als de duur overschakelt **/d** niet is opgegeven, PerfInsights wordt u gevraagd om te reproduceren het probleem tijdens het uitvoeren van vmslow, azurefiles en geavanceerde scenario's. 

Wanneer de traceringen of bewerkingen zijn voltooid, verschijnt er een nieuw bestand in dezelfde map als PerfInsights. De naam van het bestand is **CollectedData\_jjjj-MM-dd\_hh mm-ss fff.zip.** U kunt dit bestand naar de agent ondersteuning voor analyse sturen of opent u het rapport in het zip-bestand om te controleren bevindingen en aanbevelingen.

## <a name="review-the-diagnostics-report"></a>Bekijk het rapport diagnostische gegevens

Binnen de **CollectedData\_jjjj-MM-dd\_hh mm-ss fff.zip** -bestand, kunt u een HTML-rapport met details van de resultaten van PerfInsights vinden. Als u wilt controleren in het rapport, vouw de **CollectedData\_jjjj-MM-dd\_hh mm-ss fff.zip** bestand en open vervolgens de **PerfInsights Report.html** bestand.

Selecteer de **bevindingen** tabblad.

![Schermopname van rapport PerfInsights](media/how-to-use-perfInsights/findingtab.png)
![schermopname van rapport PerfInsights](media/how-to-use-perfInsights/findings.PNG)

> [!NOTE] 
> Bevindingen gecategoriseerd als hoog zijn bekende problemen die prestatieproblemen kunnen veroorzaken. Bevindingen gecategoriseerd als Gemiddeld vertegenwoordigen niet optimaal configuraties die niet noodzakelijkerwijs prestatieproblemen veroorzaken. Bevindingen aangemerkt als lage zijn alleen informatief instructies.

Bekijk de aanbevelingen en koppelingen naar alle hoge en gemiddelde bevindingen. Meer informatie over hoe ze invloed hebben op prestaties en over best practices voor optimale prestaties van configuraties.

### <a name="storage-tab"></a>Tabblad opslag

De **bevindingen** sectie vindt u verschillende bevindingen en aanbevelingen met betrekking tot opslag.

De **schijf kaart** en **Volume kaart** secties beschrijven hoe logische volumes en fysieke schijven aan elkaar zijn gerelateerd.

In het perspectief van het fysieke schijf (schijf kaart) ziet de tabel u alle logische volumes die worden uitgevoerd op de schijf. In het volgende voorbeeld **PhysicalDrive2** twee logische volumes die zijn gemaakt op meerdere partities (J en H) wordt uitgevoerd:

![Schermafbeelding van tabblad schijf](media/how-to-use-perfInsights/disktab.png)

De tabellen geven in het perspectief van het volume (kaart Volume), de fysieke schijven onder elke logische volume. U ziet voor RAID/dynamische schijven, kunt u een logisch volume uitvoeren op meerdere fysieke schijven. In het volgende voorbeeld *C:\\koppelen* een koppelpunt is geconfigureerd als *SpannedDisk* op fysieke schijven 2 en 3:

![Schermafbeelding van tabblad volume](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-tab"></a>SQL-tabblad

Als het doel-virtuele machine fungeert als host voor alle exemplaren van SQL Server, ziet u een extra tabblad in het rapport met de naam **SQL**:

![Schermafbeelding van de SQL-tabblad](media/how-to-use-perfInsights/sqltab.png)

Deze sectie bevat een **bevindingen** tabbladen en extra tabbladen voor elk van de SQL Server-exemplaren die worden gehost op de virtuele machine.

De **bevindingen** tabblad bevat een lijst van alle SQL gerelateerd prestatieproblemen gevonden, samen met de aanbevelingen.

In het volgende voorbeeld **PhysicalDrive0** (het station C wordt uitgevoerd) wordt weergegeven. Dit is omdat zowel de **modeldev** en **modellog** bestanden bevinden zich op station C, en ze zijn van verschillende typen (zoals bestands- en transactie gegevenslogboek, respectievelijk).

![Schermafbeelding van de informatie in het foutenlogboek](media/how-to-use-perfInsights/loginfo.png)

De tabbladen voor specifieke exemplaren van SQL Server bevatten een algemene sectie waarin algemene informatie over het geselecteerde exemplaar. De tabbladen bevatten ook extra gedeelten voor gedetailleerde informatie, waaronder instellingen, configuraties en opties voor gebruikers.

### <a name="diagnostic-tab"></a>Diagnostische tabblad
De **diagnostische** tabblad bevat informatie over bovenste consumenten van CPU, schijf en geheugen op de computer voor de duur van het uitvoeren van PerfInsights. U kunt ook informatie over essentiële patches vinden dat het systeem mogelijk ontbreekt de lijst met taken en belangrijke systeemgebeurtenissen. 

## <a name="references-to-the-external-tools-used"></a>Verwijzingen naar de externe hulpprogramma's gebruikt

### <a name="diskspd"></a>Diskspd

Diskspd is een opslag laden en prestaties van de generator test hulpprogramma van Microsoft. Zie voor meer informatie [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

XPerf is een opdrachtregelprogramma voor het vastleggen van traces van de werkset van de prestaties van Windows. Zie voor meer informatie [Windows prestaties Toolkit – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Volgende stappen

U kunt diagnostische logboeken en rapporten uploaden naar Microsoft Support om verder te bestuderen. Ondersteuning vragen om het verzenden van de uitvoer die wordt gegenereerd door PerfInsights om te helpen bij het proces voor het oplossen van problemen.

De volgende schermafbeelding ziet een bericht vergelijkbaar met wat kan worden weergegeven:

![Schermafbeelding van de voorbeeldbericht van Microsoft Support](media/how-to-use-perfInsights/supportemail.png)

Volg de instructies in het bericht voor de overdracht bestand werkruimte. Voor extra beveiliging die u moet uw wachtwoord wijzigen op het eerste gebruik.

Nadat u zich aanmeldt, vindt u een dialoogvenster voor het uploaden van de **CollectedData\_jjjj-MM-dd\_hh mm-ss fff.zip** -bestand dat is verzameld door PerfInsights.
