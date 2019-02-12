---
title: Parseren van tekstgegevens in Logboeken van Azure Monitor | Microsoft Docs
description: Beschrijving van verschillende opties voor het parseren van logboekgegevens in Azure Monitor-records wanneer de gegevens worden opgenomen en wanneer deze in een vergelijking van de relatieve voordelen voor elke query wordt opgehaald.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/04/2018
ms.author: bwren
ms.openlocfilehash: b6a2ca70faa36b94ace8158f33e58b5e6688ece3
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56002188"
---
# <a name="parse-text-data-in-azure-monitor-logs"></a>Tekstgegevens in Azure Monitor logboeken parseren
Sommige logboekgegevens die zijn verzameld door Azure Monitor bevat meerdere delen van gegevens in één eigenschap. Het parseren van deze gegevens in meerdere eigenschappen maken het gemakkelijker om te gebruiken in query's. Een veelvoorkomend voorbeeld is een [aangepast logboek](../../log-analytics/log-analytics-data-sources-custom-logs.md) die een hele logboekvermelding met meerdere waarden worden verzameld in één eigenschap. Door het maken van afzonderlijke eigenschappen voor de verschillende waarden, kunt u zoeken en op elk cumulatieve.

Dit artikel beschrijft de verschillende opties voor het parseren van logboekgegevens in Azure Monitor wanneer de gegevens worden opgenomen en wanneer deze in een vergelijking van de relatieve voordelen voor elke query wordt opgehaald.


## <a name="parsing-methods"></a>Methoden voor het parseren
U kunt gegevens parseren op moment van opname wanneer de gegevens worden verzameld of bij query uptijd bij het analyseren van de gegevens met een query. Elke strategie biedt unieke voordelen, zoals hieronder wordt beschreven.

### <a name="parse-data-at-collection-time"></a>Gegevens verzamelen tijdens parseren
Wanneer u gegevens tijdens het verzamelen parseren, configureert u [aangepaste velden](../../log-analytics/log-analytics-custom-fields.md) die nieuwe eigenschappen maken in de tabel. Query's hoeft te bevatten geen parseren logica en deze eigenschappen gewoon gebruiken als elk ander veld in de tabel.

Voordelen van deze methode zijn:

- Gemakkelijker te query die de verzamelde gegevens omdat u niet nodig parseren opdrachten in de query.
- Betere prestaties van query's omdat de query niet nodig om uit te voeren bij het parseren.
 
Deze methode nadelen omvatten het volgende:

- Moet vooraf zijn gedefinieerd. Kan geen bevatten gegevens die al zijn verzameld.
- Als u de parseren logica wijzigt, wordt deze alleen van toepassing op nieuwe gegevens.
- Minder opties voor het parseren dan beschikbaar in de query's.
- Verhoogt de tijd van de latentie voor het verzamelen van gegevens.
- Fouten kunnen lastig zijn om af te handelen.


### <a name="parse-data-at-query-time"></a>Parseren van gegevens op het moment dat de query
Wanneer u gegevens bij uitvoeren van query's parseren, kunt u logische opnemen in uw query voor het parseren van gegevens naar meerdere velden. De werkelijke tabel zelf wordt niet gewijzigd.

Voordelen van deze methode zijn:

- Geldt voor elk soort gegevens, inclusief de gegevens die al zijn verzameld.
- Wijzigingen in de logische kunnen direct worden toegepast op alle gegevens.
- Flexibele opties, waaronder vooraf gedefinieerde logica voor bepaalde gegevensstructuren te parseren.
 
Deze methode nadelen omvatten het volgende:

- Vereist meer complexe query's. Dit kan worden verholpen door met behulp van [functies voor het simuleren van een tabel](#Use-function-to-simulate-a-table).
- Bij het parseren van de logica in meerdere query's moeten worden gerepliceerd. Kan bepaalde logica via functies delen.
- Kunt maken overhead bij het uitvoeren van complexe logica op zeer grote record (miljarden records) ingesteld.

## <a name="parse-data-as-its-collected"></a>Gegevens parseren als deze verzameld
Zie [maken van aangepaste velden in Azure Monitor](../platform/custom-fields.md) voor meer informatie over de gegevens worden geparseerd als deze verzameld. Hiermee maakt u aangepaste eigenschappen in de tabel die kan worden gebruikt door query's net als bij een andere eigenschap.

