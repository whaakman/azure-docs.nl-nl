---
title: Schrijven van query's voor Azure Data Explorer
description: In deze procedures leert u hoe u eenvoudige en meer geavanceerde query's uitvoeren voor Azure Data Explorer.
services: data-explorer
author: mgblythe
ms.author: mblythe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: c345d0730f570e1ab0c396f2cc8f85e1bae8156c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976209"
---
# <a name="write-queries-for-azure-data-explorer"></a>Schrijven van query's voor Azure Data Explorer

In dit artikel leert u hoe u de querytaal in Azure Data Explorer gebruiken om uit te voeren van eenvoudige query's met de meest voorkomende operators. U ook krijgen enkele van de meer geavanceerde functies van de taal te zien.

## <a name="prerequisites"></a>Vereisten

U kunt de query's in dit artikel op twee manieren uitvoeren:

- Op de Azure Data Explorer *helpcluster* dat we hebben ingesteld om te leren.
    [Meld u aan het cluster](https://dataexplorer.azure.com/clusters/help/databases/samples) met een organisatie-e-mailaccount die deel uitmaakt van Azure Active directory.

- Op uw eigen cluster bevat die de StormEvents-voorbeeldgegevens. Zie voor meer informatie, [Quickstart: maken van een cluster van Azure Data Explorer en -database](create-cluster-database-portal.md) en [voorbeeldgegevens worden opgenomen in Azure Data Explorer](ingest-sample-data.md).

De set voorbeeldgegevens StormEvents bevat weergerelateerde gegevens van de [National Centers voor omgevingsinformatie](https://www.ncdc.noaa.gov/stormevents/).

## <a name="overview-of-the-query-language"></a>Overzicht van de querytaal

Een querytaal in Azure Data Explorer is een alleen-lezen-aanvraag voor het verwerken van gegevens en resultaten worden geretourneerd. De aanvraag wordt vermeld als tekst zonder opmaak, met behulp van een gegevensstroom-model dat is ontworpen om de syntaxis van de gemakkelijk te lezen, schrijven en automatiseren. De query gebruikt schema-entiteiten die zijn ingedeeld in een hiërarchie die vergelijkbaar is met SQL: databases, tabellen en kolommen.

De query bestaat uit een reeks queryinstructies, gescheiden door puntkomma's (`;`), met ten minste één instructie wordt een expressie in tabelvorm-instructie, die is een overzicht met gegevens in een tabel-achtige net van kolommen en rijen gerangschikt produceert. De resultaten van de query worden gegenereerd, in tabelvorm van de query-expressies.

De syntaxis van de instructie in tabelvorm expressie bevat de gegevens in tabelvorm stroom vanuit een tabellaire query-operator naar de andere, beginnend met de gegevensbron (bijvoorbeeld een tabel in een database of een operator die gegevens produceert) en vervolgens stroomt via een set gegevenstransformatie operators die samen zijn gebonden door het gebruik van de pipe (`|`) scheidingsteken.

De volgende query heeft bijvoorbeeld één instructie, dit een instructie in tabelvorm expressie is. De instructie begint met een verwijzing naar een tabel met de naam `StormEvents` (de database die als host fungeren in deze tabel is impliciete hier en een deel van de verbindingsinformatie bevatten). De gegevens (rijen) voor deze tabel worden vervolgens gefilterd op de waarde van de `StartTime` kolom en vervolgens gefilterd op de waarde van de `State` kolom. De query retourneert vervolgens het aantal rijen "functionerende'.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVWws1VISSxJLQGyNYwMDMx1DQ11DQw1FRLzUpBU2aArMgIpQjGvJFXB1lZByc3HP8jTxVFJQQEkm5xfmlcCAHoR9euCAAAA)**\]**

```Kusto
StormEvents
| where StartTime >= datetime(2007-11-01) and StartTime < datetime(2007-12-01)
| where State == "FLORIDA"  
| count
```

In dit geval wordt is het resultaat:

|Count|
|-----|
|   23|
| |

Zie voor meer informatie de [Query language reference](https://aka.ms/kustolangref).

## <a name="most-common-operators"></a>Meest voorkomende operators

De operators in deze sectie beschreven, zijn de bouwstenen voor informatie over query's in Azure Data Explorer. De meeste query's die u schrijft bevat verschillende van deze operators.

Query's uitvoeren op het helpcluster: Selecteer **Klik query uit te voeren** boven elke query.

Query's op uw eigen cluster uitvoeren:

1. Elke query bij de query op basis van een web-App, kopiëren en vervolgens selecteert u de query of plaats de cursor in de query.

1. Selecteer aan de bovenkant van de toepassing **uitvoeren**.

### <a name="count"></a>count

[**aantal**](https://docs.microsoft.com/azure/kusto/query/countoperator): retourneert het aantal rijen in de tabel.

De volgende query retourneert het aantal rijen in de tabel StormEvents.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRSM4vzSsBALU2eHsTAAAA)**\]**

```Kusto
StormEvents | count
```

### <a name="take"></a>toets maken

[**nemen**](https://docs.microsoft.com/azure/kusto/query/takeoperator): retourneert het opgegeven aantal rijen met gegevens.

De volgende query retourneert vijf rijen uit de tabel StormEvents. Het sleutelwoord *limiet* is een alias voor *nemen.*

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRKEnMTlUwBQDEz2b8FAAAAA%3d%3d)**\]**

```Kusto
StormEvents | take 5
```

> [!TIP]
> Er is geen garantie dat welke records worden geretourneerd, tenzij de brongegevens is gesorteerd.

### <a name="project"></a>project

[**Project**](https://docs.microsoft.com/azure/kusto/query/projectoperator): een subset van kolommen selecteert.

De volgende query retourneert een specifieke set kolommen.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUShJzE5VMAWxCorys1KTSxSCSxKLSkIyc1N1FFzzUiAMoFgJiA%2fSFlJZAGS6JOYmpqcGFOUXpBaVVAKlCjKL81NS%2fRKLihJLMstSAY%2buIINnAAAA)**\]**

