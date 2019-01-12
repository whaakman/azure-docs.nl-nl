---
title: Gebruik en kosten voor Azure Log Analytics beheren | Microsoft Docs
description: Informatie over het wijzigen van de prijsstelling en volume en retentie beleid voor uw Log Analytics-werkruimte in Azure beheren.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: ed720b0db68a11c573a763c4269349db97977eff
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231067"
---
# <a name="manage-usage-and-costs-for-log-analytics"></a>Gebruik en kosten voor Log Analytics beheren

> [!NOTE]
> In dit artikel wordt beschreven hoe u uw kosten in Log Analytics te beheren door in te stellen de bewaartermijn voor gegevens.  Raadpleeg de volgende artikelen voor meer informatie.
> - [Gegevensgebruik analyseren in Log Analytics](manage-cost-storage.md) wordt beschreven hoe u om te analyseren en ontvang een waarschuwing op het gegevensgebruik van uw.
> - [Gebruik en geschatte kosten bewaken](usage-estimated-costs.md) wordt beschreven hoe u gebruik en geschatte kosten voor meerdere Azure-bewakingsfuncties voor verschillende prijsmodellen. Ook wordt beschreven hoe u kunt wijzigen van het prijsmodel.

Log Analytics is ontworpen om te schalen en ondersteuning voor het verzamelen, indexering en opslaan van grote hoeveelheden gegevens per dag van elke bron in uw onderneming of geïmplementeerd in Azure.  Hoewel dit mogelijk een belangrijkste reden voor uw organisatie, is kostenefficiënt uiteindelijk het onderliggende stuurprogramma. Dit is ook afhankelijk van het geselecteerde plan wat dat betreft, het is belangrijk om te begrijpen dat de kosten van een Log Analytics-werkruimte NET is niet gebaseerd op het volume van gegevens die zijn verzameld, en hoe lang u hebt gekozen voor het opslaan van gegevens die zijn gegenereerd op basis van uw verbonden bronnen.  

In dit artikel bekijken we hoe u proactief bewaken groei voor het volume en de opslag van gegevens, en limieten voor het beheren van de bijbehorende kosten definiëren. 

De kosten van de gegevens zijn aanzienlijk, afhankelijk van de volgende factoren: 

- Volume van gegevens gegenereerd en die worden opgenomen in de werkruimte 
    - Aantal oplossingen voor ingeschakeld
    - Aantal systemen die worden bewaakt
    - Type van de gegevens die worden verzameld van elke bewaakte bron 
- De hoeveelheid tijd die u besluit om uw gegevens te behouden 

## <a name="understand-your-workspaces-usage-and-estimated-cost"></a>Informatie over het gebruik en geschatte kosten van uw werkruimte
Log Analytics maakt het gemakkelijk te begrijpen wat de kosten zijn waarschijnlijk worden op basis van recente gebruikspatronen.  U doet dit door gebruik **Log Analytics gebruik en geschatte kosten** om te controleren en gegevensgebruik analyseren. De ziet u hoeveel gegevens worden verzameld door elke oplossing, hoeveel gegevens worden bewaard en een schatting van uw kosten op basis van de hoeveelheid gegevens die zijn opgenomen en eventuele aanvullende bewaarperiode na de inbegrepen hoeveelheid.