## <a name="parse-data-in-query-using-patterns"></a>Parseren van de gegevens in de query met behulp van patronen
Als de gegevens die u wilt parseren kan worden aangeduid met een patroon herhaald totdat de records, kunt u verschillende operators in de [Data Explorer-querytaal](/azure/kusto/query/) om op te halen van het specifieke gedeelte van de gegevens in een of meer nieuwe eigenschappen.

### <a name="simple-text-patterns"></a>Eenvoudige patronen

Gebruik de [parseren](/azure/kusto/query/parseoperator) -operator in uw query te maken van een of meer aangepaste eigenschappen die kunnen worden geëxtraheerd uit een tekenreeksexpressie. Geeft u het patroon kan worden geïdentificeerd en de namen van de eigenschappen te maken. Dit is vooral handig voor gegevens met sleutel / waarde-tekenreeksen met een formulier die vergelijkbaar is met _sleutel = waarde_.

Houd rekening met een aangepast logboek met gegevens in de volgende indeling.

```
Time=2018-03-10 01:34:36 Event Code=207 Status=Success Message=Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
Time=2018-03-10 01:33:33 Event Code=208 Status=Warning Message=Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
Time=2018-03-10 01:35:44 Event Code=209 Status=Success Message=Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
Time=2018-03-10 01:38:22 Event Code=302 Status=Error Message=Application could not connect to database
Time=2018-03-10 01:31:34 Event Code=303 Status=Error Message=Application lost connection to database
```

De volgende query zou deze gegevens parseren naar afzonderlijke eigenschappen. De regel met _project_ wordt toegevoegd aan alleen geretourneerd van de berekende eigenschappen en niet _RawData_, waarbij de één eigenschap met de volledige vermelding van het aangepaste logboek is.

```Kusto
MyCustomLog_CL
| parse RawData with * "Time=" EventTime " Event Code=" Code " Status=" Status " Message=" Message
| project EventTime, Code, Status, Message
```

Hieronder volgt een ander voorbeeld waarin de gebruikersnaam van een UPN in deelt de _AzureActivity_ tabel.

```Kusto
AzureActivity
| parse  Caller with UPNUserPart "@" * 
| where UPNUserPart != "" //Remove non UPN callers (apps, SPNs, etc)
| distinct UPNUserPart, Caller
```


### <a name="regular-expressions"></a>Reguliere expressies
Als uw gegevens kunnen worden geïdentificeerd met een reguliere expressie, kunt u [functies die gebruikmaken van reguliere expressies](/azure/kusto/query/re2) naar afzonderlijke waarden ophalen. Het volgende voorbeeld wordt [extraheren](/azure/kusto/query/extractfunction) om te lichten, de _UPN_ veld _AzureActivity_ registreert en ga vervolgens terug afzonderlijke gebruikers.

```Kusto
AzureActivity
| extend UPNUserPart = extract("([a-z.]*)@", 1, Caller) 
| distinct UPNUserPart, Caller
```

