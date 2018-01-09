---
title: Inzicht in de taal van de query Azure IoT Hub | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijving van de SQL-achtige IoT Hub-querytaal gebruikt voor het ophalen van informatie over apparaat horende taken uit uw IoT-hub.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: elioda
ms.openlocfilehash: 450f2d38f7b641bcf6b8be061969404a1b582b4c
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a>IoT Hub-querytaal voor apparaat horende, taken en het routeren van berichten

IoT Hub biedt een krachtige SQL-achtige taal voor het ophalen van informatie met betrekking tot [apparaat horende] [ lnk-twins] en [taken][lnk-jobs], en [berichtroutering][lnk-devguide-messaging-routes]. Dit artikel biedt:

* Een inleiding tot de belangrijkste functies van de querytaal IoT Hub en
* De gedetailleerde beschrijving van de taal.

## <a name="device-twin-queries"></a>Apparaat twin query 's
[Apparaat horende] [ lnk-twins] kan willekeurige JSON-objecten bevatten als zowel labels en eigenschappen. IoT-Hub kunt u query apparaat horende als een enkele JSON-document met alle apparaten twin informatie.
Stel bijvoorbeeld dat uw IoT hub apparaat horende de volgende structuur hebben:

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "status": "enabled",
    "statusUpdateTime": "0001-01-01T00:00:00",    
    "connectionState": "Disconnected",    
    "lastActivityTime": "0001-01-01T00:00:00",
    "cloudToDeviceMessageCount": 0,
    "authenticationType": "sas",    
    "x509Thumbprint": {    
        "primaryThumbprint": null,
        "secondaryThumbprint": null
    },
    "version": 2,
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

IoT-Hub toont de horende apparaten als een documentverzameling aangeroepen **apparaten**.
De volgende query haalt dus de hele set horende apparaten:

```sql
SELECT * FROM devices
```

