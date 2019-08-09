---
title: Query's schrijven voor Azure Data Explorer
description: In deze procedure leert u hoe u eenvoudige en geavanceerdere query's kunt uitvoeren voor Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/07/2019
ms.openlocfilehash: 80d3eaaf7e588766d62f5e5885d75e61c590970e
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881184"
---
# <a name="write-queries-for-azure-data-explorer"></a>Query's schrijven voor Azure Data Explorer

In dit artikel leert u hoe u de query taal in azure Data Explorer kunt gebruiken om eenvoudige query's uit te voeren met de meest voorkomende Opera tors. U krijgt ook een aantal van de meer geavanceerde functies van de taal.

## <a name="prerequisites"></a>Vereisten

U kunt de query's in dit artikel op een van de volgende twee manieren uitvoeren:

- Op het Azure Data Explorer- *Help-cluster* dat we hebben ingesteld voor hulp bij het leren.
    [Meld u aan bij het cluster](https://dataexplorer.azure.com/clusters/help/databases/samples) met een e-mail account dat lid is van Azure Active Directory.

- Op uw eigen cluster die de StormEvents-voorbeeld gegevens bevat. Voor meer informatie raadpleegt [u Quick Start: Een Azure Data Explorer-cluster en-](create-cluster-database-portal.md) data base maken en [voorbeeld gegevens opnemen in azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="overview-of-the-query-language"></a>Overzicht van de query taal

Een query taal in azure Data Explorer is een alleen-lezen aanvraag voor het verwerken van gegevens en het retour neren van resultaten. De aanvraag wordt in tekst zonder opmaak vermeld, waarbij gebruik wordt gemaakt van een model voor gegevens stroom dat is ontworpen om de syntaxis eenvoudig te lezen, te ontwerpen en te automatiseren. De query maakt gebruik van schema-entiteiten die zijn georganiseerd in een hiërarchie die vergelijkbaar is met SQL: data bases, tabellen en kolommen.

De query bestaat uit een reeks query-instructies, gescheiden door een punt komma (`;`), waarbij ten minste één instructie een tabellaire expressie instructie is. Dit is een instructie waarmee gegevens worden geproduceerd in een tabel-net als kolommen en rijen. De instructies in de tabellaire expressie van de query genereren de resultaten van de query.

De syntaxis van de instructie tabellaire expressie heeft tabellaire gegevens stroom van de ene tabellaire query operator naar een andere, beginnend met de gegevens bron (bijvoorbeeld een tabel in een Data Base of een operator die gegevens produceert) en vervolgens door een set gegevens transformatie loopt Opera tors die bij elkaar zijn verbonden door het gebruik van het`|`pipe-scheidings teken ().

De volgende query heeft bijvoorbeeld één instructie, een tabellaire expressie-instructie. De instructie begint met een verwijzing naar een tabel met `StormEvents` de naam (de data base die als host voor deze tabel wordt gehost, en een deel van de verbindings gegevens). De gegevens (rijen) voor die tabel worden vervolgens gefilterd op de waarde `StartTime` van de kolom en vervolgens gefilterd op de `State` waarde van de kolom. De query retourneert vervolgens het aantal rijen met ' overgebleven '.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVWws1VISSxJLQGyNYwMDMx1DQ11DQw1FRLzUpBU2aArMgIpQjGvJFXB1lZByc3HP8jTxVFJQQEkm5xfmlcCAHoR9euCAAAA) **\]**

```Kusto
StormEvents
| where StartTime >= datetime(2007-11-01) and StartTime < datetime(2007-12-01)
| where State == "FLORIDA"  
| count
```

In dit geval is het resultaat:

|Count|
|-----|
|   23|
| |

Zie de naslag informatie voor de [query taal](https://aka.ms/kustolangref)voor meer gegevens.

## <a name="most-common-operators"></a>Meest voorkomende Opera tors

De Opera tors die in deze sectie worden behandeld, zijn de bouw stenen voor het leren van query's in azure Data Explorer. De meeste query's die u schrijft, bevatten een aantal van deze opera tors.

Query's uitvoeren op het Help-cluster: Selecteer **klikken om de query** boven elke query uit te voeren.

Query's uitvoeren op uw eigen cluster:

1. Kopieer elke query naar de Web-query toepassing en selecteer vervolgens de query of plaats de cursor in de query.

1. Selecteer aan de bovenkant van de toepassing **uitvoeren**.

### <a name="count"></a>count

[**aantal**](https://docs.microsoft.com/azure/kusto/query/countoperator): Retourneert het aantal rijen in de tabel.

Met de volgende query wordt het aantal rijen in de tabel StormEvents.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRSM4vzSsBALU2eHsTAAAA) **\]**

```Kusto
StormEvents | count
```

### <a name="take"></a>Houd

[**nemen**](https://docs.microsoft.com/azure/kusto/query/takeoperator): Retourneert tot het opgegeven aantal rijen gegevens.

De volgende query retourneert vijf rijen uit de tabel StormEvents. De *limiet* voor tref woorden is een alias voor het *nemen van.*

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRKEnMTlUwBQDEz2b8FAAAAA%3d%3d) **\]**

```Kusto
StormEvents | take 5
```

> [!TIP]
> Er is geen garantie dat records worden geretourneerd, tenzij de bron gegevens worden gesorteerd.

### <a name="project"></a>project

[**project**](https://docs.microsoft.com/azure/kusto/query/projectoperator): Hiermee selecteert u een subset kolommen.

De volgende query retourneert een specifieke set kolommen.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUShJzE5VMAWxCorys1KTSxSCSxKLSkIyc1N1FFzzUiAMoFgJiA%2fSFlJZAGS6JOYmpqcGFOUXpBaVVAKlCjKL81NS%2fRKLihJLMstSAY%2buIINnAAAA) **\]**

```Kusto
StormEvents
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="where"></a>waarbij

[**waar**](https://docs.microsoft.com/azure/kusto/query/whereoperator): Hiermee wordt een tabel gefilterd op de subset rijen die aan een predikaat voldoen.

Met de volgende query worden de gegevens `EventType` door `State`en gefilterd.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAEWMPQvCMBCGd8H%2fcFuWro4dBOvHkgoJOB%2fm0KjJhetRKfjjNe3g9n49r1OW1I2UdVivPvC%2bkxDM3k%2bFoG3B7F%2fMwQDmAE5Rl%2fCydceTPfjemsopPgk2VRXhB121TkV9TNRAl8MiZrz53zeww4Q3OgsXEp1%2bVYkDB7IoghpH%2bgI9OH8WnwAAAA%3d%3d) **\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="sort"></a>acties

[**sorteren**](https://docs.microsoft.com/azure/kusto/query/sortoperator): Sorteer de rijen van de invoer tabel in volg orde van een of meer kolommen.

Met de volgende query worden de gegevens in aflopende Volg `DamageProperty`orde gesorteerd op.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2NPQvCMBCGd8H%2fcFuXrI4dBOvHEoUGnM%2fm0KjphctRKfjjNe0guL0fvM%2fbKktsBuo1LxdveN1ICCbvxkRQ11Btn8y%2bAuw9tIo6h%2bd1uz%2fYnTvaquwyi8JlhA1GvNJJOJHoCJ5yV2rFB8GqqCR8p04LSdSFSAaa3s9iopvfu%2fnDfasUMnuyKIIaBvoAtvGMsb4AAAA%3d) **\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| sort by DamageProperty desc
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

> [!NOTE]
> De volg orde van de bewerkingen is belang rijk. Probeer te `take 5` plaatsen `sort by`voordat. Krijgt u verschillende resultaten?

### <a name="top"></a>Boven

[**boven**](https://docs.microsoft.com/azure/kusto/query/topoperator): Retourneert de eerste *N* records, gesorteerd op de opgegeven kolommen.

Met de volgende query worden dezelfde resultaten geretourneerd als hierboven met één operator min.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2NOwvCMBSFd8H%2fcLcsWR07CNbHkgoJOMfmohGTG24vlYA%2fXtsOgtt5cL5jhTi1I2YZ1qs3vO7ICLN3tSA0Daj9kygo8DmAFS9LeNna48kcXGfUtBMqsIFrhZ1P%2foZnpoIsFQIO%2fdQXpgf2MgFYXEyooc1hETNU%2f071H%2bRblThQQOOZvcQRP1rSng21AAAA) **\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| top 5 by DamageProperty desc
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="extend"></a>uitbreidbaar

[**uitbreiden**](https://docs.microsoft.com/azure/kusto/query/extendoperator): Berekent afgeleide kolommen.

Met de volgende query maakt u een nieuwe kolom door een waarde in elke rij te berekenen.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2OvQ7CMAyEdyTewVuWMDJ2QGr5WQJSKzGHxoIiEkeuKVTi4WmooBKbfXeffaUQ%2b6LDIO189oLHBRnhs1d9RMgyUOsbkVNgg4NSrIzicVVud2ZT7Y1KnFCEJZx6yK23ZzwwRWTpwWFbJx%2bfggOf39lKQwEyKIKrGo%2bwSEdZ0pyCkemKtUyi%2fib1j9ZjDz311H9%2fBys2LTk0lhPT4RvwA3pn6AAAAA%3d%3d) **\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| top 5 by DamageProperty desc
| extend Duration = EndTime - StartTime
| project StartTime, EndTime, Duration, State, EventType, DamageProperty, EpisodeNarrative
```

