---
title: Query op de Azure IoT Hub-berichtroutering | Microsoft Docs
description: Handleiding voor ontwikkelaars - Query synxtax voor Azure IoT hub-berichtroutering.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: 3967a1e2317bac76785d534ba04a93de552c1a40
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018533"
---
# <a name="iot-hub-message-routing-query-syntax"></a>IoT Hub-berichtroutering query-syntaxis

Berichtroutering kan gebruikers voor het routeren van verschillende gegevenstypen namelijk, telemetrieberichten van apparaten, levenscyclusgebeurtenissen voor het apparaat en dubbele apparaat wijzigt gebeurtenissen naar verschillende eindpunten. U kunt ook uitgebreide query's toepassen op deze gegevens voordat u deze voor het ontvangen van de gegevens die voor u belangrijk is. In dit artikel beschrijft de querytaal van IoT-Hub bericht routering, en biedt enkele veelvoorkomende querypatronen.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Berichtroutering, kunt u een query uitvoeren op de eigenschappen van berichten en de berichttekst, evenals de device twin tags en de apparaatdubbeleigenschappen. Als de berichttekst geen JSON is, routering van berichten, kan het bericht nog steeds routeren, maar query's kunnen niet worden toegepast op de berichttekst.  Query's worden beschreven als Booleaanse expressies waarbij een Booleaanse waarde waar de query wordt mislukt waarmee alle binnenkomende gegevens, routes en Booleaanse waarde ONWAAR is de query mislukt en er worden geen gegevens wordt doorgestuurd. Als de expressie wordt geëvalueerd op null of niet-gedefinieerde, wordt dit beschouwd als onwaar en een fout wordt gegenereerd in Logboeken met diagnostische gegevens in geval van een storing. De querysyntaxis van de moet juist zijn voor de route worden opgeslagen en worden geëvalueerd.  

## <a name="message-routing-query-based-on-message-properties"></a>Bericht routering query op basis van eigenschappen van berichten 

De IoT-Hub definieert een [algemene indeling](iot-hub-devguide-messages-construct.md) voor alle apparaat-naar-cloud-berichten voor interoperatbility alle protocollen. IoT Hub-bericht wordt ervan uitgegaan dat de volgende JSON-weergave van het bericht. Eigenschappen voor alle gebruikers worden toegevoegd en inhoud van het bericht te identificeren. Gebruikers kunnen eigenschappen van toepassingen selectief toevoegen aan het bericht. Het is raadzaam om met behulp van de namen van de unieke eigenschappen zoals IoT Hub apparaat-naar-cloud-berichten niet hoofdlettergevoelig is. Bijvoorbeeld, als u meerdere eigenschappen met dezelfde naam hebben, verzendt IoT-Hub alleen een van de eigenschappen.  

```json
{ 
  "message": { 
    "systemProperties": { 
      "contentType": "application/json", 
      "contentEncoding": "UTF-8", 
      "iothub-message-source": "deviceMessages", 
      "iothub-enqueuedtime": "2017-05-08T18:55:31.8514657Z" 
    }, 
    "appProperties": { 
      "processingPath": "{cold | warm | hot}", 
      "verbose": "{true, false}", 
      "severity": 1-5, 
      "testDevice": "{true | false}" 
    }, 
    "body": "{\"Weather\":{\"Temperature\":50}}" 
  } 
} 
```

### <a name="system-properties"></a>Systeemeigenschappen

Systeemeigenschappen te identificeren van de inhoud en de bron van de berichten. 

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| contentType | tekenreeks | De gebruiker geeft het type inhoud van het bericht. Als u wilt toestaan dat query op de berichttekst, moet deze waarde application/JSON worden ingesteld. |
| contentEncoding | tekenreeks | De gebruiker Hiermee geeft u het coderingstype van het bericht. Toegestane waarden zijn UTF-8, UTF-16, UTF-32 als de contentType is ingesteld op application/JSON. |
| ConnectionDeviceId | tekenreeks | Deze waarde is ingesteld door de IoT Hub en geeft de bron van de berichten. Dit wordt mogelijk telemetrieberichten van apparaten, device twin wijzigingsmeldingen of levenscyclusgebeurtenissen voor het apparaat. Dit kan niet worden opgevraagd. |
| iothub-enqueuedtime | tekenreeks | Deze waarde is ingesteld door de IoT Hub en de werkelijke tijd van het bericht in UTC enqueuing vertegenwoordigt. Gebruiken om te vragen, `enqueuedTime`. |

Zoals beschreven in de [IoT Hub-berichten](iot-hub-devguide-messages-construct.md), er zijn aanvullende eigenschappen in een bericht. Naast **contentType**, **contentEncoding**, en **enqueuedTime**, wordt de **connectionDeviceId** en  **connectionModuleId** kunnen ook worden opgevraagd.

### <a name="application-properties"></a>Toepassingseigenschappen

Eigenschappen voor de toepassing zijn zelfgedefinieerde tekenreeksen die kunnen worden toegevoegd aan het bericht. Deze velden zijn optioneel.  

### <a name="query-expressions"></a>Query-expressies

Een query op berichteigenschappen moet worden voorafgegaan door de `$` symbool. Query's op de eigenschappen voor de toepassing worden geopend met de naam en moet niet worden voorafgegaan door de `$`symbool. Als de naam van een toepassing eigenschap met begint `$`, klikt u vervolgens IoT Hub wordt ernaar zoeken in de eigenschappen, en niet is gevonden en vervolgens ziet het er in de toepassingseigenschappen van de. Bijvoorbeeld: 

