---
title: Gegevens met logboek zoekopdrachten niet vinden in Azure Log Analytics | Microsoft Docs
description: Met zoeken in logboeken kunt u alle machinegegevens vanuit meerdere bronnen binnen uw omgeving combineren en correleren.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: aa4608d37b06db88819e6175dcf8f94a7e13f04a
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="find-data-using-log-searches-in-log-analytics"></a>Gegevens met behulp van logboek zoekopdrachten in logboekanalyse zoeken

>[!NOTE]
> Dit artikel wordt beschreven logboek zoekopdrachten met de verouderde querytaal in logboekanalyse.  Als uw werkruimte is bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), en u naar verwijzen moet [Understanding logboek zoekt in logboekanalyse (nieuw)](log-analytics-log-search-new.md).


De kern van logboekanalyse is de functie voor het zoeken van logboek zodat u kunt combineren en correleren machinegegevens uit meerdere bronnen binnen uw omgeving. Oplossingen worden ook aangedreven door logboek search zodat u metrische gegevens om een bepaald probleemgebied gedraaid.

U kunt een query maken op de pagina zoeken en vervolgens wanneer u zoekt, u kunt de resultaten filteren met behulp van besturingselementen facet. U kunt ook een geavanceerde query's op transformatie en filter het rapport maken op uw resultaten.

Algemene logboek zoekquery's weergegeven op de meeste oplossing's. U kunt tegels klikt u op of met andere items voor meer informatie over het item met behulp van logboek search omlaag in de OMS-portal.

In deze zelfstudie behandelen we voorbeelden aan de basisprincipes uitgelegd wanneer u logboek zoeken gebruikt.

We beginnen met eenvoudige, praktische voorbeelden en vervolgens op deze maken zodat u een goed begrip van praktische gebruiksdoeleinden over het gebruik van de syntaxis van de inzichten die u wilt ophalen uit de gegevens kunt ophalen.