Expressies kunnen alle gebruikelijke Opera tors (+,-, *,/,%) bevatten en er zijn diverse nuttige functies die u kunt aanroepen.

### <a name="summarize"></a>samenvatten

[](https://docs.microsoft.com/azure/kusto/query/summarizeoperator)samenvatten: Voegt groepen rijen samen.

De volgende query retourneert het aantal gebeurtenissen door `State`.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIBYnFJ%2beX5pUo2CqAaQ1NhaRKheCSxJJUAB%2fedDI3AAAA) **\]**

```Kusto
StormEvents
| summarize event_count = count() by State
```

De **samenvatte** operator groepen groeperen rijen met dezelfde waarden in de **by** -component en gebruikt vervolgens de aggregatie functie (zoals **aantal**) om elke groep in één rij te combi neren. In dit geval is er een rij voor elke status en een kolom voor het aantal rijen in die staat.

Er is een groot aantal functies voor aggregatie en u kunt verschillende berekende kolommen maken met behulp van een operator samenvatten. U kunt bijvoorbeeld het aantal stormen in elke staat en het unieke aantal stormen per staat weer geven en vervolgens **Top** gebruiken om de recentste statussen te verkrijgen.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlUIBkk455fmlSjYKiSDaA1NHYWQyoJU%2fzSwXDFQPAUiAdYPktJUSKoE6kwsSQUZVpJfoGAKEYGblZJanAwAgbFb73QAAAA%3d) **\]**

```Kusto
StormEvents
| summarize StormCount = count(), TypeOfStorms = dcount(EventType) by State
| top 5 by StormCount desc
```

Het resultaat van een **samenvattings** bewerking is:

- Elke kolom **met** een naam in

- Een kolom voor elke berekende expressie

- Een rij voor elke combi natie van waarden

### <a name="render"></a>waardoor

[**render**](https://docs.microsoft.com/azure/kusto/query/renderoperator): Hiermee worden resultaten weer gegeven als een grafische uitvoer.

Met de volgende query wordt een kolom diagram weer gegeven.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWMsQ7CQAxDdyT%2bIWMrdSgbSxmQ2Nj6Aei4Ru0hkqA0VwTi49uUBRZL9rPdmiidJmQbt5sPjJkoaHojoGeXKJmtWbUoK6DUQQNh6osj9onPwUq4vqC1YLjORc2Dpef2OaD%2bPcEBdvu6dvZQuWG077b6LTlV5A4VotwzcRyC2gxU6ktSqQAAAA%3d%3d) **\]**

```Kusto
StormEvents
| summarize event_count=count(), mid = avg(BeginLat) by State
| sort by mid
| where event_count > 1800
| project State, event_count
| render columnchart
```

Met de volgende query wordt een eenvoudige tijd grafiek weer gegeven.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIBYnFJ%2beX5pXYgkkNTYWkSoWkzDyN4JLEopKQzNxUHQXDFE2QtqLUvJTUIoUSoFhyBlASAAyXWQJWAAAA) **\]**

```Kusto
StormEvents
| summarize event_count=count() by bin(StartTime, 1d)
| render timechart
```

Met de volgende query worden gebeurtenissen geteld op basis van de tijd modulo één dag, binning in uren en wordt een tijd diagram weer gegeven.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEADWNQQqDMBRE90LvMBtBwY0HcNkT2L2k8UuEJh9%2bfqSWHt4k4GZghpk3s7L450FB46P5g75KYYXjJJiwfZilm9WIvnZPaDGuGDC6vnRj8t7I%2fiNQ2S%2bWU9CpatfjfVZKLbLo7WGiLZnkGxJoxlqX%2bRf81ZbyiAAAAA%3d%3d) **\]**

```Kusto
StormEvents
| extend hour = floor(StartTime % 1d , 1h)
| summarize event_count=count() by hour
| sort by hour asc
| render timechart
```

Met de volgende query vergelijkt u meerdere dagelijkse reeksen in een tijd diagram.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEACWPSwvCMBCE74L%2fYSgIFXrpD%2bihaKzxkUBTXyeputKCbSCmvvDHm9TL7gwzsN8qq03DHtTa%2b3DwBb0stRdUujMJrjetTQhlS2OLuiGMEF8QIa7GvvusyJBPLaFuEQbZZjWDnGHN9nwigyhYp1wwt7c8z7jgqZM7riZSKC6cFjIv5pimS1n4SLAdFixX7OCMzFkmRdAfundNU5r6QyAPejzrrrVJP8MxTu8eN%2fqT%2bL5xL5CBdcjnyrH%2fALPTSKnkAAAA) **\]**

```Kusto
StormEvents
| extend hour= floor( StartTime % 1d , 1h)
| where State in ("GULF OF MEXICO","MAINE","VIRGINIA","WISCONSIN","NORTH DAKOTA","NEW JERSEY","OREGON")
| summarize event_count=count() by hour, State
| render timechart
```

> [!NOTE]
> De **render** -operator is een functie aan client zijde in plaats van een deel van de engine. Het is geïntegreerd in de taal voor gebruiks gemak. De webtoepassing ondersteunt de volgende opties: Barchart, columnchart, piechart, timechart en linechart. 

## <a name="scalar-operators"></a>Scalaire Opera tors

Deze sectie bevat enkele van de belangrijkste scalaire Opera tors.

### <a name="bin"></a>bin()

[**bin()** ](https://docs.microsoft.com/azure/kusto/query/binfunction): Rondt waarden af naar een geheel getal van een bepaalde bin-grootte.

Met de volgende query wordt het aantal berekend met een Bucket grootte van één dag.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVWwU0hJLEktATI1jAwMzHUNjHQNTTQVEvNSkBTZYCoyMtQEGVdcmpubWJRZlaqQCrIiPjm%2fNK9EwVYBTGtoKiRVKiRl5mnAjdJRMEzRBABIhjnmkwAAAA%3d%3d) **\]**

```Kusto
StormEvents
| where StartTime > datetime(2007-02-14) and StartTime < datetime(2007-02-21)
| summarize event_count = count() by bin(StartTime, 1d)
```

### <a name="case"></a>Case ()

[**case()** ](https://docs.microsoft.com/azure/kusto/query/casefunction): Evalueert een lijst met predikaten en retourneert de eerste resultaat expressie waarvan het predicaat is voldaan, of de eind **else** -expressie. U kunt deze operator gebruiken om gegevens te categoriseren of te groeperen:

Met de volgende query wordt een nieuwe `deaths_bucket` kolom geretourneerd en wordt het sterf getal gegroepeerd op het aantal.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAGWOwQrCQAxE74X%2bQ9hTCwX14FFBaK9e%2bgGS7gZdbFrYZEXFj7dbqgfNbfJmhml1DNzcaFDJsxdIZMbgnwSOUC8Cu%2fQq6lnUPpDVEroHtIpKKUB3pcEt7lMX7ZV0ClkUgiLPYLqlaQ%2fbdQWmx3AmU%2f2gTUJMzkf%2bYwkJY99%2fiDmuDqac545Bv3MAxb4Bic1Oy88AAAA%3d) **\]**

