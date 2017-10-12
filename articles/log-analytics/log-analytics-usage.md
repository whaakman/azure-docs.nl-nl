---
title: Gegevensgebruik analyseren in Log Analytics | Microsoft Docs
description: Gebruik het dashboard Gebruik van Log Analytics om weer te geven hoeveel gegevens worden verzonden naar de Log Analytics-service en waarom grote hoeveelheden gegevens worden verzonden.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: magoedte
ms.openlocfilehash: 9a4709f298131722e9c473a19f7eee0aebf7e1e6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-data-usage-in-log-analytics"></a>Gegevensgebruik analyseren in Log Analytics
Log Analytics bevat informatie over de hoeveelheid gegevens die is verzameld, welke computers de gegevens hebben verzonden en de verschillende typen gegevens die zijn verzonden.  Gebruik het dashboard **Gebruik van Log Analytics** om de hoeveelheid gegevens te zien die wordt verzonden naar de Log Analytics-service. Het dashboard laat zien hoeveel gegevens worden verzameld door elke oplossing en hoeveel gegevens uw computers verzenden.

## <a name="understand-the-usage-dashboard"></a>Inzicht in het dashboard met gebruiksgegevens
Het **Log Analytics-gebruiksdashboard** bevat de volgende informatie:

- Gegevensvolume
    - Gegevensvolume gedurende een bepaalde periode (op basis van uw huidige tijdsbereik)
    - Gegevensvolume per oplossing
    - Gegevens die niet zijn gekoppeld aan een computer
- Computers
    - Computers waarmee gegevens worden verzonden
    - Computers zonder gegevens in de afgelopen 24 uur
- Aanbiedingen
    - Knooppunten voor Insight en Analytics
    - Knooppunten voor automatisering en beheer
    - Knooppunten voor beveiliging
- Prestaties
    - Benodigde tijd voor het verzamelen en indexeren van gegevens
- Lijst met query's

![gebruiksdashboard](./media/log-analytics-usage/usage-dashboard01.png)

