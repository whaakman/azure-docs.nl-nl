---
title: Het gebruik van PerfInsights in Microsoft Azure | Microsoft Docs
description: Leert PerfInsights gebruiken voor het oplossen van prestatieproblemen met virtuele machine van Windows.
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 36e204c73e62e950c3f40eab7e1ce6bccd7abd83
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-use-perfinsights"></a>Het gebruik van PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) is een geautomatiseerd script die handig diagnostische gegevens verzamelt, i/o-intensieve belastingen wordt uitgevoerd en biedt een analyserapport bij het oplossen van prestatieproblemen met virtuele machine van Windows in Microsoft Azure. Dit kan worden uitgevoerd op de virtuele machines als een zelfstandige script of rechtstreeks vanuit de portal door te installeren [Azure prestaties diagnostische VM-extensie](performance-diagnostics-vm-extension.md).

Het is raadzaam dat u dit script uitvoert voordat u een ondersteuningsticket met Microsoft om de prestaties van de virtuele machine opent.

## <a name="supported-troubleshooting-scenarios"></a>Ondersteunde scenario's voor het oplossen van problemen

PerfInsights kunt verzamelen en analyseren van verschillende soorten gegevens die zijn gegroepeerd in unieke scenario's.

### <a name="collect-disk-configuration"></a>De schijfconfiguratie verzamelen 

Dit scenario worden verzameld voor de configuratie van de schijf en andere belangrijke informatie, met inbegrip van de volgende items:

-   Gebeurtenislogboeken

-   De status van het netwerk voor alle binnenkomende en uitgaande verbindingen

-   Netwerk- en firewallinstellingen configuratie-instellingen

-   Lijst met taken voor alle toepassingen die momenteel worden uitgevoerd op het systeem

-   Informatiebestand gemaakt door msinfo32 voor de virtuele machine (VM)

-   Microsoft SQL Server-database configuratie-instellingen (als de virtuele machine wordt geïdentificeerd als een server waarop SQL Server)

-   Opslag betrouwbaarheid prestatiemeteritems

-   Belangrijke hotfixes voor Windows

-   Geïnstalleerde filterstuurprogramma 's

Dit is een passief verzamelen van informatie die mag niet van invloed zijn op het systeem. 

>[!Note]
>Dit scenario wordt automatisch opgenomen in elk van de volgende scenario's.

### <a name="benchmarkstorage-performance-test"></a>De prestatietest benchmark/opslag