```Kusto
StormEvents
| summarize deaths = sum(DeathsDirect) by State
| extend deaths_bucket = case (
    deaths > 50, "large",
    deaths > 10, "medium",
    deaths > 0, "small",
    "N/A")
| sort by State asc
```

### <a name="extract"></a>extract()

[**extract()** ](https://docs.microsoft.com/azure/kusto/query/extractfunction): Hiermee wordt een overeenkomst opgehaald voor een reguliere expressie uit een teken reeks.

Met de volgende query worden specifieke kenmerk waarden opgehaald uit een tracering.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAE2OwQrCMBBE74X%2bw9BTojHYagSVHJRevXkrHqJdpVBbSVew4McbFYungeXtvKmJsetzxw4WZQh2x5og9t6daIWOfdVcJIpkY1OFrc0U8rt3XLWNTbOZnhultU4UfoD5A4zRmVkovInDOo6%2bojh6gh5MTTmQwR0uQckiGb5FMZ0s9WEsQ3uo%2fixSccT9jdqz8ORqKTECV1cSaSdfq2k6L8oAAAA%3d) **\]**

```Kusto
let MyData = datatable (Trace: string) ["A=1, B=2, Duration=123.45,...", "A=1, B=5, Duration=55.256, ..."];
MyData
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s)
```

Deze query gebruikt een instructie voor **toestaan** , die een naam (in dit geval `MyData`) aan een expressie koppelt. Voor de rest van het bereik, waarin de instructie **toestaan** wordt weer gegeven (globaal bereik of in een functie hoofdtekst bereik), kan de naam worden gebruikt om te verwijzen naar de gekoppelde waarde.

### <a name="parse_json"></a>parse_json()

[**parse_json()** ](https://docs.microsoft.com/azure/kusto/query/parsejsonfunction): Interpreteert een teken reeks als een JSON-waarde en retourneert de waarde als dynamisch. Het is belang rijk dat u de functie **extractjson ()** gebruikt wanneer u meer dan één element van een samengesteld JSON-object wilt extra heren.

Met de volgende query worden de JSON-elementen uit een matrix geëxtraheerd.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAHWPQQuCQBCF74L%2fYdmLBSJ6EGKjU17r1E0kJh1C2XZlHc0w%2f3ur1s1O896bB%2fONRGKnVwIE7MAKOwhuEtnmYiBHwRoypbpvXSf1Bl60BqjUiot04B3IFrmIol0Q%2bpPLdauIi3iyj9KWojCcNfRWx7NuqEiw48KaMRu9bO86y3HXeTPsCVXBzvg8amlpajANXqtGq4VmO5VqoyvM6dsKfkhpmAUzkf9nM9OtLi3reg79ar788AEVX8GkOAEAAA%3d%3d) **\]**

```Kusto
let MyData = datatable (Trace: string)
['{"duration":[{"value":118.0,"valcount":5.0,"min":100.0,"max":150.0,"stdDev":0.0}]}'];
MyData
| extend NewCol = parse_json(Trace)
| project NewCol.duration[0].value, NewCol.duration[0].valcount, NewCol.duration[0].min, NewCol.duration[0].max, NewCol.duration[0].stdDev
```

Met de volgende query worden de JSON-elementen geëxtraheerd.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAE2OwQqCQBCG74LvsOzFBBE9CLHRKa916hYRkw6RbLuyO5pRvXvrGtZpvn9m4P8kEts%2bSiBga1a7QXCWyBZ7AxUKZslc1SVmh%2bjJe5AdcpHnyzRLxlTpThEXxRhvV%2bVOWeYZBseFZ0t1iT0XLryj4yoMprIweDEcCFXNdnjfaOnaWzAWT43VamqPx6fW6AYr%2bn6l3iH5S95hXjiLH8Mw82TxAQvJEB%2fsAAAA) **\]**

```Kusto
let MyData = datatable (Trace: string) ['{"value":118.0,"valcount":5.0,"min":100.0,"max":150.0,"stdDev":0.0}'];
MyData
| extend NewCol = parse_json(Trace)
| project NewCol.value, NewCol.valcount, NewCol.min, NewCol.max, NewCol.stdDev
```

Met de volgende query worden de JSON-elementen geëxtraheerd met een dynamisch gegevens type.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAD2NMQvCMBBG90D%2bw5GphVLSoSARt65ubuJwJjdU0lZiWlrU%2f25MotO9x8H7LHk4bh16hAOYcDxeLUFxcqhJgdlGHHpdcnbOWDzFgnYmoZpmV8tK6GkePTmh2q8N%2fRg%2bUkbGNXAb%2beFNR4tQQd7lZc9ZGuXsBXc33Uh7iJN1jFdZcvunIf5HXCvOEqf2BwXmDCnKAAAA) **\]**

```Kusto
let MyData = datatable (Trace: dynamic)
[dynamic({"value":118.0,"counter":5.0,"min":100.0,"max":150.0,"stdDev":0.0})];
MyData
| project Trace.value, Trace.counter, Trace.min, Trace.max, Trace.stdDev
```

### <a name="ago"></a>geleden ()

[**geleden ()** ](https://docs.microsoft.com/azure/kusto/query/agofunction): Trekt de opgegeven time span af van de huidige UTC-klok tijd.

Met de volgende query worden gegevens voor de afgelopen 12 uur geretourneerd.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA1WOQQ6CQAxF9yTc4S8hQcmQuNSNR4ALTKQyJDAlnSIuPLwzJGrctM3v+7+t684R7qMEhW6MafQUMJAnsUoIdl4mQm/VVrC+h0Z6shFOINZAIc/qOql24KIEL8nIAuWYohC6sfQB9yjtPtPA8SrhmGeLjF7RjTO1Gu+cIdYPVHjeisOpLyukKTbjYml5piuvXknwIU1lGlPm2Qvzg55L+u+b9udIyOZI6LfHZf/YNK58Ay2HrbAEAQAA) **\]**

```Kusto
//The first two lines generate sample data, and the last line uses
//the ago() operator to get records for last 12 hours.
print TimeStamp= range(now(-5d), now(), 1h), SomeCounter = range(1,121)
| mv-expand TimeStamp, SomeCounter
| where TimeStamp > ago(12h)
```

### <a name="startofweek"></a>opsomming ()

[**opsomming ()** ](https://docs.microsoft.com/azure/kusto/query/startofweekfunction): Retourneert het begin van de week met de datum, verschoven met een verschuiving, indien opgegeven

Met de volgende query wordt het begin van de week met verschillende verschuivingen geretourneerd.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEACtKzEtPVchPSytOLVFIK8rPVdA1VCjJVzBUKC5JLVAw5OWqUSgoys9KTS5RKE9NzQ4uSSwqUbAFygLp%2fDSQkEZefrmGpg7UEE0dCA0AdE3lv1kAAAA%3d) **\]**

```Kusto
range offset from -1 to 1 step 1
| project weekStart = startofweek(now(), offset),offset
```