### <a name="to-work-with-usage-data"></a>Werken met gebruiksgegevens
1. Meld u met uw Azure-abonnement aan bij [Azure Portal](https://portal.azure.com) als u dit nog niet hebt gedaan.
2. Klik in het menu **Hub** op **Meer services** en typ in de lijst met resources op **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Log Analytics**.  
    ![Azure-hub](./media/log-analytics-usage/hub.png)
3. Op het **Log Analytics**-dashboard wordt een lijst met uw werkruimten weergegeven. Selecteer een werkruimte.
4. Klik op het dashboard *Werkruimte* op **Log Analytics-gebruik**.
5. Klik op het **Log Analytics-gebruiksdashboard** op **Tijd: afgelopen 24 uur** om het tijdsinterval te wijzigen.  
    ![tijdsinterval](./media/log-analytics-usage/time.png)
6. Bekijk de blades voor de gebruikscategorie waarin gebieden worden weergegeven waarin u bent geïnteresseerd. Kies een blade en klik vervolgens op een item op de blade om meer details weer te geven in [Zoeken in logboeken](log-analytics-log-searches.md).  
    ![voorbeeld van blade voor gegevensgebruik](./media/log-analytics-usage/blade.png)
7. Bekijk op het dashboard Zoeken in logboeken de resultaten die zijn geretourneerd na de zoekopdracht.  
    ![voorbeeld van gebruik van zoeken in logboeken](./media/log-analytics-usage/usage-log-search.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Een waarschuwing instellen wanneer de gegevensverzameling groter is dan verwacht
In deze sectie wordt beschreven hoe u een waarschuwing instelt als:
- Het gegevensvolume groter is dan een opgegeven hoeveelheid.
- Het gegevensvolume naar verwachting een opgegeven hoeveelheid gaat overschrijden.

[Waarschuwingen](log-analytics-alerts-creating.md) van Log Analytics maken gebruik van zoekquery’s. De volgende query geeft een resultaat wanneer er meer dan 100 GB aan gegevens in de afgelopen 24 uur is verzameld:

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`

De volgende query gebruikt een eenvoudige formule om te voorspellen wanneer meer dan 100 GB aan gegevens op een dag wordt verzonden: 

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`

Als u een waarschuwing wilt instellen bij een ander gegevensvolume, wijzigt u 100 in de query's in het aantal GB waarbij u een waarschuwing wilt.

Gebruik de stappen in [Een waarschuwingsregel maken](log-analytics-alerts-creating.md#create-an-alert-rule) om een melding te krijgen als de hoeveelheid verzamelde gegevens groter is dan verwacht.

Bij het instellen van de waarschuwing voor de eerste query - wanneer er meer dan 100 GB aan gegevens in 24 uur, stelt u het volgende in:
- **Naam** op *Gegevensvolume groter dan 100 GB in 24 uur*
- **Ernst** op *Waarschuwing*
- **Zoekquery** op `Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`
- **Tijdvenster** op *24 uur*.
- **Waarschuwingsfrequentie** op één uur omdat de gebruiksgegevens slechts één keer per uur worden bijgewerkt.
- **Waarschuwingen genereren op basis van** op het *aantal resultaten*
- **Aantal resultaten** op *Groter dan 0*

Gebruik de stappen in [Acties toevoegen aan waarschuwingsregels](log-analytics-alerts-actions.md) om een e-mail-, webhook- of runbookactie te configureren voor een waarschuwingsregel.

Bij het maken van de waarschuwing voor de tweede query - wanneer wordt voorspeld dat er meer dan 100 GB aan gegevens in 24 uur zal zijn, stelt u het volgende in:
- **Naam** op *Gegevensvolume naar verwachting groter dan 100 GB in 24 uur*
- **Ernst** op *Waarschuwing*
- **Zoekquery** op `Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`
- **Tijdvenster** op *3 uur*.
- **Waarschuwingsfrequentie** op één uur omdat de gebruiksgegevens slechts één keer per uur worden bijgewerkt.
- **Waarschuwingen genereren op basis van** op het *aantal resultaten*
- **Aantal resultaten** op *Groter dan 0*

Wanneer u een waarschuwing ontvangt, gebruikt u de stappen in de volgende sectie om te bepalen waarom het-gebruik is hoger dan verwacht.

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Het oplossen van problemen met een hoger gebruik dan verwacht
Het dashboard met gebruiksgegevens helpt u om te bepalen waarom het gebruik (en dus de kosten) groter is dan u verwacht.

Hoger gebruik wordt veroorzaakt door een of beide volgende oorzaken:
- Er worden meer gegevens dan verwacht verzonden naar Log Analytics
- Er worden meer knooppunten dan verwacht verzonden naar Log Analytics

### <a name="check-if-there-is-more-data-than-expected"></a>Controleren of er meer gegevens zijn dan verwacht 
Er zijn twee belangrijke secties van de pagina met gebruiksgegevens waarmee u kunt achterhalen wat de oorzaak is van de meeste gegevens die worden verzameld.

Het diagram *Gegevensvolume in een langere periode* toont het totale volume van de gegevens die worden verzonden en de computers die de meeste gegevens verzenden. In het diagram bovenaan kunt u zien of het algehele gegevensgebruik groeit, stabiel blijft of afneemt. De lijst met computers toont de 10 computers die de meeste gegevens verzenden.

Het diagram *Gegevensvolume per oplossing* toont de hoeveelheid gegevens die wordt verzonden door elke oplossing en de oplossingen die de meeste gegevens verzenden. Het diagram bovenaan toont het totale volume van de gegevens die in een bepaalde periode door elke oplossing worden verzonden. Aan de hand van deze informatie kunt u bepalen of een oplossing meer gegevens, ongeveer dezelfde hoeveelheid gegevens of minder gegevens gedurende een bepaalde periode verzendt. De lijst met oplossingen toont de 10 oplossingen die de meeste gegevens verzendt. 

Deze twee grafieken geven alle gegevens weer. Sommige gegevens zijn betaald, andere gegevens zijn gratis. Als u zich wilt richten op gegevens die betaald zijn, neemt u op de zoekpagina `IsBillable=true` op in de query.  

![gegevensvolumediagrammen](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

Bekijk het diagram *Gegevensvolume in een langere periode*. Klik op de naam van de computer om de oplossingen en gegevenstypen te bekijken die de meeste gegevens verzenden voor een specifieke computer. Klik op de naam van de eerste computer in de lijst.

Op de volgende schermafbeelding verzendt het gegevenstype *Log Management / Perf* de meeste gegevens voor de computer. 

![gegevensvolume voor een computer](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)

Ga vervolgens terug naar het dashboard *Gebruik* en bekijk het diagram *Gegevensvolume per oplossing*. Klik op de naam van de oplossing in de lijst voor een overzicht van de computers die de meeste gegevens voor een oplossing verzenden. Klik op de naam van de eerste oplossing in de lijst. 

Op de volgende schermafbeelding is te zien dat de computer *acmetomcat* de meeste gegevens verzendt voor het beheersysteem voor de oplossing Log Management.

![gegevensvolume voor een oplossing](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)

Voer indien nodig extra analyses uit om grote volumes binnen een oplossing of gegevenstype te identificeren. Voorbeelden van query's zijn:

+ **Beveiligingsoplossing**
  - `Type=SecurityEvent | measure count() by EventID`
+ **Logboekbeheeroplossing**
  - `Type=Usage Solution=LogManagement IsBillable=true | measure count() by DataType`
+ **Perf-gegevenstype**
  - `Type=Perf | measure count() by CounterPath`
  - `Type=Perf | measure count() by CounterName`
+ **Gebeurtenisgegevenstype**
  - `Type=Event | measure count() by EventID`
  - `Type=Event | measure count() by EventLog, EventLevelName`
+ **Syslog-gegevenstype**
  - `Type=Syslog | measure count() by Facility, SeverityLevel`
  - `Type=Syslog | measure count() by ProcessName`
+ **AzureDiagnostics**-gegevenstype
  - `Type=AzureDiagnostics | measure count() by ResourceProvider, ResourceId`

Gebruik de volgende stappen om het volume van de logboeken die worden verzameld te beperken:

| Bron van hoog gegevensvolume | Het gegevensvolume verminderen |
| -------------------------- | ------------------------- |
| Beveiligingsgebeurtenissen            | Selecteer [normale of minimale beveiligingsgebeurtenissen](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) <br> Wijzig het beleid voor beveiligingscontrole zodat alleen de gewenste gebeurtenissen worden verzameld. Controleer vooral de noodzaak voor het verzamelen van gebeurtenissen voor <br> - [filterplatform controleren](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [register controleren](https://docs.microsoft.com/windows/device-security/auditing/audit-registry)<br> - [bestandssysteem controleren](https://docs.microsoft.com/windows/device-security/auditing/audit-file-system)<br> - [kernel-object controleren](https://docs.microsoft.com/windows/device-security/auditing/audit-kernel-object)<br> - [greepbewerking controleren](https://docs.microsoft.com/windows/device-security/auditing/audit-handle-manipulation)<br> - [verwijderbare opslag controleren](https://docs.microsoft.com/windows/device-security/auditing/audit-removable-storage) |
| Prestatiemeteritems       | Wijzig de [Prestatiemeteritemconfiguratie](log-analytics-data-sources-performance-counters.md) in: <br> - Frequentie van het verzamelen van gegevens beperken <br> - Aantal prestatiemeteritems beperken |
| Gebeurtenislogboeken                 | Wijzig [Configuratie van gebeurtenislogboek](log-analytics-data-sources-windows-events.md) in: <br> - Aantal verzamelde gebeurtenislogboeken beperken <br> - Alleen vereiste gebeurtenisniveaus verzamelen. Bijvoorbeeld, gebeurtenissen op *informatie*niveau niet verzamelen |
| Syslog                     | Wijzig de [syslog-configuratie](log-analytics-data-sources-syslog.md) in: <br> - Aantal verzamelde installaties beperken <br> - Alleen vereiste gebeurtenisniveaus verzamelen. Bijvoorbeeld, gebeurtenissen op *informatie*- en *foutopsporings*niveau niet verzamelen |
| AzureDiagnostics           | Wijzig de resourcelogboekverzameling om: <br> - Het aantal resources dat logboeken naar Log Analytics verzendt te verkleinen <br> - Alleen vereiste logboeken te verzamelen |
| Oplossingsgegevens van computers die de oplossing niet nodig hebben | Gebruik [oplossingstargeting](../operations-management-suite/operations-management-suite-solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Controleren of er meer knooppunten zijn dan verwacht
Als u gebruikmaakt van de prijscategorie *per knooppunt (OMS)*, worden de kosten berekend op basis van het aantal knooppunten en oplossingen die u gebruikt. In de sectie *Aanbiedingen* van het dashboard met gebruiksgegevens kunt u zien hoeveel knooppunten van elke aanbieding er worden gebruikt.

![gebruiksdashboard](./media/log-analytics-usage/log-analytics-usage-offerings.png)

Klik op **Alles weergeven...**  om de volledige lijst met computers die gegevens verzenden voor het geselecteerde pakket weer te geven.

Gebruik [oplossingstargeting](../operations-management-suite/operations-management-suite-solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers.


## <a name="next-steps"></a>Volgende stappen
* Zie [Zoekopdrachten in logboeken in Log Analytics](log-analytics-log-searches.md) voor meer informatie over het gebruik van de zoektaal. U kunt zoekquery’s gebruiken om aanvullende analyses uit te voeren op de gebruiksgegevens.
* Gebruik de stappen in [Een waarschuwingsregel maken](log-analytics-alerts-creating.md#create-an-alert-rule) om een melding te krijgen wanneer aan een zoekcriterium wordt voldaan
* Gebruik [oplossingstargeting](../operations-management-suite/operations-management-suite-solution-targeting.md) om alleen van de vereiste groepen computers gegevens te verzamelen
* Selecteer [normale of minimale beveiligingsgebeurtenissen](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/)
* Wijzig de [prestatiemeteritemconfiguratie](log-analytics-data-sources-performance-counters.md)
* Wijzig de [gebeurtenislogboekconfiguratie](log-analytics-data-sources-windows-events.md)
* Wijzig de [syslog-configuratie](log-analytics-data-sources-syslog.md)
