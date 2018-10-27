---
title: Over het gebruik van de gebruiker gedefinieerde functies in Azure digitale dubbels | Microsoft Docs
description: Richtlijn voor het maken van de gebruiker gedefinieerde functies, vergelijkingsprogramma voor en roltoewijzingen met digitale dubbels van Azure.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: alinast
ms.openlocfilehash: 49566d21fa6897f5c1371bbea2bb602a393de66d
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140786"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Over het gebruik van de gebruiker gedefinieerde functies in Azure digitale dubbels

[De gebruiker gedefinieerde functies](./concepts-user-defined-functions.md) dat de gebruiker kan het uitvoeren van aangepaste logica op binnenkomende berichten over Telemetrie en metagegevens van de ruimtelijke grafiek, zodat de gebruiker voor het verzenden van gebeurtenissen naar vooraf gedefinieerde eindpunten. In deze handleiding vindt u een voorbeeld van fungeert op temperatuur gebeurtenissen om te detecteren en waarschuwen bij elke lezen die groter is dan een bepaalde temperatuur.

In de onderstaande voorbeelden `https://yourManagementApiUrl` verwijst naar de URI van de digitale Twins-API's:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *NaamExemplaar* | De naam van uw Azure digitale Twins-exemplaar |
| *yourLocation* | Welke regio u uw exemplaar wordt gehost op server |

## <a name="client-library-reference"></a>Naslaginformatie over-clientbibliotheek

De functies die beschikbaar als Help-methoden in de runtime van de gebruiker gedefinieerde functies zijn worden opgesomd in het volgende [referentie Client](#Client-Reference).

## <a name="create-a-matcher"></a>Een matcher maken

Vergelijkingsprogramma voor zijn graph-objecten, die bepalen welke door de gebruiker gedefinieerde functies worden uitgevoerd voor een bepaalde telemetrieberichten.

Geldige matcher voorwaarde vergelijkingen:

- `Equals`
- `NotEquals`
- `Contains`

Geldige matcher voorwaarde doelen:

- `Sensor`
- `SensorDevice`
- `SensorSpace`

Het volgende voorbeeld matcher evalueren op ' True ' voor elke gebeurtenis sensor telemetrie met `Temperature` als de waarde van het gegevenstype. U kunt meerdere vergelijkingsprogramma voor maken van een door de gebruiker gedefinieerde functie.

```text
POST https://yourManagementApiUrl/api/v1.0/matchers
{
  "Name": "Temperature Matcher",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ],
  "SpaceId": "yourSpaceIdentifier"
}
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *yourManagementApiUrl* | De volledige URL-pad voor uw API Management  |
| *yourSpaceIdentifier* | Welke regio u uw exemplaar wordt gehost op server |

## <a name="create-a-user-defined-function-udf"></a>Een door de gebruiker gedefinieerde functie (UDF's) maken

Na de vergelijkingsprogramma voor zijn gemaakt, uploadt u het fragment functie met de volgende POST-aanroep:

> [!IMPORTANT]
> - Stel het volgende in de Headers `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - De hoofdtekst is meerdelige:
>   - Het eerste deel is over de metagegevens die nodig zijn voor de UDF.
>   - Het tweede gedeelte is het javascript-logica voor compute.
> - Vervang `userDefinedBoundary` sectie `SpaceId` en `Machers` GUID's.

```plaintext
POST https://yourManagementApiUrl/api/v1.0/userdefinedfunctions with Content-Type: multipart/form-data; boundary="userDefinedBoundary"
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *yourManagementApiUrl* | De volledige URL-pad voor uw API Management  |

Hoofdtekst:

```plaintext
--userDefinedBoundary
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "yourSpaceIdentifier",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["yourMatcherIdentifier"]
}
--userDefinedBoundary
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--userDefinedBoundary--
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *yourSpaceIdentifier* | De ruimte-id  |
| *yourMatcherIdentifier* | De ID van de matcher die u wilt gebruiken |

### <a name="example-functions"></a>Voorbeeld van de functies

De sensor telemetrie lezen voor de sensor rechtstreeks met het gegevenstype ingesteld `Temperature`, die is `sensor.DataType`:

```javascript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

De `telemetry` parameter wordt aangegeven dat een `SensorId` en `Message`. De `executionContext` parameter wordt aangegeven dat de volgende kenmerken:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

In het volgende voorbeeld wordt er een bericht Meld u als sensor telemetrie lezen een vooraf gedefinieerde drempelwaarde overschrijdt. Als de diagnostische instellingen zijn ingeschakeld op het exemplaar digitale Twins, worden ook logboeken van de gebruiker gedefinieerde functies worden doorgestuurd:

```javascript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

De volgende code wordt een melding geactiveerd als het niveau van de temperatuur boven aan de vooraf gedefinieerde constante.

```javascript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Raadpleeg voor een meer complexe UDF-codevoorbeeld [controleren op beschikbare spaties met nieuwe lucht UDF](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js)

## <a name="create-a-role-assignment"></a>Een roltoewijzing maken

Er moet een roltoewijzing voor de gebruiker gedefinieerde functie om uit te voeren onder maken. Als we dit niet doet, wordt deze niet de juiste machtigingen om te communiceren met de API Management acties uitvoeren op de graph-objecten hebben. De acties die door de gebruiker gedefinieerde functie uitgevoerd, zijn niet uitgesloten van de op rollen gebaseerd toegangsbeheer in de digitale dubbels Management API's. Ze kunnen binnen het bereik worden beperkt door bepaalde functies of bepaalde access control-paden op te geven. Zie voor meer informatie, [rollen gebaseerd toegangsbeheer](./security-role-based-access-control.md) documentatie.

- Query voor rollen en haal de ID van de rol die u wilt toewijzen aan de UDF; deze doorgeven aan de RoleId hieronder.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/system/roles
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *yourManagementApiUrl* | De volledige URL-pad voor uw API Management  |

- Object-id is de UDF-ID die eerder is gemaakt
- Zoek `Path` door het opvragen van de ruimten met hun volledige pad en kopieer de `spacePaths` waarde. Plak deze in het pad hieronder bij het maken van de roltoewijzing UDF

```plaintext
GET https://yourManagementApiUrl/api/v1.0/spaces?name=yourSpaceName&includes=fullpath
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *yourManagementApiUrl* | De volledige URL-pad voor uw API Management  |
| *yourSpaceName* | De naam van de ruimte die u wilt gebruiken |