> [!NOTE]
> [Azure IoT SDK's] [ lnk-hub-sdks] ondersteuning voor paginering van veel resultaten.

IoT-Hub kunt u voor het ophalen van apparaat horende filteren met willekeurig voorwaarden. Bijvoorbeeld: voor het ontvangen van apparaat horende waar de **location.region** label is ingesteld op **VS** gebruik de volgende query:

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

Booleaanse operators en rekenkundige vergelijkingen worden ook ondersteund. Voor het ophalen van apparaat gebruik horende bevinden zich in de Verenigde Staten en geconfigureerd voor het verzenden van telemetrie minder dan elke minuut bijvoorbeeld de volgende query:

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

Voor uw gemak het is ook mogelijk om te gebruiken matrixconstanten met de **IN** en **NIN** (niet-in) operators. Bijvoorbeeld gebruik horende apparaten die Wi-Fi- of bekabelde verbindingen rapporteren voor het ophalen van de volgende query:

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

Vaak is het nodig zijn voor het identificeren van alle apparaat horende die een bepaalde eigenschap bevatten. IoT Hub biedt ondersteuning voor de functie `is_defined()` voor dit doel. Voor het exemplaar naar ophalen apparaat horende waarmee de `connectivity` eigenschap gebruik de volgende query:

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

Raadpleeg de [WHERE-component] [ lnk-query-where] sectie voor de volledige verwijzing van de filtermogelijkheden.

Groepering en aggregaties worden ook ondersteund. Om het aantal apparaten in elke telemetrie gebruik configuratiestatus bijvoorbeeld de volgende query:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

Deze groepering query zou een resultaat te retourneren vergelijkbaar met het volgende voorbeeld. Hier drie apparaten rapporteren geslaagde configuratie, twee nog steeds toepassen van de configuratie en een heeft een fout gerapporteerd. 

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

Projectie van query's kunnen ontwikkelaars om alleen de eigenschappen die ze interesseren te retourneren. Bijvoorbeeld, verbroken om op te halen van de laatste activiteitstijd van alle apparaten gebruik de volgende query:

```sql
SELECT LastActivityTime FROM devices WHERE status = 'enabled'
```

### <a name="c-example"></a>C#-voorbeeld
De queryfunctionaliteit die wordt blootgelegd door de [C# service SDK] [ lnk-hub-sdks] in de **RegistryManager** klasse.
Hier volgt een voorbeeld van een eenvoudige query:

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

Opmerking hoe de **query** object is gemaakt met een paginagrootte (maximaal 100) en vervolgens meerdere pagina's kunnen worden opgehaald door het aanroepen van de **GetNextAsTwinAsync** methoden meerdere keren.
Opmerking dat het queryobject meerdere bevat **volgende***, afhankelijk van de deserialisatie-optie die door de query, zoals apparaatobjecten twin of de taak, of gewoon JSON vereist om te worden gebruikt als u projecties.

### <a name="nodejs-example"></a>Node.js-voorbeeld
De queryfunctionaliteit die wordt blootgelegd door de [Azure IoT service SDK voor Node.js] [ lnk-hub-sdks] in de **register** object.
Hier volgt een voorbeeld van een eenvoudige query:

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed to fetch the results: ' + err.message);
    } else {
        // Do something with the results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

Opmerking hoe de **query** object is gemaakt met een paginagrootte (maximaal 100) en vervolgens meerdere pagina's kunnen worden opgehaald door het aanroepen van de **nextAsTwin** methoden meerdere keren.
Opmerking dat het queryobject meerdere bevat **volgende***, afhankelijk van de deserialisatie-optie die door de query, zoals apparaatobjecten twin of de taak, of gewoon JSON vereist om te worden gebruikt als u projecties.

### <a name="limitations"></a>Beperkingen
> [!IMPORTANT]
> Apparaat horende queryresultaten, kunnen een paar minuten vertraging met betrekking tot de meest recente waarden bevatten. Als het uitvoeren van query's afzonderlijk apparaat horende door-id, is het altijd beter om de ophalen apparaat twin API, die altijd de meest recente waarden bevat en is beperking hoger limieten gebruiken.

Op dit moment vergelijkingen bijvoorbeeld alleen tussen primitieve typen (geen objecten) worden ondersteund `... WHERE properties.desired.config = properties.reported.config` wordt alleen ondersteund als deze eigenschappen primitieve waarden hebben.

## <a name="get-started-with-jobs-queries"></a>Aan de slag met taken query 's
[Taken] [ lnk-jobs] bieden een manier om het uitvoeren van bewerkingen op sets van apparaten. Elk apparaat twin bevat de informatie van de taken die het onderdeel in een verzameling met de naam is **taken**.
Logisch,

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

Deze verzameling is momenteel als waarop **devices.jobs** querytaal van de IoT-Hub.

> [!IMPORTANT]
> De eigenschap taken wordt op dit moment nooit geretourneerd tijdens het opvragen van apparaat horende (dat wil zeggen, query's met ' apparaten'). Kan alleen worden benaderd rechtstreeks met query's met `FROM devices.jobs`.
>
>

Bijvoorbeeld, als u alle taken (afgelopen en geplande) die invloed hebben op één apparaat, kunt u de volgende query:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

Houd er rekening mee hoe deze query bevat de status van de apparaat-specifieke (en mogelijk het antwoord directe methode) van elke taak geretourneerd.
Het is ook mogelijk om te filteren met willekeurig Booleaanse voorwaarden op alle eigenschappen van het object in de **devices.jobs** verzameling.
Gebruik bijvoorbeeld de volgende query voor het ophalen van alle voltooide apparaat twin update taken die zijn gemaakt na September 2016 voor een specifiek apparaat:

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

U kunt ook de resultaten per apparaat van een enkele taak ophalen.

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a>Beperkingen
Op dit moment wordt een query op **devices.jobs** bieden geen ondersteuning voor:

* Projecties, daarom alleen `SELECT *` is mogelijk.
* De voorwaarden die naar het apparaat twin naast taakeigenschappen verwijzen (Zie de vorige sectie).
* Uitvoeren van aggregaties, zoals count, avg, groeperen op.

## <a name="device-to-cloud-message-routes-query-expressions"></a>Apparaat-naar-cloud berichtroutes query-expressies

Met behulp van [apparaat-naar-cloud routes][lnk-devguide-messaging-routes], kunt u IoT Hub apparaat-naar-cloud-berichten naar verschillende eindpunten op basis van expressies geëvalueerd op basis van afzonderlijke berichten verzenden.

De route [voorwaarde] [ lnk-query-expressions] de dezelfde querytaal van IoT Hub gebruikt als voorwaarden in twin en taak query's. Route voorwaarden worden geëvalueerd op het berichtkoppen en de hoofdtekst. Uw routering query-expressie is mogelijk alleen berichtkoppen, alleen de hoofdtekst van het bericht of beide headers bericht en berichttekst. IoT Hub wordt ervan uitgegaan van een specifiek schema voor de kopteksten en de hoofdtekst van het bericht om te routeren van berichten. De volgende secties wordt beschreven wat is vereist voor de IoT-Hub te routeren correct.

### <a name="routing-on-message-headers"></a>Routering op berichtkoppen

IoT Hub wordt ervan uitgegaan dat de volgende JSON-weergave van berichtkoppen voor het routeren van berichten:

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

Bericht Systeemeigenschappen worden voorafgegaan door de `'$'` symbool.
Eigenschappen van de gebruiker worden altijd geopend met hun naam. Als de naam van een eigenschap gebeurt overeenkomen met een systeemeigenschap (zoals `$to`), de eigenschap wordt opgehaald met de `$to` expressie.
U kunt altijd toegang hebt tot de systeemeigenschap met behulp van haakjes `{}`: u kunt bijvoorbeeld de expressie gebruiken `{$to}` voor toegang tot de systeemeigenschap `to`. Eigenschapnamen ophalen altijd de bijbehorende systeemeigenschap.

Houd er rekening mee dat de namen van eigenschappen zijn niet hoofdlettergevoelig.

> [!NOTE]
> Alle berichteigenschappen zijn tekenreeksen. Eigenschappen, zoals beschreven in de [ontwikkelaarshandleiding][lnk-devguide-messaging-format], zijn momenteel niet beschikbaar voor gebruik in query's.
>

Als u bijvoorbeeld een `messageType` eigenschap, kunt u alle telemetrie op één eindpunt en alle waarschuwingen naar een ander eindpunt te routeren. U kunt de volgende expressie voor het routeren van de telemetrie schrijven:

```sql
messageType = 'telemetry'
```

En de expressie voor het routeren van de waarschuwing berichten in de volgende:

```sql
messageType = 'alert'
```

Booleaanse expressies en functies worden ook ondersteund. Deze functie kunt u bijvoorbeeld onderscheid maken tussen ernst:

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

Raadpleeg de [expressie en voorwaarden] [ lnk-query-expressions] sectie voor een volledige lijst van ondersteunde operatoren en functies.

### <a name="routing-on-message-bodies"></a>Routering op de berichttekst

IoT Hub kan alleen worden doorgestuurd op basis van de berichttekst inhoud als de berichttekst correct is gevormd JSON gecodeerd in UTF-8, UTF-16- of UTF-32. Instellen van het type inhoud van het bericht om `application/json` en de inhoudsversleuteling op een van de ondersteunde coderingen UTF in berichtkoppen. Als een van de headers niet is opgegeven, wordt niet IoT Hub proberen om te evalueren van een queryexpressie met betrekking tot de hoofdtekst tegen het bericht. Als uw bericht is niet een JSON-bericht, of als het bericht geeft niet het type inhoud en de codering van inhoud, kan u nog steeds berichtroutering gebruiken voor het routeren van het bericht op basis van de berichtkoppen.

U kunt `$body` in de query-expressie voor het routeren van het bericht. U kunt een eenvoudige hoofdtekst-verwijzing, hoofdtekst matrixverwijzing of meerdere hoofdtekst verwijzingen in de query-expressie. Uw query-expressie kunt ook een verwijzing instantie met een verwijzing van de header bericht combineren. De volgende zijn bijvoorbeeld alle geldige query-expressies:

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a>Basisprincipes van een IoT Hub-query
Elke IoT Hub-query bestaat van is geselecteerd en van componenten met optionele waar als de componenten GROUP BY. Elke query wordt uitgevoerd op een verzameling van JSON-documenten, bijvoorbeeld apparaat horende. De component FROM geeft aan dat de te worden herhaald op documentenverzameling (**apparaten** of **devices.jobs**). Klik wordt in de component WHERE gefilterd. Met aggregaties, worden de resultaten van deze stap gegroepeerd als opgegeven in de component GROUP BY en voor elke groep, een rij wordt gegenereerd zoals opgegeven in de component SELECT.

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a>FROM-component
De **van < from_specification >** component kunt wordt ervan uitgegaan dat slechts twee waarden: **van apparaten** naar query apparaat horende, of **van devices.jobs** voor de per apparaat taakdetails query.

## <a name="where-clause"></a>WHERE-component
De **waarbij < filter_condition >** component is optioneel. Hiermee geeft u een of meer voorwaarden dat de JSON-in de verzameling van documenten moeten voldoen om te worden opgenomen als onderdeel van het resultaat. De opgegeven voorwaarden op "true" moet worden opgenomen in het resultaat moet worden geëvalueerd door elk JSON-document.

De toegestane voorwaarden worden beschreven in de sectie [expressies en voorwaarden][lnk-query-expressions].

## <a name="select-clause"></a>SELECT-component
De **Selecteer < select_list >** is verplicht en Hiermee geeft u de waarden die worden opgehaald uit de query. Deze geeft de JSON-waarden moeten worden gebruikt voor het genereren van de nieuwe JSON-objecten.
De Projectiefase genereert voor elk element van de gefilterde (en desgewenst gegroepeerde) subset van de verzameling van een nieuwe JSON-object gemaakt met de opgegeven waarden in de component SELECT.

Hieronder vindt u de grammatica van de component SELECT:

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

**%{Attribute_name/** verwijst naar een eigenschap van het JSON-document in de verzameling FROM. Enkele voorbeelden van SELECT-component kunnen worden gevonden in de [aan de slag met apparaat twin query's] [ lnk-query-getstarted] sectie.

Op dit moment selectie componenten anders dan **Selecteer*** worden alleen ondersteund in cumulatieve query's in horende apparaten.

## <a name="group-by-clause"></a>GROUP BY-component
De **GROUP BY < group_specification >** component is een optionele stap die kan worden uitgevoerd na het opgegeven in de component WHERE, en voordat de projectie Selecteer in het opgegeven filter. Deze groepen documenten op basis van de waarde van een kenmerk. Deze groepen worden gebruikt voor het genereren van geaggregeerde waarden zoals opgegeven in de component SELECT.

Er is een voorbeeld van een GROUP BY-query:

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

De formele syntaxis voor de GROUP BY is:

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

**%{Attribute_name/** verwijst naar een eigenschap van het JSON-document in de verzameling FROM.

De component GROUP BY wordt momenteel alleen ondersteund bij het opvragen van apparaat horende.

## <a name="expressions-and-conditions"></a>Expressies en voorwaarden
Op een hoog niveau een *expressie*:

* Resulteert in een exemplaar van een JSON-type (zoals Boolean, getal, string, matrix of object).
* Wordt gedefinieerd door het bewerken van gegevens die afkomstig zijn van de JSON-document apparaat en de constanten die met ingebouwde operatoren en functies.

*Voorwaarden* expressies die in een Booleaanse waarde resulteren zijn. Een constante anders dan Booleaanse waarde **true** wordt beschouwd als **false** (inclusief **null**, **niet-gedefinieerde**, een willekeurig exemplaar object of de matrix een willekeurige tekenreeks en duidelijk de Booleaanse waarde **false**).

De syntaxis voor expressies is:

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

Om te begrijpen wat elke symbool in de syntaxis van de expressies voor staat, raadpleegt u de volgende tabel:

| Symbool | Definitie |
| --- | --- |
| %{attribute_name/ | Een eigenschap van het JSON-document in de **FROM** verzameling. |
| binary_operator | Een binaire operator die worden vermeld in de [Operators](#operators) sectie. |
| functie_naam| Een functie die worden vermeld in de [functies](#functions) sectie. |
| decimal_literal |Een float uitgedrukt in decimale notatie. |
| hexadecimal_literal |Een getal uitgedrukt in de tekenreeks '0 x' gevolgd door een reeks van hexadecimale cijfers. |
| string_literal |Letterlijke tekenreeks zijn vertegenwoordigd door een reeks van nul of meer Unicode-tekens of escapereeksen Unicode-tekenreeksen. Letterlijke tekenreeks zijn ingesloten in enkele aanhalingstekens of dubbele aanhalingstekens. Hiermee heft toegestaan: `\'`, `\"`, `\\`, `\uXXXX` voor Unicode-tekens die zijn gedefinieerd door 4 hexadecimale cijfers. |

### <a name="operators"></a>Operators
De volgende operators worden ondersteund:

| Familie | Operators |
| --- | --- |
| Rekenkundige bewerking |+, -, *, /, % |
| Logische |EN, OF NIET |
| Vergelijking |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>Functies
Tijdens het opvragen van horende en zijn die de enige ondersteunde is functie:

| Functie | Beschrijving |
| -------- | ----------- |
| IS_DEFINED(Property) | Retourneert een Booleaanse waarde die aangeeft of de eigenschap een waarde is toegewezen (met inbegrip van `null`). |

In situaties routes, worden de volgende wiskundige functies ondersteund:

| Functie | Beschrijving |
| -------- | ----------- |
| ABS(x) | Retourneert de absolute (positieve) waarde van de opgegeven numerieke expressie. |
| EXP(x) | Berekent de exponentiële waarde van de opgegeven numerieke expressie (e ^ x). |
| Power(x,y) | Retourneert de waarde van de opgegeven expressie voor de opgegeven macht (x ^ y).|
| Square(x) | Retourneert het kwadraat van de numerieke waarde die is opgegeven. |
| CEILING(x) | Retourneert de kleinste waarde van geheel getal groter dan of gelijk zijn aan de opgegeven numerieke expressie. |
| Floor(x) | Retourneert het grootste gehele getal kleiner dan of gelijk zijn aan de opgegeven numerieke expressie. |
| Sign(x) | Retourneert de positief (+ 1), nul (0) of minteken (-1) van de opgegeven numerieke expressie.|
| WORTEL(x) | Retourneert de vierkantswortel van de numerieke waarde die is opgegeven. |

In situaties routes, worden het volgende type controleren en de casten functies ondersteund:

| Functie | Beschrijving |
| -------- | ----------- |
| AS_NUMBER | De invoerreeks converteert naar een getal. `noop`Als de invoer is een getal. `Undefined` als tekenreeks geen een getal vertegenwoordigt.|
| IS_ARRAY | Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een matrix is. |
| IS_BOOL | Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een Booleaanse waarde is. |
| IS_DEFINED | Retourneert een Booleaanse waarde die aangeeft of de eigenschap een waarde is toegewezen. |
| IS_NULL | Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie null is. |
| IS_NUMBER | Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie is van een getal. |
| IS_OBJECT | Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een JSON-object is. |
| IS_PRIMITIVE | Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie is van een primitief (string, Boolean, numerieke of `null`). |
| IS_STRING | Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een tekenreeks is. |

In situaties routes, worden de volgende tekenreeksfuncties ondersteund:

| Functie | Beschrijving |
| -------- | ----------- |
| CONCAT (x, y,...) | Retourneert een tekenreeks die het resultaat van het samenvoegen van twee of meer tekenreekswaarden. |
| LENGTH(x) | Retourneert het aantal tekens van de opgegeven tekenreeksexpressie.|
| LOWER(x) | Retourneert een tekenreeksexpressie na hoofdletter gegevens converteren naar kleine letters. |
| UPPER(x) | Retourneert een tekenreeksexpressie na kleine letter gegevens converteren naar hoofdletters. |
| De SUBTEKENREEKS (tekenreeks, start [, lengte]) | Retourneert deel van een tekenreeksexpressie die beginnen bij de op nul gebaseerde positie opgegeven teken en blijft de opgegeven lengte of het einde van de tekenreeks. |
| INDEX_OF (tekenreeks, fragment) | Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste expressie in de opgegeven tekenreeks is of -1 als de tekenreeks is niet gevonden.|
| STARTS_WITH (x, y) | Retourneert een Boolean die aangeeft of de eerste expressie tekenreeks begint met het tweede. |
| ENDS_WITH (x, y) | Retourneert een Boolean die aangeeft of de eerste tekenreeksexpressie eindigt op de tweede. |
| CONTAINS(x,y) | Retourneert een Boolean die aangeeft of de eerste expressie tekenreeks de tweede bevat. |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het uitvoeren van query's in uw apps met behulp van [Azure IoT SDK's][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
