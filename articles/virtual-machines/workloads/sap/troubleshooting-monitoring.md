---
title: Probleemoplossing en bewaking van SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Problemen oplossen en SAP HANA op een Azure (grote exemplaren) bewaken.
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/31/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5583f3d1949614dbba4d2f91d72e4ac6b4d03d1c
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="how-to-troubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a>Het oplossen en SAP HANA (grote exemplaren) op Azure controleren


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a>Bewaking in SAP HANA in Azure (grote exemplaren)

SAP HANA in Azure (grote exemplaren) gaat niet anders andere IaaS-implementatie, moet u controleren wat het besturingssysteem en de toepassing doet en hoe deze de volgende bronnen gebruiken:

- CPU
- Geheugen
- Netwerkbandbreedte
- Schijfruimte

Als Azure virtuele Machines die u nodig hebt om te achterhalen of de resource-klassen hierboven genoemde voldoende of of deze uitgeput ophalen. Hieronder vindt u meer details op elk van de verschillende klassen:

**Resource-CPU-verbruik:** de verhouding die SAP gedefinieerd voor bepaalde werkbelasting tegen HANA om ervoor te zorgen dat er moeten voldoende CPU-bronnen beschikbaar om te werken via de gegevens die zijn opgeslagen in het geheugen wordt afgedwongen. Echter, kunnen er gevallen waarbij HANA een groot aantal query's worden uitgevoerd als gevolg van ontbrekende indexen of vergelijkbare problemen CPU verbruikt. Dit betekent dat, moet u CPU-resourceverbruik van de HANA grote exemplaar eenheid en CPU-resources verbruikt door de specifieke HANA-services controleren.

**Geheugengebruik:** Is belangrijk voor het bewaken van binnen HANA, evenals buiten HANA op de eenheid. Binnen HANA, moet u controleren hoe de gegevens geheugen toegewezen om te kunnen blijven binnen de vereiste sizing richtlijnen van SAP HANA verbruikt. U wilt er ook voor het bewaken van geheugengebruik op grote instantieniveau om ervoor te zorgen dat aanvullende geïnstalleerde niet-HANA software niet te veel geheugen in beslag nemen, en daarom met HANA voor geheugen concurreren.

**De netwerkbandbreedte:** de Azure-VNet-gateway een beperkte bandbreedte van gegevens naar het Azure VNet te verplaatsen, dus is het handig om te controleren door de gegevens worden ontvangen door alle Azure virtuele machines binnen een VNet om te bepalen hoe dicht u staat op de ondergrenzen van de Azure-gateway SKU u die door selfservicegebruikers te schrijven. Op de eenheid HANA grote exemplaar het verstandig ook binnenkomende en uitgaande netwerkverkeer te bewaken en om de volumes die worden verwerkt na verloop van tijd bij te houden.

**Schijfruimte:** verbruik van schijfruimte wordt meestal verhoogd gedurende een bepaalde periode. Er zijn diverse redenen hiervoor, maar de meeste van alle zijn: gegevensvolume toeneemt, neemt de uitvoering van transactielogboeken, traceringsbestanden opslaan en uitvoeren van opslag-momentopnamen. Daarom is het belangrijk om het gebruik van schijfruimte controleren en beheren van de schijfruimte die is gekoppeld aan de eenheid HANA grote exemplaar.

Voor de **Type II SKU's** grote exemplaren HANA van de server wordt geleverd met de vooraf geladen diagnostische hulpprogramma's. U kunt gebruikmaken van deze diagnostische hulpmiddelen voor het uitvoeren van de statuscontrole van het systeem. Voer de volgende opdracht om de gezondheid van het logboekbestand op /var/log/health_check.
```
/opt/sgi/health_check/microsoft_tdi.sh
```
Wanneer u met het Microsoft Support team een probleem kunt oplossen werkt, kunt u ook gevraagd te bieden van de logboekbestanden met behulp van deze diagnostische hulpprogramma. U kunt het bestand met de volgende opdracht voor de zip.
```
tar  -czvf health_check_logs.tar.gz /var/log/health_check
```