Deze query maakt gebruik van de operator **Range** , waarmee een tabel met één kolom met waarden wordt gegenereerd. Zie ook: [**startofday ()** ](https://docs.microsoft.com/azure/kusto/query/startofdayfunction), [**opsomming ()** ](https://docs.microsoft.com/azure/kusto/query/startofweekfunction), [**startofyear ()** ](https://docs.microsoft.com/azure/kusto/query/startofyearfunction)), [**startofmonth ()** ](https://docs.microsoft.com/azure/kusto/query/startofmonthfunction), [**endofday ()** ](https://docs.microsoft.com/azure/kusto/query/endofdayfunction), [**endofweek ()** ](https://docs.microsoft.com/azure/kusto/query/endofweekfunction), [**endofmonth ()** ](https://docs.microsoft.com/azure/kusto/query/endofmonthfunction)en [**endofyear ()** ](https://docs.microsoft.com/azure/kusto/query/endofyearfunction).

### <a name="between"></a>tussen ()

[**tussen ()** ](https://docs.microsoft.com/azure/kusto/query/betweenoperator): Komt overeen met de invoer binnen het inclusieve bereik.

Met de volgende query worden de gegevens op basis van een bepaald datum bereik gefilterd.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrApGRuaaCnp4ChrixgaYmyKTk%2fNK8EgBluyagXgAAAA%3d%3d) **\]**

```Kusto
StormEvents
| where StartTime between (datetime(2007-07-27) .. datetime(2007-07-30))
| count
```

Met de volgende query worden de gegevens op basis van een bepaald datum bereik gefilterd, met een`3d`geringe afwijking van drie dagen () vanaf de begin datum.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrApGRuaaCnp6CcYomSF9yfmleCQCGAqjRTAAAAA%3d%3d) **\]**

```Kusto
StormEvents
| where StartTime between (datetime(2007-07-27) .. 3d)
| count
```

## <a name="tabular-operators"></a>Tabellaire Opera tors

Kusto heeft veel tabellaire Opera Tors, waarvan sommige zijn opgenomen in andere secties van dit artikel. Hier richten we ons ophet parseren. 

### <a name="parse"></a>parseren

[**parseren**](https://docs.microsoft.com/azure/kusto/query/parseoperator): Evalueert een teken reeks expressie en parseert de waarde ervan in een of meer berekende kolommen. Er zijn drie manieren om te parseren: eenvoudig (de standaard instelling), regex en ongeforceerd.

De volgende query parseert een tracering en extraheert de relevante waarden met behulp van een standaard waarde voor het parseren. De expressie (aangeduid als StringConstant) is een reguliere teken reeks waarde en de overeenkomst is strikt: uitgebreide kolommen moeten overeenkomen met de vereiste typen.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2UTU%2fDMAyG75X6H6xcxlCkpRlsUNQjN6gQ2wnEoevMFsiaKk2HJvHjabqvlI91l11QLrH12vETW5Zo4H411kmKEME0MdWZSISz2yVmpvaHhdEim3V979n3OrU%2fhFgZ8boaSZHiI0pMiipEY6FKnWKcLDB6EDlKkeEoneO0lKgpGGUSWYcUER9SKOw1LhcT1BHvU5AqfR%2bLKpbxXjDscRYMgF2FFyxkwRMFvX7ngCLXuBSqLO5%2bT9S%2ftrJuh54OI7g8iMFaMdhxGOy0GJz9i25w%2fjdG0IoRHNWNNe1ph2pwEKNlqI7HsEPley83vrfZCL73CXmiq%2fr32wA%2bhJnDOZAGEQHXBNIEIq4VSpXNbAIXkbjAO8UOmuz4bWoXlrhWWO0vqyA2%2bAcw2f7B1rORd60calat3jA1TRbq1A6NxsC%2bLdCoCuj3p74AKTs4pmcFAAA%3d) **\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse EventTrace with * "resourceName=" resourceName ", totalSlices=" totalSlices:long * "sliceNumber=" sliceNumber:long * "lockTime=" lockTime ", releaseTime=" releaseTime:date "," * "previousLockTime=" previouLockTime:date ")" *  
| project resourceName ,totalSlices , sliceNumber , lockTime , releaseTime , previouLockTime
```

De volgende query parseert een tracering en extraheert de relevante waarden met `kind = regex`behulp van. De StringConstant kan een reguliere expressie zijn.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2UQU%2fCQBCF7036HyZ7gWKRbVHQmgY9eNPGCCcoh9KOsLK0ZLtFMf54l6LQBgUuXEyTTbP7pt3vvclwlPC47IkgRHAhCqR6Rhyher%2fAWOb7TioFi8eGrg10rZLvO%2bAlkr0su5yF%2bIwcg1SVCEyTTIToBTN0n9gcOYuxG04wyjgKE2QiA56XpK7dNiFdvXrZbITCtZsm8CSc9piqpXbDajdsarWAXjkX1KFW3wSx%2fs8exVzggiVZ%2bvD7h5rXK5lRMU%2bHYV3uxaAHMehxGPS0GDb9F2nY9t8Y1kEM66g01rSnbarWXowDTXU8xqqpdG14o2vfE0HXPmEeCHX%2fKYsjNR8EjvEdtqMB3picAKme1zrGIKh%2f3NX7w5pLoEgLt6SM56c1PzpTq6oqYpIitMOTeAxAlKb6c3Wjs3GBbAzJJUV8UjQjP91BJztuOGryKbHvGwQgxxbJK4ayTFKKBbahQCkA2DX7C29veJJmBQAA) **\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse kind = regex EventTrace with "(.*?)[a-zA-Z]*=" resourceName @", totalSlices=\s*\d+\s*.*?sliceNumber=" sliceNumber:long  ".*?(previous)?lockTime=" lockTime ".*?releaseTime=" releaseTime ".*?previousLockTime=" previousLockTime:date "\\)"  
| project resourceName , sliceNumber , lockTime , releaseTime , previousLockTime
```