```plaintext
POST https://yourManagementApiUrl/api/v1.0/roleassignments
{
  "RoleId": "yourDesiredRoleIdentifier",
  "ObjectId": "yourUserDefinedFunctionId",
  "ObjectIdType": "UserDefinedFunctionId",
  "Path": "yourAccessControlPath"
}
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| *yourManagementApiUrl* | De volledige URL-pad voor uw API Management  |
| *yourDesiredRoleIdentifier* | De id voor de gewenste rol |
| *yourUserDefinedFunctionId* | De ID voor de UDF die u wilt gebruiken |
| *yourAccessControlPath* | De access control-pad |

## <a name="send-telemetry-to-be-processed"></a>Verzenden van telemetrie moeten worden verwerkt

Telemetrie die is gegenereerd door de sensor die worden beschreven in de grafiek moet Activeer de uitvoering van de gebruiker gedefinieerde functie dat is geüpload. Zodra de telemetrie wordt opgehaald door de gegevensverwerker, wordt een planning worden uitgevoerd voor het aanroepen van de gebruiker gedefinieerde functie gemaakt.

1. Ophalen van de vergelijkingsprogramma voor voor de sensor die lezen afmelden is gegenereerd.
1. Afhankelijk van wat vergelijkingsprogramma voor beoordeling van voltooid, de bijbehorende door de gebruiker gedefinieerde functies worden opgehaald.
1. Elke gebruiker gedefinieerde functie uitgevoerd.

## <a name="client-reference"></a>Referentie voor client

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Opgegeven een ruimte-id, haalt de ruimte van de grafiek.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `id`  | `guid` | ruimte-id |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Opgegeven een sensor-id, haalt de sensor van de grafiek.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `id`  | `guid` | sensor-id |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Gegeven een apparaat-id, haalt het apparaat van de grafiek.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `id`  | `guid` | Apparaat-id |

### <a name="getsensorvaluesensorid-datatype--value"></a>⇒ getSensorValue (sensorId, gegevenstype) `value`

Basis van een sensor-id en het gegevenstype, de huidige waarde voor die sensor worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `sensorId`  | `guid` | sensor-id |
| `dataType`  | `string` | gegevens van het sensortype |

### <a name="getspacevaluespaceid-valuename--value"></a>⇒ getSpaceValue (spaceId, valueName) `value`

Een id ruimte en de naam van de opgegeven, de huidige waarde voor die eigenschap ruimte worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `spaceId`  | `guid` | ruimte-id |
| `valueName` | `string` | naam van de eigenschap ruimte |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>⇒ getSensorHistoryValues (sensorId, gegevenstype) `value[]`

De historische waarden voor die sensor een sensor-id en het gegevenstype opgegeven, worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `sensorId` | `guid` | sensor-id |
| `dataType` | `string` | gegevens van het sensortype |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>⇒ getSpaceHistoryValues (spaceId, gegevenstype) `value[]`

Een id ruimte en de naam van de opgegeven, haalt de historische waarden voor die eigenschap voor de ruimte.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `spaceId` | `guid` | ruimte-id |
| `valueName` | `string` | naam van de eigenschap ruimte |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

De onderliggende ruimten voor die bovenliggende ruimte krijgen een ruimte-id, worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `spaceId` | `guid` | ruimte-id |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

De onderliggende sensoren voor die bovenliggende ruimte krijgen een ruimte-id, worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `spaceId` | `guid` | ruimte-id |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

De onderliggende apparaten voor die bovenliggende ruimte krijgen een ruimte-id, worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `spaceId` | `guid` | ruimte-id |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Uitgaande van een apparaat-id, de onderliggende sensoren voor dat apparaat bovenliggende worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `deviceId` | `guid` | Apparaat-id |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

De bovenliggende ruimte krijgen een ruimte-id, worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `childSpaceId` | `guid` | ruimte-id |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

De bovenliggende ruimte krijgen een sensor-id, worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `childSensorId` | `guid` | sensor-id |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Uitgaande van een apparaat-id, de bovenliggende ruimte worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `childDeviceId` | `guid` | Apparaat-id |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Het bovenliggende apparaat krijgen een sensor-id, worden opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `childSensorId` | `guid` | sensor-id |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>⇒ getSpaceExtendedProperty (spaceId, propertyName) `extendedProperty`

De eigenschap en de bijbehorende waarde krijgen een ruimte-id, ophalen vanuit de ruimte.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `spaceId` | `guid` | ruimte-id |
| `propertyName` | `string` | naam van de eigenschap ruimte |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>⇒ getSensorExtendedProperty (sensorId, propertyName) `extendedProperty`

De eigenschap en de bijbehorende waarde krijgen een sensor-id, ophalen van de sensor.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `sensorId` | `guid` | sensor-id |
| `propertyName` | `string` | naam van de eigenschap sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>⇒ getDeviceExtendedProperty (apparaat-id, propertyName) `extendedProperty`

Uitgaande van een apparaat-id, de eigenschap en de waarde van het apparaat opgehaald.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `deviceId` | `guid` | Apparaat-id |
| `propertyName` | `string` | de eigenschap apparaatnaam |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue (sensorId, gegevenstype, waarde)

Hiermee stelt een waarde voor de sensor-object met het opgegeven gegevenstype.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `sensorId` | `guid` | sensor-id |
| `dataType`  | `string` | gegevens van het sensortype |
| `value`  | `string` | waarde |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue (spaceId, gegevenstype, waarde)

Hiermee stelt een waarde voor de ruimte-object met het opgegeven gegevenstype.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `spaceId` | `guid` | ruimte-id |
| `dataType` | `string` | gegevenstype |
| `value` | `string` | waarde |

### <a name="logmessage"></a>log(Message)

Hiermee wordt het volgende bericht in de gebruiker gedefinieerde functie geregistreerd.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `message` | `string` | bericht moeten worden vastgelegd |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification (topologyObjectId, topologyObjectType, nettolading)

Verzendt een aangepast Meldingsbericht worden verzonden.

**Type**: globale functie

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `topologyObjectId`  | `guid` | Graph-object-id (ex.) ruimte / sensor /device-ID)|
| `topologyObjectType`  | `string` | (ex.) ruimte / sensor / apparaat)|
| `payload`  | `string` | de JSON-nettolading met de melding wordt verzonden |

## <a name="return-types"></a>Typen retourneren

Hieronder vindt u modellen met een beschrijving van de geretourneerde objecten uit de bovenstaande verwijzing van de client.

### <a name="space"></a>Ruimte

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000000",
  "Name": "Space",
  "FriendlyName": "Conference Room",
  "TypeId": 0,
  "ParentSpaceId": "00000000-0000-0000-0000-000000000001",
  "SubtypeId": 0
}
```