```Kusto
StormEvents
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="where"></a>waar

[**waar**](https://docs.microsoft.com/azure/kusto/query/whereoperator): een tabel gefilterd op de subset rijen die voldoet aan een predicaat.

De gegevens door de volgende query worden gefilterd `EventType` en `State`.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAEWMPQvCMBCGd8H%2fcFuWro4dBOvHkgoJOB%2fm0KjJhetRKfjjNe3g9n49r1OW1I2UdVivPvC%2bkxDM3k%2bFoG3B7F%2fMwQDmAE5Rl%2fCydceTPfjemsopPgk2VRXhB121TkV9TNRAl8MiZrz53zeww4Q3OgsXEp1%2bVYkDB7IoghpH%2bgI9OH8WnwAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="sort"></a>Sorteren

[**sorteren**](https://docs.microsoft.com/azure/kusto/query/sortoperator): de rijen van de invoertabel in volgorde sorteren op een of meer kolommen.

De volgende query worden de gegevens in aflopende volgorde door `DamageProperty`.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2NPQvCMBCGd8H%2fcFuXrI4dBOvHEoUGnM%2fm0KjphctRKfjjNe0guL0fvM%2fbKktsBuo1LxdveN1ICCbvxkRQ11Btn8y%2bAuw9tIo6h%2bd1uz%2fYnTvaquwyi8JlhA1GvNJJOJHoCJ5yV2rFB8GqqCR8p04LSdSFSAaa3s9iopvfu%2fnDfasUMnuyKIIaBvoAtvGMsb4AAAA%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| sort by DamageProperty desc
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

> [!NOTE]
> De volgorde van bewerkingen is belangrijk. Probeer het plaatsen van `take 5` voordat `sort by`. Waar u verschillende resultaten krijgen?

### <a name="top"></a>Boven

[**Top**](https://docs.microsoft.com/azure/kusto/query/topoperator): retourneert de eerste *N* records worden gesorteerd op de opgegeven kolommen.

De volgende query retourneert hetzelfde resultaat als hierboven met een minder operator.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2NOwvCMBSFd8H%2fcLcsWR07CNbHkgoJOMfmohGTG24vlYA%2fXtsOgtt5cL5jhTi1I2YZ1qs3vO7ICLN3tSA0Daj9kygo8DmAFS9LeNna48kcXGfUtBMqsIFrhZ1P%2foZnpoIsFQIO%2fdQXpgf2MgFYXEyooc1hETNU%2f071H%2bRblThQQOOZvcQRP1rSng21AAAA)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| top 5 by DamageProperty desc
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="extend"></a>Uitbreiden

[**uitbreiden**](https://docs.microsoft.com/azure/kusto/query/extendoperator): berekeningen afgeleide kolommen.

De volgende query maakt een nieuwe kolom door een waarde in elke rij computing.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2OvQ7CMAyEdyTewVuWMDJ2QGr5WQJSKzGHxoIiEkeuKVTi4WmooBKbfXeffaUQ%2b6LDIO189oLHBRnhs1d9RMgyUOsbkVNgg4NSrIzicVVud2ZT7Y1KnFCEJZx6yK23ZzwwRWTpwWFbJx%2bfggOf39lKQwEyKIKrGo%2bwSEdZ0pyCkemKtUyi%2fib1j9ZjDz311H9%2fBys2LTk0lhPT4RvwA3pn6AAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| top 5 by DamageProperty desc
| extend Duration = EndTime - StartTime
| project StartTime, EndTime, Duration, State, EventType, DamageProperty, EpisodeNarrative
```

Expressies kunnen opnemen de gebruikelijke operators (+, -, *, /, %), en er is een bereik van handige functies die u kunt aanroepen.

### <a name="summarize"></a>samenvatten

[**samenvatten**](https://docs.microsoft.com/azure/kusto/query/summarizeoperator): aggregeert Rijgroepen.

De volgende query retourneert het aantal gebeurtenissen per `State`.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIBYnFJ%2beX5pUo2CqAaQ1NhaRKheCSxJJUAB%2fedDI3AAAA)**\]**

```Kusto
StormEvents
| summarize event_count = count() by State
```

De **samenvatten** operator gegroepeerd rijen met dezelfde waarden in de **door** -component, en wordt vervolgens gebruikt de statistische functie (zoals **aantal**) te combineren van elke groep in een enkele rij. Dus in dit geval wordt is er een rij voor elke status en een kolom voor het aantal rijen in deze staat.

Er is een bereik van aggregatiefuncties u kunt meerdere gebruiken in een **samenvatten** operator voor het produceren van enkele berekende kolommen. Bijvoorbeeld, u kan de telling van storm in elke status en het unieke nummer van storm per status ophalen en vervolgens met **boven** om op te halen van de Staten meest storm beïnvloed.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlUIBkk455fmlSjYKiSDaA1NHYWQyoJU%2fzSwXDFQPAUiAdYPktJUSKoE6kwsSQUZVpJfoGAKEYGblZJanAwAgbFb73QAAAA%3d)**\]**

```Kusto
StormEvents
| summarize StormCount = count(), TypeOfStorms = dcount(EventType) by State
| top 5 by StormCount desc
```

Het resultaat van een **samenvatten** bewerking heeft:

- Elke kolom met de naam in **door**

- Een kolom voor elke berekende expressie

- Een rij voor elke combinatie van waarden

### <a name="render"></a>Render

[**renderen**](https://docs.microsoft.com/azure/kusto/query/renderoperator): resultaten als een grafische uitvoer wordt weergegeven.

De volgende query wordt een kolomdiagram weergegeven.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWMsQ7CQAxDdyT%2bIWMrdSgbSxmQ2Nj6Aei4Ru0hkqA0VwTi49uUBRZL9rPdmiidJmQbt5sPjJkoaHojoGeXKJmtWbUoK6DUQQNh6osj9onPwUq4vqC1YLjORc2Dpef2OaD%2bPcEBdvu6dvZQuWG077b6LTlV5A4VotwzcRyC2gxU6ktSqQAAAA%3d%3d)**\]**

```Kusto
StormEvents
| summarize event_count=count(), mid = avg(BeginLat) by State
| sort by mid
| where event_count > 1800
| project State, event_count
| render columnchart
```

De volgende query geeft een grafiek met eenvoudige weer.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIBYnFJ%2beX5pXYgkkNTYWkSoWkzDyN4JLEopKQzNxUHQXDFE2QtqLUvJTUIoUSoFhyBlASAAyXWQJWAAAA)**\]**

```Kusto
StormEvents
| summarize event_count=count() by bin(StartTime, 1d)
| render timechart
```

De volgende query uit telt gebeurtenissen op het moment dat modulo één dag binned in uren en een grafiek weergegeven.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEADWNQQqDMBRE90LvMBtBwY0HcNkT2L2k8UuEJh9%2bfqSWHt4k4GZghpk3s7L450FB46P5g75KYYXjJJiwfZilm9WIvnZPaDGuGDC6vnRj8t7I%2fiNQ2S%2bWU9CpatfjfVZKLbLo7WGiLZnkGxJoxlqX%2bRf81ZbyiAAAAA%3d%3d)**\]**

```Kusto
StormEvents
| extend hour = floor(StartTime % 1d , 1h)
| summarize event_count=count() by hour
| sort by hour asc
| render timechart
```

De volgende query vergelijkt meerdere dagelijkse reeksen op een grafiek.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEACWPSwvCMBCE74L%2fYSgIFXrpD%2bihaKzxkUBTXyeputKCbSCmvvDHm9TL7gwzsN8qq03DHtTa%2b3DwBb0stRdUujMJrjetTQhlS2OLuiGMEF8QIa7GvvusyJBPLaFuEQbZZjWDnGHN9nwigyhYp1wwt7c8z7jgqZM7riZSKC6cFjIv5pimS1n4SLAdFixX7OCMzFkmRdAfundNU5r6QyAPejzrrrVJP8MxTu8eN%2fqT%2bL5xL5CBdcjnyrH%2fALPTSKnkAAAA)**\]**

```Kusto
StormEvents
| extend hour= floor( StartTime % 1d , 1h)
| where State in ("GULF OF MEXICO","MAINE","VIRGINIA","WISCONSIN","NORTH DAKOTA","NEW JERSEY","OREGON")
| summarize event_count=count() by hour, State
| render timechart
```

> [!NOTE]
> De **renderen** operator is een client-side-onderdeel in plaats van onderdeel van de engine. Het geïntegreerd in de taal voor het gebruiksgemak. De web-App biedt ondersteuning voor de volgende opties: barchart, columnchart, piechart, tijdgrafiek, en linechart. 

## <a name="scalar-operators"></a>Scalaire operators

Deze sectie worden enkele van de belangrijkste scalaire operators.

### <a name="bin"></a>BIN()