De volgende query parseert een tracering en extraheert de relevante waarden met `kind = relaxed`behulp van. De StringConstant is een reguliere teken reeks waarde en de overeenkomst is ongeforceerd: uitgebreide kolommen kunnen gedeeltelijk overeenkomen met de vereiste typen.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2US0%2fCQBDH7036HSZ7wZpN2BYFrenRGzZG4KLxUNoRVpYu2W5REj%2b83fKw9QE1kYvppTOZx%2f%2b3MxmBGm5WQxXFCAEkkS6%2bsUA4uV5iqku%2fn2nF04ljWw%2b21Sr9PoRS86fVQPAY71BglBUpCjOZqxjDaI7BLV%2bg4CkO4ikmuUBFQUsdiTIlC7wehcz8hvl8jCrwOhSEjGdDXuQyr%2b322h5zu8Au%2fDPmM%2feeglr32ROxULjkMs%2f63xfqXJowp0WPh%2bGe78VgBzFYMwx2XAyP%2fYtpeN7PGO5BDLfRNNa0x12q7l6MA0vVHMMslW09XtnW5iLY1hssIlXon%2fE0CYom0SsmQP6IMxz1%2b7%2b7AnXQdX6TNXMIvHA9hVMgNYEEqiaQuj5StXwh04kpUNVLqup3ETsCsoMxpavSSdXyi7NrIohJ%2foJDtoRbzybcMeFQjkjJZ4x1nYVWtEPtleHjjaGmCujnVu%2fWU75tHgYAAA%3d%3d) **\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse kind=relaxed "Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=NULL, sliceNumber=23, lockTime=02/17/2016 08:40:01, releaseTime=NULL, previousLockTime=02/17/2016 08:39:01)" with * "resourceName=" resourceName ", totalSlices=" totalSlices:long * "sliceNumber=" sliceNumber:long * "lockTime=" lockTime ", releaseTime=" releaseTime:date "," * "previousLockTime=" previousLockTime:date ")" *  
| project resourceName ,totalSlices , sliceNumber , lockTime , releaseTime , previousLockTime
```

## <a name="time-series-analysis"></a>Tijdreeksanalyse

### <a name="make-series"></a>reeks maken

[**maken-serie**](https://docs.microsoft.com/azure/kusto/query/make-seriesoperator): voegt groepen rijen als samenvatten [](https://docs.microsoft.com/azure/kusto/query/summarizeoperator)samen, maar genereert een (tijd)-reeks vector per combi natie van waarden.

Met de volgende query wordt een set Time Series geretourneerd voor het aantal Storm-gebeurtenissen per dag. De query heeft betrekking op een periode van drie maanden voor elke status, waarbij de ontbrekende opslag locaties worden gevuld met de constante 0:

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUchNzE7VLU4tykwtVsizTc4vzSvR0FRISU1LLM0psTVQyM9TCC5JLCoJycxNVcjMUyhKzEtP1UhJLEktAYpoGBkYmOsaGAKRpo4CmqixrjFI1DBFUyGpEmRKSSoAazsM0n0AAAA%3d) **\]**

```Kusto
StormEvents
| make-series n=count() default=0 on StartTime in range(datetime(2007-01-01), datetime(2007-03-31), 1d) by State
```

Als u een reeks (tijd) hebt gemaakt, kunt u reeks functies Toep assen om afwijkende vormen, seizoensgebonden patronen en nog veel meer te detecteren.

Met de volgende query worden de eerste drie statussen opgehaald die de meeste gebeurtenissen in een specifieke dag hadden:

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2OsQoCMRBEe8F%2f2DIBAzmvsLrSLzj7EC%2brBs3mSPbkBD%2feLDYibPVmZmdGziUdn0hct5s3JH9HU7FErEDDlBdipSHgxS8PHixkgpF94VNMCJGgeLqiCp6RG1F7aw%2fGdu30Dv5ob3qhXdBwfskXRmnElZECfDtdbbgq0qJwnqEX76%2fmyCW%2ftkV1Ek9pWSwgNdOt7foAJIuybs8AAAA%3d) **\]**

```Kusto
StormEvents
| make-series n=count() default=0 on StartTime in range(datetime(2007-01-01), datetime(2007-03-31), 1d) by State
| extend series_stats(n)
| top 3 by series_stats_n_max desc
| render timechart
```

Bekijk de volledige lijst met [serie functies](https://docs.microsoft.com/azure/kusto/query/scalarfunctions#series-processing-functions)voor meer informatie.

## <a name="advanced-aggregations"></a>Geavanceerde aggregaties

Eerder in dit artikel hebben we elementaire aggregaties, zoals **Count** en samenvatten, behandeld. In deze sectie vindt u meer geavanceerde opties.

### <a name="top-nested"></a>boven-genest

[**top-nested**](https://docs.microsoft.com/azure/kusto/query/topnestedoperator): Produceert hiërarchische belangrijkste resultaten, waarbij elk niveau een drill-down is op basis van de waarden van het vorige niveau.

Deze operator is handig voor visualisatie scenario's in het dash board of wanneer het nodig is om een vraag te beantwoorden als de volgende: "De hoogste N waarden van K1 zoeken (met behulp van enige aggregatie); Zoek voor elk daarvan wat de hoogste waarden van K2 zijn (met behulp van een andere aggregatie); ..."

De volgende query retourneert een hiërarchische tabel `State` met op het hoogste niveau, gevolgd `Sources`door.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjJL9DNSy0uSU1RMFLIT1MILkksSVVIqlQoLs3VcEpNz8zzSSzR1OHlQlJoDFaYX1qUTEilIUila16KT35yYklmfh6GcgDrXwk5jgAAAA%3d%3d) **\]**

```Kusto
StormEvents
| top-nested 2 of State by sum(BeginLat),
top-nested 3 of Source by sum(BeginLat),
top-nested 1 of EndLocation by sum(BeginLat)
```

### <a name="pivot-plugin"></a>Pivot ()-invoeg toepassing

[**Pivot ()-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/pivotplugin): Hiermee roteert u een tabel door de unieke waarden van de ene kolom in de invoer tabel in meerdere kolommen in de uitvoer tabel in te scha kelen. De operator voert aggregaties uit waar deze vereist zijn voor alle resterende kolom waarden in de uiteindelijke uitvoer.

Met de volgende query past u een filter toe en draait u de rijen in kolommen.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSgoys9KTS5RCC5JLEnVUQBLhFQWpILkyjNSi1IhMgrFJYlFJcXlmSUZCkqOPkoIabgOhYzEYgWl8My8FLBsalliTilIZ0FmWX6JBtgUTQDlv21NfQAAAA%3d%3d) **\]**

```Kusto
StormEvents
| project State, EventType
| where State startswith "AL"
| where EventType has "Wind"
| evaluate pivot(State)
```

### <a name="dcount"></a>DCount ()

[**DCount ()** ](https://docs.microsoft.com/azure/kusto/query/dcount-aggfunction): Retourneert een schatting van het aantal afzonderlijke waarden van een expressie in de groep. Gebruik [**Count ()** ](https://docs.microsoft.com/azure/kusto/query/countoperator) om alle waarden te tellen.

De volgende query telt DISTINCT `Source` by `State`.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlUIzi8tSk4tVrBVSEnOL80r0YAIaCokVSoElySWpAIAFKgSBDoAAAA%3d) **\]**

```Kusto
StormEvents
| summarize Sources = dcount(Source) by State
```

### <a name="dcountif"></a>dcountif()

[**dcountif()** ](https://docs.microsoft.com/azure/kusto/query/dcountif-aggfunction): Retourneert een schatting van het aantal afzonderlijke waarden van de expressie voor rijen waarvoor het predikaat resulteert in waar.

Met de volgende query worden de afzonderlijke waarden `Source` van `DamageProperty < 5000`where geteld.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRKEnMTlUwNDDg5apRKC7NzU0syqxKVQjOLy1KTi1WsFVISc4vzSvJTNOACOkouCTmJqanBhTlF6QWlVQq2CiYGhgYaCokVSoElySWpAIAuk%2fTX14AAAA%3d) **\]**

```Kusto
StormEvents 
| take 100
| summarize Sources = dcountif(Source, DamageProperty < 5000) by State
```

### <a name="dcount_hll"></a>dcount_hll()

[**dcount_hll()** ](https://docs.microsoft.com/azure/kusto/query/dcount-hllfunction): Berekent de **DCount** van HyperLogLog-resultaten (gegenereerd door [**HLL**](https://docs.microsoft.com/azure/kusto/query/hll-aggfunction) of [**hll_merge**](https://docs.microsoft.com/azure/kusto/query/hll-merge-aggfunction).

De volgende query maakt gebruik van het HLL-algoritme voor het genereren van het aantal.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlXIyMkJSi1WsAUxNFwScxPTUwOK8gtSi0oqNRWSKhWSMvM0gksSi0pCMnNTdQwNcjUx9PumFqWnpkCMiM8FcTQgpoKVFhTlZ6UmlyikJOeX5pXEg6yB69EEAKm9wyCXAAAA) **\]**

```Kusto
StormEvents
| summarize hllRes = hll(DamageProperty) by bin(StartTime,10m)
| summarize hllMerged = hll_merge(hllRes)
| project dcount_hll(hllMerged)
```

### <a name="arg_max"></a>arg_max()

[**arg_max()** ](https://docs.microsoft.com/azure/kusto/query/arg-max-aggfunction): Zoekt naar een rij in de groep die een expressie maximaliseert en retourneert de waarde van een andere expressie (of * om de gehele rij te retour neren).

Met de volgende query wordt de tijd van het laatste floode rapport in elke status geretourneerd.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQDzQyoLUhVsbRWU3HLy81OUQLLFpbm5iUWZVakKiUXp8bmJFRrBJYlFJSGZuak6ClqaCkmVCkCBklSQ2oKi%2fKzU5BKIgI4CkkLXvBQoA2YNAHO1S0OFAAAA) **\]**

```Kusto
StormEvents
| where EventType == "Flood"
| summarize arg_max(StartTime, *) by State
| project State, StartTime, EndTime, EventType
```

### <a name="makeset"></a>makeset()

[**makeset()** ](https://docs.microsoft.com/azure/kusto/query/makeset-aggfunction): Retourneert een dynamische (JSON)-matrix van de reeks unieke waarden die een expressie in de groep neemt.

De volgende query retourneert alle tijden wanneer een overstroming door elke status is gerapporteerd en maakt een matrix van de set afzonderlijke waarden.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWLQQ6CQBAE7yb8ocNJE76wR3mA8IEFOxF1mM3siIHweAVPHqsq1bianCeOnovDiveNRuzczokIAWX9VL2WW80vkWjDQuzuwqTmGQESH8z0Y%2bPRvB2EJ3QzvuTcvmR6Z%2b8%2fUf3NH6ZkMFeAAAAA) **\]**

```Kusto
StormEvents
| where EventType == "Flood"
| summarize FloodReports = makeset(StartTime) by State
| project State, FloodReports
```

### <a name="mv-expand"></a>mv-expand

[**mv-expand**](https://docs.microsoft.com/azure/kusto/query/mvexpandoperator): Breidt verzameling (en) van meerdere waarden uit van een dynamische kolom, zodat elke waarde in de verzameling een afzonderlijke rij krijgt. Alle andere kolommen in een uitgevouwen rij worden gedupliceerd. Het is het tegenovergestelde van makelist.

Met de volgende query worden voorbeeld gegevens gegenereerd door een set te maken en deze vervolgens te gebruiken om de **MV-Expand-** mogelijkheden te demonstreren.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWOQQ6CQAxF9yTcoWGliTcws1MPIFygyk9EKTPpVBTj4Z2BjSz%2f738v7WF06r1vD2xcp%2bCoNq9yHDFYLIsvvW5Q0JybKYCco2omqnyNTxHW7oPFckbwajFZhB%2bIsE1trNZ0gi1dpuRmQ%2baC%2bjuuthS7Fbwvi%2f%2bP8lpGvAMP7Wr3A6BceSu7AAAA) **\]**

```Kusto
let FloodDataSet = StormEvents
| where EventType == "Flood"
| summarize FloodReports = makeset(StartTime) by State
| project State, FloodReports;
FloodDataSet
| mv-expand FloodReports
```

### <a name="percentiles"></a>percentielen ()

[**percentielen ()** ](https://docs.microsoft.com/azure/kusto/query/percentiles-aggfunction): Retourneert een schatting voor het opgegeven [**dichtstbijzijnde-positie percentiel**](https://docs.microsoft.com/azure/kusto/query/percentiles-aggfunction) van de populatie die is gedefinieerd door een expressie. De nauw keurigheid is afhankelijk van de densiteit van de populatie in de regio van het percentiel. Kan alleen worden gebruikt in de context van aggregatie in [](https://docs.microsoft.com/azure/kusto/query/summarizeoperator)samenvatten.

De volgende query berekent percentielen voor Storm duur.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUUitKEnNS1FIKS1KLMnMz1OwVXDNSwnJzE1V0FUILkksKgGxQQrLM1KLUhHq7BQMirEI2ygYZ4CEi0tzcxOLMqtSFQpSi5KBlmXmpBZrwJTpKJjqKBgZACkgtgBiS1NNAEC7XiaYAAAA) **\]**

```Kusto
StormEvents
| extend duration = EndTime - StartTime
| where duration > 0s
| where duration < 3h
| summarize percentiles(duration, 5, 20, 50, 80, 95)
```

Met de volgende query worden percentielen voor Storm-duur berekend op basis van status en worden de gegevens genormaliseerd door opslag locaties van vijf minuten (`5m`).

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1NSwrCMBTcC95hli1EKEpBQd31BHUvafOgAZNI8uIPD28SEBVcDDMM8%2bnZedNdyHKYz56gG5NVUNFL1s5ih86qgzaEBXqWnrPOwetEnj65PZrwx95iNWU7RGOk1w8C5avj6KLlNF64qjHcMWhbvXsCralFPmT6rZ%2fJj2lAnyh8pwWWTaKEdcKmLYul%2fgLODFs%2b4AAAAA%3d%3d) **\]**

```Kusto
StormEvents
| extend duration = EndTime - StartTime
| where duration > 0s
| where duration < 3h
| summarize event_count = count() by bin(duration, 5m), State
| summarize percentiles(duration, 5, 20, 50, 80, 95) by State
```

### <a name="cross-dataset"></a>Meerdere gegevensset

In deze sectie worden elementen beschreven waarmee u complexere query's kunt maken, gegevens kunt koppelen aan tabellen en query's in data bases en clusters.

### <a name="let"></a>bekijken

[**laten**](https://docs.microsoft.com/azure/kusto/query/letstatement): Verbetert de modulariteit en hergebruik. Met de instructie **toestaan** kunt u een potentieel complexe expressie opsplitsen in meerdere delen, die allemaal zijn gekoppeld aan een naam en die onderdelen samen opstellen. Een instructie can kan ook worden gebruikt voor het maken van door de gebruiker gedefinieerde functies en weer gaven (expressies over tabellen waarvan de resultaten eruitzien als een nieuwe tabel). Expressies die zijn gebonden aan een instructie can van het type scalair, van het type tabellaire of door de gebruiker gedefinieerde functie (lambdas).

In het volgende voor beeld wordt een variabele van het type tabellair gemaakt die in een volgende expressie wordt gebruikt.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAMtJLVHwyUzPKMnLzEsPLskvyi1WsOXlArNcy1LzSop5uWoUyjNSi1IVwPyQyoJUBVtbBSW4LiVrXq4coDGOZYk5iXnJGakkGQPXBTIGzSUgPVn5mXkKGmhmayrk5ykElySWpIKUpGQWl2TmJZdARACul3kY0gAAAA%3d%3d) **\]**

```Kusto
let LightningStorms =
StormEvents
| where EventType == "Lightning";
let AvalancheStorms =
StormEvents
| where EventType == "Avalanche";
LightningStorms
| join (AvalancheStorms) on State
| distinct State
```

### <a name="join"></a>join

[**join**](https://docs.microsoft.com/azure/kusto/query/joinoperator): De rijen van twee tabellen samen voegen om een nieuwe tabel te vormen door de waarden van de opgegeven kolom (men) van elke tabel te vergelijken. Kusto biedt ondersteuning voor een breed scala aan join-typen: **fullouter**, **inner**, **innerunique**, **leftanti**, **leftantisemi**, **leftouter**, **leftsemi**, **rightanti**, **rightantisemi**, **rightouter** , **rightsemi**.

In het volgende voor beeld worden twee tabellen samengevoegd met een inner join.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA8tJLVGIULBVSEksAcKknFQN79RKq+KSosy8dB2FsMSc0lRDq5z8vHRNXq5oXi4FIFBPVNcx1IGyk9R1jJDYxjB2srqOCS9XrDUvVw7Qhkj8Nhih2wA0ydAAySgjZI4xnJMCtMQAYkuEQo1CVn5mnkJ2Zl6KbWZeXmoR0Nb8PAWgZQAFPLdO5AAAAA==) **\]**

```Kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X 
| join kind=inner Y on Key
```

> [!TIP]
> Gebruik **where** -en **project** -Opera tors om het aantal rijen en kolommen in de invoer tabellen te verminderen vóór de koppeling. Als de ene tabel altijd kleiner is dan de andere, gebruikt u deze als linker (piped) zijde van de koppeling. De kolommen voor de samenvoegings overeenkomst moeten dezelfde naam hebben. Gebruik de **project** -operator, indien nodig, om de naam van een kolom in een van de tabellen te wijzigen.

### <a name="serialize"></a>serialiseren

[**serialisatie**](https://docs.microsoft.com/azure/kusto/query/serializeoperator): De rijenset wordt geserialiseerd zodat u functies kunt gebruiken waarvoor geserialiseerde gegevens zijn vereist, zoals **row_number ()** .

De volgende query slaagt omdat de gegevens worden geserialiseerd.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIzi%2fNK9HQVEiqVAguSSxJBcumFmUm5gBlQZzUipLUvBSFovzy%2bLzS3KTUIgVbJI6GJgB4pV4NWgAAAA%3d%3d) **\]**

```Kusto
StormEvents
| summarize count() by State
| serialize
| extend row_number = row_number()
```

De rij-set wordt ook als geserialiseerd beschouwd als het resultaat is van: **Sorteer**-, **Top**-of **bereik** operators, eventueel gevolgd door **project**, **project-** uit, **uitbreiden**, **where**, **parse**, **MV-Expand** of **Neem** Opera tors.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIzi%2fNK9HQVEiqVAguSSxJBcvmF5XABRQSi5NBgqkVJal5KQpF%2beXxeaW5SalFCrZIHA1NAEGimf5iAAAA) **\]**

```Kusto
StormEvents
| summarize count() by State
| sort by State asc
| extend row_number = row_number()
```

### <a name="cross-database-and-cross-cluster-queries"></a>Query's tussen meerdere data bases en meerdere clusters

[Query's tussen meerdere data bases en meerdere clusters](https://docs.microsoft.com/azure/kusto/query/cross-cluster-or-database-queries): U kunt een query uitvoeren op een Data Base op hetzelfde cluster door `database("MyDatabase").MyTable`deze te verwijzen als. U kunt een query uitvoeren op een Data Base op een extern cluster door `cluster("MyCluster").database("MyDatabase").MyTable`hiernaar te verwijzen.

De volgende query wordt van één cluster aangeroepen en query's gegevens uit `MyCluster` het cluster. Als u deze query wilt uitvoeren, gebruikt u uw eigen cluster naam en database naam.

```Kusto
cluster("MyCluster").database("Wiki").PageViews
| where Views < 2000
| take 1000;
```

### <a name="user-analytics"></a>Gebruikers analyse

Deze sectie bevat elementen en query's die laten zien hoe eenvoudig het is om gebruikers gedrag in Kusto uit te voeren.

### <a name="activity_counts_metrics-plugin"></a>activity_counts_metrics-invoeg toepassing

[**activity_counts_metrics-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/activity-counts-metrics-plugin): Hiermee berekent u de metrische gegevens van de activiteit (totale aantal waarden, waarden voor unieke tellingen, unieke telling van nieuwe waarden en geaggregeerde unieke tellingen). Metrische gegevens worden berekend voor elk tijd venster, waarna ze worden vergeleken en worden geaggregeerd naar en met alle vorige Windows-tijden.

De volgende query analyseert de gebruikers acceptatie door het dagelijkse activiteiten aantal te berekenen.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAJXSPQvCMBAG4L3Q%2f5CtFlLoFyiVDn4M6mqdREpsggTaKs1VEfzxXm0LDiEimcJz3CW8VwogClgDKWcgQFZiEvrB1PNnnh%2b4c9sqsUDUXMPxyA9Z8%2bsjDfhwz0hKsBzPuRSTgxLNlicKGllfKMmwBw6sbsnY0bWto205C4cS3Rso2tpgO4MtDbbSWvixzGD6eb1ttBYZev42%2fbzI8L%2fe9n9b3NkJQ8xs60XEnZUt1hBWgLxLeObFta1B5ZXAKAs1BPuVKO03iXb7gp36tXDfExVB%2f2ICAAA%3d) **\]**

```Kusto
let start=datetime(2017-08-01);
let end=datetime(2017-08-04);
let window=1d;
let T = datatable(UserId:string, Timestamp:datetime)
[
'A', datetime(2017-08-01),
'D', datetime(2017-08-01),
'J', datetime(2017-08-01),
'B', datetime(2017-08-01),
'C', datetime(2017-08-02),
'T', datetime(2017-08-02),
'J', datetime(2017-08-02),
'H', datetime(2017-08-03),
'T', datetime(2017-08-03),
'T', datetime(2017-08-03),
'J', datetime(2017-08-03),
'B', datetime(2017-08-03),
'S', datetime(2017-08-03),
'S', datetime(2017-08-04),
];
T
| evaluate activity_counts_metrics(UserId, Timestamp, start, end,
window)
```

### <a name="activity_engagement-plugin"></a>activity_engagement-invoeg toepassing

[**activity_engagement-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/activity-engagement-plugin): Hiermee wordt de activiteit betrokkenheid verhouding berekend op basis van de kolom ID in een verschuivende tijdlijn venster. de **activity_engagement-invoeg toepassing** kan worden gebruikt voor het berekenen van Dau, WAU en Mau (dagelijkse, wekelijkse en maandelijkse actieve gebruikers).

De volgende query retourneert de verhouding van het totale aantal afzonderlijke gebruikers dat dagelijks gebruikmaakt van een toepassing, vergeleken met het totale aantal afzonderlijke gebruikers dat de toepassing wekelijks gebruikt, in een zwevende periode van zeven dagen.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1RQWrDMBC8G%2fyHvUVOHGy1lByKD6GBviDkUIoR1tpVsS0jr0MCeXxXiigpVAiBVjOzM6uigHcc0SlCcGrUdgCtSIFtYZnRgWrInA0ZnNOkR4J6JuUIKo9CMgOKp1LutqXknb1GDI76P8RzQHCXDqHW6gqt43ZRkeydNxNOIHWa3AAv5Ctei2xvx06IQNtGTlZInT0AHQN9BpFt5EO59kHmKvQVUUivX8q1y3L4c9%2fIks%2bt5LoMwsMZLxMrgtHVXcb7pOuEthWemEFvBkPARL%2fSpCjgTfXN0vuBHvbH4rQ%2fsikyNjg6q37xL3GsV47cqQ4HHEl8rIxefeZhNHmMmIehsB2dp8nunnZy9hsbiriDWuqTWqpfxdBsLb2ZGzhm8y%2f6b2i%2bWO8HLFcMGe8BAAA%3d) **\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-01-01);
let _end = datetime(2017-01-31);
range _day from _start to _end step 1d
| extend d = tolong((_day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+100*r-1), 1)
| mv-expand id=_users to typeof(long) limit 1000000
// Calculate DAU/WAU ratio
| evaluate activity_engagement(['id'], _day, _start, _end, 1d, 7d)
| project _day, Dau_Wau=activity_ratio*100
| render timechart
```

