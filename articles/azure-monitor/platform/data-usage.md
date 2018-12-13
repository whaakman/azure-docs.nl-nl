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
ms.topic: conceptual
ms.date: 08/11/2018
ms.author: magoedte
ms.openlocfilehash: af41e2af87afbbeb8629d07508d4e5244351a4df
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53277339"
---
# <a name="analyze-data-usage-in-log-analytics"></a>Gegevensgebruik analyseren in Log Analytics

> [!NOTE]
> In dit artikel wordt beschreven hoe u gegevensgebruik analyseren in Log Analytics.  Raadpleeg de volgende artikelen voor meer informatie.
> - [Kosten beheren door het gegevensvolume en retentie in Log Analytics beheren](manage-cost-storage.md) wordt beschreven hoe u uw kosten te beheren door het veranderen van de retentieperiode van uw gegevens.
> - [Gebruik en geschatte kosten bewaken](../../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) wordt beschreven hoe u gebruik en geschatte kosten voor meerdere Azure-bewakingsfuncties voor verschillende prijsmodellen. Ook wordt beschreven hoe u kunt wijzigen van het prijsmodel.

## <a name="understand-usage"></a>Inzicht in gebruik

Gebruik **Log Analytics gebruik en geschatte kosten** om te controleren en gegevensgebruik analyseren. De ziet u hoeveel gegevens worden verzameld door elke oplossing, hoeveel gegevens worden bewaard en een schatting van uw kosten op basis van de hoeveelheid gegevens die zijn opgenomen en eventuele aanvullende bewaarperiode na de inbegrepen hoeveelheid.

![Gebruik en geraamde kosten](media/data-usage/usage-estimated-cost-dashboard-01.png)<br>

Als u wilt uw gegevens in meer detail te verkennen, klikt u op het pictogram aan de bovenkant van een van de grafieken in het **gebruik en geschatte kosten** pagina. Nu kunt u werken met deze query voor het verkennen van meer informatie over uw gebruik.  

![Logboeken weergeven](media/data-usage/logs.png)<br>

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Het oplossen van problemen met een hoger gebruik dan verwacht
Hoger gebruik wordt veroorzaakt door een of beide volgende oorzaken:
- Er worden meer gegevens dan verwacht verzonden naar Log Analytics
- Meer knooppunten dan verwacht verzenden van gegevens naar Log Analytics of bepaalde knooppunten verzenden meer gegevens dan normaal

We gaan kijken hoe kunnen we meer informatie over beide van deze oorzaken. 

