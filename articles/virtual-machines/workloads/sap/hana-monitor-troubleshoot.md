---
title: Bewaking en probleemoplossing van HANA zijde op SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Bewaking en probleemoplossing van HANA kant op SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 10709f4f2fcc341840753ef4c4eb479e29fb58d5
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356502"
---
# <a name="monitoring-and-troubleshooting-from-hana-side"></a>Bewaking en probleemoplossing van HANA aan clientzijde

Als u wilt effectief analyseren van problemen met betrekking tot SAP HANA op Azure (grote instanties), is het handig om de hoofdoorzaak van een probleem vast te stellen. SAP is een grote hoeveelheid documentatie om u te helpen gepubliceerd.

Toepasselijke Veelgestelde vragen met betrekking tot de prestaties van SAP HANA kunnen worden gevonden in de volgende SAP-opmerkingen:

- [SAP-notitie #2222200: veelgestelde vragen over: SAP HANA-netwerk](https://launchpad.support.sap.com/#/notes/2222200)
- [SAP-notitie #2100040: veelgestelde vragen over: SAP HANA-CPU](https://launchpad.support.sap.com/#/notes/0002100040)
- [SAP-notitie #199997: veelgestelde vragen over: SAP HANA-geheugen](https://launchpad.support.sap.com/#/notes/2177064)
- [SAP-notitie #200000: veelgestelde vragen over: Optimalisatie van SAP HANA-prestaties](https://launchpad.support.sap.com/#/notes/2000000)
- [SAP-notitie #199930: veelgestelde vragen over: SAP HANA-i/o-analyse](https://launchpad.support.sap.com/#/notes/1999930)
- [SAP-notitie #2177064: veelgestelde vragen over: SAP HANA-Service opnieuw starten en loopt vast](https://launchpad.support.sap.com/#/notes/2177064)

## <a name="sap-hana-alerts"></a>SAP HANA-waarschuwingen

Als eerste stap, controleert u de huidige logboeken van de SAP HANA-waarschuwing. In SAP HANA Studio, gaat u naar **-beheerconsole: waarschuwingen: weergeven: alle waarschuwingen**. Op dit tabblad ziet u alle SAP HANA-waarschuwingen voor specifieke waarden (vrij fysiek geheugen, CPU-gebruik, enzovoort) die buiten de set minimale en maximale drempelwaarden vallen. Standaard controles worden automatisch vernieuwd om de 15 minuten.

![In SAP HANA Studio, gaat u naar de beheerconsole: waarschuwingen: weergeven: alle waarschuwingen](./media/troubleshooting-monitoring/image1-show-alerts.png)

## <a name="cpu"></a>CPU

Een oplossing is voor een waarschuwing geactiveerd vanwege onjuiste drempelinstelling, opnieuw wordt ingesteld op de standaardwaarde of een meer redelijke waarde voor drempel.

![Opnieuw instellen op de standaardwaarde of een meer redelijke waarde voor drempel](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

De volgende waarschuwingen kunnen duiden op problemen voor CPU-resources:

- Host CPU-gebruik (waarschuwing 5)
- Meest recente opslagpuntbewerking (waarschuwing 28)
- Opslagpunt duur (waarschuwing 54)

Hoge CPU-verbruik ziet u in uw SAP HANA-database van een van de volgende:

- 5 (Host CPU-gebruik) van de waarschuwing wordt gegenereerd voor de huidige of vroegere CPU-gebruik
- Het CPU-gebruik weergegeven op het overzichtsscherm

![CPU-gebruik weergegeven op het overzichtsscherm](./media/troubleshooting-monitoring/image3-cpu-usage.png)

De Load-grafiek kan hoog CPU-verbruik of hoge verbruik in het verleden weergeven:

![De Load-grafiek kan hoog CPU-verbruik of hoge verbruik weergeven in het verleden](./media/troubleshooting-monitoring/image4-load-graph.png)

Een waarschuwing geactiveerd vanwege hoog CPU-verbruik wordt mogelijk veroorzaakt door verschillende oorzaken hebben, met inbegrip van, maar niet beperkt tot: uitvoeren van bepaalde transacties, het laden van gegevens, verkeerd van taken, langlopende SQL-instructies en ongeldige query-prestaties (bijvoorbeeld met BW op HANA kubussen).

Raadpleeg de [SAP HANA-probleemoplossing: CPU gerelateerde zorgt ervoor dat en -oplossingen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) site voor gedetailleerde stappen voor probleemoplossing.

## <a name="operating-system"></a>Besturingssysteem

Een van de belangrijkste controles voor SAP HANA op Linux is ervoor te zorgen dat transparant grote pagina's zijn uitgeschakeld, raadpleeg dan [SAP Opmerking #2131662 – transparante grote pagina's (THP) op SAP HANA-Servers](https://launchpad.support.sap.com/#/notes/2131662).

- U kunt controleren of transparante grote pagina's zijn ingeschakeld via de volgende opdracht voor Linux: **cat /sys/kernel/mm/transparent\_hugepage/ingeschakeld**
- Als _altijd_ is ingesloten tussen vierkante haken zoals hieronder, betekent dit dat de transparant grote pagina's zijn ingeschakeld: [altijd] madvise nooit; als _nooit_ staat tussen vierkante haken zoals hieronder, betekent dit dat de transparant enorme Pagina's zijn uitgeschakeld: altijd madvise [nooit]

De volgende Linux-opdracht moet er niets worden geretourneerd: **rpm - qa | grep ulimit.** Als het erop lijkt _ulimit_ is geïnstalleerd, verwijdert u deze onmiddellijk.

## <a name="memory"></a>Geheugen

U kunt zien dat de hoeveelheid geheugen die zijn toegewezen door de SAP HANA-database groter is dan verwacht. De volgende waarschuwingen wijzen op problemen met hoog geheugengebruik:

- Host fysieke geheugengebruik (waarschuwing 1)
- Geheugengebruik van de naamserver (waarschuwing 12)
- Totale geheugengebruik van kolom Store tabellen (waarschuwing 40)
- Geheugengebruik van services (waarschuwing 43)
- Geheugengebruik van de belangrijkste opslag van de kolom Store tabellen (waarschuwing 45)
- Runtime-dumpbestanden (waarschuwing 46)

Raadpleeg de [SAP HANA-probleemoplossing: geheugenproblemen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) site voor gedetailleerde stappen voor probleemoplossing.

## <a name="network"></a>Netwerk

Raadpleeg [SAP Opmerking #2081065 – het oplossen van SAP HANA netwerk](https://launchpad.support.sap.com/#/notes/2081065) en het netwerk de stappen in deze SAP-notitie voor probleemoplossing uitvoeren.

1. Analyse van retourtijd tussen server en client.
  A. Voer de SQL-script [ _HANA\_netwerk\_Clients_](https://launchpad.support.sap.com/#/notes/1969700)_._
  
2. Analyseer de communicatie tussen knooppunten.
  A. Voer SQL-script [ _HANA\_netwerk\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._

3. Linux-opdracht uitvoeren **ifconfig** (in de uitvoer ziet als een pakketverlies optreden).
4. Linux-opdracht uitvoeren **tcpdump**.

Gebruik ook de open-source [IPERF](https://iperf.fr/) hulpprogramma (of vergelijkbaar) voor het meten van prestaties van het netwerk echte toepassing.

Raadpleeg de [SAP HANA-probleemoplossing: netwerken prestaties en problemen met de netwerkverbinding](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) site voor gedetailleerde stappen voor probleemoplossing.

## <a name="storage"></a>Storage

Vanuit het perspectief van een eindgebruiker, een toepassing (of het systeem als geheel) wordt traag uitgevoerd, is niet meer reageert of zelfs lijkt het alsof vastlopen als er problemen met i/o-prestaties zijn. In de **Volumes** tabblad in SAP HANA Studio, ziet u de gekoppelde volumes en welke volumes worden gebruikt door elke service.

![In het tabblad Volumes in SAP HANA Studio, kunt u de gekoppelde volumes en welke volumes worden gebruikt door elke service bekijken](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Volumes die zijn gekoppeld in het onderste gedeelte van het scherm ziet u details van de volumes, zoals bestanden en i/o-statistieken.

![Volumes die zijn gekoppeld in het onderste gedeelte van het scherm ziet u details van de volumes, zoals bestanden en i/o-statistieken](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Raadpleeg de [SAP HANA-probleemoplossing: i/o gerelateerde belangrijkste oorzaken en oplossingen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) en [SAP HANA-probleemoplossing: schijf gerelateerde belangrijkste oorzaken en oplossingen](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) site voor gedetailleerde stappen voor probleemoplossing.

## <a name="diagnostic-tools"></a>Diagnostische hulpprogramma 's

Uitvoeren van een SAP HANA-statuscontrole via HANA\_configuratie\_Minichecks. Dit hulpprogramma retourneert mogelijk belangrijke technische problemen die als waarschuwingen in SAP HANA Studio al moeten zijn opgetreden.

Raadpleeg [SAP Opmerking #1969700-verzameling van de SQL-instructie voor SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) en download het Statements.zip SQL-bestand dat is gekoppeld aan deze opmerking. Store dit ZIP-bestand op de lokale harde schijf.

In SAP HANA Studio, op de **systeemgegevens** tabblad, in met de rechtermuisknop op de **naam** kolom en selecteer **SQL importinstructies**.

![In SAP HANA Studio, op het tabblad systeemgegevens met de rechtermuisknop op in de kolom naam en selecteer SQL-instructies voor importeren](./media/troubleshooting-monitoring/image7-import-statements-a.png)

Selecteer het SQL-Statements.zip-bestand die lokaal zijn opgeslagen, en een map met de bijbehorende SQL-instructies worden geïmporteerd. Op dit moment kunnen de vele verschillende diagnostische controles worden uitgevoerd met deze SQL-instructies.

Bijvoorbeeld, als u wilt testen van SAP HANA-Systeemreplicatie bandbreedtevereisten, met de rechtermuisknop op de **bandbreedte** instructie onder **replicatie: bandbreedte** en selecteer **Open** in SQL-Console.

De volledige SQL-instructie wordt geopend zodat invoerparameters (wijziging sectie) om te worden gewijzigd en worden vervolgens uitgevoerd.

![De volledige SQL-instructie wordt geopend zodat invoerparameters (wijziging sectie) om te worden gewijzigd en worden vervolgens uitgevoerd](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Een ander voorbeeld is met de rechtermuisknop op in de instructies onder **replicatie: overzicht**. Selecteer **Execute** in het contextmenu:

![Een ander voorbeeld is met de rechtermuisknop op de instructies onder de replicatie op: overzicht. Voer in het contextmenu selecteren](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Dit resulteert in informatie die bij het oplossen van problemen helpt:

![Dit leidt tot informatie die bij het oplossen van problemen helpt](./media/troubleshooting-monitoring/image10-import-statements-d.png)

Doe hetzelfde voor HANA\_configuratie\_Minichecks en controleer of alle _X_ markeert de _C_ kolom (Kritiek).

Voorbeeld van uitvoer:

**HANA\_configuratie\_MiniChecks\_Rev102.01 + 1** voor algemene SAP HANA wordt gecontroleerd.

![HANA\_configuratie\_MiniChecks\_Rev102.01 + 1 voor algemene SAP HANA-controles](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**HANA\_Services\_overzicht** voor een overzicht van wat SAP HANA-services die momenteel worden uitgevoerd.

![HANA\_Services\_overzicht voor een overzicht van wat SAP HANA-services die momenteel worden uitgevoerd](./media/troubleshooting-monitoring/image12-services-overview.png)

**HANA\_Services\_statistieken** voor SAP HANA-service informatie (CPU, geheugen, etc.).

![HANA\_Services\_servicegegevens voor statistieken voor SAP HANA ](./media/troubleshooting-monitoring/image13-services-statistics.png)

**HANA\_configuratie\_overzicht\_Rev110 +** voor algemene informatie over de SAP HANA-instantie.

![HANA\_configuratie\_overzicht\_Rev110 + voor algemene informatie over de SAP HANA-instantie](./media/troubleshooting-monitoring/image14-configuration-overview.png)

**HANA\_configuratie\_Parameters\_Rev70 +** om te controleren op SAP HANA-parameters.

![HANA\_configuratie\_Parameters\_Rev70 + om te controleren op SAP HANA-parameters](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

**Volgende stappen**

- Raadpleeg [hoge beschikbaarheid in SUSE met behulp van de stonith instellen ingesteld](ha-setup-with-stonith.md).