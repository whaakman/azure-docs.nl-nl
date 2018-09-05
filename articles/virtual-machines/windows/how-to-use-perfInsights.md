---
title: PerfInsights gebruiken in Microsoft Azure | Microsoft Docs
description: Leert hoe u met PerfInsights problemen met Windows-VM prestaties oplossen.
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
ms.openlocfilehash: 1f3a24cebe5061f7e3ca3897692b068531780431
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43668118"
---
# <a name="how-to-use-perfinsights"></a>PerfInsights gebruiken

[PerfInsights](http://aka.ms/perfinsightsdownload) is een hulpprogramma voor zelfhulp diagnostische gegevens die u verzamelt en analyseert de diagnostische gegevens en biedt een rapport voor het oplossen van problemen met prestaties van Windows-virtuele machine in Azure. PerfInsights kan worden uitgevoerd op virtuele machines als een zelfstandig hulpprogramma, of rechtstreeks vanuit de portal door te installeren [Azure prestaties diagnostische VM-extensie](performance-diagnostics-vm-extension.md).

Als u problemen met de prestaties met virtuele machines, ondervindt voordat u contact opnemen met ondersteuning, moet u dit hulpprogramma uitvoeren.

## <a name="supported-troubleshooting-scenarios"></a>Ondersteunde scenario's voor het oplossen van problemen

PerfInsights kunt verzamelen en analyseren van verschillende soorten informatie. De volgende secties voor algemene scenario's.

### <a name="quick-performance-analysis"></a>Analyse van snelle prestaties

In dit scenario worden verzameld voor de configuratie van de schijf en andere belangrijke gegevens, met inbegrip van:

-   Gebeurtenislogboeken

-   Netwerkstatus voor alle inkomende en uitgaande verbindingen

-   Netwerk- en firewallinstellingen configuratie-instellingen

-   Lijst met taken voor alle toepassingen die momenteel worden uitgevoerd op het systeem

-   Microsoft SQL Server-database configuratie-instellingen (als de virtuele machine wordt geïdentificeerd als een server waarop SQL Server wordt uitgevoerd)

-   Tellers voor opslag-betrouwbaarheid

-   Belangrijke Windows-hotfixes

-   Geïnstalleerde filterstuurprogramma 's

Dit is een passieve verzamelen van informatie die mag niet van invloed zijn op het systeem. 

>[!Note]
>In dit scenario wordt automatisch opgenomen in elk van de volgende scenario's:

### <a name="benchmarking"></a>Benchmarking

In dit scenario wordt uitgevoerd de [Diskspd](https://github.com/Microsoft/diskspd) benchmarktest (IOPS en MBPS) voor alle stations die zijn gekoppeld aan de virtuele machine. 

> [!Note]
> In dit scenario kan invloed hebben op het systeem en mag niet worden uitgevoerd op een live productieomgeving. Indien nodig, kunt u dit scenario uitvoeren in een speciale onderhoudsvenster om eventuele problemen te voorkomen. Een grotere werkbelasting die wordt veroorzaakt door een trace- of -benchmark-test kan nadelige invloed heeft op de prestaties van uw virtuele machine.
>

### <a name="performance-analysis"></a>Analyse van prestaties

In dit scenario wordt uitgevoerd een [prestatiemeteritem](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) traceren met behulp van de items die zijn opgegeven in het bestand RuleEngineConfig.json. Als de virtuele machine wordt geïdentificeerd als een server waarop SQL Server wordt uitgevoerd, wordt een prestaties teller tracering wordt uitgevoerd. Dit gebeurt met behulp van de items die zijn gevonden in het bestand RuleEngineConfig.json. Dit scenario omvat ook prestatiegegevens van diagnostische gegevens.

### <a name="azure-files-analysis"></a>Azure bestanden analysis

In dit scenario wordt een speciale prestaties teller vastleggen, samen met een netwerktracering uitgevoerd. De opname omvat alle Server Message Block (SMB) client shares prestatiemeteritems. Hier volgen enkele belangrijke SMB-share clientprestatietellers die deel van het vastleggen uitmaken:

| **Type**     | **SMB-client shares teller** |
|--------------|-------------------------------|
| IOPS         | Gegevens aanvragen/sec             |
|              | Gelezen aanvragen per seconde             |
|              | Schrijven van aanvragen/sec            |
| Latentie      | Gem. per seconde/gegevensaanvraag         |
|              | Gemiddelde leestijd                 |
|              | Gemiddelde schrijftijd                |
| I/o-grootte      | Gem. Aanvraag voor bytes/gegevens       |
|              | Gem. Bytes/Read               |
|              | Gem. Bytes/schrijven              |
| Doorvoer   | Gegevens Bytes per seconde                |
|              | Gelezen Bytes per seconde                |
|              | Geschreven Bytes per seconde               |
| Lengte van wachtrij | Gem. Wachtrijlengte voor lezen        |
|              | Gem. Lengte van wachtrij schrijven       |
|              | Gem. Wachtrijlengte van gegevens        |

### <a name="advanced-performance-analysis"></a>Geavanceerde analyse

Als u een geavanceerde prestatieanalyse uitvoert, selecteert u traceringen om parallel uit voeren. Als u wilt, kunt u ze op alle (prestatiemeteritems, Xperf, netwerk en StorPort) uitvoeren.  

> [!Note]
> In dit scenario kan invloed hebben op het systeem en mag niet worden uitgevoerd op een live productieomgeving. Indien nodig, kunt u dit scenario uitvoeren in een speciale onderhoudsvenster om eventuele problemen te voorkomen. Een grotere werkbelasting die wordt veroorzaakt door een trace- of -benchmark-test kan nadelige invloed heeft op de prestaties van uw virtuele machine.
>

## <a name="what-kind-of-information-is-collected-by-perfinsights"></a>Wat voor soort informatie wordt verzameld door PerfInsights?

Informatie over Windows-VM, schijven of pools opslagconfiguratie, prestatiemeteritems, logboeken en verschillende traceringen worden verzameld. Dat hangt ervan af op de prestaties scenario dat u gebruikt. De volgende tabel bevat details:

|Gegevens die zijn verzameld                              |  |  | Performance-scenario 's |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                               | Analyse van snelle prestaties | Benchmarking | Analyse van prestaties | Azure bestanden analysis | Geavanceerde analyse |
| Gegevens uit de gebeurtenislogboeken       | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Systeemgegevens                | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Volume-kaart                        | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Schijf-kaart                          | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Actieve taken                     | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Tellers voor opslag-betrouwbaarheid      | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Opslaggegevens               | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Fsutil-uitvoer                     | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Stuurprogramma-informatie filteren                | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Netstat-uitvoer                    | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Netwerkconfiguratie             | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Firewall-configuratie            | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| SQL Server-configuratie          | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Prestaties diagnostische traceringen *  | Ja                        | Ja                                | Ja                      | Ja                  | Ja                  |
| Prestaties teller trace **      |                            |                                    | Ja                      |                      | Ja                  |
| SMB-prestatiemeteritems trace **              |                            |                                    |                          | Ja                  |                      |
| SQL Server-teller tracering **       |                            |                                    | Ja                      |                      | Ja                  |
| XPerf trace                       |                            |                                    |                          |                      | Ja                  |
| StorPort-tracering                    |                            |                                    |                          |                      | Ja                  |
| Netwerktracering                     |                            |                                    |                          | Ja                  | Ja                  |
| Diskspd benchmark trace ***       |                            | Ja                                |                          |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Diagnostische traceerlogboeken voor prestaties (*)

Een op regels gebaseerde engine op de achtergrond voor het verzamelen van gegevens en doorlopende prestatieproblemen uitgevoerd. De volgende regels worden momenteel ondersteund:

- HighCpuUsage regel: detecteert perioden van hoge CPU-gebruik en de bovenste verbruikers van de CPU-gebruik tijdens deze perioden wordt weergegeven.
- HighDiskUsage regel: perioden van intensief gebruik van fysieke schijven worden gedetecteerd en ziet u de eerste schijf gebruik consumenten tijdens deze perioden.
- HighResolutionDiskMetric regel: toont IOPS, doorvoer en i/o latentie metrische gegevens per 50 milliseconden voor elke fysieke schijf. Zo kunt u snel identificeren schijf beperking perioden.
- HighMemoryUsage regel: hoge geheugen-gebruik perioden detecteert en ziet u de bovenste geheugen gebruik consumenten tijdens deze perioden.

> [!NOTE] 
> Windows-versies met de .NET Framework 4.5 of hoger worden momenteel ondersteund.

### <a name="performance-counter-trace-"></a>Prestaties teller tracering (*)

Verzamelt de volgende prestatiemeteritems:

- \Process, \Processor \Memory, \Thread, \PhysicalDisk en \LogicalDisk
- \Cache\Dirty pagina's, \Cache\Lazy schrijfbewerkingen per seconde, \Server\Pool niet-wisselbaar geheugen, storingen, en \Server\Pool wisselbaar geheugen:
- Geselecteerde items onder \Network-Interface, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network Adapter, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \ Netwerk-QoS Policy\Packets \Per Network Interface Card processoractiviteit en \Microsoft Winsock BSP

#### <a name="for-sql-server-instances"></a>Voor SQL Server-exemplaren
- \Sql server: Buffer Manager, \SQLServer:Resource Pool statistieken en \SQLServer:SQL Statistics\
- \SQLServer:locks, \SQLServer:General, statistieken
- \SQLServer:Access methoden

#### <a name="for-azure-files"></a>Voor Azure Files
\SMB Clientshares

### <a name="diskspd-benchmark-trace-"></a>Diskspd benchmark tracering (*)
Diskspd i/o-werkbelasting tests uit (Besturingssysteemschijf [schrijven] en groep-stations [lezen/schrijven])

## <a name="run-the-perfinsights-tool-on-your-vm"></a>Het hulpprogramma PerfInsights uitvoeren op uw virtuele machine

### <a name="what-do-i-have-to-know-before-i-run-the-tool"></a>Wat heb ik moet weten voordat ik het hulpprogramma uitvoeren? 

#### <a name="tool-requirements"></a>Hulpprogramma-vereisten

-  Dit hulpprogramma moet worden uitgevoerd op de virtuele machine waarop het prestatieprobleem. 

-  De volgende besturingssystemen worden ondersteund: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016; Windows 8.1 en Windows 10.

#### <a name="possible-problems-when-you-run-the-tool-on-production-vms"></a>Mogelijke problemen wanneer u het hulpprogramma op de productie-VM's uitvoeren

-  Voor het scenario voor benchmarking of het 'Geavanceerde analyse van prestaties'-scenario dat is geconfigureerd voor het gebruik van Xperf of Diskspd, het hulpprogramma mogelijk nadelige invloed heeft op de prestaties van de virtuele machine. Deze scenario's moeten niet worden uitgevoerd in een live productieomgeving.

-  Voor het scenario voor benchmarking of het 'Geavanceerde analyse van prestaties'-scenario dat is geconfigureerd voor het gebruik van Diskspd, moet u ervoor zorgen dat er geen andere activiteit op de achtergrond de i/o-werkbelasting verstoort.

-  Het hulpprogramma gebruikt standaard de tijdelijke opslagschijf voor het verzamelen van gegevens. Als tracering ingeschakeld voor een langere tijd blijft, is de hoeveelheid gegevens die worden verzameld mogelijk relevant kunnen zijn. Dit kan de beschikbaarheid van de ruimte op de tijdelijke schijf verminderen en kan daarom invloed hebben op elke toepassing die is gebaseerd op dit station.

### <a name="how-do-i-run-perfinsights"></a>Hoe kan ik PerfInsights uitvoeren? 

U kunt PerfInsights uitvoeren op een virtuele machine door het installeren van [Azure prestaties diagnostische VM-extensie](performance-diagnostics-vm-extension.md). U kunt deze ook uitvoeren als een zelfstandig hulpprogramma. 

**Installeren en uitvoeren van PerfInsights vanuit Azure portal**

Zie voor meer informatie over deze optie [installeren Azure prestaties diagnostische VM-extensie](performance-diagnostics-vm-extension.md#install-the-extension).  

**PerfInsights uitvoeren in de zelfstandige modus**

Als u wilt de PerfInsights-hulpprogramma uitvoert, de volgende stappen uit:


1. Download [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Het bestand PerfInsights.zip de blokkering opheffen. U doet dit door met de rechtermuisknop op het bestand PerfInsights.zip en selecteer **eigenschappen**. In de **algemene** tabblad **opheffen van blokkeringen**, en selecteer vervolgens **OK**. Dit zorgt ervoor dat het hulpprogramma wordt uitgevoerd zonder eventuele extra beveiliging wordt gevraagd.  

    ![Schermafbeelding van PerfInsights eigenschappen, met het opheffen van blokkeringen gemarkeerd](media/how-to-use-perfInsights/unlock-file.png)

3.  Het gecomprimeerde bestand in de PerfInsights.zip uitbreiden naar de tijdelijke schijf (standaard, dit is doorgaans station D bevindt). 

4.  Open Windows-opdrachtprompt als beheerder en voer PerfInsights.exe om de beschikbare commandline parameters weer te geven.

    ```
    cd <the path of PerfInsights folder>
    PerfInsights
    ```
    ![Schermafbeelding van PerfInsights commandline uitvoer geproduceerd](media/how-to-use-perfInsights/PerfInsightsCommandline.png)
    
    De eenvoudige syntaxis voor het uitvoeren van PerfInsights scenario's is:
    
    ```
    PerfInsights /run <ScenarioName> [AdditionalOptions]
    ```

    U kunt het onderstaande voorbeeld om uit te voeren van prestaties analysis scenario voor 5 minuten:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    Het volgende voorbeeld kunt u het geavanceerde scenario met Xperf en prestaties teller traceringen voor 5 minuten uitvoeren:
    
    ```
    PerfInsights /run advanced xp /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    U kunt het onderstaande voorbeeld prestaties analysis scenario voor 5 minuten uitvoeren en het resultaat zip-bestand uploaden naar het storage-account:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics /sa <StorageAccountName> /sk <StorageAccountKey>
    ```

    U alle beschikbare scenario's en opties kunt opzoeken met behulp van de **/lijst** opdracht:
    
    ```
    PerfInsights /list
    ```

    >[!Note]
    >Voordat u een scenario, vraagt PerfInsights de gebruiker gaat ermee akkoord om diagnostische gegevens te delen en om de gebruiksrechtovereenkomst te accepteren. Gebruik **/AcceptDisclaimerAndShareDiagnostics** optie voor het overslaan van deze vragen. 
    >
    >Als u een actieve ondersteuningsticket bij Microsoft en actieve PerfInsights per verzoek van de ondersteuningsmedewerker u met werkt hebt, zorg ervoor dat voor de ondersteuning van ticket getal met behulp van de **/sr** optie.
    >
    >Standaard probeert PerfInsights zichzelf bijwerken naar de nieuwste versie, indien beschikbaar. Gebruik **/SkipAutoUpdate** of **/sau** parameter om over te slaan voor automatische updates.  
    >
    >Als de duur van de switch **/d** niet is opgegeven, PerfInsights wordt gevraagd u aan om te reproduceren het probleem tijdens het uitvoeren van vmslow, Azure Files en geavanceerde scenario's. 

Wanneer de traceringen of bewerkingen zijn voltooid, wordt een nieuw bestand in dezelfde map als PerfInsights weergegeven. De naam van het bestand is **PerformanceDiagnostics\_jjjj-MM-dd\_hh: mm: ss-fff.zip.** U kunt dit bestand verzenden naar de agent ondersteuning voor analyse of open het rapport in het zip-bestand bevindingen en aanbevelingen te bekijken.

## <a name="review-the-diagnostics-report"></a>Bekijk het rapport diagnostische gegevens

Binnen de **PerformanceDiagnostics\_jjjj-MM-dd\_hh: mm: ss-fff.zip** -bestand, kunt u een HTML-rapport waarin wordt uitgelegd van de resultaten van de PerfInsights vinden. Als u wilt controleren van het rapport, vouw de **PerformanceDiagnostics\_jjjj-MM-dd\_hh: mm: ss-fff.zip** bestand en open vervolgens de **PerfInsights Report.html** bestand.

Selecteer de **bevindingen** tabblad.

![Schermopname van het rapport PerfInsights](media/how-to-use-perfInsights/findingtab.png)
![Screenshot van PerfInsights-rapport](media/how-to-use-perfInsights/findings.PNG)

> [!NOTE] 
> Bevindingen gecategoriseerd als hoog zijn bekende problemen die prestatieproblemen kunnen veroorzaken. Bevindingen gecategoriseerd als normaal vertegenwoordigen niet-optimale configuraties die niet noodzakelijkerwijs prestatieproblemen veroorzaken. Bevindingen ingedeeld als laag zijn alleen informatieve instructies.

Bekijk de aanbevelingen en koppelingen voor alle hoge en gemiddelde bevindingen. Meer informatie over hoe ze invloed hebben op prestaties en over aanbevolen procedures voor optimale prestaties configuraties.

### <a name="storage-tab"></a>Tabblad opslag

De **bevindingen** sectie vindt u verschillende bevindingen en aanbevelingen met betrekking tot opslag.

De **schijf kaart** en **Volume kaart** secties beschrijven hoe logische volumes en fysieke schijven aan elkaar zijn gerelateerd.

In het perspectief van het fysieke schijf (schijf-Map) ziet de tabel u alle logische volumes die worden uitgevoerd op de schijf. In het volgende voorbeeld **PhysicalDrive2** twee logische volumes die zijn gemaakt op meerdere partities (J en H) wordt uitgevoerd:

![Schermafbeelding van tabblad schijf](media/how-to-use-perfInsights/disktab.png)

In het perspectief van het volume (Volume-Map) ziet de tabellen u de fysieke schijven in elke logische volume. U ziet dat voor RAID/dynamische schijven, u kunt een logische volume uitvoeren op meerdere fysieke schijven. In het volgende voorbeeld *C:\\koppelen* een koppelpunt is geconfigureerd als *SpannedDisk* op fysieke schijven met 2 en 3:

![Schermafbeelding van tabblad volume](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-tab"></a>SQL-tabblad

Als de doel-VM als host fungeert voor een SQL Server-exemplaren, ziet u een extra tabblad in het rapport met de naam **SQL**:

![Schermafbeelding van de SQL-tabblad](media/how-to-use-perfInsights/sqltab.png)

In deze sectie bevat een **bevindingen** tabblad, en de aanvullende tabbladen voor elk van de SQL Server-exemplaren die worden gehost op de virtuele machine.

De **bevindingen** tabblad bevat een lijst van alle SQL prestatieproblemen gevonden, samen met de aanbevelingen die betrekking hebben.

In het volgende voorbeeld **PhysicalDrive0** (het C-station wordt uitgevoerd) wordt weergegeven. Dit komt doordat zowel de **modeldev** en **modellog** bestanden bevinden zich op station C, en ze zijn van verschillende typen (zoals bestands- en transactie gegevenslogboek, respectievelijk).

![Schermafbeelding van logboekgegevens](media/how-to-use-perfInsights/loginfo.png)

De tabbladen voor specifieke exemplaren van SQL Server bevatten een algemene sectie waarin algemene informatie over het geselecteerde exemplaar worden weergegeven. De tabbladen bevatten ook extra gedeelten voor geavanceerde informatie, waaronder instellingen, configuraties en opties voor gebruikers.

### <a name="diagnostic-tab"></a>Tabblad Diagnostische
De **diagnostische** tabblad bevat informatie over bovenste verbruikers van CPU, schijf en geheugen op de computer voor de duur van het uitvoeren van PerfInsights. U vindt hier ook informatie over essentiële patches dat het systeem mogelijk ontbreekt de lijst met taken en belangrijke systeemgebeurtenissen. 

## <a name="references-to-the-external-tools-used"></a>Verwijzingen naar de externe hulpprogramma's die worden gebruikt

### <a name="diskspd"></a>Diskspd

Diskspd is een opslag load-generator en prestaties testen hulpprogramma van Microsoft. Zie voor meer informatie, [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

XPerf is een opdrachtregelprogramma voor het vastleggen van traces uit de werkset van de prestaties van Windows. Zie voor meer informatie, [Windows prestaties Toolkit – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Volgende stappen

U kunt Logboeken met diagnostische gegevens en rapporten uploaden naar Microsoft Support om verder te bestuderen. Ondersteuning kan u vragen die u de uitvoer die wordt gegenereerd door PerfInsights verzendt om te helpen bij het proces voor het oplossen van problemen.

De volgende schermafbeelding ziet een bericht vergelijkbaar met wat u mogelijk ontvangt:

![Schermopname van voorbeeldbericht van Microsoft Support](media/how-to-use-perfInsights/supportemail.png)

Volg de instructies in het bericht voor toegang tot het bestand transfer-werkruimte. Voor extra beveiliging die u moet uw wachtwoord wijzigen in de eerste keer wordt gebruikt.

Nadat u zich hebt aangemeld, vindt u een dialoogvenster voor het uploaden van de **PerformanceDiagnostics\_jjjj-MM-dd\_hh: mm: ss-fff.zip** bestand die is verzameld door PerfInsights.
