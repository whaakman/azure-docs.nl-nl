---
title: Gebruik en kosten voor Azure Monitor logboeken beheren | Microsoft Docs
description: Informatie over het wijzigen van de prijsstelling en beheren van volume en retentie beleid voor uw Log Analytics-werkruimte in Azure Monitor.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: magoedte
ms.subservice: ''
ms.openlocfilehash: 3cad3722a9d0a52b1a0e66c760e948ceb3c1671c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061050"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Het gebruik en kosten met Azure Monitor logboeken beheren

> [!NOTE]
> In dit artikel wordt beschreven hoe u uw kosten in Azure Monitor te beheren door in te stellen de bewaarperiode van gegevens voor uw Log Analytics-werkruimte.  Raadpleeg het volgende artikel voor meer informatie.
> - [Gebruik en geschatte kosten bewaken](usage-estimated-costs.md) wordt beschreven hoe u gebruik en geschatte kosten voor meerdere Azure-bewakingsfuncties voor verschillende prijsmodellen. Ook wordt beschreven hoe u kunt wijzigen van het prijsmodel.

Logboeken in Azure Monitor is ontworpen om te schalen en ondersteuning voor het verzamelen, indexering en opslaan van grote hoeveelheden gegevens per dag van elke bron in uw onderneming of geïmplementeerd in Azure.  Hoewel dit mogelijk een belangrijkste reden voor uw organisatie, is kostenefficiënt uiteindelijk het onderliggende stuurprogramma. Wat dat betreft is het belangrijk om te weten dat de kosten van een Log Analytics-werkruimte is niet gebaseerd op de hoeveelheid gegevens die worden verzameld, dit is ook afhankelijk van het plan is geselecteerd, en hoe lang u hebt gekozen voor het opslaan van gegevens die zijn gegenereerd op basis van uw verbonden bronnen.  

In dit artikel bekijken we hoe u proactief bewaken groei voor het volume en de opslag van gegevens, en limieten voor het beheren van de bijbehorende kosten definiëren. 

De kosten van de gegevens zijn aanzienlijk, afhankelijk van de volgende factoren: 

- Volume van gegevens gegenereerd en die worden opgenomen in de werkruimte 
    - Aantal oplossingen voor ingeschakeld
    - Aantal systemen die worden bewaakt
    - Type van de gegevens die worden verzameld van elke bewaakte bron 
- De hoeveelheid tijd die u besluit om uw gegevens te behouden 

## <a name="understand-your-workspaces-usage-and-estimated-cost"></a>Informatie over het gebruik en geschatte kosten van uw werkruimte

Azure Monitor Logboeken maakt het gemakkelijk te begrijpen wat de kosten zijn waarschijnlijk worden op basis van recente gebruikspatronen. U doet dit door gebruik **Log Analytics gebruik en geschatte kosten** om te controleren en gegevensgebruik analyseren. De ziet u hoeveel gegevens worden verzameld door elke oplossing, hoeveel gegevens worden bewaard en een schatting van uw kosten op basis van de hoeveelheid gegevens die zijn opgenomen en eventuele aanvullende bewaarperiode na de inbegrepen hoeveelheid.