> [!TIP]
> Bij het berekenen van DAU/MAU, wijzigt u de eind gegevens en de periode van het verhuizings venster (OuterActivityWindow).

### <a name="activity_metrics-plugin"></a>activity_metrics-invoeg toepassing

[**activity_metrics-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/activity-metrics-plugin): Hiermee worden de metrische gegevens van de activiteit (DISTINCT Count values, het aantal nieuwe waarden, de Bewaar periode en het verloop tempo) berekend op basis van het venster huidige periode en het vorige periode venster.

Met de volgende query worden de verloop-en bewaar frequentie voor een bepaalde gegevensset berekend.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG2SwW7CMAyG70i8g2%2bk0KoNE%2bIwscsOe4hpqqLGQFjaVKkLVNrDLw7RxjRyqBTr%2fz%2f3t1OW8IYdekUIXnXataAVKXB7GAf0oBoyZ0MGh%2fnMIkE9kPIEO1YhmRbFupLbopJFtc6ekwY7%2fV%2bxKZ4kK0KXA0Kt1QR7H9olIrmbbyDsQer57AvwSlxhFjnruoMQ0VYkT1ZKnd0JfRByBpGt5F255iDDLvYVCaSXm2rpsxz%2b3FfrKnwLGeoygtszXvtABKN3Nwz%2fJ009ur1gYwbWtIZAVvGw53JEn%2fK9PJwSi3rvTthQlOWBPp%2bVJbwq24yWN3FB%2fLQTeAwByLgOeD8x0lnZkRVpL1PdInnTDOJ9YfTiI0%2fE24DyONIctvpB0x94zfBlSJBDcxz97509PgDCM%2bAMzTEgvwEO44wSMAIAAA%3d%3d) **\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-01-02);
let _end = datetime(2017-05-31);
range _day from _start to _end step 1d
| extend d = tolong((_day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+200*r-1), 1)
| mv-expand id=_users to typeof(long) limit 1000000
| where _day > datetime(2017-01-02)
| project _day, id
// Calculate weekly retention rate
| evaluate activity_metrics(['id'], _day, _start, _end, 7d)
| project _day, retention_rate*100, churn_rate*100
| render timechart
```

### <a name="new_activity_metrics-plugin"></a>new_activity_metrics-invoeg toepassing

[**new_activity_metrics-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/new-activity-metrics-plugin): Berekent de metrische gegevens over de levens duur (DISTINCT Count values, het unieke aantal nieuwe waarden, de Bewaar periode en het verloop tempo) voor de cohort van nieuwe gebruikers. Het concept van deze invoeg toepassing is vergelijkbaar met de [**activity_metrics-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/activity-metrics-plugin), maar is gericht op nieuwe gebruikers.

Met de volgende query wordt een Bewaar-en verloop frequentie berekend met een week-over-week-venster voor de nieuwe gebruikers cohort (gebruikers die de eerste week hebben gearriveerd).

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1Ry27DIBC8W%2fI%2f7C04wbJJFeVQ5VapP9BbVVnIrGMaGyy8eVjqxxcwh1QqBx7LzCwzVBW8o0EnCcFJo%2bwISpIE28F1RgeyJX3TpHHOswEJmpmkIzgFFJIeke1rcSzrQ1mL4jVh0Kj%2fEC8R4bucEd7kAp3z3ZIg2ZU2E04gVJ79AD4oVIIU2cGaM2OBVSZKUQlVPOGcxwUHrNiJp3ITbMyn2JUlHbU91FtXcPhz3u1rP5fC10UUHm%2f4mLwiaHVaZcIzaZnQdiwQCxj0qAlEHUeeVRV8yAuCNcMC1CN02s0Ed8QLtLa33igbpK9M0skRCd3q4CaHa%2fgBg%2fcmJb40%2ft7pdmafG602XzxExpN3HsPicFQ8z1IcQWhy9htbisk2EU92XZ1vZkhb04Sv5tD2V7fufwFYtolnAgIAAA%3d%3d) **\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-05-01);
let _end = datetime(2017-05-31);
range Day from _start to _end step 1d
| extend d = tolong((Day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+200*r-1), 1)
| mv-expand id=_users to typeof(long) limit 1000000
// Take only the first week cohort (last parameter)
| evaluate new_activity_metrics(['id'], Day, _start, _end, 7d, _start)
| project from_Day, to_Day, retention_rate, churn_rate
```