Dit scenario wordt uitgevoerd de [diskspd](https://github.com/Microsoft/diskspd) benchmarktest (IOPS en MBPS) voor alle stations die zijn gekoppeld aan de virtuele machine. 

> [!Note]
> Dit scenario kan invloed hebben op het systeem en mag niet worden uitgevoerd op een live productiesysteem. Voer indien nodig in dit scenario in een speciale onderhoudsvenster om eventuele problemen te voorkomen. Een grotere werkbelasting die wordt veroorzaakt door een tracering of benchmark-test kan een nadelige invloed heeft op de prestaties van uw virtuele machine.
>

### <a name="general-vm-slow-analysis"></a>Algemene VM langzaam analyse 

Dit scenario wordt uitgevoerd een [prestatiemeteritem](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) traceren met behulp van de items die zijn opgegeven in het bestand Generalcounters.txt. Als de virtuele machine wordt geïdentificeerd als een server waarop SQL Server wordt uitgevoerd, wordt een teller prestatietracering uitgevoerd met behulp van de items die zijn gevonden in het bestand Sqlcounters.txt. Dit omvat ook prestaties Diagnostics-gegevens.

### <a name="vm-slow-analysis-and-benchmark"></a>Trage VM analyse en benchmark

Dit scenario wordt uitgevoerd een [prestatiemeteritem](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) trace die wordt gevolgd door een [diskspd](https://github.com/Microsoft/diskspd) benchmarktest. 

> [!Note]
> Dit scenario kan invloed hebben op het systeem en mag niet worden uitgevoerd op een live productiesysteem. Voer indien nodig in dit scenario in een speciale onderhoudsvenster om eventuele problemen te voorkomen. Een grotere werkbelasting die wordt veroorzaakt door een tracering of benchmark-test kan een nadelige invloed heeft op de prestaties van uw virtuele machine.
>

### <a name="azure-files-analysis"></a>Azure bestanden analyse 

Dit scenario voert een speciale prestaties teller vastleggen samen met een netwerktracering maken. Het vastleggen omvat alle 'SMB-Client Shares' prestatiemeteritems. Hier volgen enkele belangrijke SMB-share clientprestatietellers die deel van het vastleggen uitmaken:

| **Type**     | **SMB-client shares teller** |
|--------------|-------------------------------|
| IOPS         | Gegevens aanvragen per seconde             |
|              | Aanvragen per seconde gelezen             |
|              | Schrijven van aanvragen per seconde            |
| Latentie      | Gem. per seconde/gegevensaanvraag         |
|              | Gemiddelde leestijd                 |
|              | Gemiddelde schrijftijd                |
| I/o-grootte      | Gem. Bytes/gegevensaanvraag       |
|              | Gem. Aantal gelezen bytes               |
|              | Gem. Geschreven bytes              |
| Doorvoer   | Gegevens Bytes per seconde                |
|              | Gelezen Bytes per seconde                |
|              | Geschreven Bytes per seconde               |
| Wachtrijlengte | Gem. Wachtrijlengte voor lezen        |
|              | Gem. Wachtrijlengte voor schrijven       |
|              | Gem. Wachtrijlengte van gegevens        |

### <a name="custom-configuration"></a>Aangepaste configuratie 

Wanneer u een aangepaste configuratie hebt uitgevoerd, uitvoert u alle traces (prestatiecontrole, prestatiemeteritems, xperf, netwerk, storport) parallel, afhankelijk van hoeveel andere traceringen zijn geselecteerd. Nadat de tracering is voltooid, wordt door het hulpprogramma de benchmark diskspd uitgevoerd als deze optie is geselecteerd. 

> [!Note]
> Dit scenario kan invloed hebben op het systeem en mag niet worden uitgevoerd op een live productiesysteem. Voer indien nodig in dit scenario in een speciale onderhoudsvenster om eventuele problemen te voorkomen. Een grotere werkbelasting die wordt veroorzaakt door een tracering of benchmark-test kan een nadelige invloed heeft op de prestaties van uw virtuele machine.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Wat voor soort informatie wordt verzameld door het script?

Registreert informatie over de virtuele machine van Windows, schijven of groepen opslagconfiguratie, prestatiemeteritems, en verschillende traceringen worden verzameld, afhankelijk van de prestaties scenario gebruikt:

|Gegevens die worden verzameld                              |  |  | Scenario's voor prestaties |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Schijfconfiguratie verzamelen | Testen van de prestaties van de benchmark/opslag | Algemene VM langzaam analyse | Trage VM analyse en benchmark | Azure bestanden analyse | Aangepaste configuratie |
| Gegevens van gebeurtenislogboeken      | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Informatie over het bestandssysteem               | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Volume-kaart                       | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Schijf-kaart                         | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Actieve taken                    | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Opslag betrouwbaarheid prestatiemeteritems     | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Storage-gegevens              | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Fsutil uitvoer                    | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Filterinformatie stuurprogramma               | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Netstat-uitvoer                   | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Netwerkconfiguratie            | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Firewall-configuratie           | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Configuratie van SQL Server         | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Prestaties diagnostische traceringen * |                            |                                    | Ja                      |                                |                      | Ja                  |
| Prestatiemeteritem Trace **     |                            |                                    |                          |                                |                      | Ja                  |
| SMB-prestatiemeteritems Trace **             |                            |                                    |                          |                                | Ja                  |                      |
| Teller voor SQL Server Trace **      |                            |                                    |                          |                                |                      | Ja                  |
| XPerf tracering                      |                            |                                    |                          |                                |                      | Ja                  |
| StorPort-tracering                   |                            |                                    |                          |                                |                      | Ja                  |
| Netwerktracering                    |                            |                                    |                          |                                | Ja                  | Ja                  |
| Diskspd Benchmark trace ***      |                            | Ja                                |                          | Ja                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Diagnostische traceerlogboeken voor prestaties (*)

Een op regels gebaseerde engine op de achtergrond voor het verzamelen van gegevens en prestatieproblemen analyseren voortdurend uitgevoerd. De volgende regels worden momenteel ondersteund:

- HighCpuUsage regel: hoog CPU-gebruik perioden detecteert en de bovenste consumenten van de CPU-gebruik tijdens deze perioden weergegeven.
- HighDiskUsage regel: hoge schijf gebruik punten op fysieke schijven detecteert en ziet u de eerste schijf gebruik consumenten tijdens deze perioden.
- HighResolutionDiskMetric regel: toont IOPS, doorvoer en i/o latentie metrische gegevens per 50 milliseconden voor elke fysieke schijf. Het helpt snel schijf perioden beperking kan identificeren.
- HighMemoryUsage regel: veel geheugen gebruik perioden detecteert en het bovenste geheugen gebruik consumenten tijdens deze perioden weergegeven.

> [!NOTE] 
> Windows-versies met .NET Framework 3.5 of hoger worden momenteel ondersteund.

### <a name="performance-counter-trace-"></a>Teller prestatietracering (*)

Verzamelt de volgende prestatiemeteritems:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk, \LogicalDisk
- \Cache\Dirty pagina's, \Cache\Lazy schrijfbewerkingen per seconde, \Server\Pool wisselbaar, fouten, fouten \Server\Pool wisselbaar geheugen:
- Geselecteerde items onder \Network Interface, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network Adapter, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \ QoS-Policy\Packets, \Per Network Interface Card processoractiviteit, \Microsoft Winsock BSP-netwerk

#### <a name="for-sql-server-instances"></a>Voor SQL Server-exemplaren
- Server: bufferbeheer van \sql, \SQLServer:Resource Pool statistieken, \SQLServer:SQL Statistics\
- \SQLServer:locks, \SQLServer:General, statistieken
- \SQLServer:Access methoden

#### <a name="for-azure-files"></a>Voor Azure-bestanden
\SMB Clientshares

### <a name="diskspd-benchmark-trace-"></a>Diskspd Benchmark-tracering (*)
Diskspd IO werkbelasting tests [Besturingssysteemschijf (schrijven) en groep-schijven (lezen/schrijven)]

## <a name="run-the-perfinsights-on-your-vm"></a>De PerfInsights uitvoeren op de virtuele machine

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>Wat heb ik moet weten voordat ik het script uitvoeren? 

**Scriptvereisten**

1.  Dit script moet worden uitgevoerd op de virtuele machine met het prestatieprobleem. 

2.  De volgende besturingssystemen worden ondersteund: Windows Server 2008 R2, 2012, 2012 R2, 2016; Windows 8.1 en Windows 10.

**Mogelijke problemen wanneer u het script voor productie-virtuele machines uitvoert:**

1.  Het script mogelijk nadelige invloed heeft op de prestaties van de virtuele machine als deze wordt gebruikt samen met het 'Benchmark' of 'Aangepaste'-scenario dat is geconfigureerd met behulp van XPerf of DiskSpd. Wees voorzichtig wanneer u het script in een productieomgeving uitvoert.

2.  Wanneer u het script samen met het 'Benchmark' of 'Aangepaste'-scenario dat wordt geconfigureerd met behulp van DiskSpd gebruiken, moet u dat er geen andere activiteit op de achtergrond de i/o-werkbelasting op de schijven geteste verstoort.

3.  Het script maakt standaard gebruik van het station voor tijdelijke opslag om gegevens te verzamelen. Als u tracering blijft ingeschakeld gedurende een langere periode, kan de hoeveelheid gegevens die worden verzameld relevant zijn. Hierdoor kan de beschikbaarheid van de ruimte op de tijdelijke schijf, dus die invloed hebben op alle toepassingen die afhankelijk van dit station is.

### <a name="how-do-i-run-perfinsights"></a>Hoe kan ik PerfInsights uitvoeren? 

U kunt PerfInsights uitvoeren op een virtuele machine door het installeren van [Azure prestaties diagnostische VM-extensie](performance-diagnostics-vm-extension.md) of als een zelfstandige-script uit te voeren. 

**Installeren en PerfInsights uitvoeren vanuit de Azure-portal**

PerfInsights kan nu worden uitgevoerd met een VM-extensie aangeroepen extensie voor diagnostische gegevens van Azure-prestaties. Zie voor meer informatie [extensie voor diagnostische gegevens voor installeren Azure prestaties](performance-diagnostics-vm-extension.md#install-the-extension).  

**PerfInsights script uitvoeren in de zelfstandige modus**

Het script PerfInsights uitgevoerd, de volgende stappen uit:


1. Download [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Het bestand PerfInsights.zip deblokkeren. U doet dit door met de rechtermuisknop op het bestand PerfInsights.zip, selecteert u **eigenschappen**. In de **algemene** tabblad **blokkering** en selecteer vervolgens **OK**. Hiermee zorgt u ervoor dat het script wordt uitgevoerd zonder extra beveiliging wordt gevraagd.  

    ![Ontgrendelen van het zip-bestand](media/how-to-use-perfInsights/unlock-file.png)

3.  Het gecomprimeerde bestand voor PerfInsights.zip uitbreiden naar de tijdelijke schijf (standaard is dit meestal station D). Het gecomprimeerde bestand moet bevatten de volgende bestanden en mappen:

    ![bestanden in de gecomprimeerde map](media/how-to-use-perfInsights/file-folder.png)

4.  Open Windows PowerShell als beheerder en voer het script PerfInsights.ps1.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    U moet wellicht voert u naar 'y' als u wordt gevraagd te bevestigen dat u wilt het uitvoeringsbeleid wijzigen.

    Klik in het dialoogvenster Disclaimer krijgt u de optie voor diagnostische gegevens delen met Microsoft Support. U moet ook toestemming geven om de gebruiksrechtovereenkomst om door te gaan. Selecteer de gewenste opties en klik vervolgens op **-Script uitvoeren**.

    ![Disclaimer vak](media/how-to-use-perfInsights/disclaimer.png)

5.  De aanvraagnummer verzenden als deze beschikbaar is wanneer u het script (dit is voor onze statistieken) uitvoeren. Klik vervolgens op **OK**.
    
    ![ondersteunings-ID invoeren](media/how-to-use-perfInsights/enter-support-number.png)

6.  Selecteer het station voor tijdelijke opslag. Het Script kunt automatische detectie de stationsletter van het station. Als er problemen in deze fase optreden, wordt u mogelijk gevraagd om het station (het standaardstation is D) te selecteren. Hier gegenereerde logboeken worden opgeslagen in het logboek\_verzamelingsmap. Nadat u invoert of accepteer de stationsletter, klikt u op **OK**.

    ![Geef station](media/how-to-use-perfInsights/enter-drive.png)

7.  Selecteer een scenario voor het oplossen van problemen in de opgegeven lijst.

       ![Scenario's selecteren](media/how-to-use-perfInsights/select-scenarios.png)

8.  U kunt ook PerfInsights zonder gebruikersinterface uitvoeren.

    De volgende opdracht wordt uitgevoerd de 'algemene VM langzaam analyse' scenario zonder een prompt UI probleemoplossing of vastleggen van gegevens gedurende 30 seconden. U wordt gevraagd toestemming te geven tot dezelfde afwijzing en overeenkomst die worden vermeld in stap 4.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

    Als u wilt dat PerfInsights in stille modus uit te voeren, gebruikt u de **- AcceptDisclaimerAndShareDiagnostics** parameter. Gebruik bijvoorbeeld de volgende opdracht:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Hoe kan ik problemen oplossen tijdens het uitvoeren van het script?

Als het script is beëindigd, kunt u een inconsistente status opschonen door het uitvoeren van het script samen met de - switch opschonen als volgt:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Als er problemen tijdens de automatische detectie van de tijdelijke schijf optreden, wordt u mogelijk gevraagd om het station (het standaardstation is D) te selecteren.

![Voer station](media/how-to-use-perfInsights/enter-drive.png)

Het script wordt verwijderd van de hulpprogramma's en verwijdert u tijdelijke mappen.

### <a name="troubleshooting-other-script-issues"></a>Andere problemen met het script 

Als er problemen optreden wanneer u het script uitvoert, drukt u op Ctrl + C om de uitvoering van het script worden onderbroken. Zie de sectie 'Opschonen na niet normaal beëindigd' voor het verwijderen van tijdelijke objecten.

Als u fout optreden zelfs na verschillende pogingen blijven, raden wij aan dat u het script in de 'foutopsporingsmodus' uitvoeren met behulp van de '-fouten opsporen in ' parameteroptie bij het opstarten.

Nadat de fout zich voordoet, Kopieer de volledige uitvoer van de PowerShell-console en verzonden naar de agent van Microsoft Support helpt u bij het oplossen van het probleem.

### <a name="how-do-i-run-the-script-in-custom-configuration-mode"></a>Hoe voer het script in de modus voor aangepaste configuratie?

U selecteert de **aangepaste** configuratie, kunt u verschillende traceringen parallel (gebruik SHIFT ingedrukt om meerdere selecteren) inschakelen:

![scenario's selecteren](media/how-to-use-perfInsights/select-scenario.png)

Wanneer u de prestaties van diagnostische gegevens selecteert, teller prestatietracering, XPerf Trace, netwerktracering of Storport Trace-scenario's, volg de instructies in de dialoogvensters en probeer het probleem te reproduceren trage prestaties na het starten van de traceringen.

Het volgende dialoogvenster kunt u een tracering starten:

![Tracering starten](media/how-to-use-perfInsights/start-trace-message.png)

U hebt om te stoppen anders de traceringen, om te bevestigen dat de opdracht in een tweede dialoogvenster.

![tracering stoppen](media/how-to-use-perfInsights/stop-trace-message.png)
![tracering stoppen](media/how-to-use-perfInsights/ok-trace-message.png)

Als de traceringen of bewerkingen zijn voltooid, wordt een nieuw bestand gegenereerd in D:\\logboek\_verzameling (of de tijdelijke schijf) met de naam **CollectedData\_jjjj-MM-dd\_hh\_mm\_ss.zip.** U kunt dit bestand verzenden naar de agent ondersteuning voor analyse.

## <a name="review-the-diagnostics-report-created-by-perfinsights"></a>Bekijk het rapport diagnostische gegevens is gemaakt door PerfInsights

Binnen de **CollectedData\_jjjj-MM-dd\_hh\_mm\_ss.zip bestand** die is gegenereerd door PerfInsights, kunt u een HTML-rapport met details van de resultaten van PerfInsights vinden. Als u wilt controleren in het rapport, vouw de **CollectedData\_jjjj-MM-dd\_hh\_mm\_ss.zip** bestand en open vervolgens de **PerfInsights Report.html** het bestand.

Selecteer de **bevindingen** tabblad.

![tabblad Zoeken](media/how-to-use-perfInsights/findingtab.png)

**Opmerkingen bij de**

-   Berichten in rood bekend configuratieproblemen die prestatieproblemen kunnen veroorzaken.

-   Geel zijn waarschuwingen voor niet-optimale configuraties die niet noodzakelijkerwijs prestatieproblemen veroorzaken.

-   Berichten in blauw zijn alleen informatief instructies.

Bekijk de HTTP-koppelingen voor alle foutberichten in rood meer gedetailleerde informatie over de bevindingen en kunnen invloed op de prestaties of de aanbevolen procedures voor optimale prestaties van configuraties ophalen.

### <a name="disk-configuration-tab"></a>Tabblad voor schijf-configuratie

De **overzicht** sectie vindt u verschillende weergaven van de opslagconfiguratie, inclusief de gegevens van Diskpart en opslagruimten

De **DiskMap** en **VolumeMap** secties wordt beschreven in een dubbele perspectief logische volumes en fysieke schijven aan elkaar zijn gerelateerd.

In het perspectief van het fysieke schijf (DiskMap) ziet de tabel u alle logische volumes die worden uitgevoerd op de schijf. In het volgende voorbeeld wordt uitgevoerd PhysicalDrive2 twee logische Volumes die zijn gemaakt op meerdere partities (J en H):

![tabblad gegevens](media/how-to-use-perfInsights/disktab.png)

In het perspectief van het Volume (*VolumeMap*), de tabellen geven de fysieke schijven onder elke logische volume. U ziet dat een logisch volume van meerdere fysieke schijven voor RAID/dynamische schijven wordt mogelijk uitgevoerd. In het volgende voorbeeld *C:\\koppelen* is van een koppelpunt geconfigureerd als *SpannedDisk* op PhysicalDisks \#2 en \#3:

![tabblad volume](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-server-tab"></a>SQL Server-tabblad

Als het doel-virtuele machine fungeert als host voor alle exemplaren van SQL Server, ziet u een extra tabblad in de lijst met de naam **SQL Server**:

![SQL-tabblad](media/how-to-use-perfInsights/sqltab.png)

Deze sectie bevat een 'overzicht' en aanvullende sub tabbladen voor elk van de SQL Server-exemplaren die worden gehost op de virtuele machine.

De sectie 'Overzicht' bevat een handig tabel overzicht van alle fysieke schijven (systeem- en gegevensschijven) die worden uitgevoerd en die een mengeling van gegevensbestanden en transactielogboekbestanden bevatten.

In het volgende voorbeeld *PhysicalDrive0* (het station C wordt uitgevoerd) wordt weergegeven, omdat zowel de *modeldev* en *modellog* bestanden bevinden zich op station C, en ze zijn van verschillende typen (zoals het gegevensbestand en het transactielogboek, respectievelijk):

![LogInfo](media/how-to-use-perfInsights/loginfo.png)

De SQL Server-exemplaar-specifieke tabbladen bevatten een algemene sectie waarin algemene informatie over het geselecteerde exemplaar en extra secties voor gedetailleerde informatie, waaronder instellingen, configuraties en opties voor gebruikers.

## <a name="references-to-the-external-tools-used"></a>Verwijzingen naar de externe hulpprogramma's gebruikt

### <a name="diskspd"></a>Diskspd

DISKSPD is een opslag laden generator en prestaties van de test hulpprogramma van de Windows- en Windows Server- en Cloud-serverinfrastructuur engineering-teams. Zie voor meer informatie [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

XPerf is een opdrachtregelprogramma voor het vastleggen van traces van de Windows Performance Tools Kit.

Zie voor meer informatie [Windows prestaties Toolkit – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Volgende stappen

### <a name="upload-diagnostics-logs-and-reports-to-microsoft-support-for-further-review"></a>Diagnostische logboeken en rapporten uploaden naar Microsoft Support om verder te bestuderen

Wanneer u met het Microsoft Support-personeel werkt, mogelijk wordt u gevraagd voor het verzenden van de uitvoer die wordt gegenereerd door PerfInsights om te helpen het probleemoplossingsproces.

De agent ondersteuning voor u maakt een werkruimte DTM en ontvangt u een e-mailbericht een koppeling naar bevat de [DTM portal (https://filetransfer.support.microsoft.com/EFTClient/Account/Login.htm) en een unieke gebruikersnaam en het wachtwoord.

Dit bericht wordt verzonden via **CTS Automated Diagnostics Services** (ctsadiag@microsoft.com).

![Voorbeeld van het bericht](media/how-to-use-perfInsights/supportemail.png)

Voor extra beveiliging moet u uw wachtwoord bij het eerste gebruik te wijzigen.

Nadat u zich bij DTM aanmelden, vindt u een dialoogvenster voor het uploaden van de **CollectedData\_jjjj-MM-dd\_hh\_mm\_ss.zip** -bestand dat is verzameld door PerfInsights.