![Gebruik en geraamde kosten](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Als u wilt uw gegevens in meer detail te verkennen, klikt u op het pictogram aan de bovenkant van een van de grafieken in het **gebruik en geschatte kosten** pagina. Nu kunt u werken met deze query voor het verkennen van meer informatie over uw gebruik.  

![Logboeken weergeven](media/manage-cost-storage/logs.png)

Uit de **gebruik en geschatte kosten** pagina kunt u uw gegevensvolume bekijken voor de maand. Dit omvat alle gegevens ontvangen en opgeslagen in uw Log Analytics-werkruimte.  Klik op **informatie over het gebruik** vanaf de bovenkant van de pagina om het dashboard met gebruiksgegevens met informatie op data volume trends die door de bron, computers en aanbieding weer te geven. Klik op weergeven en een limiet instellen of wijzigen van de bewaarperiode **gegevensvolumebeheer**.
 
Log Analytics kosten worden toegevoegd aan uw Azure-factuur. U kunt details van uw Azure onder de sectie facturering van Azure portal of in een factuur bekijken de [Azure Billing Portal](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Daglimiet

Een dagelijkse limiet configureren en de dagelijkse opname van gegevens voor uw werkruimte beperken, maar wees voorzichtig als het doel mag geen aan de dagelijkse limiet bereikt.  Anders verliest u de gegevens voor de rest van de dag die invloed kan zijn op andere Azure-services en oplossingen waarvan functionaliteit mogelijk afhankelijk van de meest recente gegevens beschikbaar worden gesteld in de werkruimte.  Als gevolg hiervan de mogelijkheid om te zien en krijg een waarschuwing wanneer de voorwaarden van de status van resources ondersteuning van IT-services worden beïnvloed.  De dagelijkse limiet is bedoeld om te worden gebruikt als een manier voor het beheren van de onverwachte toename in aantal gegevens van beheerde resources en blijf binnen uw limiet, of als u wilt niet-geplande kosten in rekening gebracht voor uw werkruimte beperken.  

Wanneer de dagelijkse limiet is bereikt, stopt het verzamelen van factureerbare gegevenstypen voor de rest van de dag. De banner van een waarschuwing wordt weergegeven aan de bovenkant van de pagina voor de geselecteerde Log Analytics-werkruimte en een bewerkingsgebeurtenis wordt verzonden naar de *bewerking* tabel onder **LogManagement** categorie. Het verzamelen van gegevens wordt hervat nadat de tijd voor opnieuw instellen die zijn gedefinieerd onder *dagelijkse limiet wordt ingesteld op*. Het is raadzaam om het definiëren van een waarschuwingsregel op basis van deze bewerkingsgebeurtenis, geconfigureerd om te melden wanneer de dagelijkse limiet is bereikt. 

> [!NOTE]
> De dagelijkse limiet, stopt niet het verzamelen van gegevens van Azure Security Center.

### <a name="identify-what-daily-data-limit-to-define"></a>Identificeren welke dagelijkse limiet voor gegevens definiëren

Beoordeling [Log Analytics-gebruik en geraamde kosten](usage-estimated-costs.md) voor informatie over de trend voor het opnemen van gegevens en wat is de dagelijkse volumelimiet om te definiëren. Deze moet worden overwogen zorgvuldig, omdat het niet mogelijk om te controleren van uw bronnen nadat de limiet is bereikt. 

### <a name="manage-the-maximum-daily-data-volume"></a>Het maximale dagelijkse gegevensvolume beheren

De volgende stappen wordt beschreven hoe u een limiet voor het beheren van de hoeveelheid gegevens die Log Analytics-werkruimte wordt per dag opnemen configureren.  

1. Selecteer in de werkruimte in het linkerdeelvenster **Gebruik en geschatte kosten**.
2. Op de **gebruik en geraamde kosten** pagina voor de geselecteerde werkruimte, klikt u op **gegevensvolumebeheer** vanaf de bovenkant van de pagina. 
3. Dagelijkse limiet is **OFF** standaard – klikt u op **ON** wilt inschakelen, en stelt de limiet voor het volume van gegevens in GB per dag.

    ![Log Analytics configureren gegevenslimiet](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Ontvang een waarschuwing wanneer de dagelijkse limiet bereikt

Terwijl we een visuele hint aanwezig in Azure portal wanneer uw limiet drempelwaarde wordt voldaan, wordt dit gedrag niet per se uitlijnen voor het beheren van operationele problemen die onmiddellijke aandacht.  Voor het ontvangen van een waarschuwingsmelding, kunt u een nieuwe waarschuwingsregel maken in Azure Monitor.  Zie voor meer informatie, [maken, weergeven en beheren van waarschuwingen](alerts-metric.md).

Als u aan de slag te gaan, moet u hier de aanbevolen instellingen voor de waarschuwing zijn:

- Doel: Selecteer uw Log Analytics-resource
- De criteria: 
   - Signaalnaam: Zoeken in aangepaste logboeken
   - Zoekopdracht: Bewerking | waar Details 'overschrijding ' van het quotum heeft
   - Op basis van: Aantal resultaten
   - Voorwaarde: Groter dan
   - Drempelwaarde voor: 0
   - Periode: 5 (minuten)
   - Frequentie van: 5 (minuten)
- Naam waarschuwingsregel: Dagelijkse gegevenslimiet bereikt
- Ernst: Waarschuwing (Sev 1)

Zodra de waarschuwing is gedefinieerd en de limiet is bereikt, wordt een waarschuwing wordt geactiveerd en voert het antwoord dat is gedefinieerd in de actiegroep. Deze kennis van uw team via e-mail en SMS-berichten of acties met behulp van webhooks, Automation-runbooks automatiseren of [integreren met een externe ITSM-oplossing](itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Wijzigen van de bewaartermijn voor gegevens

De volgende stappen wordt beschreven hoe u configureren hoe lang logboek gegevens worden bewaard door in uw werkruimte.
 
1. Selecteer in de werkruimte in het linkerdeelvenster **Gebruik en geschatte kosten**.
2. Klik op bovenaan de pagina **Gebruik en geschatte kosten** op **Gegevensvolumebeheer**.
3. Verplaats de schuifregelaar om te vergroten of verkleinen het aantal dagen en klik vervolgens op in het deelvenster **OK**.  Als u van gebruikmaakt de *gratis* laag, kunt u zich niet wijzigen van de bewaartermijn voor gegevens en moet u upgraden naar de prijscategorie betaald als u wilt beheren met deze instelling.

    ![Werkruimte behoud instelling wijzigen](media/manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="legacy-pricing-tiers"></a>Oudere Prijscategorieën

Abonnementen die een Log Analytics-werkruimte of Application Insights-resource in het had vóór 2 April 2018, of zijn gekoppeld aan een Enterprise Agreement die is gestart op 1 februari 2019, blijven toegang hebben tot de oudere Prijscategorieën: **Gratis**, **zelfstandig (Per GB)** en **Per knooppunt (OMS)** .  Toegang tot werkruimten in de prijscategorie gratis heeft de dagelijkse opname van gegevens beperkt tot 500 MB (met uitzondering van de gegevenstypen die worden verzameld door Azure Security Center) en het bewaren van gegevens is beperkt tot 7 dagen. De prijscategorie gratis is alleen bedoeld voor evaluatiedoeleinden. Toegang tot werkruimten in de zelfstandige of Per Prijscategorieën van knooppunten hebben gebruiker configureerbare retentie van maximaal 2 jaar. 

> [!NOTE]
> Kies de Log Analytics voor het gebruik van de rechten die horen bij de aanschaf van OMS E1-Suite, OMS E2 Suite of OMS-invoegtoepassing voor System Center, *Per knooppunt* prijscategorie.

De eerste gebruikers van Log Analytics hebt ook toegang tot de oorspronkelijke Prijscategorieën **Standard** en **Premium**, die het bewaren van gegevens van 30 en 365 dagen respectievelijk hebt opgelost. 

## <a name="changing-pricing-tier"></a>Prijscategorie wijzigen

Als uw Log Analytics-werkruimte toegang tot de oudere Prijscategorieën heeft, tussen verouderde Prijscategorieën wijzigen:

1. Selecteer in de Azure-portal in het deelvenster voor abonnementen van Log Analytics een werkruimte.

2. In het werkruimtedeelvenster onder **algemene**, selecteer **prijscategorie**.  

3. Onder **prijscategorie**, selecteer een prijscategorie en klik vervolgens op **Selecteer**.  
    ![Prijsplan geselecteerd](media/manage-cost-storage/workspace-pricing-tier-info.png)

Als u verplaatsen van uw werkruimte in de huidige prijscategorie wilt, moet u wijzigen van uw abonnement controleren [prijsmodel in Azure Monitor](usage-estimated-costs.md#moving-to-the-new-pricing-model) die de prijscategorie van alle werkruimten in dat abonnement wordt gewijzigd.

> [!NOTE]
> U kunt meer informatie over het instellen van de prijscategorie wanneer [met een Azure Resource Manager-sjabloon](template-workspace-configuration.md#create-a-log-analytics-workspace) voor het maken van een werkruimte en ervoor te zorgen dat de sjabloonimplementatie van uw Azure Resource Manager-slaagt, ongeacht of u de abonnement wordt weergegeven in de oude of nieuwe prijsmodel. 


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Het oplossen van waarom Log Analytics is niet meer gegevens verzamelen

Als u zich op de oude gratis-laag en meer dan 500 MB aan gegevens op een dag hebt verzonden, stopt het verzamelen van gegevens voor de rest van de dag. De dagelijkse limiet wordt bereikt, is een veelvoorkomende reden die Log Analytics stopt het verzamelen van gegevens of gegevens lijkt te ontbreken.  Log Analytics maakt een gebeurtenis van het type bewerking wanneer het verzamelen van gegevens wordt gestart en gestopt. Voer de volgende query in het zoekvak om te controleren als u de dagelijkse limiet is bereikt en er gegevens ontbreken: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Wanneer het verzamelen van gegevens stopt, is het de OperationStatus **waarschuwing**. Wanneer het verzamelen van gegevens wordt gestart, is het de OperationStatus **geslaagd**. De volgende tabel beschrijft de redenen die het verzamelen van gegevens gestopt en een voorgestelde actie voor het verzamelen van gegevens hervatten:  

|Reden verzameling stopt| Oplossing| 
|-----------------------|---------|
|Dagelijkse limiet van verouderde gratis-laag is bereikt |Wachten tot de volgende dag voor de verzameling automatisch opnieuw opstarten of wijzigen in een betaalde prijscategorie.|
|Dagelijkse limiet van uw werkruimte is bereikt|Wachten op de verzameling automatisch opnieuw wordt gestart of toename van de dagelijkse limiet voor volume dat wordt beschreven in het maximale dagelijkse gegevensvolume beheren. De tijd voor opnieuw instellen van dagelijkse limiet is wordt weergegeven op de **gegevensvolumebeheer** pagina. |
|Azure-abonnement is de status onderbroken vanwege:<br> Gratis proefversie is beëindigd<br> Azure geslaagd is verlopen<br> Maandelijkse bestedingslimiet bereikt (bijvoorbeeld op een MSDN of Visual Studio-abonnement)|Converteren naar een betaald abonnement<br> Limiet verwijderen of wacht u totdat de limiet wordt opnieuw ingesteld|

Als u wilt worden gewaarschuwd wanneer het verzamelen van gegevens wordt gestopt, gebruikt u de stappen beschreven in *maken dagelijkse gegevenslimiet* waarschuwing om te worden geïnformeerd wanneer het verzamelen van gegevens wordt gestopt. Gebruik de stappen [een actiegroep maken](action-groups.md) een e-mail, webhook- of -actie voor de waarschuwingsregel configureren. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Het oplossen van problemen met een hoger gebruik dan verwacht

Hoger gebruik wordt veroorzaakt door een of beide volgende oorzaken:
- Meer knooppunten dan verwacht verzonden naar Log Analytics-werkruimte
- Meer gegevens dan verwacht worden verzonden naar Log Analytics-werkruimte

## <a name="understanding-nodes-sending-data"></a>Knooppunten die gegevens verzenden

Voor meer informatie over het aantal computers die rapporteren heartbeats elke dag in de afgelopen maand, gebruik

```kusto
Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```

Als u een lijst met computers wordt gefactureerd als knooppunten als de werkruimte in het knooppunt voor verouderde is Per prijscategorie, zoek naar knooppunten die worden verzonden **kosten in rekening gebracht gegevenstypen** (bepaalde gegevenstypen zijn gratis). U doet dit door gebruik van de `_IsBillable` [eigenschap](log-standard-properties.md#_isbillable) en gebruikt het meest linkse veld van de volledig gekwalificeerde domeinnaam. Hiermee wordt de lijst met computers met de gegevens worden gefactureerd:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Het aantal factureerbare knooppunten die kan worden geschat als: 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| billableNodes=dcount(computerName)
```

> [!NOTE]
> Gebruik deze `union withsource = tt *` spaarzaam scans in verschillende gegevenstypen zijn duur in het uitvoeren van query's. Deze query vervangt de oude manier van het uitvoeren van query's informatie per-computer met het gegevenstype van het gebruik.  

Er is een meer nauwkeurige berekening van wat wordt daadwerkelijk in rekening gebracht om op te halen van het aantal computers per uur worden gefactureerd gegevenstypen worden verzonden. (Voor toegang tot werkruimten in de verouderde prijscategorie Per knooppunt, Log Analytics berekend het aantal knooppunten die moeten worden gefactureerd op uurbasis.) 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="understanding-ingested-data-volume"></a>Inzicht in het opgenomen gegevensvolume

Op de **gebruik en geschatte kosten** pagina, de *opname van gegevens per oplossing* grafiek toont de totale hoeveelheid gegevens die worden verzonden en hoeveel er worden verzonden door elke oplossing. Hiermee kunt u bepalen trends, zoals of de algehele gegevensgebruik (of het gebruik door een bepaalde oplossing) groeit, stabiel blijft of afneemt. De query die wordt gebruikt voor het genereren van dit is

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

Houd er rekening mee dat de component "waar IsBillable = true" gegevenstypen van bepaalde oplossingen waarvoor er geen kosten opname zijn filtert. 

U kunt inzoomen verder Zie gegevenstrends voor specifieke gegevenstypen, bijvoorbeeld als u wilt kijken naar de gegevens vanwege een IIS-logboeken:

```kusto
Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart
```

### <a name="data-volume-by-computer"></a>Gegevensvolume per computer

Om te zien de **grootte** van factureerbare gebeurtenissen die per computer, gebruikt u de `_BilledSize` [eigenschap](log-standard-properties.md#_billedsize), waarmee u de grootte in bytes:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize Bytes=sum(_BilledSize) by  computerName | sort by Bytes nulls last
```

De `_IsBillable` [eigenschap](log-standard-properties.md#_isbillable) geeft aan of de opgenomen gegevens kosten in rekening gebracht.

Om te zien van het aantal **factureerbare** gebeurtenissen die per computer 

```kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount=count() by computerName  | sort by count_ nulls last
```

Als u zien van de aantallen voor factureerbare gegevenstypen zijn gegevens te verzenden naar een specifieke computer wilt, gebruikt:

```kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last
```

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Gegevensvolume per Azure-resource, resourcegroep of abonnement

Voor gegevens van de knooppunten die worden gehost in Azure krijgt u de **grootte** factureerbare gebeurtenissen die zijn opgenomen __per computer__, gebruikt u de _ResourceId [eigenschap](log-standard-properties.md#_resourceid), waarmee u het volledige pad naar de bron:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Voor gegevens van de knooppunten die worden gehost in Azure krijgt u de **grootte** factureerbare gebeurtenissen die zijn opgenomen __per Azure-abonnement__, parseren de `_ResourceId` eigenschap:

```kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

Wijzigen van `subscriptionId` naar `resourceGroup` factureerbare opgenomen gegevensvolume per Azure-resourcegroep worden weergegeven. 


> [!NOTE]
> Sommige van de velden van het gegevenstype gebruik terwijl u nog steeds in het schema zijn afgeschaft en wordt dat niet meer door hun waarden worden ingevuld. Dit zijn **Computer** en de velden met betrekking tot de opname (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**,  **BatchesCapped** en **AverageProcessingTimeMs**.

### <a name="querying-for-common-data-types"></a>Query's uitvoeren voor veelvoorkomende gegevenstypen

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
| Beveiligingsgebeurtenissen            | Selecteer [normale of minimale beveiligingsgebeurtenissen](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier) <br> Wijzig het beleid voor beveiligingscontrole zodat alleen de gewenste gebeurtenissen worden verzameld. Controleer vooral de noodzaak voor het verzamelen van gebeurtenissen voor <br> - [filterplatform controleren](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [register controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [bestandssysteem controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [kernel-object controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [greepbewerking controleren](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> -Verwijderbare opslag controleren |
| Prestatiemeteritems       | Wijzig de [Prestatiemeteritemconfiguratie](data-sources-performance-counters.md) in: <br> - Frequentie van het verzamelen van gegevens beperken <br> - Aantal prestatiemeteritems beperken |
| Gebeurtenislogboeken                 | Wijzig [Configuratie van gebeurtenislogboek](data-sources-windows-events.md) in: <br> - Aantal verzamelde gebeurtenislogboeken beperken <br> - Alleen vereiste gebeurtenisniveaus verzamelen. Bijvoorbeeld, gebeurtenissen op *informatie*niveau niet verzamelen |
| Syslog                     | Wijzig de [syslog-configuratie](data-sources-syslog.md) in: <br> - Aantal verzamelde installaties beperken <br> - Alleen vereiste gebeurtenisniveaus verzamelen. Bijvoorbeeld, gebeurtenissen op *informatie*- en *foutopsporings*niveau niet verzamelen |
| AzureDiagnostics           | Wijzig de resourcelogboekverzameling om: <br> - Het aantal resources dat logboeken naar Log Analytics verzendt te verkleinen <br> - Alleen vereiste logboeken te verzamelen |
| Oplossingsgegevens van computers die de oplossing niet nodig hebben | Gebruik [oplossingstargeting](../insights/solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers. |

### <a name="getting-security-and-automation-node-counts"></a>Aan beveiligings- en Automation-knooppunt wordt geteld

Als u op 'Per knooppunt (OMS)' prijscategorie, wordt in gebracht rekening op basis van het aantal knooppunten en oplossingen u gebruikt, het aantal inzichten en analyseknooppunten waarvoor u worden kosten in rekening gebracht in de tabel worden weergegeven op de **gebruik en geschatte kosten**pagina.  

Als u wilt zien van het aantal afzonderlijke knooppunten voor beveiliging, kunt u de query:

```kusto
union
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
| count
```

Als u wilt zien van het aantal afzonderlijke knooppunten voor automatisering, gebruikt u de query:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="create-an-alert-when-data-collection-is-high"></a>Maken van een waarschuwing wanneer gegevensverzameling hoog is

In deze sectie wordt beschreven hoe u een waarschuwing instelt als:
- Het gegevensvolume groter is dan een opgegeven hoeveelheid.
- Het gegevensvolume naar verwachting een opgegeven hoeveelheid gaat overschrijden.

Azure-waarschuwingen bieden ondersteuning voor [logboekwaarschuwingen](alerts-unified-log.md) die gebruikmaken van zoekquery’s. 

De volgende query geeft een resultaat wanneer er meer dan 100 GB aan gegevens in de afgelopen 24 uur is verzameld:

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type 
| where DataGB > 100
```

De volgende query gebruikt een eenvoudige formule om te voorspellen wanneer meer dan 100 GB aan gegevens op een dag wordt verzonden: 

```kusto
union withsource = $table Usage 
| where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true 
| extend Type = $table 
| summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type 
| where EstimatedGB > 100
```

Als u een waarschuwing wilt instellen bij een ander gegevensvolume, wijzigt u 100 in de query's in het aantal GB waarbij u een waarschuwing wilt.

Gebruik de stappen in [Een nieuwe logboekwaarschuwing maken](alerts-metric.md) om een melding te krijgen wanneer de hoeveelheid verzamelde gegevens groter is dan verwacht.

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

Maak een nieuwe [Actiegroep](action-groups.md) of geef een bestaande op, zodat u een melding ontvangt wanneer aan de criteria voor een logboekwaarschuwing wordt voldaan.

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

Maak een nieuwe [Actiegroep](action-groups.md) of geef een bestaande op, zodat u een melding ontvangt wanneer aan de criteria voor een logboekwaarschuwing wordt voldaan.

Wanneer u een waarschuwing ontvangt, gebruikt u de stappen in de volgende sectie om te bepalen waarom het-gebruik is hoger dan verwacht.

## <a name="limits-summary"></a>Samenvatting van limieten

Er zijn enkele aanvullende beperkingen voor Log Analytics, sommige hiervan afhankelijk zijn van de prijscategorie met Log Analytics. Deze worden beschreven [hier](https://docs.microsoft.com/azure/azure-subscription-service-limits#log-analytics-limits).


## <a name="next-steps"></a>Volgende stappen

- Zie [zoekopdrachten in Logboeken van Azure Monitor](../log-query/log-query-overview.md) voor informatie over het gebruik van de zoektaal. U kunt zoekquery’s gebruiken om aanvullende analyses uit te voeren op de gebruiksgegevens.
- Gebruik de stappen in [Een nieuwe logboekwaarschuwing maken](alerts-metric.md) om een melding te krijgen wanneer aan een zoekcriterium wordt voldaan.
- Gebruik [oplossingstargeting](../insights/solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers.
- Voor het configureren van een doeltreffende beleid voor het verzamelen, Bekijk [filterbeleid van Azure Security Center](../../security-center/security-center-enable-data-collection.md).
- Wijzig de [prestatiemeteritemconfiguratie](data-sources-performance-counters.md).
- Bekijk de [configuratie van gebeurtenislogboek](data-sources-windows-events.md) om de instellingen voor het verzamelen van gebeurtenissen te wijzigen.
- Bekijk [syslog-configuratie](data-sources-syslog.md) om de instellingen voor syslog-verzamelingen te wijzigen.