[**BIN()**](https://docs.microsoft.com/azure/kusto/query/binfunction): waarden naar een geheel getal afgerond meervoud van de grootte van een opgegeven opslaglocatie.

De volgende query berekent het aantal en de Bucketgrootte van een van één dag.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVWwU0hJLEktATI1jAwMzHUNjHQNTTQVEvNSkBTZYCoyMtQEGVdcmpubWJRZlaqQCrIiPjm%2fNK9EwVYBTGtoKiRVKiRl5mnAjdJRMEzRBABIhjnmkwAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime > datetime(2007-02-14) and StartTime < datetime(2007-02-21)
| summarize event_count = count() by bin(StartTime, 1d)
```

### <a name="case"></a>case()

[**case()**](https://docs.microsoft.com/azure/kusto/query/casefunction): evalueert een lijst met predicaten is en retourneert de eerste resultaat expressie waarvan predicaat is voldaan of de laatste **anders** expressie. U kunt deze operator categoriseren of de groep gegevens:

De volgende query retourneert een nieuwe kolom `deaths_bucket` en de sterfgevallen door aantal groepen.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAGWOwQrCQAxE74X%2bQ9hTCwX14FFBaK9e%2bgGS7gZdbFrYZEXFj7dbqgfNbfJmhml1DNzcaFDJsxdIZMbgnwSOUC8Cu%2fQq6lnUPpDVEroHtIpKKUB3pcEt7lMX7ZV0ClkUgiLPYLqlaQ%2fbdQWmx3AmU%2f2gTUJMzkf%2bYwkJY99%2fiDmuDqac545Bv3MAxb4Bic1Oy88AAAA%3d)**\]**

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

[**extract()**](https://docs.microsoft.com/azure/kusto/query/extractfunction): haalt een overeenkomt met een reguliere expressie uit een tekenreeks.

De volgende query uit waarden van specifieke kenmerken van een tracering worden uitgepakt.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAE2OwQrCMBBE74X%2bw9BTojHYagSVHJRevXkrHqJdpVBbSVew4McbFYungeXtvKmJsetzxw4WZQh2x5og9t6daIWOfdVcJIpkY1OFrc0U8rt3XLWNTbOZnhultU4UfoD5A4zRmVkovInDOo6%2bojh6gh5MTTmQwR0uQckiGb5FMZ0s9WEsQ3uo%2fixSccT9jdqz8ORqKTECV1cSaSdfq2k6L8oAAAA%3d)**\]**

```Kusto
let MyData = datatable (Trace: string) ["A=1, B=2, Duration=123.45,...", "A=1, B=5, Duration=55.256, ..."];
MyData
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s)
```

Deze query gebruikt een **laten** instructie, die wordt gebonden een naam (in dit geval `MyData`) op een expressie. Voor de rest van het bereik, waarin de **laten** instructie wordt weergegeven (globaal bereik of in een bereik van de hoofdtekst van de functie), de naam kan worden gebruikt om te verwijzen naar de afhankelijke waarde.

### <a name="parsejson"></a>parse_json()

[**parse_json()**](https://docs.microsoft.com/azure/kusto/query/parsejsonfunction): een tekenreeks als een JSON-waarde wordt geïnterpreteerd en retourneert de waarde als dynamisch. Het is beter met behulp van de **extractjson()** werken wanneer u nodig hebt om op te halen van meer dan een element van een samengestelde JSON-object.

De volgende query haalt de JSON-elementen van een matrix.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAHWPQQuCQBCF74L%2fYdmLBSJ6EGKjU17r1E0kJh1C2XZlHc0w%2f3ur1s1O896bB%2fONRGKnVwIE7MAKOwhuEtnmYiBHwRoypbpvXSf1Bl60BqjUiot04B3IFrmIol0Q%2bpPLdauIi3iyj9KWojCcNfRWx7NuqEiw48KaMRu9bO86y3HXeTPsCVXBzvg8amlpajANXqtGq4VmO5VqoyvM6dsKfkhpmAUzkf9nM9OtLi3reg79ar788AEVX8GkOAEAAA%3d%3d)**\]**

```Kusto
let MyData = datatable (Trace: string)
['{"duration":[{"value":118.0,"valcount":5.0,"min":100.0,"max":150.0,"stdDev":0.0}]}'];
MyData
| extend NewCol = parse_json(Trace)
| project NewCol.duration[0].value, NewCol.duration[0].valcount, NewCol.duration[0].min, NewCol.duration[0].max, NewCol.duration[0].stdDev
```

De volgende query haalt de JSON-elementen.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAE2OwQqCQBCG74LvsOzFBBE9CLHRKa916hYRkw6RbLuyO5pRvXvrGtZpvn9m4P8kEts%2bSiBga1a7QXCWyBZ7AxUKZslc1SVmh%2bjJe5AdcpHnyzRLxlTpThEXxRhvV%2bVOWeYZBseFZ0t1iT0XLryj4yoMprIweDEcCFXNdnjfaOnaWzAWT43VamqPx6fW6AYr%2bn6l3iH5S95hXjiLH8Mw82TxAQvJEB%2fsAAAA)**\]**

```Kusto
let MyData = datatable (Trace: string) ['{"value":118.0,"valcount":5.0,"min":100.0,"max":150.0,"stdDev":0.0}'];
MyData
| extend NewCol = parse_json(Trace)
| project NewCol.value, NewCol.valcount, NewCol.min, NewCol.max, NewCol.stdDev
```

De volgende query haalt de JSON-elementen van het type dynamische gegevens.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAD2NMQvCMBBG90D%2bw5GphVLSoSARt65ubuJwJjdU0lZiWlrU%2f25MotO9x8H7LHk4bh16hAOYcDxeLUFxcqhJgdlGHHpdcnbOWDzFgnYmoZpmV8tK6GkePTmh2q8N%2fRg%2bUkbGNXAb%2beFNR4tQQd7lZc9ZGuXsBXc33Uh7iJN1jFdZcvunIf5HXCvOEqf2BwXmDCnKAAAA)**\]**

```Kusto
let MyData = datatable (Trace: dynamic)
[dynamic({"value":118.0,"counter":5.0,"min":100.0,"max":150.0,"stdDev":0.0})];
MyData
| project Trace.value, Trace.counter, Trace.min, Trace.max, Trace.stdDev
```

### <a name="ago"></a>ago()

[**ago()**](https://docs.microsoft.com/azure/kusto/query/agofunction): het opgegeven interval van de huidige UTC clock tijd trekt.

De volgende query retourneert de gegevens voor de afgelopen 12 uur.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA1WOQQ6CQAxF9yTc4S8hQcmQuNSNR4ALTKQyJDAlnSIuPLwzJGrctM3v+7+t684R7qMEhW6MafQUMJAnsUoIdl4mQm/VVrC+h0Z6shFOINZAIc/qOql24KIEL8nIAuWYohC6sfQB9yjtPtPA8SrhmGeLjF7RjTO1Gu+cIdYPVHjeisOpLyukKTbjYml5piuvXknwIU1lGlPm2Qvzg55L+u+b9udIyOZI6LfHZf/YNK58Ay2HrbAEAQAA)**\]**

```Kusto
//The first two lines generate sample data, and the last line uses
//the ago() operator to get records for last 12 hours.
print TimeStamp= range(now(-5d), now(), 1h), SomeCounter = range(1,121)
| mvexpand TimeStamp, SomeCounter
| where TimeStamp > ago(12h)
```

### <a name="startofweek"></a>startofweek()

[**startofweek()**](https://docs.microsoft.com/azure/kusto/query/startofweekfunction): retourneert het begin van de week waarin, de datum, verplaatst door een offset als opgegeven

De volgende query retourneert het begin van de week met een andere offset.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEACtKzEtPVchPSytOLVFIK8rPVdA1VCjJVzBUKC5JLVAw5OWqUSgoys9KTS5RKE9NzQ4uSSwqUbAFygLp%2fDSQkEZefrmGpg7UEE0dCA0AdE3lv1kAAAA%3d)**\]**

```Kusto
range offset from -1 to 1 step 1
| project weekStart = startofweek(now(), offset),offset
```

Deze query gebruikt de **bereik** operator, die een tabel met één kolom met waarden worden gegenereerd. Zie ook: [ **startofday()**](https://docs.microsoft.com/azure/kusto/query/startofdayfunction), [ **startofweek()**](https://docs.microsoft.com/azure/kusto/query/startofweekfunction), [ **startofyear()** ](https://docs.microsoft.com/azure/kusto/query/startofyearfunction)), [ **startofmonth()**](https://docs.microsoft.com/azure/kusto/query/startofmonthfunction), [ **endofday()**](https://docs.microsoft.com/azure/kusto/query/endofdayfunction), [ **endofweek()**  ](https://docs.microsoft.com/azure/kusto/query/endofweekfunction), [ **endofmonth()**](https://docs.microsoft.com/azure/kusto/query/endofmonthfunction), en [ **endofyear()**](https://docs.microsoft.com/azure/kusto/query/endofyearfunction).

### <a name="between"></a>Between()

[**Between()**](https://docs.microsoft.com/azure/kusto/query/betweenoperator): komt overeen met de invoer die zich binnen het bereik liggen.

De volgende query uit de gegevens worden gefilterd op een bepaald datumbereik.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrApGRuaaCnp4ChrixgaYmyKTk%2fNK8EgBluyagXgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime between (datetime(2007-07-27) .. datetime(2007-07-30))
| count
```