Nadat u nagegaan wat de zoektechnieken hebt, kunt u controleren de [logboekanalyse Meld zoeken verwijzing](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Basic-filters gebruiken
Het eerste wat u moet weten is dat het eerste deel van een zoekopdracht query voordat een ' | ' verticale pipe-teken is altijd een *filter*. U kunt dit beschouwen als een WHERE-component in TSQL--bepaalt *wat* subset van gegevens voor het ophalen van buiten de oeren werkruimte. Zoeken in het gegevensarchief is grotendeels over het opgeven van de kenmerken van de gegevens die u ophalen, wilt dus is het natuurlijke dat een query met de component WHERE beginnen wilt.

De meest eenvoudige filters kunt u zijn *trefwoorden*, zoals 'fout' of 'out' of een computernaam op. Deze typen van eenvoudige query's worden meestal diverse vormen van gegevens binnen de dezelfde resultatenset retourneren. Dit komt doordat Log Analytics uit verschillende *typen* van gegevens in het systeem.

### <a name="to-conduct-a-simple-search"></a>Voor het uitvoeren van een eenvoudige zoekopdrachten
1. Klik in de OMS-portal op **logboek zoeken**.  
    ![tegel Search](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Typ in het queryveld `error` en klik vervolgens op **Search**.  
    ![Fout bij zoeken](./media/log-analytics-log-searches/oms-search-error.png)  
    Bijvoorbeeld: de query voor `error` geretourneerd in de volgende afbeelding 100.000 **gebeurtenis** records (verzameld door Log-beheer), 18 **ConfigurationAlert** records (gegenereerd door de configuratie bepalen) en 12 **ConfigurationChange** records (vastgelegd door de bijhouden).   
    ![zoekresultaten](./media/log-analytics-log-searches/oms-search-results01.png)  

Deze filters zijn niet echt objectklassen typen. *Type* is zojuist een code, of een eigenschap of een tekenreeks/naam/categorie, die is gekoppeld aan een stukje informatie. Een aantal documenten in het systeem zijn gelabeld als **Type: ConfigurationAlert** en sommige zijn gelabeld als **Type: Perf**, of **gebeurtenistype:**, enzovoort. Elke zoekresultaat, document, record of vermelding geeft de onbewerkte eigenschappen en hun waarden voor elk van deze onderdelen van gegevens en kunt u de veldnamen in het filter opgeven wanneer u wilt ophalen, alleen de records waarin het veld heeft die waarde krijgen.

*Type* is gewoon een veld dat alle records hebt, is niet anders dan een ander veld. Dit is ingesteld op basis van de waarde van het veld Type. Deze record heeft een andere vorm of vorm. Incidenteel, **Type = Perf**, of **Type gebeurtenis =** is ook de syntaxis die u nodig hebt voor meer informatie over query uitvoeren op prestatiegegevens of gebeurtenissen.

U kunt na de veldnaam en voor de waarde een dubbele punt (:) of een gelijkteken (=) gebruiken. **Gebeurtenistype:** en **Type gebeurtenis =** zijn equivalent in de zin, kunt u de gewenste stijl.

Ja, als het Type = Perf records hebben een veld met de naam 'CounterName' en vervolgens kunt u een query die lijkt op `Type=Perf CounterName="% Processor Time"`.

Hiermee krijgt u alleen de prestatiegegevens als de prestatiemeternaam is '% processortijd'.

### <a name="to-search-for-processor-time-performance-data"></a>Om te zoeken naar de processor tijd prestatiegegevens
* Typ in het veld van de query zoeken`Type=Perf CounterName="% Processor Time"`

U kunt ook niet meer specifiek en **InstanceName = _ 'Totaal'** in de query die een Windows-prestatiemeteritem is. U kunt ook een facet en een andere selecteren **veldwaarde:**. Het filter wordt automatisch toegevoegd aan het filter op de balk query. U kunt dit zien in de volgende afbeelding. Er wordt aangegeven waar u moet klikken om toe te voegen **InstanceName: '_Totaal'** aan de query zonder iets te typen.

![facet zoeken](./media/log-analytics-log-searches/oms-search-facet.png)

Er wordt nu uw query`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

In dit voorbeeld hebt u niet opgeven **Type = Perf** om dit te bereiken. Omdat de velden CounterName en InstanceName bestaan alleen voor records van het Type = prestaties, de query is dit specifiek genoeg om terug te keren hetzelfde resultaat als de vorige langer een:

```
CounterName="% Processor Time" InstanceName="_Total"
```

Dit komt doordat de filters in de query worden geëvalueerd als *en* met elkaar. Effectief, hoe meer velden die u toevoegt aan de criteria, krijgt u minder specifiek en verfijnd resultaten.

Bijvoorbeeld: de query `Type=Event EventLog="Windows PowerShell"` is identiek aan `Type=Event AND EventLog="Windows PowerShell"`. Retourneert alle gebeurtenissen die zijn aangemeld en verzameld uit het gebeurtenislogboek van Windows PowerShell. Als u een filter meerdere keren toevoegen als u de dezelfde facet herhaaldelijk selecteert en vervolgens het probleem uitsluitend cosmetisch is--deze mogelijk bruikbaar blijft de zoekbalk, maar deze nog steeds dezelfde resultaten retourneert omdat de impliciete operator en er altijd is.

U kunt eenvoudig de impliciete operator en met behulp van een niet-operator expliciet omkeren. Bijvoorbeeld:

`Type:Event NOT(EventLog:"Windows PowerShell")`of de bijbehorende equivalent `Type=Event EventLog!="Windows PowerShell"` retourneren van alle gebeurtenissen van andere logboeken die niet het logboek voor Windows PowerShell.

Of u kunt andere Booleaanse operator gebruikt, zoals 'Of'. De volgende query retourneert records die het gebeurtenislogboek een toepassing of het systeem is.

```
EventLog=Application OR EventLog=System
```

Met behulp van de bovenstaande query, krijgt u vermeldingen voor beide logboeken in de dezelfde resultatenset.

Echter, als u de of verwijdert door het impliciete en in plaats, vervolgens de volgende query wordt geen queryplan resultaten omdat er een logboekvermelding die deel uitmaakt van zowel Logboeken niet. Elke logboekvermelding is geschreven naar slechts één van de twee logboeken.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Aanvullende filters gebruiken
De volgende query retourneert vermeldingen voor 2 gebeurtenislogboeken voor alle computers die gegevens verzonden.

```
EventLog=Application OR EventLog=System
```

![zoekresultaten](./media/log-analytics-log-searches/oms-search-results03.png)

Een van de velden of filters te selecteren wordt de query beperken met een bepaalde computer, met uitzondering van alle andere protocollen. De resulterende query zou als volgt uit.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Die gelijk is aan de volgende vanwege de impliciete AND.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Elke query wordt geëvalueerd in de volgende expliciete volgorde. Let op het haakje.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Net als het veld gebeurtenislogboek kunt u alleen gegevens voor een set specifieke computers ophalen door toe te voegen of. Bijvoorbeeld:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Op deze manier dit de volgende query **% CPU-tijd** voor alleen de geselecteerde twee computers.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a>Veldtypen
Wanneer u filters maakt, moet u de verschillen in het werken met verschillende typen velden die zijn geretourneerd door het logboek zoekopdrachten begrijpen.

**De doorzoekbare velden** weergeven in blauw weergegeven in de zoekresultaten.  U kunt de doorzoekbare velden in specifieke zoekcriteria naar het veld zoals de volgende:

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

**Vrije tekst doorzoekbare velden** grijs in de zoekresultaten worden weergegeven.  Ze kunnen niet worden gebruikt met de zoekcriteria die specifiek zijn voor het veld zoals doorzoekbare velden.  Ze worden alleen bij het uitvoeren van een query in alle velden, zoals de volgende doorzocht.

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a>Booleaanse operators
Met datetime en numerieke velden, kunt u zoeken naar waarden die *groter is dan*, *minder dan*, en *kleiner dan of gelijk*. U kunt eenvoudig operators zoals >, <>, =, < =,! = in de zoekbalk query.

U kunt een specifiek gebeurtenislogboek opvragen voor een bepaalde periode. Bijvoorbeeld, wordt de afgelopen 24 uur uitgedrukt met de volgende symbool expressie.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Om te zoeken met behulp van een Booleaanse operator
* Typ in het veld van de query zoeken`EventLog=System TimeGenerated>NOW-24HOURS`  
    ![zoeken met Booleaanse waarde](./media/log-analytics-log-searches/oms-search-boolean.png)

Hoewel u het tijdsinterval grafisch beheren kunt en meestal kunt u dat doet, moet u er voordelen van een tijdfilter inclusief rechtstreeks in de query zijn. Bijvoorbeeld, dit goed werkt met dashboards waar u de tijd voor elke tegel ongeacht negeren kunt de *globale* selector tijd op de dashboardpagina. Zie voor meer informatie [tijd belangrijk is in het Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Bij het filteren door de tijd, houd er rekening mee dat u krijgt resultaten voor de *snijpunt* van de twee perioden: de versie opgegeven in de OMS-portal (S1) en de versie opgegeven in de query (S2).

![snijpunt](./media/log-analytics-log-searches/oms-search-intersection.png)

Dit betekent dat, als de perioden elkaar niet overlappen, bijvoorbeeld in de OMS-portal waar u desgewenst **deze week** en in de query waarin u definiëren **afgelopen week**, is er geen snijpunt en ontvangt u geen resultaten opgeleverd.

Vergelijkingsoperators gebruikt voor het veld TimeGenerated zijn ook nuttig zijn in andere gevallen. Bijvoorbeeld, met numerieke velden.

Bijvoorbeeld gegeven dat waarschuwingen van de beoordeling van de configuratie van de volgende ernstwaarden hebben:

* 0 = informatie
* 1 = waarschuwing
* 2 = kritiek

U kunt doorzoeken op waarschuwingen en kritieke waarschuwingen en informatieve netwerken met de volgende query ook uitsluiten:

```
Type=ConfigurationAlert  Severity>=1
```


U kunt ook query's bereik gebruiken. Dit betekent dat u het begin en einde bereik van waarden in een reeks kunt opgeven. Bijvoorbeeld als u gebeurtenissen uit het gebeurtenislogboek van Operations Manager waar de gebeurtenis-id groter dan of gelijk zijn aan 2100 maar niet groter zijn dan 2199 is wilt, zou klikt u vervolgens de volgende query terugzetten.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> De syntaxis van het bereik moet u de veldwaarde: dubbele punt (:) scheidingsteken is en *niet* het gelijkteken (=). Zet de boven- en ondergrenzen einde van het bereik tussen vierkante haken en scheiden met twee punten (.).
>
>

## <a name="manipulate-search-results"></a>Zoekresultaten manipuleren
Wanneer u gegevens zoekt, moet u uw zoekopdracht te verfijnen en een goede niveau van controle over de resultaten. Wanneer resultaten worden opgehaald, kunt u opdrachten voor het transformeren van ze toepassen.

Opdrachten in logboekanalyse zoekopdrachten *moet* Volg na het teken verticale pipe (|). Een filter moet altijd het eerste deel van een queryreeks. Definieert de gegevensset waarmee u samenwerkt en vervolgens 'doorgesluisd' de resultaten in een opdrachtregel. U kunt vervolgens de pipe aanvullende opdrachten toe te voegen. Dit is los vergelijkbaar met de Windows PowerShell-pijplijn.

In het algemeen probeert de taal van de zoekopdracht logboekanalyse te volgen richtlijnen zodat vergelijkbaar met de IT-professionals, en het leerproces vereenvoudigen en PowerShell-stijl.

Opdrachten hebben namen van termen, zodat u kunt in één oogopslag zien wat ze doen.  

### <a name="sort"></a>Sorteren
De opdracht sorteren kunt u de sorteervolgorde op een of meer velden definiëren. Zelfs als u niet, standaard gebruikt, wordt een tijd aflopende volgorde afgedwongen. De meest recente resultaten zijn altijd boven aan de lijst met zoekresultaten. Dit betekent dat wanneer u een zoekopdracht met uitvoert `Type=Event EventID=1234` wat daadwerkelijk wordt uitgevoerd voor u is:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Gebeurt dit omdat het type van de gebruikerservaring die u bekend met in Logboeken bent. Bijvoorbeeld, in de Windows-Logboeken.

Sorteren kunt u wijzigen hoe resultaten worden geretourneerd. De volgende voorbeelden laten zien hoe dit werkt.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


De bovenstaande eenvoudige voorbeelden laten zien hoe opdrachten werken--ze de vorm van de resultaten die het filter geretourneerd wijzigen.

### <a name="limit-and-top"></a>Limiet en boven
Een andere minder bekende opdracht is een LIMIET. De limiet is een PowerShell-achtige term. Limiet is identiek aan de eerste opdracht. De volgende query's retourneren dezelfde resultaten.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Om te zoeken met boven
* Typ in het veld van de query zoeken`Type=Event EventID=600 | Top 1`   
    ![Zoek boven](./media/log-analytics-log-searches/oms-search-top.png)

In de afbeelding hierboven, zijn er 358 duizend records met EventID = 600. De velden, facetten en filters aan de linkerkant altijd tonen informatie over de resultaten *door het filter gedeelte* van de query, het gedeelte voordat een pipe-teken is. De **resultaten** deelvenster retourneert alleen de meest recente 1 resultaat, omdat de opdracht in de vorm en de resultaten getransformeerd.

### <a name="select"></a>Selecteer
De opdracht SELECT gedraagt zich als Select-Object in PowerShell. Het resultaat gefilterde resultaten die niet beschikt over alle hun oorspronkelijke eigenschappen. In plaats daarvan wordt alleen de eigenschappen die u opgeeft geselecteerd.

#### <a name="to-run-a-search-using-the-select-command"></a>Een zoekopdracht met behulp van de select-opdracht uitvoeren
1. Typ in zoeken `Type=Event` en klik vervolgens op **Search**.
2. Klik op **+ meer** in een van de resultaten om weer te geven van alle eigenschappen die de resultaten hebben.
3. Sommige van deze expliciet selecteren en de query is gewijzigd in `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Selecteer zoeken](./media/log-analytics-log-searches/oms-search-select.png)

Met deze opdracht is bijzonder nuttig wanneer u wilt zoeken uitvoer en kiest u alleen de delen van gegevens die echt belangrijk voor uw onderzoek, hetgeen vaak niet de volledige record. Dit is ook handig wanneer de records van verschillende typen hebben *sommige* algemene eigenschappen, maar niet *alle* van hun eigenschappen gelden. De uitvoer genereren die meer toe op een tabel lijkt of geschikt voor geëxporteerd naar een CSV-bestand en vervolgens massaged in Excel.

## <a name="use-the-measure-command"></a>Gebruik de opdracht meting
METING is een van de meest veelzijdige opdrachten in logboekanalyse zoekopdrachten. U kunt toepassen statistische *functies* voor uw gegevens en statistische resultaten gegroepeerd op een bepaald veld. Er zijn meerdere statistische functies die ondersteuning biedt voor meting.

### <a name="measure-count"></a>Meting count()
De eerste statistische functie werken met en een van de eenvoudigste om te begrijpen is de *count()* functie.

Resultaat is van een zoekopdracht zoals `Type=Event`, ook wel facetten aan de linkerkant van de zoekresultaten filters weer. De filters tonen een verdeling van waarden met een bepaald veld voor de resultaten in de zoekopdracht uitgevoerd.

![zoeken maateenheid count](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Bijvoorbeeld, in de bovenstaande afbeelding ziet u de **Computer** veld en geeft aan dat binnen de bijna 739 duizend gebeurtenissen in de resultaten, 68 uniek en anders waarden voor de **Computer** veld zijn in deze records. De tegel bevat alleen de eerste 5, die de meest voorkomende 5 waarden die zijn geschreven zijn met de **Computer** velden), gesorteerd op het aantal documenten die de specifieke waarde in het veld bevatten. In de afbeelding ziet u dat-tussen deze gebeurtenissen bijna 369 duizend – 90 duizend afkomstig zijn van de computer OpsInsights04.contoso.com, 83 duizend vanaf de computer DB03.contoso.com, enzovoort.

Wat gebeurt er als u wilt zien van alle waarden, omdat de tegel alleen de top 5 bevat?

Dit is wat de opdracht meting met de functie count() kunt doen. Deze functie heeft geen parameters gebruikt. U alleen het veld waarop u groeperen wilt op: Geef de **Computer** veld in dit geval:

`Type=Event | Measure count() by Computer`

![zoeken maateenheid count](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Echter **Computer** is slechts een veld gebruikt *in* elke stukje informatie: Er zijn geen relationele databases die zijn betrokken, en er is geen afzonderlijke **Computer** overal object. De waarden *in* de gegevens wordt beschreven welke entiteit gegenereerd deze, en een aantal andere kenmerken en aspecten van de gegevens – daarom de term *facet*. U kunt echter alleen ook groeperen op andere velden. Omdat de oorspronkelijke resultaten van bijna 739 duizend gebeurtenissen die zijn doorgesluisd naar de opdracht meting ook een veld met de naam hebben **EventID**, kunt u dezelfde techniek om groeperen op dat veld en krijgt u een aantal gebeurtenissen dat door de gebeurtenis-id toepassen:

```
Type=Event | Measure count() by EventID
```

Als u niet bent geïnteresseerd in het aantal werkelijke records die een specifieke waarde bevatten, maar in plaats daarvan als u een lijst met de waarden zelf wilt, kunt u toevoegen een *Selecteer* aan het einde van deze opdracht in en selecteer de eerste kolom:

```
Type=Event | Measure count() by EventID | Select EventID
```

Vervolgens kunt u meer geavanceerde ophalen en vooraf sorteer de resultaten in de query of u kunt alleen de kolommen in het raster te klikken.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Om te zoeken met behulp van de maateenheid count
* Typ in het veld van de query zoeken`Type=Event | Measure count() by EventID`
* Append `| Select EventID` aan het einde van de query.
* Ten slotte toevoegen `| Sort EventID asc` aan het einde van de query.

Er zijn een aantal belangrijke zaken te merken en leg de nadruk op:

Eerst zijn de resultaten niet de oorspronkelijke onbewerkte resultaten meer. In plaats daarvan zijn ze cumulatieve resultaten – in wezen groepen van de resultaten. Dit geen probleem, maar moet u weten dat u bent interactie met een heel andere vorm van gegevens die afwijkt van de oorspronkelijke onbewerkte vorm die wordt gemaakt als gevolg van de aggregatie/statistische functie.

Tweede, **aantal meten** momenteel retourneert alleen de top 100 verschillende resultaten. Deze beperking geldt niet voor de statistische functies. U moet dus meestal een filter te gebruiken nauwkeurigere eerst om te zoeken naar specifieke items voordat u een meting count() toepassen.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>De max en min-functies gebruiken met de opdracht meting
Er zijn verschillende scenario's waarbij **meting Max()** en **meting zijn** zijn nuttig. Echter, omdat elke functie tegengestelde laten we van elkaar hebt Max() zien en u kunt experimenteren met zijn zelf.

Als u een query voor beveiligingsgebeurtenissen, hebben ze een **niveau** eigenschap die kan verschillen. Bijvoorbeeld:

```
Type=SecurityEvent
```

![zoeken maatregel telling starten](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Als u wilt weergeven van de hoogste waarde voor alle van de beveiligingsgroep kunt gebeurtenissen krijgt een gemeenschappelijke Computer, de group by-veld, u

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![maximale computer zoeken meting](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Die wordt weergegeven voor de computers waarop **niveau** records, de meeste van deze hebben ten minste 8, niveau veel had een niveau van 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![maximale zoektijd meting gegenereerd computer](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Deze functie werkt goed samen met de getallen, maar het werkt ook met DateTime-velden. Dit is handig om te controleren op het laatste of de meest recente tijdstempel van elk deel van de gegevens die zijn geïndexeerd voor elke computer. Bijvoorbeeld: wanneer is de meest recente beveiligingsgebeurtenis gerapporteerd voor elke computer?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Gebruik de functie Avg. met de opdracht meting
De statistische functie gebruikt met meting van Avg() kunt u voor het berekenen van de gemiddelde waarde van een veld en de Groepsresultaten door de dezelfde of een ander veld. Dit is handig in tal van gevallen, zoals de prestatiegegevens.

We beginnen met de prestatiegegevens. Houd er rekening mee dat Log Analytics momenteel prestatiemeteritems voor Windows- en Linux-machines verzamelt.

Om te zoeken naar *alle* prestatiegegevens, de meest eenvoudige query is:

```
Type=Perf
```

![Gem. start zoeken](./media/log-analytics-log-searches/oms-search-avg01.png)

Het eerste wat u kunt zien is dat Log Analytics u drie perspectieven ziet: lijst ziet u waarin de werkelijke records achter de grafieken; Tabel waarin een tabellarische weergave van gegevens van prestatiemeteritems; metrische gegevens en geeft en grafieken voor de prestatiemeteritems.

In de afbeelding hierboven zijn er twee sets van velden gemarkeerd die duiden op het volgende:

* De eerste set identificeert naam Windows Prestatiemeter, objectnaam en exemplaarnaam in de query-filter. Dit zijn de velden die u waarschijnlijk meest worden gebruikt als facetten/filters
* **Tegenwaarde** de werkelijke waarde van de teller. In dit voorbeeld wordt de waarde is *75*.
* **TimeGenerated** 12:51, in 24-uurs tijdnotatie is.

Hier volgt een overzicht van de metrische gegevens in een grafiek.

![Gem. start zoeken](./media/log-analytics-log-searches/oms-search-avg02.png)

Na het lezen over de prestaties record vorm en hebben meer informatie over andere zoektechnieken, kunt u de meting Avg() gebruiken voor het cumuleren van dit type numerieke gegevens.

Hier volgt een voorbeeld van een eenvoudige:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Gem. samplevalue zoeken](./media/log-analytics-log-searches/oms-search-avg03.png)

In dit voorbeeld selecteert u de totale CPU-tijd prestaties teller en gemiddelde per Computer. Als u uw resultaten tot alleen de laatste zes uur beperken wilt, kunt u het besturingselement van het filter tijd gebruiken of Geef in de query als volgt:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Om te zoeken met de opdracht meting met behulp van de functie Avg.
* Typ in het zoekvak query `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.

U kunt aggregeren en correleren van gegevens *over* computers. Stel bijvoorbeeld dat u een set van hosts in een bepaald soort farm waar elk knooppunt is gelijk aan andere een hebt en ze hier allemaal dezelfde typen van werk en moet worden ongeveer met gelijke taakverdeling. U kunt hun items die in een gaan met de volgende query en gemiddelden ophalen voor de gehele farm kan ophalen. U kunt starten door het kiezen van de computers met het volgende voorbeeld:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Nu dat u de computers hebt, u ook alleen wilt selecteren, twee prestatie-indicatoren (KPI's): % CPU-gebruik en % vrije schijfruimte. Ja, dat deel van de query als volgt uit:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

U kunt nu computers en prestatiemeteritems met het volgende voorbeeld toevoegen:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Omdat er een specifieke selectie de **Avg() meten** opdracht kunt terugkeren het gemiddelde niet door de computer, maar in de farm gewoon door groepering door CounterName. Bijvoorbeeld:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Hiermee krijgt u een handig compacte weergave van een aantal KPI's voor uw omgeving.

![groepering van Search Gem.](./media/log-analytics-log-searches/oms-search-avg04.png)

U kunt eenvoudig de query in een dashboard. U kan bijvoorbeeld de zoekopdracht opslaan en een dashboard maken vanuit deze met de naam *Web Farm KPI's*. Zie voor meer informatie over het gebruik van dashboards, [maken van een aangepast dashboard in logboekanalyse](log-analytics-dashboards.md).

![Search-dashboard, Gem.](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Gebruik de functie sum met de opdracht meting
De functie sum is vergelijkbaar met andere functies van de meting-opdracht. U ziet een voorbeeld over het gebruik van de functie sum op [W3C IIS-logboeken zoeken in Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

U kunt Max() en zijn met de getallen, datums en tijden en tekenreeksen. Met tekenreeksen, ze alfabetische volgorde worden gesorteerd en u eerste en laatste.

U kunt de bladwijzers echter niet gebruiken met iets anders dan numerieke velden. Dit geldt ook voor Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Gebruik de percentielfunctie met de opdracht meting
De percentielfunctie is vergelijkbaar met Avg() of bladwijzers in die u het alleen voor numerieke velden gebruiken kunt. U kunt percentiel tussen 1 en 99 op een numeriek veld gebruiken. U kunt ook beide **percentiel** en **pct** opdrachten. Hier volgen enkele voorbeelden:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Gebruik waar u de opdracht
De waar opdracht als een filter werkt, maar deze kan worden toegepast in de pijplijn cumulatieve resultaten die zijn gemaakt door een opdracht meting – verder te filteren in plaats van onbewerkte resultaten die worden gefilterd aan het begin van een query.

Bijvoorbeeld:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

U kunt een andere pipe toevoegen ' | ' teken en waar u de opdracht die moet worden alleen ophalen computers waarvan gemiddelde CPU hoger dan 80% met het volgende voorbeeld is:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Als u bekend met de Microsoft System Center - Operations Manager bent, kunt u zien waar u de opdracht in termen van management pack. Als in het voorbeeld zou een regel, is het eerste deel van de query de gegevensbron en waar u de opdracht de detectie van conditie voor zou zijn.

U kunt de query als een tegel in **mijn Dashboard**, als een monitor van sorteringen om te zien wanneer de computer CPU's zijn te veel gebruikt. Zie voor meer informatie over dashboards, [maken van een aangepast dashboard in logboekanalyse](log-analytics-dashboards.md). U kunt ook maken en dashboards met de mobiele app gebruiken. Zie voor meer informatie [OMS mobiele App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). In de tegels onder twee van de volgende afbeelding ziet u de monitor een lijst weergegeven en als een getal. In wezen, wilt u altijd het aantal moet nul zijn en de lijst leeg zijn. Anders geeft een meldingsvoorwaarde aan. Indien nodig, kunt u deze bekijken op welke computers onder druk zijn.

![mobiele dashboard](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Gebruik de operator in
De *IN* -operator, samen met *NOT IN* kunt u subsearches die zoekopdrachten met een andere zoekopdracht als argument gebruiken. Ze zijn opgenomen in de accolades {} binnen een andere *primaire* of *buitenste* zoeken. Het resultaat van een subsearch, vaak een lijst met verschillende resultaten, wordt vervolgens gebruikt als argument voor de primaire zoekactie.

U kunt subsearches subsets van uw gegevens dat u een beschrijving kan niet rechtstreeks in een zoekexpressie, maar die kunnen worden gegenereerd vanuit een zoekopdracht overeenkomen. Bijvoorbeeld, als u geïnteresseerd bent in het zoeken van alle gebeurtenissen uit met een zoekopdracht *computers met ontbrekende beveiligingsupdates*, moet u voor het ontwerpen van een subsearch die eerst dat identificeert *computers met ontbrekende beveiligingsupdates* voordat er gebeurtenissen die horen bij deze hosts.

U kunt dus express *computers momenteel ontbreekt vereiste beveiligingsupdates* met de volgende query:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![IN voorbeeld zoeken](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Zodra u de lijst hebt, kunt u de zoekopdracht als een binnenste zoekopdracht om de lijst met computers in een buitenste (primaire) zoeken die wordt gezocht naar gebeurtenissen voor deze computers. U doen dit door de binnenste zoekopdracht insluitende accolades en de resultaten voeding als mogelijke waarden voor een filterveld in de buitenste search met behulp van de operator. De query zou zijn vergelijkbaar met:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![IN voorbeeld zoeken](./media/log-analytics-log-searches/oms-search-in02-revised.png)

Ook voor de aankondiging het tijdfilter gebruikt in het binnenste zoeken omdat de Update-evaluatie System een momentopname van alle computers om de 24 uur maakt. Kunt u de interne query meer licht en nauwkeurige door zoekt alleen naar een dag. De buitenste zoekopdracht in plaats daarvan maakt gebruik van de tijdzone in de gebruikersinterface ophalen van gebeurtenissen van de afgelopen 7 dagen. Zie [Booleaanse operators](#boolean-operators) voor meer informatie over tijd operators.

Omdat u echt gebruik alleen de resultaten van de binnenste zoekopdracht als een filter waarde voor de buitenste: u kunt nog steeds opdrachten in de buitenste zoekopdracht toepassen. U kunt bijvoorbeeld nog steeds de bovenstaande gebeurtenissen met een andere meting opdracht groeperen:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![IN voorbeeld zoeken](./media/log-analytics-log-searches/oms-search-in03-revised.png)

U wilt over het algemeen de binnenste query snel worden uitgevoerd, omdat logboekanalyse aan servicezijde time-outs voor heeft en een kleine hoeveelheid resultaten retourneren. Als de interne query meer resultaten oplevert, de lijst met resultaten afgekapt, die de buitenste zoekopdracht om terug te keren onjuiste resultaten kan veroorzaken.

Een andere regel wordt de interne zoekopdracht momenteel moet worden geleverd *geaggregeerde* resultaten. Met andere woorden, deze moet bevatten een *meting* opdracht; u kan niet op dit moment onbewerkte resultaten in een buitenste zoekopdracht feed.

Bovendien kan er slechts één IN-operator en moet het laatste filter in de query. Meerdere IN operators kunnen niet worden of zou – hierdoor in principe kan meerdere subsearches uitgevoerd: het belangrijkste is dat slechts één sub/interne zoeken is mogelijk dat elke buitenste zoekopdracht.

Zelfs bij deze limieten IN kan veel soorten gecorreleerde zoekopdrachten en kunt u definiëren iets soortgelijks als in groepen, zoals computers, gebruikers of bestanden – ongeacht de velden in uw gegevens bevatten. Hier vindt u meer voorbeelden:

**Alle updates ontbreken van computers waarop automatische updates-instelling wordt uitgeschakeld**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Alle foutgebeurtenissen van computers met SQL Server (= waar SQL-analyse is uitgevoerd)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Alle beveiligingsgebeurtenissen van computers die domeincontrollers (= waar AD analyse is uitgevoerd)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Die andere accounts zijn aangemeld bij dezelfde computers waarop BACONLAND\jochan account heeft aangemeld?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Gebruik de afzonderlijke opdracht
Als de naam wordt voorgesteld, biedt deze opdracht een lijst met verschillende waarden voor een veld. Het is verrassend eenvoudige maar wel handig. Hetzelfde kan worden verkregen met meting count() opdracht ook, zoals hieronder wordt weergegeven.

```
Type=Event | Measure count() by Computer
```

![Voorbeeld van de opdracht DISTINCT zoeken](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Als u geïnteresseerd bent in is echter alleen een lijst met afzonderlijke waarden en niet het aantal documenten met waarden en klik vervolgens DISTINCT kan bieden schonere en gemakkelijker te lezen van de uitvoer en kortere syntaxis, zoals hieronder weergegeven.

```
Type=Event | Distinct Computer
```
![Voorbeeld van de opdracht DISTINCT zoeken](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Gebruik de functie countdistinct met de opdracht meting
De functie countdistinct telt het aantal afzonderlijke waarden in elke groep. Het kan bijvoorbeeld worden gebruikt om het aantal unieke computers melden voor elk Type:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Gebruik de opdracht measure-interval
Met bijna Realtime prestatiegegevens verzamelen, kunt u verzamelen en visualiseren van elk prestatiemeteritem in logboekanalyse. Eenvoudig in te voeren van de query **Type: Perf** duizenden metrische grafieken op basis van het aantal items en servers in uw omgeving Log Analytics wordt geretourneerd. Met metrische aggregatie op aanvraag, kunt u de algemene metrische gegevens in uw omgeving op een hoog niveau en diepgaand naar meer gedetailleerde gegevens als u wilt bekijken.

Stel dat u wilt weten wat de gemiddelde CPU-capaciteit is in alle uw computers. Kijken naar het gemiddelde CPU voor elke computer mogelijk niet meer nuttig zijn omdat resultaten kunnen ophalen vloeiend moeten worden gemaakt. Als u wilt zoeken naar meer informatie, kunt u samenvoegen in uw resultaat in een kleinere tijd venster segmenten gedownload en zoek een tijdreeks over verschillende dimensies. Bijvoorbeeld, kunt u uitvoeren het per uur gemiddelde CPU-gebruik op alle computers als volgt:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![gemiddelde interval meting](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Standaard worden deze resultaten worden weergegeven in een lijndiagram met meerdere reeksen interactieve.  Dit diagram ondersteunt reeks schakelen (met y-as schaling aanpassen), zoomen en aanwijzen.  De tabeloptie weergeven is nog steeds beschikbaar voor het weergeven van de onbewerkte gegevens, indien nodig.

U kunt ook andere velden groeperen. In dit voorbeeld ik zoek de % prestatiemeteritems voor een specifieke computer en ik wil weten wat de percentielen elk uur 70 van elke teller is:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Er is alleen Houd er rekening mee dat deze query's niet beperkt tot prestatiemeteritems zijn. U kunt deze toepassen op alle metrische gegevens. In dit voorbeeld zoek ik op de W3C-IIS-logboeken. Ik wil weten wat de maximale tijd die nodig is tijdens een interval van 5 minuten voor het verwerken van elke aanvraag is:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Gebruik van meerdere statistische functies in één query
U kunt meerdere cumulatieve componenten opgeven in de opdracht van een meting.  Elk criterium kan alias onafhankelijk zijn.  Als u krijgt een alias niet de naam van het resulterende veld worden de statistische functie die is gebruikt (dat wil zeggen ' avg(CounterValue)' voor avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Hier volgt nog een voorbeeld:

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over logboek zoekopdrachten:

* Gebruik [aangepaste velden in logboekanalyse](log-analytics-custom-fields.md) logboek zoekopdrachten uitbreiden.
* Controleer de [logboekanalyse Meld zoeken verwijzing](log-analytics-search-reference.md) om weer te geven van alle zoekvelden en facetten beschikbaar in Log Analytics.