## <a name="monitoring-and-troubleshooting-from-hana-side"></a>Bewaking en probleemoplossing van HANA kant

Effectief analyseren van problemen met betrekking tot SAP HANA in Azure (grote exemplaren), is het nuttig om de hoofdoorzaak van een probleem vast te stellen. SAP heeft een grote hoeveelheid documentatie om u te helpen gepubliceerd.

Van toepassing Veelgestelde vragen die betrekking hebben op prestaties voor SAP HANA vindt u in de volgende SAP-opmerkingen:

- [SAP-notitie #2222200: veelgestelde vragen over: SAP HANA-netwerk](https://launchpad.support.sap.com/#/notes/2222200)
- [SAP-notitie #2100040: veelgestelde vragen over: SAP HANA CPU](https://launchpad.support.sap.com/#/notes/0002100040)
- [SAP-notitie #199997: veelgestelde vragen over: SAP HANA-geheugen](https://launchpad.support.sap.com/#/notes/2177064)
- [SAP-notitie #200000: veelgestelde vragen over: Optimalisatie van de SAP HANA-prestaties](https://launchpad.support.sap.com/#/notes/2000000)
- [SAP-notitie #199930: veelgestelde vragen over: SAP HANA i/o-analyse](https://launchpad.support.sap.com/#/notes/1999930)
- [SAP-notitie #2177064: veelgestelde vragen over: SAP HANA-Service opnieuw starten en loopt vast](https://launchpad.support.sap.com/#/notes/2177064)

**SAP HANA-waarschuwingen**

Als eerste stap, controleert u de huidige logboeken van de SAP HANA-waarschuwing. SAP HANA-Studio, Ga naar **-beheerconsole: waarschuwingen: weergeven: alle waarschuwingen**. Op dit tabblad ziet alle SAP HANA-waarschuwingen voor specifieke waarden (beschikbaar fysiek geheugen, CPU-gebruik, enzovoort) die buiten de set minimale en maximale drempelwaarden vallen. Standaard controles worden automatisch vernieuwd om de 15 minuten.

![SAP HANA-Studio, Ga naar de-beheerconsole: waarschuwingen: weergeven: alle waarschuwingen](./media/troubleshooting-monitoring/image1-show-alerts.png)

**CPU**

Een oplossing is voor een waarschuwing als gevolg van onjuiste drempelinstelling geactiveerd, opnieuw wordt ingesteld op de standaardwaarde of een meer redelijke drempelwaarde.

![Opnieuw instellen op de standaardwaarde of een meer redelijke drempelwaarde](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

De volgende waarschuwingen kunnen wijzen op problemen van de CPU-resources:

- Host-CPU-gebruik (waarschuwing 5)
- Meest recente opslagpuntbewerking (waarschuwing 28)
- Duur van het opslagpunt (waarschuwing 54)

Hoog CPU-verbruik ziet u op uw SAP HANA-database een van de volgende opties:

- 5 (Host-CPU-gebruik) van de waarschuwing wordt gegenereerd voor de huidige of eerdere CPU-gebruik
- Het CPU-gebruik weergegeven op het scherm overzicht

![CPU-gebruik weergegeven op het scherm overzicht](./media/troubleshooting-monitoring/image3-cpu-usage.png)

De Load-grafiek kan hoog CPU-verbruik of hoge verbruik weergeven in het verleden:

![De Load-grafiek kan hoog CPU-verbruik of hoog verbruik weergeven in het verleden](./media/troubleshooting-monitoring/image4-load-graph.png)

Een waarschuwing geactiveerd vanwege intensief CPU-gebruik kan worden veroorzaakt door een aantal redenen, waaronder maar niet beperkt tot: de uitvoering van bepaalde transacties, het laden van gegevens, de verkeerd-om taken, langlopende SQL-instructies en ongeldige query-prestaties (bijvoorbeeld met BW op HANA kubussen).

Raadpleeg de [SAP HANA probleemoplossing: CPU gerelateerde veroorzaakt en oplossingen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) site voor gedetailleerde stappen voor probleemoplossing.

**Besturingssysteem**

Een van de belangrijkste controles voor SAP HANA op Linux is ervoor te zorgen dat transparante grote pagina's zijn uitgeschakeld, raadpleeg dan [SAP-notitie #2131662 – transparante grote pagina's (THP) op Servers voor SAP HANA](https://launchpad.support.sap.com/#/notes/2131662).

- U kunt controleren of transparante grote pagina's zijn ingeschakeld via de volgende opdracht voor Linux: **cat /sys/kernel/mm/transparent\_hugepage/ingeschakeld**
- Als _altijd_ is ingesloten door haakjes zoals hieronder, betekent dit dat de transparant grote pagina's zijn ingeschakeld: [altijd] madvise nooit; als _nooit_ is ingesloten door haakjes zoals hieronder, betekent dit dat de enorme transparante Pagina's zijn uitgeschakeld: altijd madvise [nooit]

De volgende Linux-opdracht moet er niets retourneren: **rpm - qa | grep ulimit.** Als het erop lijkt _ulimit_ is geïnstalleerd, verwijdert u deze onmiddellijk.

**Geheugen**

U kunt zien dat de hoeveelheid geheugen die is toegewezen door de SAP HANA-database groter is dan verwacht. De volgende waarschuwingen wijzen op problemen met hoog geheugengebruik:

- Host fysieke geheugengebruik (waarschuwing 1)
- Geheugengebruik van de naamserver (waarschuwing 12)
- Totale geheugengebruik van kolom Store tabellen (waarschuwing 40)
- Geheugengebruik van services (waarschuwing 43)
- Geheugengebruik van de belangrijkste opslag van kolom Store tabellen (waarschuwing 45)
- Runtime-dumpbestanden (waarschuwing 46)

Raadpleeg de [SAP HANA probleemoplossing: geheugenproblemen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) site voor gedetailleerde stappen voor probleemoplossing.

**Netwerk**

Raadpleeg [SAP-notitie #2081065 – SAP HANA-netwerk het oplossen van problemen](https://launchpad.support.sap.com/#/notes/2081065) en voert u de stappen in deze SAP-notitie van netwerkproblemen.

1. Analyse van round trip-tijd tussen server en client.
  A. Voer de SQL-script [ _HANA\_netwerk\_Clients_](https://launchpad.support.sap.com/#/notes/1969700)_._
  
2. Analyseer de communicatie tussen knooppunten.
  A. SQL-script uitvoeren [ _HANA\_netwerk\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._

3. Voer de opdracht Linux **ifconfig** (de uitvoer ziet als een pakketverlies optreden).
4. Voer de opdracht Linux **tcpdump**.

Gebruik tevens de open source [IPERF](https://iperf.fr/) hulpprogramma (of vergelijkbaar) voor het meten van de netwerkprestaties echte toepassing.

Raadpleeg de [SAP HANA probleemoplossing: prestaties toegang en verbindingsproblemen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) site voor gedetailleerde stappen voor probleemoplossing.

**Storage**

Vanuit het perspectief van een eindgebruiker wordt een toepassing (of het systeem als geheel) wordt traag uitgevoerd, reageert niet of kan zelfs lijken vastlopen als er problemen met i/o-prestaties zijn. In de **Volumes** tabblad in SAP HANA Studio ziet u de gekoppelde volumes, en welke volumes worden gebruikt door elke service.

![In het tabblad Volumes in SAP HANA Studio ziet u de gekoppelde volumes en welke volumes worden gebruikt door elke service](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Volumes die zijn gekoppeld in het onderste gedeelte van het scherm ziet u details van de volumes, zoals bestanden en i/o-statistieken.

![Volumes die zijn gekoppeld in het onderste gedeelte van het scherm ziet u details van de volumes, zoals bestanden en i/o-statistieken](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Raadpleeg de [SAP HANA probleemoplossing: i/o-gerelateerde hoofdoorzaken en oplossingen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) en [SAP HANA probleemoplossing: schijf gerelateerde hoofdoorzaken en oplossingen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) site voor gedetailleerde stappen voor probleemoplossing.

**Diagnostische hulpprogramma 's**

Uitvoeren van een SAP HANA-statuscontrole via HANA\_configuratie\_Minichecks. Dit hulpprogramma retourneert een potentieel kritiek technische problemen die als waarschuwingen in SAP HANA Studio al moeten zijn opgetreden.

Raadpleeg [SAP-notitie #1969700-verzameling van de SQL-instructie voor SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) en download het bestand SQL Statements.zip is gekoppeld aan de desbetreffende opmerking. Sla dit ZIP-bestand op de lokale vaste schijf.

SAP HANA Studio op de **systeemgegevens** tabblad, met de rechtermuisknop in de **naam** en selecteert u **SQL importinstructies**.

![In SAP HANA Studio, op het tabblad systeemgegevens met de rechtermuisknop in de kolom naam en selecteer SQL-instructies voor importeren](./media/troubleshooting-monitoring/image7-import-statements-a.png)

Selecteer het SQL-Statements.zip-bestand die lokaal zijn opgeslagen, en een map met de bijbehorende SQL-instructies worden geïmporteerd. Op dit moment kunnen veel verschillende diagnostische controles worden uitgevoerd met deze SQL-instructies.

Bijvoorbeeld, als u wilt testen SAP HANA System Replication bandbreedtevereisten, met de rechtermuisknop op de **bandbreedte** instructie onder **replicatie: bandbreedte** en selecteer **Open** in SQL-Console.

De volledige SQL-instructie wordt geopend zodat invoerparameters (wijziging sectie) worden gewijzigd en vervolgens uitvoeren.

![De volledige SQL-instructie wordt geopend zodat invoerparameters (wijziging sectie) worden gewijzigd en vervolgens uitvoeren](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Een ander voorbeeld is met de rechtermuisknop op in de overzichten onder **replicatie: overzicht**. Selecteer **Execute** in het contextmenu:

![Een ander voorbeeld is met de rechtermuisknop op de replicatie-instructies op: overzicht. Voer in het contextmenu selecteren](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Dit resulteert in informatie die bij het oplossen van helpt:

![Dit leidt tot informatie die bij het oplossen van problemen helpt](./media/troubleshooting-monitoring/image10-import-statements-d.png)

Doe hetzelfde voor HANA\_configuratie\_Minichecks en controleer of alle _X_ gemarkeerd de _C_ kolom (Kritiek).

Voorbeeld van uitvoer:

**HANA\_configuratie\_MiniChecks\_Rev102.01 + 1** voor algemene SAP HANA-controles.

![HANA\_configuratie\_MiniChecks\_Rev102.01 + 1 voor algemene SAP HANA-controles](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**HANA\_Services\_overzicht** voor een overzicht van wat SAP HANA-services die momenteel worden uitgevoerd.

![HANA\_Services\_overzicht voor een overzicht van wat SAP HANA-services die momenteel worden uitgevoerd](./media/troubleshooting-monitoring/image12-services-overview.png)

**HANA\_Services\_statistieken** voor SAP HANA servicegegevens (CPU, geheugen, etc.).

![HANA\_Services\_statistieken voor SAP HANA-servicegegevens ](./media/troubleshooting-monitoring/image13-services-statistics.png)

**HANA\_configuratie\_overzicht\_Rev110 +** voor algemene informatie over het SAP HANA-exemplaar.

![HANA\_configuratie\_overzicht\_Rev110 + voor algemene informatie over het SAP HANA-exemplaar](./media/troubleshooting-monitoring/image14-configuration-overview.png)

**HANA\_configuratie\_Parameters\_Rev70 +** om te controleren SAP HANA-parameters.

![HANA\_configuratie\_Parameters\_Rev70 + SAP HANA-parameters controleren](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