De volgende query uit de gegevens worden gefilterd op een bepaald datumbereik, met de kleine variatie van drie dagen (`3d`) vanaf de begindatum.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrApGRuaaCnp6CcYomSF9yfmleCQCGAqjRTAAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime between (datetime(2007-07-27) .. 3d)
| count
```

## <a name="tabular-operators"></a>In tabelvorm operators

Kusto bevat veel in tabelvorm operators, waarvan sommige in andere gedeelten van dit artikel worden besproken. Hier ligt de focus op **parseren**. 

### <a name="parse"></a>parseren

[**parseren**](https://docs.microsoft.com/azure/kusto/query/parseoperator): een tekenreeksexpressie is geëvalueerd en de waarde ervan in een of meer berekende kolommen worden geparseerd. Er zijn drie manieren om te parseren: eenvoudig (de standaardinstelling), regex, en soepele.

De volgende query uit een tracering parseert en extraheert de relevante waarden, met behulp van een standaardwaarde van eenvoudige parseren. De expressie (aangeduid als StringConstant) is een normale string-waarde en de overeenkomst is strikte: uitgebreide kolommen moeten overeenkomen met de vereiste typen.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2UTU%2fDMAyG75X6H6xcxlCkpRlsUNQjN6gQ2wnEoevMFsiaKk2HJvHjabqvlI91l11QLrH12vETW5Zo4H411kmKEME0MdWZSISz2yVmpvaHhdEim3V979n3OrU%2fhFgZ8boaSZHiI0pMiipEY6FKnWKcLDB6EDlKkeEoneO0lKgpGGUSWYcUER9SKOw1LhcT1BHvU5AqfR%2bLKpbxXjDscRYMgF2FFyxkwRMFvX7ngCLXuBSqLO5%2bT9S%2ftrJuh54OI7g8iMFaMdhxGOy0GJz9i25w%2fjdG0IoRHNWNNe1ph2pwEKNlqI7HsEPley83vrfZCL73CXmiq%2fr32wA%2bhJnDOZAGEQHXBNIEIq4VSpXNbAIXkbjAO8UOmuz4bWoXlrhWWO0vqyA2%2bAcw2f7B1rORd60calat3jA1TRbq1A6NxsC%2bLdCoCuj3p74AKTs4pmcFAAA%3d)**\]**

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

De volgende query uit een tracering parseert en extraheert de relevante waarden, met behulp van `kind = regex`. De StringConstant kan een reguliere expressie zijn.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2UQU%2fCQBCF7036HyZ7gWKRbVHQmgY9eNPGCCcoh9KOsLK0ZLtFMf54l6LQBgUuXEyTTbP7pt3vvclwlPC47IkgRHAhCqR6Rhyher%2fAWOb7TioFi8eGrg10rZLvO%2bAlkr0su5yF%2bIwcg1SVCEyTTIToBTN0n9gcOYuxG04wyjgKE2QiA56XpK7dNiFdvXrZbITCtZsm8CSc9piqpXbDajdsarWAXjkX1KFW3wSx%2fs8exVzggiVZ%2bvD7h5rXK5lRMU%2bHYV3uxaAHMehxGPS0GDb9F2nY9t8Y1kEM66g01rSnbarWXowDTXU8xqqpdG14o2vfE0HXPmEeCHX%2fKYsjNR8EjvEdtqMB3picAKme1zrGIKh%2f3NX7w5pLoEgLt6SM56c1PzpTq6oqYpIitMOTeAxAlKb6c3Wjs3GBbAzJJUV8UjQjP91BJztuOGryKbHvGwQgxxbJK4ayTFKKBbahQCkA2DX7C29veJJmBQAA)**\]**

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

De volgende query uit een tracering parseert en extraheert de relevante waarden, met behulp van `kind = relaxed`. De StringConstant is een normale string-waarde en de overeenkomst is beperkte: uitgebreide kolommen kunnen gedeeltelijk overeenkomen met de vereiste typen.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2US0%2fCQBDH7036HSZ7wZpN2BYFrenRGzZG4KLxUNoRVpYu2W5REj%2b83fKw9QE1kYvppTOZx%2f%2b3MxmBGm5WQxXFCAEkkS6%2bsUA4uV5iqku%2fn2nF04ljWw%2b21Sr9PoRS86fVQPAY71BglBUpCjOZqxjDaI7BLV%2bg4CkO4ikmuUBFQUsdiTIlC7wehcz8hvl8jCrwOhSEjGdDXuQyr%2b322h5zu8Au%2fDPmM%2feeglr32ROxULjkMs%2f63xfqXJowp0WPh%2bGe78VgBzFYMwx2XAyP%2fYtpeN7PGO5BDLfRNNa0x12q7l6MA0vVHMMslW09XtnW5iLY1hssIlXon%2fE0CYom0SsmQP6IMxz1%2b7%2b7AnXQdX6TNXMIvHA9hVMgNYEEqiaQuj5StXwh04kpUNVLqup3ETsCsoMxpavSSdXyi7NrIohJ%2foJDtoRbzybcMeFQjkjJZ4x1nYVWtEPtleHjjaGmCujnVu%2fWU75tHgYAAA%3d%3d)**\]**

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

## <a name="time-series-analysis"></a>Tijdseries analyseren

### <a name="make-series"></a>merk-serie

[**merk-serie**](https://docs.microsoft.com/azure/kusto/query/make-seriesoperator): aggregeert samen Rijgroepen zoals [samenvatten](https://docs.microsoft.com/azure/kusto/query/summarizeoperator), maar een reeks (tijd) genereert vector per elke combinatie van waarden.

De volgende query retourneert een set van tijdreeks voor het aantal gebeurtenissen per dag storm. De query bevat informatie over een periode van drie maanden voor elke status, ontbrekende opslaglocaties vullen met de constante 0:

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUchNzE7VLU4tykwtVsizTc4vzSvR0FRISU1LLM0psTVQyM9TCC5JLCoJycxNVcjMUyhKzEtP1UhJLEktAYpoGBkYmOsaGAKRpo4CmqixrjFI1DBFUyGpEmRKSSoAazsM0n0AAAA%3d)**\]**

```Kusto
StormEvents
| make-series n=count() default=0 on StartTime in range(datetime(2007-01-01), datetime(2007-03-31), 1d) by State
```

Zodra u een set (tijd) uit de serie hebt gemaakt, kunt u de reeksfuncties voor het detecteren van afwijkend vormen, seizoensgebonden patronen en veel meer kunt toepassen.

De volgende query haalt de bovenste drie statussen die de meeste gebeurtenissen in een specifieke dag had:

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2OsQoCMRBEe8F%2f2DIBAzmvsLrSLzj7EC%2brBs3mSPbkBD%2feLDYibPVmZmdGziUdn0hct5s3JH9HU7FErEDDlBdipSHgxS8PHixkgpF94VNMCJGgeLqiCp6RG1F7aw%2fGdu30Dv5ob3qhXdBwfskXRmnElZECfDtdbbgq0qJwnqEX76%2fmyCW%2ftkV1Ek9pWSwgNdOt7foAJIuybs8AAAA%3d)**\]**

```Kusto
StormEvents
| make-series n=count() default=0 on StartTime in range(datetime(2007-01-01), datetime(2007-03-31), 1d) by State
| extend series_stats(n)
| top 3 by series_stats_n_max desc
| render timechart
```

Raadpleeg voor meer informatie, de volledige lijst met [reeksfuncties](https://docs.microsoft.com/azure/kusto/query/scalarfunctions#series-processing-functions).

## <a name="advanced-aggregations"></a>Geavanceerde aggregaties

We eenvoudige aggregaties zoals besproken **aantal** en **samenvatten**, eerder in dit artikel. In deze sectie wordt een meer geavanceerde opties.

### <a name="top-nested"></a>Top-geneste

[**Top-geneste**](https://docs.microsoft.com/azure/kusto/query/topnestedoperator): hiërarchische bovenste resultaten, waarbij elk niveau Inzoomen op basis van waarden voor vorige niveau is.

Deze operator is handig voor scenario's dashboard-visualisatie, of wanneer dit nodig is om een vraag als volgt te beantwoorden: "de top N-waarden van K1 (met behulp van sommige aggregatie); vinden voor elk van deze vinden wat zijn de waarden boven-M van K2 (met behulp van een andere aggregatie); ..."

De volgende query retourneert een hiërarchische tabel met `State` op het hoogste niveau, gevolgd door `Sources`.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjJL9DNSy0uSU1RMFLIT1MILkksSVVIqlQoLs3VcEpNz8zzSSzR1OHlQlJoDFaYX1qUTEilIUila16KT35yYklmfh6GcgDrXwk5jgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| top-nested 2 of State by sum(BeginLat),
top-nested 3 of Source by sum(BeginLat),
top-nested 1 of EndLocation by sum(BeginLat)
```

