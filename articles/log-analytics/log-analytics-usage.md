---
title: Gegevensgebruik analyseren in Log Analytics | Microsoft Docs
description: Gebruik het dashboard Gebruik en geschatte kosten in Log Analytics om te evalueren hoeveel gegevens naar Log Analytics worden verzonden en te identificeren wat onvoorziene stijgingen zou kunnen veroorzaken.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: d02c3ad3e1ca2812049608cad2eacced3686dad3
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128561"
---
# <a name="analyze-data-usage-in-log-analytics"></a>Gegevensgebruik analyseren in Log Analytics
Log Analytics bevat informatie over de hoeveelheid gegevens die is verzameld, vanuit welke bronnen de gegevens zijn verzonden, en de verschillende typen gegevens die zijn verzonden.  Gebruik het **Log Analytics-gebruiksdashboard** om het gegevensgebruik te controleren en analyseren. Het dashboard laat zien hoeveel gegevens worden verzameld door elke oplossing en hoeveel gegevens uw computers verzenden.

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

![Gebruiks- en kostendashboard](./media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>
)

### <a name="to-work-with-usage-data"></a>Werken met gebruiksgegevens
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.<br><br> ![Azure Portal](./media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
3. Selecteer een werkruimte in de lijst met Log Analytics-werkruimten.
4. Selecteer **Gebruik en geschatte kosten** in de lijst in het linkerdeelvenster.
5. Op het dashboard **Gebruik en geschatte kosten** kunt u het tijdsbereik wijzigen door **Tijd: afgelopen 24 uur** te selecteren en het tijdsinterval te wijzigen.<br><br> ![tijdsinterval](./media/log-analytics-usage/usage-time-filter-01.png)<br><br>
6. Bekijk de blades voor de gebruikscategorie waarin gebieden worden weergegeven waarin u bent geïnteresseerd. Kies een blade en klik vervolgens op een item op de blade om meer details weer te geven in [Zoeken in logboeken](log-analytics-log-searches.md).<br><br> ![voorbeeld van kpi voor gegevensgebruik](media/log-analytics-usage/data-volume-kpi-01.png)<br><br>
7. Bekijk op het dashboard Zoeken in logboeken de resultaten die zijn geretourneerd na de zoekopdracht.<br><br> ![voorbeeld van gebruik van zoeken in logboeken](./media/log-analytics-usage/usage-log-search-01.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Een waarschuwing instellen wanneer de gegevensverzameling groter is dan verwacht
In deze sectie wordt beschreven hoe u een waarschuwing instelt als:
- Het gegevensvolume groter is dan een opgegeven hoeveelheid.
- Het gegevensvolume naar verwachting een opgegeven hoeveelheid gaat overschrijden.

Azure-waarschuwingen bieden ondersteuning voor [logboekwaarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-log.md) die gebruikmaken van zoekquery’s. 

De volgende query geeft een resultaat wanneer er meer dan 100 GB aan gegevens in de afgelopen 24 uur is verzameld:

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

De volgende query gebruikt een eenvoudige formule om te voorspellen wanneer meer dan 100 GB aan gegevens op een dag wordt verzonden: 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

Als u een waarschuwing wilt instellen bij een ander gegevensvolume, wijzigt u 100 in de query's in het aantal GB waarbij u een waarschuwing wilt.

Gebruik de stappen in [Een nieuwe logboekwaarschuwing maken](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) om een melding te krijgen wanneer de hoeveelheid verzamelde gegevens groter is dan verwacht.

Bij het instellen van de waarschuwing voor de eerste query - wanneer er meer dan 100 GB aan gegevens in 24 uur, stelt u het volgende in:  

- **Waarschuwingsvoorwaarde definiëren** - geef uw Log Analytics-werkruimte op als het resourcedoel.
- **Waarschuwingscriteria** - geef het volgende op:
   - **Signaalnaam** - selecteer **Aangepast zoeken in logboeken**
   - **Zoekquery** op `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`
   - **Waarschuwingslogica** is **Gebaseerd op** het *aantal resultaten*, en **Voorwaarde** is *Groter dan* een **Drempelwaarde** van *0*
   - **Tijdsperiode** van *1440* minuten en **Waarschuwingsfrequentie** van elke *60* minuten, omdat de gebruiksgegevens maar één keer per uur worden bijgewerkt.
- **Waarschuwingsdetails definiëren** - geef het volgende op:
   - **Naam** op *Gegevensvolume groter dan 100 GB in 24 uur*
   - **Ernst** op *Waarschuwing*

Maak een nieuwe [Actiegroep](../monitoring-and-diagnostics/monitoring-action-groups.md) of geef een bestaande op, zodat u een melding ontvangt wanneer aan de criteria voor een logboekwaarschuwing wordt voldaan.

Bij het maken van de waarschuwing voor de tweede query - wanneer wordt voorspeld dat er meer dan 100 GB aan gegevens in 24 uur zal zijn, stelt u het volgende in:

- **Waarschuwingsvoorwaarde definiëren** - geef uw Log Analytics-werkruimte op als het resourcedoel.
- **Waarschuwingscriteria** - geef het volgende op:
   - **Signaalnaam** - selecteer **Aangepast zoeken in logboeken**
   - **Zoekquery** op `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`
   - **Waarschuwingslogica** is **Gebaseerd op** het *aantal resultaten*, en **Voorwaarde** is *Groter dan* een **Drempelwaarde** van *0*
   - **Tijdsperiode** van *180* minuten en **Waarschuwingsfrequentie** van elke *60* minuten, omdat de gebruiksgegevens maar één keer per uur worden bijgewerkt.
- **Waarschuwingsdetails definiëren** - geef het volgende op:
   - **Naam** op *Gegevensvolume naar verwachting groter dan 100 GB in 24 uur*
   - **Ernst** op *Waarschuwing*

Maak een nieuwe [Actiegroep](../monitoring-and-diagnostics/monitoring-action-groups.md) of geef een bestaande op, zodat u een melding ontvangt wanneer aan de criteria voor een logboekwaarschuwing wordt voldaan.

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

Op de volgende schermafbeelding verzendt het gegevenstype *Log Management / Perf* de meeste gegevens voor de computer.<br><br> ![gegevensvolume voor een computer](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)<br><br>

Ga vervolgens terug naar het dashboard *Gebruik* en bekijk het diagram *Gegevensvolume per oplossing*. Klik op de naam van de oplossing in de lijst voor een overzicht van de computers die de meeste gegevens voor een oplossing verzenden. Klik op de naam van de eerste oplossing in de lijst. 

In de volgende schermafbeelding ziet u dat de computer *mycon* de meeste gegevens verzendt voor het beheersysteem voor de oplossing Log Management.<br><br> ![gegevensvolume voor een oplossing](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)<br><br>

Voer indien nodig extra analyses uit om grote volumes binnen een oplossing of gegevenstype te identificeren. Voorbeelden van query's zijn:

+ **Beveiligingsoplossing**
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ **Logboekbeheeroplossing**
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ **Perf-gegevenstype**
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ **Gebeurtenisgegevenstype**
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ **Syslog-gegevenstype**
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ **AzureDiagnostics**-gegevenstype
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

Gebruik de volgende stappen om het volume van de logboeken die worden verzameld te beperken:

| Bron van hoog gegevensvolume | Het gegevensvolume verminderen |
| -------------------------- | ------------------------- |
| Beveiligingsgebeurtenissen            | Selecteer [normale of minimale beveiligingsgebeurtenissen](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) <br> Wijzig het beleid voor beveiligingscontrole zodat alleen de gewenste gebeurtenissen worden verzameld. Controleer vooral de noodzaak voor het verzamelen van gebeurtenissen voor <br> - [filterplatform controleren](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [register controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [bestandssysteem controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [kernel-object controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [greepbewerking controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> -Verwijderbare opslag controleren |
| Prestatiemeteritems       | Wijzig de [Prestatiemeteritemconfiguratie](log-analytics-data-sources-performance-counters.md) in: <br> - Frequentie van het verzamelen van gegevens beperken <br> - Aantal prestatiemeteritems beperken |
| Gebeurtenislogboeken                 | Wijzig [Configuratie van gebeurtenislogboek](log-analytics-data-sources-windows-events.md) in: <br> - Aantal verzamelde gebeurtenislogboeken beperken <br> - Alleen vereiste gebeurtenisniveaus verzamelen. Bijvoorbeeld, gebeurtenissen op *informatie*niveau niet verzamelen |
| Syslog                     | Wijzig de [syslog-configuratie](log-analytics-data-sources-syslog.md) in: <br> - Aantal verzamelde installaties beperken <br> - Alleen vereiste gebeurtenisniveaus verzamelen. Bijvoorbeeld, gebeurtenissen op *informatie*- en *foutopsporings*niveau niet verzamelen |
| AzureDiagnostics           | Wijzig de resourcelogboekverzameling om: <br> - Het aantal resources dat logboeken naar Log Analytics verzendt te verkleinen <br> - Alleen vereiste logboeken te verzamelen |
| Oplossingsgegevens van computers die de oplossing niet nodig hebben | Gebruik [oplossingstargeting](../operations-management-suite/operations-management-suite-solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Controleren of er meer knooppunten zijn dan verwacht
Als u gebruikmaakt van de prijscategorie *per knooppunt (OMS)*, worden de kosten berekend op basis van het aantal knooppunten en oplossingen die u gebruikt. In de sectie *Aanbiedingen* van het dashboard met gebruiksgegevens kunt u zien hoeveel knooppunten van elke aanbieding er worden gebruikt.<br><br> ![gebruiksdashboard](./media/log-analytics-usage/log-analytics-usage-offerings.png)<br><br>

Klik op **Alles weergeven...**  om de volledige lijst met computers die gegevens verzenden voor het geselecteerde pakket weer te geven.

Gebruik [oplossingstargeting](../operations-management-suite/operations-management-suite-solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers.

## <a name="next-steps"></a>Volgende stappen
* Zie [Zoekopdrachten in logboeken in Log Analytics](log-analytics-log-searches.md) voor meer informatie over het gebruik van de zoektaal. U kunt zoekquery’s gebruiken om aanvullende analyses uit te voeren op de gebruiksgegevens.
* Gebruik de stappen in [Een nieuwe logboekwaarschuwing maken](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) om een melding te krijgen wanneer aan een zoekcriterium wordt voldaan.
* Gebruik [oplossingstargeting](../operations-management-suite/operations-management-suite-solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers.
* Lees [Filterbeleid van Azure Security Center](../security-center/security-center-enable-data-collection.md) om een effectief beleid voor het verzamelen van beveiligingsgebeurtenissen te configureren.
* Wijzig de [prestatiemeteritemconfiguratie](log-analytics-data-sources-performance-counters.md).
* Bekijk de [configuratie van gebeurtenislogboek](log-analytics-data-sources-windows-events.md) om de instellingen voor het verzamelen van gebeurtenissen te wijzigen.
* Bekijk [syslog-configuratie](log-analytics-data-sources-syslog.md) om de instellingen voor syslog-verzamelingen te wijzigen.