> [!NOTE]
> Sommige velden van het type gebruik gegevens terwijl u nog steeds in het schema zijn afgeschaft en hun waarden niet meer worden ingevuld. Dit zijn **Computer** en de velden met betrekking tot de opname (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**,  **BatchesCapped** en **AverageProcessingTimeMs**.
> Zie hieronder voor de nieuwe manier om op te vragen van de hoeveelheid opgenomen gegevens per computer. 

### <a name="data-volume"></a>Gegevensvolume 
Op de **gebruik en geschatte kosten** pagina, de *opname van gegevens per oplossing* grafiek toont de totale hoeveelheid gegevens die worden verzonden en hoeveel er worden verzonden door elke oplossing. Hiermee kunt u bepalen trends, zoals of de algehele gegevensgebruik (of het gebruik door een bepaalde oplossing) groeit, stabiel blijft of afneemt. De query die wordt gebruikt voor het genereren van dit is

`Usage| where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

Houd er rekening mee dat de component "waar IsBillable = true" gegevenstypen van bepaalde oplossingen waarvoor er geen kosten opname zijn filtert. 

U kunt inzoomen verder Zie gegevenstrends voor specifieke gegevenstypen, bijvoorbeeld als u wilt kijken naar de gegevens vanwege een IIS-logboeken:

`Usage| where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

### <a name="nodes-sending-data"></a>Knooppunten die gegevens verzenden

Voor meer informatie over het aantal computers (knooppunten) waarvoor gegevens zijn gerapporteerd in de afgelopen maand, gebruik

`Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(ComputerIP) by bin(TimeGenerated, 1d)    
| render timechart`

Om te zien de **grootte** van factureerbare gebeurtenissen die per computer, gebruikt u de `_BilledSize` eigenschap waarmee u de grootte in bytes:

`union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last `

Gebruik deze query's spaarzaam scans voor gegevens gegevens typres zijn kostbaar zijn om uit te voeren. Deze query vervangt de oude manier om deze query's uitvoeren met het gegevenstype van het gebruik. 

Om te zien de **aantal** van gebeurtenissen die per computer, gebruikt u het volgende:

`union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last`

Als het aantal factureerbare gebeurtenissen die per computer weergeven, gebruikt u 

`union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last`

Als u zien van de aantallen voor factureerbare gegevenstypen zijn gegevens te verzenden naar een specifieke computer wilt, gebruikt:

`union withsource = tt *
| where Computer == "*computer name*"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last `

Om u te verdiepen in de bron van gegevens voor een bepaald type, volgen hier enkele handige voorbeelden van query's:

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

### <a name="tips-for-reducing-data-volume"></a>Tips voor het gegevensvolume verminderen

Enkele suggesties voor het verminderen van het volume van de logboeken die worden verzameld zijn onder andere:

| Bron van hoog gegevensvolume | Het gegevensvolume verminderen |
| -------------------------- | ------------------------- |
| Beveiligingsgebeurtenissen            | Selecteer [normale of minimale beveiligingsgebeurtenissen](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) <br> Wijzig het beleid voor beveiligingscontrole zodat alleen de gewenste gebeurtenissen worden verzameld. Controleer vooral de noodzaak voor het verzamelen van gebeurtenissen voor <br> - [filterplatform controleren](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [register controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [bestandssysteem controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [kernel-object controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [greepbewerking controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> -Verwijderbare opslag controleren |
| Prestatiemeteritems       | Wijzig de [Prestatiemeteritemconfiguratie](data-sources-performance-counters.md) in: <br> - Frequentie van het verzamelen van gegevens beperken <br> - Aantal prestatiemeteritems beperken |
| Gebeurtenislogboeken                 | Wijzig [Configuratie van gebeurtenislogboek](data-sources-windows-events.md) in: <br> - Aantal verzamelde gebeurtenislogboeken beperken <br> - Alleen vereiste gebeurtenisniveaus verzamelen. Bijvoorbeeld, gebeurtenissen op *informatie*niveau niet verzamelen |
| Syslog                     | Wijzig de [syslog-configuratie](data-sources-syslog.md) in: <br> - Aantal verzamelde installaties beperken <br> - Alleen vereiste gebeurtenisniveaus verzamelen. Bijvoorbeeld, gebeurtenissen op *informatie*- en *foutopsporings*niveau niet verzamelen |
| AzureDiagnostics           | Wijzig de resourcelogboekverzameling om: <br> - Het aantal resources dat logboeken naar Log Analytics verzendt te verkleinen <br> - Alleen vereiste logboeken te verzamelen |
| Oplossingsgegevens van computers die de oplossing niet nodig hebben | Gebruik [oplossingstargeting](../../azure-monitor/insights/solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers. |

### <a name="getting-node-counts"></a>Telt het aantal aan knooppunt 

Als u op 'Per knooppunt (OMS)' prijscategorie, wordt in gebracht rekening op basis van het aantal knooppunten en oplossingen u gebruikt, het aantal inzichten en analyseknooppunten waarvoor u worden kosten in rekening gebracht in de tabel worden weergegeven op de **gebruik en geschatte kosten**pagina.  

Als u wilt zien van het aantal afzonderlijke knooppunten voor beveiliging, kunt u de query:

`union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count`

Als u wilt zien van het aantal afzonderlijke knooppunten voor automatisering, gebruikt u de query:

` ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc`

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Een waarschuwing instellen wanneer de gegevensverzameling groter is dan verwacht
In deze sectie wordt beschreven hoe u een waarschuwing instelt als:
- Het gegevensvolume groter is dan een opgegeven hoeveelheid.
- Het gegevensvolume naar verwachting een opgegeven hoeveelheid gaat overschrijden.

Azure-waarschuwingen bieden ondersteuning voor [logboekwaarschuwingen](../../azure-monitor/platform/alerts-unified-log.md) die gebruikmaken van zoekquery’s. 

De volgende query geeft een resultaat wanneer er meer dan 100 GB aan gegevens in de afgelopen 24 uur is verzameld:

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

De volgende query gebruikt een eenvoudige formule om te voorspellen wanneer meer dan 100 GB aan gegevens op een dag wordt verzonden: 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

Als u een waarschuwing wilt instellen bij een ander gegevensvolume, wijzigt u 100 in de query's in het aantal GB waarbij u een waarschuwing wilt.

Gebruik de stappen in [Een nieuwe logboekwaarschuwing maken](../../azure-monitor/platform/alerts-metric.md) om een melding te krijgen wanneer de hoeveelheid verzamelde gegevens groter is dan verwacht.

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

Maak een nieuwe [Actiegroep](../../azure-monitor/platform/action-groups.md) of geef een bestaande op, zodat u een melding ontvangt wanneer aan de criteria voor een logboekwaarschuwing wordt voldaan.

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

Maak een nieuwe [Actiegroep](../../azure-monitor/platform/action-groups.md) of geef een bestaande op, zodat u een melding ontvangt wanneer aan de criteria voor een logboekwaarschuwing wordt voldaan.

Wanneer u een waarschuwing ontvangt, gebruikt u de stappen in de volgende sectie om te bepalen waarom het-gebruik is hoger dan verwacht.

## <a name="next-steps"></a>Volgende stappen
* Zie [Zoekopdrachten in logboeken in Log Analytics](../log-query/log-query-overview.md) voor meer informatie over het gebruik van de zoektaal. U kunt zoekquery’s gebruiken om aanvullende analyses uit te voeren op de gebruiksgegevens.
* Gebruik de stappen in [Een nieuwe logboekwaarschuwing maken](../../azure-monitor/platform/alerts-metric.md) om een melding te krijgen wanneer aan een zoekcriterium wordt voldaan.
* Gebruik [oplossingstargeting](../insights/solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers.
* Lees [Filterbeleid van Azure Security Center](../../security-center/security-center-enable-data-collection.md) om een effectief beleid voor het verzamelen van beveiligingsgebeurtenissen te configureren.
* Wijzig de [prestatiemeteritemconfiguratie](data-sources-performance-counters.md).
* Bekijk de [configuratie van gebeurtenislogboek](data-sources-windows-events.md) om de instellingen voor het verzamelen van gebeurtenissen te wijzigen.
* Bekijk [syslog-configuratie](data-sources-syslog.md) om de instellingen voor syslog-verzamelingen te wijzigen.