### <a name="pivot-plugin"></a>Pivot()-invoegtoepassing

[**de invoegtoepassing Pivot()**](https://docs.microsoft.com/azure/kusto/query/pivotplugin): een tabel door het inschakelen van de unieke waarden uit één kolom in de invoertabel in meerdere kolommen in de uitvoertabel draait. De operator voert aggregaties uit waar ze zijn vereist op alle overige waarden in de kolom in de uiteindelijke uitvoer.

De volgende query uit een filter wordt toegepast en de rijen in kolommen opengaat.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSgoys9KTS5RCC5JLEnVUQBLhFQWpILkyjNSi1IhMgrFJYlFJcXlmSUZCkqOPkoIabgOhYzEYgWl8My8FLBsalliTilIZ0FmWX6JBtgUTQDlv21NfQAAAA%3d%3d)**\]**

```Kusto
StormEvents
| project State, EventType
| where State startswith "AL"
| where EventType has "Wind"
| evaluate pivot(State)
```

### <a name="dcount"></a>DCount()

[**DCount()**](https://docs.microsoft.com/azure/kusto/query/dcount-aggfunction): retourneert een schatting van het aantal afzonderlijke waarden van een expressie in de groep. Gebruik [ **count()** ](https://docs.microsoft.com/azure/kusto/query/countoperator) alle waarden tellen.

De volgende query uit afzonderlijke telt `Source` door `State`.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlUIzi8tSk4tVrBVSEnOL80r0YAIaCokVSoElySWpAIAFKgSBDoAAAA%3d)**\]**

```Kusto
StormEvents
| summarize Sources = dcount(Source) by State
```

### <a name="dcountif"></a>dcountif()

[**dcountif()**](https://docs.microsoft.com/azure/kusto/query/dcountif-aggfunction): een schatting van het aantal afzonderlijke waarden van de expressie voor rijen die het predicaat wordt geëvalueerd op ' True ' geretourneerd.

De volgende query telt de afzonderlijke waarden van `Source` waar `DamageProperty < 5000`.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRKEnMTlUwNDDg5apRKC7NzU0syqxKVQjOLy1KTi1WsFVISc4vzSvJTNOACOkouCTmJqanBhTlF6QWlVQq2CiYGhgYaCokVSoElySWpAIAuk%2fTX14AAAA%3d)**\]**

```Kusto
StormEvents 
| take 100
| summarize Sources = dcountif(Source, DamageProperty < 5000) by State
```

### <a name="dcounthll"></a>dcount_hll()

[**dcount_hll()**](https://docs.microsoft.com/azure/kusto/query/dcount-hllfunction): berekent de **dcount** van HyperLogLog resultaten (die worden gegenereerd door [ **hll** ](https://docs.microsoft.com/azure/kusto/query/hll-aggfunction) of [ **hll_merge**](https://docs.microsoft.com/azure/kusto/query/hll-merge-aggfunction).

De volgende query gebruikt het algoritme HLL voor het genereren van het aantal.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlXIyMkJSi1WsAUxNFwScxPTUwOK8gtSi0oqNRWSKhWSMvM0gksSi0pCMnNTdQwNcjUx9PumFqWnpkCMiM8FcTQgpoKVFhTlZ6UmlyikJOeX5pXEg6yB69EEAKm9wyCXAAAA)**\]**

```Kusto
StormEvents
| summarize hllRes = hll(DamageProperty) by bin(StartTime,10m)
| summarize hllMerged = hll_merge(hllRes)
| project dcount_hll(hllMerged)
```

### <a name="argmax"></a>arg_max()

[**arg_max()**](https://docs.microsoft.com/azure/kusto/query/arg-max-aggfunction): Hiermee zoekt u een rij in de groep die maximale een expressie en retourneert de waarde van een andere expressie (of * om terug te keren van de hele rij).

De volgende query retourneert de tijd van het laatste rapport voor grote in elke status.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQDzQyoLUhVsbRWU3HLy81OUQLLFpbm5iUWZVakKiUXp8bmJFRrBJYlFJSGZuak6ClqaCkmVCkCBklSQ2oKi%2fKzU5BKIgI4CkkLXvBQoA2YNAHO1S0OFAAAA)**\]**

```Kusto
StormEvents
| where EventType == "Flood"
| summarize arg_max(StartTime, *) by State
| project State, StartTime, EndTime, EventType
```

### <a name="makeset"></a>makeset()

[**makeset()**](https://docs.microsoft.com/azure/kusto/query/makeset-aggfunction): retourneert een dynamische (JSON)-matrix van de set met verschillende waarden die met een expressie in de groep.

De volgende query retourneert alle tijden wanneer een grote is gemeld door elke status en maakt een matrix van de set met unieke waarden.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWLQQ6CQBAE7yb8ocNJE76wR3mA8IEFOxF1mM3siIHweAVPHqsq1bianCeOnovDiveNRuzczokIAWX9VL2WW80vkWjDQuzuwqTmGQESH8z0Y%2bPRvB2EJ3QzvuTcvmR6Z%2b8%2fUf3NH6ZkMFeAAAAA)**\]**