### <a name="space-methods"></a>Ruimte-methoden

#### <a name="parent--space"></a>Parent() ⇒ `space`

Retourneert het bovenliggende ruimte van de huidige ruimte.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Retourneert het onderliggende sensoren van de huidige ruimte.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Retourneert het onderliggende apparaten van de huidige ruimte.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Retourneert de uitgebreide eigenschap en de waarde voor de huidige ruimte.

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `propertyName` | `string` | naam van de uitgebreide eigenschap |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Retourneert de waarde van de huidige ruimte.

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `valueName` | `string` | naam van de waarde |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Retourneert de historische waarden van de huidige ruimte.

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `valueName` | `string` | naam van de waarde |

#### <a name="notifypayload"></a>Notify(Payload)

Verzendt een melding met de nettolading van de opgegeven.

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `payload` | `string` | JSON-nettolading in de melding moeten worden opgenomen |

### <a name="device"></a>Apparaat

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000002",
  "Name": "Device",
  "FriendlyName": "Temperature Sensing Device",
  "Description": "This device contains a sensor that captures temperature readings.",
  "Type": "None",
  "Subtype": "None",
  "TypeId": 0,
  "SubtypeId": 0,
  "HardwareId": "ABC123",
  "GatewayId": "ABC",
  "SpaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Apparaatmethoden

#### <a name="parent--space"></a>Parent() ⇒ `space`

Retourneert de bovenliggende ruimte van het huidige apparaat.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Retourneert het onderliggende sensoren van het huidige apparaat.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Retourneert de uitgebreide eigenschap en de waarde voor het huidige apparaat.

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `propertyName` | `string` | naam van de uitgebreide eigenschap |

#### <a name="notifypayload"></a>Notify(Payload)

Verzendt een melding met de nettolading van de opgegeven.

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `payload` | `string` | JSON-nettolading in de melding moeten worden opgenomen |

### <a name="sensor"></a>Sensor

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000003",
  "Port": "30",
  "PollRate": 3600,
  "DataType": "Temperature",
  "DataSubtype": "None",
  "Type": "Classic",
  "PortType": "None",
  "DataUnitType": "FahrenheitTemperature",
  "SpaceId": "00000000-0000-0000-0000-000000000000",
  "DeviceId": "00000000-0000-0000-0000-000000000001",
  "PortTypeId": 0,
  "DataUnitTypeId": 0,
  "DataTypeId": 0,
  "DataSubtypeId": 0,
  "TypeId": 0  
}
```

### <a name="sensor-methods"></a>Sensor-methoden

#### <a name="space--space"></a>Space() ⇒ `space`

Retourneert de bovenliggende ruimte van de huidige sensor.

#### <a name="device--device"></a>Device() ⇒ `device`

Retourneert het bovenliggende apparaat van de huidige sensor.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Retourneert de uitgebreide eigenschap en de waarde voor de huidige sensor.

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `propertyName` | `string` | naam van de uitgebreide eigenschap |

#### <a name="value--value"></a>Value() ⇒ `value`

Retourneert de waarde van de huidige sensor.

#### <a name="history--value"></a>History() ⇒ `value[]`

Retourneert de historische waarden van de huidige sensor.

#### <a name="notifypayload"></a>Notify(Payload)

Verzendt een melding met de nettolading van de opgegeven.

| Param  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| `payload` | `string` | JSON-nettolading in de melding moeten worden opgenomen |

### <a name="value"></a>Waarde

```JSON
{
  "DataType": "Temperature",
  "Value": "70",
  "CreatedTime": ""
}
```

### <a name="extended-property"></a>Uitgebreide eigenschap

```JSON
{
  "Name": "OccupancyStatus",
  "Value": "Occupied"
}
```

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over het maken van digitale dubbels eindpunten voor het verzenden van gebeurtenissen die moeten worden, [digitale dubbels maken eindpunten](how-to-egress-endpoints.md).

Lees voor meer informatie over digitale dubbels eindpunten [meer informatie over eindpunten](concepts-events-routing.md).