Query uitvoeren op systeem eigenschap contentEncoding 

```sql
$contentEncoding = 'UTF-8'
```

Query uitvoeren op de toepassing de eigenschap processingPath:

```sql
processingPath = 'hot'
```

Als u wilt combineren deze query's, kunt u Booleaanse expressies en functies:

```sql
$contentEncoding = 'UTF-8' AND processingPath = 'hot'
```

Een volledige lijst met ondersteunde operatoren en functies worden vermeld [expressie en voorwaarden](iot-hub-devguide-query-language.md#expressions-and-conditions)

## <a name="message-routing-query-based-on-message-body"></a>Bericht routering query op basis van de hoofdtekst van bericht 

Als u wilt inschakelen voor het uitvoeren van query's op de berichttekst, moet het bericht in een JSON gecodeerd in UTF-8, UTF-16- of UTF-32. De `contentType` moet worden ingesteld op `application/JSON` en `contentEncoding` op een van de ondersteunde UTF-coderingen in de systeemeigenschap. Als deze eigenschappen niet opgegeven zijn, wordt de query-expressie in de hoofdtekst van het bericht niet geëvalueerd door IoT Hub. 

Het volgende voorbeeld laat zien hoe een bericht maken met een juist opgemaakte en gecodeerde JSON-hoofdtekst: 

```javascript
var messageBody = JSON.stringify(Object.assign({}, {
    "Weather": {
        "Temperature": 50,
        "Time": "2017-03-09T00:00:00.000Z",
        "PrevTemperatures": [
            20,
            30,
            40
        ],
        "IsEnabled": true,
        "Location": {
            "Street": "One Microsoft Way",
            "City": "Redmond",
            "State": "WA"
        },
        "HistoricalData": [
            {
                "Month": "Feb",
                "Temperature": 40
            },
            {
                "Month": "Jan",
                "Temperature": 30
            }
        ]
    }
}));

// Encode message body using UTF-8  
var messageBytes = Buffer.from(messageBody, "utf8");

var message = new Message(messageBytes);

// Set message body type and content encoding 
message.contentEncoding = "utf-8";
message.contentType = "application/json";

// Add other custom application properties   
message.properties.add("Status", "Active");

deviceClient.sendEvent(message, (err, res) => {
    if (err) console.log('error: ' + err.toString());
    if (res) console.log('status: ' + res.constructor.name);
});
```

### <a name="query-expressions"></a>Query-expressies

Een query op de hoofdtekst van het bericht moet worden voorafgegaan door de `$body`. U kunt de verwijzing naar een instantie, hoofdtekst van de verwijzing naar een matrix of meerdere hoofdtekst verwijzingen gebruiken in de query-expressie. Uw query-expressie kunt ook een verwijzing hoofdtekst combineren met Systeemeigenschappen bericht en naslaginformatie over eigenschappen van toepassing. De volgende zijn bijvoorbeeld alle geldige query-expressies: 

```sql
$body.Weather.HistoricalData[0].Month = 'Feb' 
```

```sql
$body.Weather.Temperature = 50 AND $body.Weather.IsEnabled 
```

```sql
length($body.Weather.Location.State) = 2 
```

```sql
$body.Weather.Temperature = 50 AND processingPath = 'hot'
```

## <a name="message-routing-query-based-on-device-twin"></a>Bericht routering query op basis van apparaatdubbel 

Berichtroutering, kunt u een query uitvoeren op [Apparaatdubbel](iot-hub-devguide-device-twins.md) tags en eigenschappen die JSON-objecten zijn. Houd er rekening mee dat voor het uitvoeren van query's op moduledubbel niet wordt ondersteund. Hieronder ziet u een voorbeeld van Apparaatdubbel-tags en eigenschappen.

```JSON
{
    "tags": { 
        "deploymentLocation": { 
            "building": "43", 
            "floor": "1" 
        } 
    }, 
    "properties": { 
        "desired": { 
            "telemetryConfig": { 
                "sendFrequency": "5m" 
            }, 
            "$metadata" : {...}, 
            "$version": 1 
        }, 
        "reported": { 
            "telemetryConfig": { 
                "sendFrequency": "5m", 
                "status": "success" 
            },
            "batteryLevel": 55, 
            "$metadata" : {...}, 
            "$version": 4 
        } 
    } 
} 
```

### <a name="query-expressions"></a>Query-expressies

Een query op de hoofdtekst van het bericht moet worden voorafgegaan door de `$twin`. Uw query-expressie kunt ook een dubbele eigenschap of tag verwijzing combineren met een verwijzing hoofdtekst, bericht Systeemeigenschappen en naslaginformatie over eigenschappen van toepassing. Het is raadzaam om met behulp van unieke namen in labels en eigenschappen als de query niet hoofdlettergevoelig is. Ook met behulp van weerhouden `twin`, `$twin`, `body`, of `$body`, aan de eigenschapnamen van een. De volgende zijn bijvoorbeeld alle geldige query-expressies: 

```sql
$twin.properties.desired.telemetryConfig.sendFrequency = '5m'
```

```sql
$body.Weather.Temperature = 50 AND $twin.properties.desired.telemetryConfig.sendFrequency = '5m'
```

```sql
$twin.tags.deploymentLocation.floor = 1 
```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [berichtroutering](iot-hub-devguide-messages-d2c.md).
* Probeer de [message routing zelfstudie](tutorial-routing.md).