```Kusto
StormEvents
| where EventType == "Flood"
| summarize FloodReports = makeset(StartTime) by State
| project State, FloodReports
```

### <a name="mvexpand"></a>mvexpand

[**mvexpand**](https://docs.microsoft.com/azure/kusto/query/mvexpandoperator): verzameling(en) meerdere waarden uit een kolom dynamisch ingevoerde uitgebreid zodat elke waarde in de verzameling een afzonderlijke rij wordt. De andere kolommen in een uitgevouwen rij worden gedupliceerd. Het is het tegenovergestelde van makelist.

De volgende query uit voorbeeldgegevens worden gegenereerd door het maken van een set en vervolgens wordt gebruikt ter illustratie van de **mvexpand** mogelijkheden.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWOQQ6CQAxF9yTcoWGliTcws1MPIFygyk9EKTPpVBTj4Z2BjSz%2f738v7WF06r1vD2xcp%2bCoNq9yHDFYLIsvvW5Q0JybKYCco2omqnyNTxHW7oPFckbwajFZhB%2bIsE1trNZ0gi1dpuRmQ%2baC%2bjuuthS7Fbwvi%2f%2bP8lpGvAMP7Wr3A6BceSu7AAAA)**\]**

```Kusto
let FloodDataSet = StormEvents
| where EventType == "Flood"
| summarize FloodReports = makeset(StartTime) by State
| project State, FloodReports;
FloodDataSet
| mvexpand FloodReports
```

### <a name="percentiles"></a>percentiles()

[**percentiles()**](https://docs.microsoft.com/azure/kusto/query/percentiles-aggfunction): retourneert een van de schatting voor de opgegeven [ **dichtstbijzijnde positie percentiel** ](https://docs.microsoft.com/azure/kusto/query/percentiles-aggfunction) van de bevolking gedefinieerd door een expressie. De nauwkeurigheid is afhankelijk van de dichtheid van populatie in de regio van het percentiel. Kan alleen worden gebruikt in de context van aggregatie in [ **samenvatten**](https://docs.microsoft.com/azure/kusto/query/summarizeoperator).

De volgende query berekent percentielen voor de duur van storm.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUUitKEnNS1FIKS1KLMnMz1OwVXDNSwnJzE1V0FUILkksKgGxQQrLM1KLUhHq7BQMirEI2ygYZ4CEi0tzcxOLMqtSFQpSi5KBlmXmpBZrwJTpKJjqKBgZACkgtgBiS1NNAEC7XiaYAAAA)**\]**

```Kusto
StormEvents
| extend duration = EndTime - StartTime
| where duration > 0s
| where duration < 3h
| summarize percentiles(duration, 5, 20, 50, 80, 95)
```

De volgende query percentielen berekend voor de duur van de storm per staat en de gegevens per vijf minuten durende opslaglocaties normaliseert (`5m`).

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1NSwrCMBTcC95hli1EKEpBQd31BHUvafOgAZNI8uIPD28SEBVcDDMM8%2bnZedNdyHKYz56gG5NVUNFL1s5ih86qgzaEBXqWnrPOwetEnj65PZrwx95iNWU7RGOk1w8C5avj6KLlNF64qjHcMWhbvXsCralFPmT6rZ%2fJj2lAnyh8pwWWTaKEdcKmLYul%2fgLODFs%2b4AAAAA%3d%3d)**\]**

```Kusto
StormEvents
| extend duration = EndTime - StartTime
| where duration > 0s
| where duration < 3h
| summarize event_count = count() by bin(duration, 5m), State
| summarize percentiles(duration, 5, 20, 50, 80, 95) by State
```

### <a name="cross-dataset"></a>Cross-gegevensset

Deze sectie worden de elementen waarmee u kunt complexere query's maken, gegevens samenvoegen in tabellen en query's naar andere databases en clusters.

### <a name="let"></a>Laat

[**Laat**](https://docs.microsoft.com/azure/kusto/query/letstatement): verbetert modulariteit mogelijk te maken en hergebruik. De **laten** instructie kunt u een complexe expressie op te splitsen in meerdere delen, elk gekoppeld aan een naam, en deze onderdelen samen opstellen. Een **laten** instructie kan ook worden gebruikt om de gebruiker gedefinieerde functies en weergaven (expressies over tabellen waarvan de resultaten een nieuwe tabel eruit) te maken. Expressies gebonden bent aan een **laten** instructie van een scalair type, van het type in tabelvorm of de gebruiker gedefinieerde functie (lambdas) kan zijn.

Het volgende voorbeeld maakt u een variabele in tabelvorm type en deze in een latere-expressie gebruikt.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAMtJLVHwyUzPKMnLzEsPLskvyi1WsOXlArNcy1LzSop5uWoUyjNSi1IVwPyQyoJUBVtbBSW4LiVrXq4coDGOZYk5iXnJGakkGQPXBTIGzSUgPVn5mXkKGmhmayrk5ykElySWpIKUpGQWl2TmJZdARACul3kY0gAAAA%3d%3d)**\]**

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

[**JOIN**](https://docs.microsoft.com/azure/kusto/query/joinoperator): de rijen van twee tabellen om te vormen een nieuwe tabel door de overeenkomende waarden van de opgegeven kolommen uit elke tabel samenvoegen. Kusto ondersteunt een breed scala aan join-typen: **fullouter**, **binnenste**, **innerunique**, **leftanti**, **leftantisemi** , **leftouter**, **leftsemi**, **rightanti**, **rightantisemi**, **rightouter** , **rightsemi**.

Het volgende voorbeeld worden twee tabellen gekoppeld met een inner join.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA8tJLVGIULBVSEksAcKknFQN79RKq+KSosy8dB2FsMSc0lRDq5z8vHRNXq5oXi4FIFBPVNcx1IGyk9R1jJDYxjB2srqOCS9XrDUvVw7Qhkj8Nhih2wA0ydAAySgjZI4xnJMCtMQAYkuEQo1CVn5mnkJ2Zl6KbWZeXmoR0Nb8PAWgZQAFPLdO5AAAAA==)**\]**

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
> Gebruik **waar** en **project** operatoren voor het verminderen van het aantal rijen en kolommen in de invoer tabellen, voordat de join. Als een tabel altijd kleiner zijn dan de andere is, kunt u deze als aan de linkerkant (piped) van de join gebruiken. De kolommen voor de join-overeenkomst moeten dezelfde naam hebben. Gebruik de **project** operator indien nodig om een kolom in een van de tabellen te wijzigen.

### <a name="serialize"></a>serialiseren