### <a name="session_count-plugin"></a>session_count-invoeg toepassing

[**session_count-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/session-count-plugin): Hiermee wordt het aantal sessies berekend op basis van de kolom ID over een tijd lijn.

Met de volgende query wordt het aantal sessies geretourneerd. Een sessie wordt als actief beschouwd als een gebruikers-ID ten minste één keer per periode van 100-tijd sleuven wordt weer gegeven, terwijl het venster voor het weer geven van de sessie 41-tijd sleuven is.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWPQYvCQAyF74X%2bh3dZUCjYgfUgMkcP3r2XoZPqaM3INK4u7I%2ffzOwiNQRC8pKPl5EEnXfiYJEcHwmHcKUxMGFI8QoDidhoYBK6wdTVD%2bgpxB5dd6FvPSuzcwyMS2BvAzMlLP5gez%2fDrNt%2fCN4Z1iwRua5Kk2GPE6WZkY%2bMsRZt1m4pnqmXl9qouK2r1Qo75cUB5RlPQ%2bAgoWDzpPj%2bcuPdCWGiaVKp6%2bOdZbH3zYxmNFuNUhp8mmU%2bTWpWv8or%2fckl%2bQXutT48NwEAAA%3d%3d) **\]**

```Kusto
let _data = range Timeline from 1 to 9999 step 1
| extend __key = 1
| join kind=inner (range Id from 1 to 50 step 1 | extend __key=1) on __key
| where Timeline % Id == 0
| project Timeline, Id;
// End of data definition
_data
| evaluate session_count(Id, Timeline, 1, 10000, 100, 41)
| render linechart
```