![Gebruik en geraamde kosten](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Als u wilt uw gegevens in meer detail te verkennen, klikt u op het pictogram aan de bovenkant van een van de grafieken in het **gebruik en geschatte kosten** pagina. Nu kunt u werken met deze query voor het verkennen van meer informatie over uw gebruik.  

![Logboeken weergeven](media/manage-cost-storage/logs.png)

Uit de **gebruik en geschatte kosten** pagina kunt u uw gegevensvolume bekijken voor de maand. Dit omvat alle gegevens ontvangen en opgeslagen in uw Log Analytics-werkruimte.  Klik op **informatie over het gebruik** vanaf de bovenkant van de pagina om het dashboard met gebruiksgegevens met informatie over trends van volume door de bron, computers en aanbieding weer te geven. Klik op weergeven en een limiet instellen of wijzigen van de bewaarperiode **gegevensvolumebeheer**.
 
Log Analytics kosten worden toegevoegd aan uw Azure-factuur. U kunt details van uw Azure onder de sectie facturering van Azure portal of in een factuur bekijken de [Azure Billing Portal](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Daglimiet
Een dagelijkse limiet configureren en de dagelijkse opname van gegevens voor uw werkruimte beperken, maar wees voorzichtig als het doel mag geen aan de dagelijkse limiet bereikt.  Anders verliest u de gegevens voor de rest van de dag die invloed kan zijn op andere Azure-services en oplossingen waarvan functionaliteit mogelijk afhankelijk van de meest recente gegevens beschikbaar worden gesteld in de werkruimte.  Als gevolg hiervan de mogelijkheid om te zien en krijg een waarschuwing wanneer de voorwaarden van de status van resources ondersteuning van IT-services worden beïnvloed.  De dagelijkse limiet is bedoeld om te worden gebruikt als een manier voor het beheren van de onverwachte toename in aantal gegevens van beheerde resources en blijf binnen uw limiet, of als u wilt beperken gewoon ongeplande charges voor uw werkruimte.  

Wanneer de dagelijkse limiet is bereikt, stopt het verzamelen van factureerbare gegevenstypen voor de rest van de dag. De banner van een waarschuwing wordt weergegeven aan de bovenkant van de pagina voor de geselecteerde Log Analytics-werkruimte en een bewerkingsgebeurtenis wordt verzonden naar de *bewerking* tabel onder **LogManagement** categorie. Het verzamelen van gegevens wordt hervat nadat de tijd voor opnieuw instellen die zijn gedefinieerd onder *dagelijkse limiet wordt ingesteld op*. Het is raadzaam om het definiëren van een waarschuwingsregel op basis van deze bewerkingsgebeurtenis, geconfigureerd om te melden wanneer de dagelijkse limiet is bereikt. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identificeren welke dagelijkse limiet voor gegevens definiëren 
Beoordeling [Log Analytics-gebruik en geraamde kosten](usage-estimated-costs.md) voor informatie over de trend voor het opnemen van gegevens en wat is de dagelijkse volumelimiet om te definiëren. Deze moet worden overwogen zorgvuldig, omdat het niet mogelijk om te controleren van uw bronnen nadat de limiet is bereikt. 

### <a name="manage-the-maximum-daily-data-volume"></a>Het maximale dagelijkse gegevensvolume beheren 
De volgende stappen wordt beschreven hoe u een limiet voor het beheren van de hoeveelheid gegevens die Log Analytics wordt per dag opnemen configureren.  

1. Selecteer in de werkruimte in het linkerdeelvenster **Gebruik en geschatte kosten**.
2. Op de **gebruik en geraamde kosten** pagina voor de geselecteerde werkruimte, klikt u op **gegevensvolumebeheer** vanaf de bovenkant van de pagina. 
5. Dagelijkse limiet is **OFF** standaard – klikt u op **ON** wilt inschakelen, en stelt de limiet voor het volume van gegevens in GB per dag.<br><br> ![Log Analytics configureren gegevenslimiet](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Ontvang een waarschuwing wanneer de dagelijkse limiet bereikt
Terwijl we een visuele hint aanwezig in Azure portal wanneer uw limiet drempelwaarde wordt voldaan, wordt dit gedrag niet per se uitlijnen voor het beheren van operationele problemen die onmiddellijke aandacht.  Voor het ontvangen van een waarschuwingsmelding, kunt u een nieuwe waarschuwingsregel maken in Azure Monitor.  Zie voor meer informatie, [maken, weergeven en beheren van waarschuwingen](alerts-metric.md).      

Als u aan de slag te gaan, moet u hier de aanbevolen instellingen voor de waarschuwing zijn:

* Doel: Selecteer uw Log Analytics-resource
* De criteria: 
   * Signaalnaam: Zoeken in aangepaste logboeken
   * Zoekopdracht: Bewerking | waar Details 'overschrijding ' van het quotum heeft
   * Op basis van: Aantal resultaten
   * Voorwaarde: Groter dan
   * Drempelwaarde: 0
   * Periode: 5 (minuten)
   * Frequentie van: 5 (minuten)
* Naam waarschuwingsregel: Dagelijkse gegevenslimiet bereikt
* Ernst: Waarschuwing (Sev 1)

Zodra de waarschuwing is gedefinieerd en de limiet is bereikt, wordt een waarschuwing wordt geactiveerd en voert het antwoord dat is gedefinieerd in de actiegroep. Deze kennis van uw team via e-mail en SMS-berichten of acties met behulp van webhooks, Automation-runbooks automatiseren of [integreren met een externe ITSM-oplossing](itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Wijzigen van de bewaartermijn voor gegevens 
De volgende stappen wordt beschreven hoe u configureren hoe lang logboek gegevens worden bewaard door in uw werkruimte.
 
1. Selecteer in de werkruimte in het linkerdeelvenster **Gebruik en geschatte kosten**.
2. Klik op bovenaan de pagina **Gebruik en geschatte kosten** op **Gegevensvolumebeheer**.
5. Verplaats de schuifregelaar om te vergroten of verkleinen het aantal dagen en klik vervolgens op in het deelvenster **OK**.  Als u van gebruikmaakt de *gratis* laag, kunt u zich niet wijzigen van de bewaartermijn voor gegevens en moet u upgraden naar de prijscategorie betaald als u wilt beheren met deze instelling.<br><br> ![Werkruimte behoud instelling wijzigen](media/manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="legacy-pricing-tiers"></a>Oudere Prijscategorieën

Klanten met een Enterprise overeenkomst ondertekend voor 1 juli 2018 of die al een Log Analytics-werkruimte in een abonnement hebt gemaakt, u nog steeds toegang hebben tot de *gratis* plan. Als uw abonnement is niet gekoppeld aan een bestaande EA-inschrijving, het *gratis* laag is niet beschikbaar wanneer u een werkruimte in een nieuw abonnement na 2 April 2018 maken.  Gegevens zijn beperkt tot 7 dagen retentie voor de *gratis* laag.  Voor de verouderde *zelfstandige* of *Per knooppunt* lagen, evenals de huidige 2018 één prijscategorie, gegevens die zijn verzameld is beschikbaar voor de afgelopen 31 dagen. De *gratis* laag dagelijkse opname-limiet van 500 MB heeft, en als u merkt dat u consistent meer bedragen dan de toegestane volume, kunt u uw werkruimte wijzigen in een ander schema voor het verzamelen van gegevens buiten deze limiet. 

> [!NOTE]
> Kies de Log Analytics voor het gebruik van de rechten die horen bij de aanschaf van OMS E1-Suite, OMS E2 Suite of OMS-invoegtoepassing voor System Center, *Per knooppunt* prijscategorie.

## <a name="changing-pricing-tier"></a>Prijscategorie wijzigen

Als uw Log Analytics-werkruimte toegang tot de oudere Prijscategorieën heeft, tussen verouderde Prijscategorieën wijzigen:

1. Selecteer in de Azure-portal in het deelvenster voor abonnementen van Log Analytics een werkruimte.

2. In het werkruimtedeelvenster onder **algemene**, selecteer **prijscategorie**.  

3. Onder **prijscategorie**, selecteer een prijscategorie en klik vervolgens op **Selecteer**.  
    ![Prijsplan geselecteerd](media/manage-cost-storage/workspace-pricing-tier-info.png)

Als u verplaatsen van uw werkruimte in de huidige prijscategorie wilt, moet u [wijzigen van uw abonnement controleren prijsmodel in Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/usage-estimated-costs#moving-to-the-new-pricing-model) die de prijscategorie van alle werkruimten in dat abonnement wordt gewijzigd.

> [!NOTE]
> Als de werkruimte is gekoppeld aan een Automation-account, moet u vóórdat u de prijscategorie *Zelfstandig (per GB)* kunt selecteren eerst alle oplossingen **Automation and Control** verwijderen en het Automation-account loskoppelen. Klik op de blade van de werkruimte onder **Algemeen** op **Oplossingen** om oplossingen te bekijken en te verwijderen. Klik op de blade **Prijscategorie** op de naam van het Automation-account om het Automation-account los te koppelen.


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Het oplossen van waarom Log Analytics is niet meer gegevens verzamelen
Als u zich op de oude gratis-laag en meer dan 500 MB aan gegevens op een dag hebt verzonden, stopt het verzamelen van gegevens voor de rest van de dag. De dagelijkse limiet wordt bereikt, is een veelvoorkomende reden die Log Analytics stopt het verzamelen van gegevens of gegevens lijkt te ontbreken.  Log Analytics maakt een gebeurtenis van het type bewerking wanneer het verzamelen van gegevens wordt gestart en gestopt. Voer de volgende query in het zoekvak om te controleren als u de dagelijkse limiet is bereikt en er gegevens ontbreken: 

`Operation | where OperationCategory == 'Data Collection Status' `

Wanneer het verzamelen van gegevens stopt, wordt de OperationStatus waarschuwing. Wanneer het verzamelen van gegevens wordt gestart, wordt de OperationStatus is voltooid. De volgende tabel beschrijft de redenen die het verzamelen van gegevens gestopt en een voorgestelde actie voor het verzamelen van gegevens hervatten:  

|Reden verzameling stopt| Oplossing| 
|-----------------------|---------|
|Dagelijkse limiet van verouderde gratis-laag is bereikt |Wachten tot de volgende dag voor de verzameling automatisch opnieuw opstarten of wijzigen in een betaalde prijscategorie.|
|Dagelijkse limiet van uw werkruimte is bereikt|Wachten op de verzameling automatisch opnieuw wordt gestart of vergroot de dagelijkse gegevenslimiet volume dat wordt beschreven in [beheren van de maximale dagelijkse gegevensvolume](#manage-the-maximum-daily-volume). De tijd voor opnieuw instellen van dagelijkse limiet is wordt weergegeven op de **gegevensvolumebeheer** pagina. |
|Azure-abonnement is de status onderbroken vanwege:<br> Gratis proefversie is beëindigd<br> Azure geslaagd is verlopen<br> Maandelijkse bestedingslimiet bereikt (bijvoorbeeld op een MSDN of Visual Studio-abonnement)|Converteren naar een betaald abonnement<br> Limiet verwijderen of wacht u totdat de limiet wordt opnieuw ingesteld|

Als u wilt worden gewaarschuwd wanneer het verzamelen van gegevens wordt gestopt, gebruikt u de stappen beschreven in *maken dagelijkse gegevenslimiet* waarschuwing wilt worden gewaarschuwd als het verzamelen van gegevens gestopt en volg de stappen gebruikt u de stappen de acties om u te waarschuwen regels configureren van een e-mailbericht, toevoegen webhook- of runbook-actie voor de waarschuwingsregel. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Het oplossen van problemen met een hoger gebruik dan verwacht
Hoger gebruik wordt veroorzaakt door een of beide volgende oorzaken:
- Er worden meer gegevens dan verwacht verzonden naar Log Analytics
- Er worden meer knooppunten dan verwacht verzonden naar Log Analytics

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

Voor meer informatie over het aantal computers (knooppunten) waarvoor gegevens elke dag in de afgelopen maand zijn gerapporteerd, gebruiken

`Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart`

Voor een lijst van computers die verzenden **kosten in rekening gebracht gegevenstypen** (bepaalde gegevenstypen zijn gratis), gebruikmaken van de `_IsBilled` eigenschap:

`union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName`

Gebruik deze `union withsource = tt *` spaarzaam zoals scans voor gegevens gegevens typres duur zijn in het uitvoeren van query's. 

Dit kan worden uitgebreid om terug te keren de telling van computers per uur die verzenden in rekening gebracht gegevenstypen:

`union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc`

Om te zien de **grootte** van factureerbare gebeurtenissen die per computer, gebruikt u de `_BilledSize` eigenschap waarmee u de grootte in bytes:

`union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last `

Deze query vervangt de oude manier om deze query's uitvoeren met het gegevenstype van het gebruik. 

Om te zien de **aantal** van gebeurtenissen die per computer, gebruikt u het volgende:

`union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last`

Als het aantal factureerbare gebeurtenissen die per computer weergeven, gebruikt u 

`union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last`

Als u zien van de aantallen voor factureerbare gegevenstypen zijn gegevens te verzenden naar een specifieke computer wilt, gebruikt:

`union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last `

> [!NOTE]
> Sommige van de velden van het gegevenstype gebruik terwijl u nog steeds in het schema zijn afgeschaft en wordt dat niet meer door hun waarden worden ingevuld. Dit zijn **Computer** en de velden met betrekking tot de opname (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**,  **BatchesCapped** en **AverageProcessingTimeMs**.

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
| Oplossingsgegevens van computers die de oplossing niet nodig hebben | Gebruik [oplossingstargeting](../insights/solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers. |

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

Azure-waarschuwingen bieden ondersteuning voor [logboekwaarschuwingen](alerts-unified-log.md) die gebruikmaken van zoekquery’s. 

De volgende query geeft een resultaat wanneer er meer dan 100 GB aan gegevens in de afgelopen 24 uur is verzameld:

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

De volgende query gebruikt een eenvoudige formule om te voorspellen wanneer meer dan 100 GB aan gegevens op een dag wordt verzonden: 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

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

## <a name="next-steps"></a>Volgende stappen
* Zie [Zoekopdrachten in logboeken in Log Analytics](../log-query/log-query-overview.md) voor meer informatie over het gebruik van de zoektaal. U kunt zoekquery’s gebruiken om aanvullende analyses uit te voeren op de gebruiksgegevens.
* Gebruik de stappen in [Een nieuwe logboekwaarschuwing maken](alerts-metric.md) om een melding te krijgen wanneer aan een zoekcriterium wordt voldaan.
* Gebruik [oplossingstargeting](../insights/solution-targeting.md) om gegevens te verzamelen van alleen de vereiste groepen computers.
* Lees [Filterbeleid van Azure Security Center](../../security-center/security-center-enable-data-collection.md) om een effectief beleid voor het verzamelen van beveiligingsgebeurtenissen te configureren.
* Wijzig de [prestatiemeteritemconfiguratie](data-sources-performance-counters.md).
* Bekijk de [configuratie van gebeurtenislogboek](data-sources-windows-events.md) om de instellingen voor het verzamelen van gebeurtenissen te wijzigen.
* Bekijk [syslog-configuratie](data-sources-syslog.md) om de instellingen voor syslog-verzamelingen te wijzigen.