[**serialiseren**](https://docs.microsoft.com/azure/kusto/query/serializeoperator): de rij instellen, zodat u kunt functies waarvoor geserialiseerde gegevens, zoals serialiseert **row_number()**.

De volgende query is gelukt, omdat de gegevens is geserialiseerd.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIzi%2fNK9HQVEiqVAguSSxJBcumFmUm5gBlQZzUipLUvBSFovzy%2bLzS3KTUIgVbJI6GJgB4pV4NWgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| summarize count() by State
| serialize
| extend row_number = row_number()
```

De rijenset wordt ook beschouwd als geserialiseerd als deze een resultaat van is: **sorteren**, **boven**, of **bereik** operators, eventueel gevolgd door **project**, **project opslag**, **uitbreiden**, **waar**, **parseren**, **mvexpand**, of **nemen** operators.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIzi%2fNK9HQVEiqVAguSSxJBcvmF5XABRQSi5NBgqkVJal5KQpF%2beXxeaW5SalFCrZIHA1NAEGimf5iAAAA)**\]**

```Kusto
StormEvents
| summarize count() by State
| sort by State asc
| extend row_number = row_number()
```

### <a name="cross-database-and-cross-cluster-queries"></a>Query's tussen databases en cross-cluster

[Query's tussen databases en cross-cluster](https://docs.microsoft.com/azure/kusto/query/cross-cluster-or-database-queries): U kunt een database in hetzelfde cluster opvragen door te verwijzen als `database("MyDatabase").MyTable`. U kunt een database op een extern cluster opvragen door ernaar te verwijzen als `cluster("MyCluster").database("MyDatabase").MyTable`.

De volgende query wordt aangeroepen vanuit een cluster en gegevens opvragen uit `MyCluster` cluster. Als u wilt deze query uitvoert, moet u uw eigen clusternaam en databasenaam gebruiken.

```Kusto
cluster("MyCluster").database("Wiki").PageViews
| where Views < 2000
| take 1000;
```

### <a name="user-analytics"></a>Analyse van gebruikers

Deze sectie bevat de elementen en query's die laten hoe eenvoudig het is zien voor het uitvoeren van de analyse van gebruikersgedrag in Kusto.

### <a name="activitycountsmetrics-plugin"></a>activity_counts_metrics-invoegtoepassing

[**de invoegtoepassing activity_counts_metrics**](https://docs.microsoft.com/azure/kusto/query/activity-counts-metrics-plugin): berekent de metrische gegevens van nuttige activiteiten (totale aantal waarden, uniek aantal waarden, uniek aantal van de nieuwe waarden en samengevoegde uniek aantal). Metrische gegevens worden berekend voor elke tijdvenster en ze zijn vergeleken, en samengevoegd op en met alle voorgaande tijdvensters.

De volgende query analyseert acceptatie van de gebruiker door het berekenen van de dagelijkse activiteit telt.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAJXSPQvCMBAG4L3Q%2f5CtFlLoFyiVDn4M6mqdREpsggTaKs1VEfzxXm0LDiEimcJz3CW8VwogClgDKWcgQFZiEvrB1PNnnh%2b4c9sqsUDUXMPxyA9Z8%2bsjDfhwz0hKsBzPuRSTgxLNlicKGllfKMmwBw6sbsnY0bWto205C4cS3Rso2tpgO4MtDbbSWvixzGD6eb1ttBYZev42%2fbzI8L%2fe9n9b3NkJQ8xs60XEnZUt1hBWgLxLeObFta1B5ZXAKAs1BPuVKO03iXb7gp36tXDfExVB%2f2ICAAA%3d)**\]**

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

### <a name="activityengagement-plugin"></a>activity_engagement-invoegtoepassing

[**de invoegtoepassing activity_engagement**](https://docs.microsoft.com/azure/kusto/query/activity-engagement-plugin): berekent op basis van de kolom-ID gedurende een sliding window van de tijdlijn van activiteit engagement verhouding. **de invoegtoepassing activity_engagement** kan worden gebruikt voor het berekenen van dagelijks actieve gebruikers, WAU en MAU (dagelijkse, wekelijkse en maandelijkse actieve gebruikers).

De volgende query retourneert de verhouding van het totale aantal unieke gebruikers met behulp van een toepassing per dag in vergelijking met het totale aantal unieke gebruikers met behulp van de toepassing wekelijks, op een Verschuivend van zeven dagen.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1RQWrDMBC8G%2fyHvUVOHGy1lByKD6GBviDkUIoR1tpVsS0jr0MCeXxXiigpVAiBVjOzM6uigHcc0SlCcGrUdgCtSIFtYZnRgWrInA0ZnNOkR4J6JuUIKo9CMgOKp1LutqXknb1GDI76P8RzQHCXDqHW6gqt43ZRkeydNxNOIHWa3AAv5Ctei2xvx06IQNtGTlZInT0AHQN9BpFt5EO59kHmKvQVUUivX8q1y3L4c9%2fIks%2bt5LoMwsMZLxMrgtHVXcb7pOuEthWemEFvBkPARL%2fSpCjgTfXN0vuBHvbH4rQ%2fsikyNjg6q37xL3GsV47cqQ4HHEl8rIxefeZhNHmMmIehsB2dp8nunnZy9hsbiriDWuqTWqpfxdBsLb2ZGzhm8y%2f6b2i%2bWO8HLFcMGe8BAAA%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-01-01);
let _end = datetime(2017-01-31);
range _day from _start to _end step 1d
| extend d = tolong((_day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+100*r-1), 1)
| mvexpand id=_users to typeof(long) limit 1000000
// Calculate DAU/WAU ratio
| evaluate activity_engagement(['id'], _day, _start, _end, 1d, 7d)
| project _day, Dau_Wau=activity_ratio*100
| render timechart
```

> [!TIP]
> Bij het berekenen van dagelijks actieve gebruikers/MAU, gegevens van de end en de zwevend venster periode (OuterActivityWindow) wijzigen.

### <a name="activitymetrics-plugin"></a>activity_metrics-invoegtoepassing

[**de invoegtoepassing activity_metrics**](https://docs.microsoft.com/azure/kusto/query/activity-metrics-plugin): nuttig metrische gegevens van activiteiten (uniek aantal waarden, uniek aantal van de nieuwe waarden, Retentiepercentage en verloop) op basis van de huidige periode venster vergeleken met de vorige periode venster berekent.

De volgende query berekent het verloop en retentie-tarief voor een bepaalde gegevensset.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG2SwW7CMAyG70i8g2%2bk0KoNE%2bIwscsOe4hpqqLGQFjaVKkLVNrDLw7RxjRyqBTr%2fz%2f3t1OW8IYdekUIXnXataAVKXB7GAf0oBoyZ0MGh%2fnMIkE9kPIEO1YhmRbFupLbopJFtc6ekwY7%2fV%2bxKZ4kK0KXA0Kt1QR7H9olIrmbbyDsQer57AvwSlxhFjnruoMQ0VYkT1ZKnd0JfRByBpGt5F255iDDLvYVCaSXm2rpsxz%2b3FfrKnwLGeoygtszXvtABKN3Nwz%2fJ009ur1gYwbWtIZAVvGw53JEn%2fK9PJwSi3rvTthQlOWBPp%2bVJbwq24yWN3FB%2fLQTeAwByLgOeD8x0lnZkRVpL1PdInnTDOJ9YfTiI0%2fE24DyONIctvpB0x94zfBlSJBDcxz97509PgDCM%2bAMzTEgvwEO44wSMAIAAA%3d%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-01-02);
let _end = datetime(2017-05-31);
range _day from _start to _end step 1d
| extend d = tolong((_day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+200*r-1), 1)
| mvexpand id=_users to typeof(long) limit 1000000
| where _day > datetime(2017-01-02)
| project _day, id
// Calculate weekly retention rate
| evaluate activity_metrics(['id'], _day, _start, _end, 7d)
| project _day, retention_rate*100, churn_rate*100
| render timechart
```

### <a name="newactivitymetrics-plugin"></a>new_activity_metrics-invoegtoepassing

[**de invoegtoepassing new_activity_metrics**](https://docs.microsoft.com/azure/kusto/query/new-activity-metrics-plugin): berekent de metrische gegevens van nuttige activiteiten (uniek aantal waarden, uniek aantal van de nieuwe waarden, Retentiepercentage en verloop) voor het cohort van nieuwe gebruikers. Het concept van deze invoegtoepassing is vergelijkbaar met [ **activity_metrics invoegtoepassing**](https://docs.microsoft.com/azure/kusto/query/activity-metrics-plugin), maar is gericht op nieuwe gebruikers.

De volgende query wordt een tarief retentie en het verloop met een week-over-week-venster voor het gebruikerscohort voor nieuwe (gebruikers dat is ontvangen op de eerste week) berekend.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1Ry27DIBC8W%2fI%2f7C04wbJJFeVQ5VapP9BbVVnIrGMaGyy8eVjqxxcwh1QqBx7LzCwzVBW8o0EnCcFJo%2bwISpIE28F1RgeyJX3TpHHOswEJmpmkIzgFFJIeke1rcSzrQ1mL4jVh0Kj%2fEC8R4bucEd7kAp3z3ZIg2ZU2E04gVJ79AD4oVIIU2cGaM2OBVSZKUQlVPOGcxwUHrNiJp3ITbMyn2JUlHbU91FtXcPhz3u1rP5fC10UUHm%2f4mLwiaHVaZcIzaZnQdiwQCxj0qAlEHUeeVRV8yAuCNcMC1CN02s0Ed8QLtLa33igbpK9M0skRCd3q4CaHa%2fgBg%2fcmJb40%2ft7pdmafG602XzxExpN3HsPicFQ8z1IcQWhy9htbisk2EU92XZ1vZkhb04Sv5tD2V7fufwFYtolnAgIAAA%3d%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-05-01);
let _end = datetime(2017-05-31);
range Day from _start to _end step 1d
| extend d = tolong((Day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+200*r-1), 1)
| mvexpand id=_users to typeof(long) limit 1000000
// Take only the first week cohort (last parameter)
| evaluate new_activity_metrics(['id'], Day, _start, _end, 7d, _start)
| project from_Day, to_Day, retention_rate, churn_rate
```

### <a name="sessioncount-plugin"></a>session_count-invoegtoepassing

[**de invoegtoepassing session_count**](https://docs.microsoft.com/azure/kusto/query/session-count-plugin): berekent het aantal sessies dat is gebaseerd op de kolom-ID voor een tijdlijn.

De volgende query retourneert het aantal sessies. Een sessie wordt beschouwd als actief als een gebruikers-ID wordt weergegeven ten minste één keer op een tijdsbestek van 100-time-sleuven, terwijl de sessie uiterlijk-back-venster 41-time-sleuven is.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWPQYvCQAyF74X%2bh3dZUCjYgfUgMkcP3r2XoZPqaM3INK4u7I%2ffzOwiNQRC8pKPl5EEnXfiYJEcHwmHcKUxMGFI8QoDidhoYBK6wdTVD%2bgpxB5dd6FvPSuzcwyMS2BvAzMlLP5gez%2fDrNt%2fCN4Z1iwRua5Kk2GPE6WZkY%2bMsRZt1m4pnqmXl9qouK2r1Qo75cUB5RlPQ%2bAgoWDzpPj%2bcuPdCWGiaVKp6%2bOdZbH3zYxmNFuNUhp8mmU%2bTWpWv8or%2fckl%2bQXutT48NwEAAA%3d%3d)**\]**

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