### <a name="funnel_sequence-plugin"></a>funnel_sequence-invoeg toepassing

[**funnel_sequence-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/funnel-sequence-plugin): Berekent het unieke aantal gebruikers dat een reeks statussen heeft gemaakt; toont de distributie van vorige en volgende statussen die hebben geleid tot of werden gevolgd door de reeks.

Met de volgende query wordt aangegeven welke gebeurtenis plaatsvindt voor en na alle Tornado-gebeurtenissen in 2007.

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAGWOPYvCQBCG%2b0D%2bw3QmEEmieIqNVQrBRgxYiMhcdqKLyWzcnQiCP95V70DuYIrh5Xk%2f0hRWxpw1H8EwbMTYtrgSiwMnKNqJrtw8DNIU1vkcticUOGHXETv4ptpYgtJYRmWAnrbFGx39QbEWsv%2fIj7YwuHsZmx6FoO6ZqTk4uvTEFUVFp51RtFSJH4hWSt1SAsqj4r9olGXTYZb7i5Mw%2bJRnvzLkKhl%2fTXzAq668dc%2bAG2Orq2g3%2bBk22MfxA23MLGQQAQAA) **\]**

```Kusto
// Looking on StormEvents statistics:
// Q1: What happens before Tornado event?
// Q2: What happens after Tornado event?
StormEvents
| evaluate funnel_sequence(EpisodeId, StartTime, datetime(2007-01-01), datetime(2008-01-01), 1d,365d, EventType, dynamic(['Tornado']))
```

### <a name="funnel_sequence_completion-plugin"></a>funnel_sequence_completion-invoeg toepassing

[**funnel_sequence_completion-invoeg toepassing**](https://docs.microsoft.com/azure/kusto/query/funnel-sequence-completion-plugin): Berekent de trechter van voltooide reeks stappen binnen verschillende Peri Oden.

Met de volgende query wordt de voltooiings trechter van de `Hail -> Tornado -> Thunderstorm -> Wind` reeks gecontroleerd: in "algemene" tijden van één uur, vier uur en één`[1h, 4h, 1d]`dag ().

**\[** [**Klik om de query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA12QTYvCMBCG74L/YW6tkIV2XT9g8SjsnlvwICKhM9JAOqlJqrj4402CW0RIIB/PPLwzmjwcnZfWwwZQevKqo/yzKFYfRRnW7Hs60ZEhxjdi/UZcFaO5VuqPAjhfLvD/w9F5IG7iM95YdqrJ99mPVDoTkNXGskSTju3ASNZ5Y7t43wVhdhj9PVll0L1aylbAV9glJqyKldsLsXfTyR3oIvUQAsNpYCY95jg2puuDUhnOt71yBukXBVRxCnVoTjwnIlLX4rUzAUlf3/pEPYViDDd7AOyqowFQAQAA) **\]**

```Kusto
let _start = datetime(2007-01-01);
let _end = datetime(2008-01-01);
let _windowSize = 365d;
let _sequence = dynamic(['Hail', 'Tornado', 'Thunderstorm', 'Wind']);
let _periods = dynamic([1h, 4h, 1d]);
StormEvents
| evaluate funnel_sequence_completion(EpisodeId, StartTime, _start, _end, _windowSize, EventType, _sequence, _periods)
```

## <a name="functions"></a>Functies

In deze sectie worden [**functies**](https://docs.microsoft.com/azure/kusto/query/functions)beschreven: herbruikbare query's die zijn opgeslagen op de server. Functies kunnen worden aangeroepen door query's en andere functies (recursieve functies worden niet ondersteund).

> [!NOTE]
> U kunt geen functies maken in het Help-cluster. Dit is alleen-lezen. Gebruik uw eigen test cluster voor dit onderdeel.

In het volgende voor beeld wordt een functie gemaakt die een status`MyState`naam () als argument gebruikt.

```Kusto
.create function with (folder="Demo")
MyFunction (MyState: string)
{
StormEvents
| where State =~ MyState
}
```

In het volgende voor beeld wordt een functie aangeroepen, waarmee gegevens worden opgehaald voor de status van Texas.

```Kusto
MyFunction ("Texas")
| summarize count()
```

In het volgende voor beeld wordt de functie verwijderd die in de eerste stap is gemaakt.

```Kusto
.drop function MyFunction
```

## <a name="next-steps"></a>Volgende stappen

[Naslag informatie voor Kusto-query taal](https://aka.ms/kustolangref)