Om in te schakelen efficiënte parseren op grote schaal, Azure Monitor maakt gebruik van re2 versie van reguliere expressies, dit is vergelijkbaar, maar niet identiek zijn aan enkele van de andere varianten van de reguliere expressie. Raadpleeg de [re2-expressiesyntaxis](https://aka.ms/kql_re2syntax) voor meer informatie.


## <a name="parse-delimited-data-in-a-query"></a>In een query met scheidingstekens-gegevens parseren
Gegevens met scheidingstekens gescheiden velden met een algemene teken, zoals een komma in een CSV-bestand. Gebruik de [splitsen](/azure/kusto/query/splitfunction) functie parseren gescheiden gegevens met een scheidingsteken die u opgeeft. U kunt dit met [uitbreiden](/azure/kusto/query/extendoperator) operator om te retourneren van alle velden in de gegevens of om op te geven van afzonderlijke velden moeten worden opgenomen in de uitvoer.

> [!NOTE]
> Aangezien split resulteert in een dynamische object, moet de resultaten kunnen expliciet worden geconverteerd naar gegevenstypen, zoals tekenreeks moet worden gebruikt in de operators en filters.

Houd rekening met een aangepast logboek met gegevens in de volgende CSV-indeling.

```
2018-03-10 01:34:36, 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
2018-03-10 01:33:33, 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
2018-03-10 01:35:44, 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
2018-03-10 01:38:22, 302,Error,Application could not connect to database
2018-03-10 01:31:34, 303,Error,Application lost connection to database
```

De volgende query zou deze gegevens parseren en samen te vatten door twee van de berekende eigenschappen. De eerste regel splitst de _RawData_ eigenschap in een string-matrix. Elk van de volgende regels geeft een naam aan afzonderlijke eigenschappen en voegt deze toe aan de uitvoer met behulp van functions ze kunt converteren naar het juiste gegevenstype.

```Kusto
MyCustomCSVLog_CL
| extend CSVFields  = split(RawData, ',')
| extend EventTime  = todatetime(CSVFields[0])
| extend Code       = toint(CSVFields[1]) 
| extend Status     = tostring(CSVFields[2]) 
| extend Message    = tostring(CSVFields[3]) 
| where getyear(EventTime) == 2018
| summarize count() by Status,Code
```

## <a name="parse-predefined-structures-in-a-query"></a>Parseren van vooraf gedefinieerde structuur in een query
Als uw gegevens zijn opgemaakt in een bekende structuur, u mogelijk gebruikt u een van de functies in de [Data Explorer-querytaal](/azure/kusto/query/) voor het parseren van vooraf gedefinieerde structuren:

- [JSON](/azure/kusto/query/parsejsonfunction)
- [XML](/azure/kusto/query/parse-xmlfunction)
- [IPv4](/azure/kusto/query/parse-ipv4function)
- [URL](/azure/kusto/query/parseurlfunction)
- [URL-query](/azure/kusto/query/parseurlqueryfunction)
- [Bestandspad](/azure/kusto/query/parsepathfunction)
- [Gebruikersagent](/azure/kusto/query/parse-useragentfunction)
- [Versietekenreeks](/azure/kusto/query/parse-versionfunction)

De volgende voorbeeldquery parseert de _eigenschappen_ veld van de _AzureActivity_ tabel, die is opgebouwd in JSON. De resultaten worden opgeslagen op een dynamische eigenschap genaamd _parsedProp_, waaronder de persoon die de benoemde waarde in de JSON. Deze waarden worden gebruikt om te filteren en resultaten van de query samen te vatten.

```Kusto
AzureActivity
| extend parsedProp = parse_json(Properties) 
| where parsedProp.isComplianceCheck == "True" 
| summarize count() by ResourceGroup, tostring(parsedProp.tags.businessowner)
```

Deze functies bij het parseren is processorintensief, zodat deze alleen als uw query maakt gebruik van meerdere eigenschappen van de opgemaakte gegevens moeten worden gebruikt. Eenvoudig gebruik van jokertekens verwerking wordt anders worden sneller.

Het volgende voorbeeld ziet de verdeling van de domeincontroller TGT Voorverificatie type. Het type bestaat alleen in het veld EventData, dit een XML-tekenreeks is, maar geen andere gegevens van dit veld is vereist. In dit geval [parseren](/azure/kusto/query/parseoperator) wordt gebruikt voor het de vereiste hoeveelheid gegevens te kiezen.

```Kusto
SecurityEvent
| where EventID == 4768
| parse EventData with * 'PreAuthType">' PreAuthType '</Data>' * 
| summarize count() by PreAuthType
```

## <a name="use-function-to-simulate-a-table"></a>Gebruik de functie voor het simuleren van een tabel
Mogelijk hebt u meerdere query's die de dezelfde parseren van een bepaalde tabel uitvoeren. In dit geval [maken van een functie](functions.md) die de geparseerde gegevens in plaats van het repliceren van de parseren logica in elke query retourneert. U kunt de functiealias in plaats van de oorspronkelijke tabel vervolgens in andere query's gebruiken.

Houd rekening met het bovenstaande voorbeeld van het aangepaste logboek met door komma's gescheiden. Een functie maken met de volgende query uit om te kunnen gebruiken de geparseerde gegevens in meerdere query's, en sla het op met de alias _MyCustomCSVLog_.

```Kusto
MyCustomCSVLog_CL
| extend CSVFields = split(RawData, ',')
| extend DateTime  = tostring(CSVFields[0])
| extend Code      = toint(CSVFields[1]) 
| extend Status    = tostring(CSVFields[2]) 
| extend Message   = tostring(CSVFields[3]) 
```

U kunt nu de alias _MyCustomCSVLog_ in plaats van de naam van de werkelijke tabel in query's als volgt uit.

```Kusto
MyCustomCSVLog
| summarize count() by Status,Code
```


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [query's bijgehouden](log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.