### <a name="funnelsequence-plugin"></a>funnel_sequence-invoegtoepassing

[**de invoegtoepassing funnel_sequence**](https://docs.microsoft.com/azure/kusto/query/funnel-sequence-plugin): berekent de tellingen van gebruikers die een reeks statussen hebben genomen; de verdeling van vorige en volgende statussen die hebben geleid tot of zijn gevolgd door de takenreeks weergeeft.

De volgende query laat zien welke gebeurtenis plaatsvindt vóór en na alle Tornado gebeurtenissen in 2007.

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAGWOPYvCQBCG%2b0D%2bw3QmEEmieIqNVQrBRgxYiMhcdqKLyWzcnQiCP95V70DuYIrh5Xk%2f0hRWxpw1H8EwbMTYtrgSiwMnKNqJrtw8DNIU1vkcticUOGHXETv4ptpYgtJYRmWAnrbFGx39QbEWsv%2fIj7YwuHsZmx6FoO6ZqTk4uvTEFUVFp51RtFSJH4hWSt1SAsqj4r9olGXTYZb7i5Mw%2bJRnvzLkKhl%2fTXzAq668dc%2bAG2Orq2g3%2bBk22MfxA23MLGQQAQAA)**\]**

```Kusto
// Looking on StormEvents statistics:
// Q1: What happens before Tornado event?
// Q2: What happens after Tornado event?
StormEvents
| evaluate funnel_sequence(EpisodeId, StartTime, datetime(2007-01-01), datetime(2008-01-01), 1d,365d, EventType, dynamic(['Tornado']))
```

### <a name="funnelsequencecompletion-plugin"></a>funnel_sequence_completion-invoegtoepassing

[**de invoegtoepassing funnel_sequence_completion**](https://docs.microsoft.com/azure/kusto/query/funnel-sequence-completion-plugin): berekent de trechter van voltooide reeks stappen binnen verschillende perioden.

De volgende query controleert de trechter voltooiing van de reeks: `Hail -> Tornado -> Thunderstorm -> Wind` in 'algemene' duur van één uur, vier uur en één dag (`[1h, 4h, 1d]`).

**\[**[**Klik op query uit te voeren**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA12QTYvCMBCG74L/YW6tkIV2XT9g8SjsnlvwICKhM9JAOqlJqrj4402CW0RIIB/PPLwzmjwcnZfWwwZQevKqo/yzKFYfRRnW7Hs60ZEhxjdi/UZcFaO5VuqPAjhfLvD/w9F5IG7iM95YdqrJ99mPVDoTkNXGskSTju3ASNZ5Y7t43wVhdhj9PVll0L1aylbAV9glJqyKldsLsXfTyR3oIvUQAsNpYCY95jg2puuDUhnOt71yBukXBVRxCnVoTjwnIlLX4rUzAUlf3/pEPYViDDd7AOyqowFQAQAA)**\]**

```Kusto
let _start = datetime(2007-01-01);
let _end = datetime(2008-01-01);
let _windowSize = 365d;
let _sequence = dynamic(['Hail', 'Tornado', 'Thunderstorm', 'Wind']);
let _periods = dynamic([1h, 4h, 1d]);
StormEvents
| evaluate funnel_sequence_completion(EpisodeId, StartTime, _start, _end, _windowSize, EventType, _sequence, _periods)
```

## <a name="functions"></a>Functions

In deze sectie bevat informatie over [ **functies**](https://docs.microsoft.com/azure/kusto/query/functions): herbruikbare query's die zijn opgeslagen op de server. Functies kunnen worden aangeroepen door query's en andere functies (recursive-functies worden niet ondersteund).

> [!NOTE]
> U kunt geen functies maken op het helpcluster, dit alleen-lezen is. Uw eigen testcluster gebruiken voor dit onderdeel.

Het volgende voorbeeld wordt een functie die met de naam van een provincie (`MyState`) als argument.

```Kusto
.create function with (folder="Demo")
MyFunction (MyState: string)
{
StormEvents
| where State =~ MyState
}
```

Het volgende voorbeeld wordt een functie waarmee gegevens worden opgehaald voor de status van Texas.

```Kusto
MyFunction ("Texas")
| summarize count()
```

Het volgende voorbeeld wordt de functie die in de eerste stap is gemaakt.

```Kusto
.drop function MyFunction
```

## <a name="next-steps"></a>Volgende stappen

[Kusto-Query Language reference](https://aka.ms/kustolangref)
