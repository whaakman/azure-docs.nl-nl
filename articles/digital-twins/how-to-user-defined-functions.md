---
title: Over het gebruik van de gebruiker gedefinieerde functies in Azure digitale dubbels | Microsoft Docs
description: Richtlijn voor het maken van de gebruiker gedefinieerde functies, vergelijkingsprogramma voor en roltoewijzingen met digitale dubbels van Azure.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: alinast
ms.openlocfilehash: 6a757dca48dc3ff41adfe6f8802fad40e7a4ca81
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636829"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Over het gebruik van de gebruiker gedefinieerde functies in Azure digitale dubbels

[De gebruiker gedefinieerde functies](./concepts-user-defined-functions.md) (UDF's), kan de gebruiker uitvoeren van aangepaste logica op binnenkomende berichten over Telemetrie en metagegevens van de ruimtelijke graph. De gebruiker kan vervolgens gebeurtenissen verzenden naar vooraf gedefinieerde eindpunten. In deze handleiding worden door een voorbeeld van het optreden van temperatuur gebeurtenissen om te detecteren en waarschuwingen op elke lezen die groter is dan een bepaalde temperatuur.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>Naslaginformatie over-clientbibliotheek

De functies die beschikbaar als Help-methoden in de runtime van de gebruiker gedefinieerde functies zijn worden vermeld in de [referentie client](#Client-Reference) sectie.

## <a name="create-a-matcher"></a>Een matcher maken

Vergelijkingsprogramma voor zijn graph-objecten die te bepalen wat de gebruiker gedefinieerde functies worden uitgevoerd voor een bepaalde telemetrieberichten.

- Geldige matcher voorwaarde vergelijkingen:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Geldige matcher voorwaarde doelen:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

Het volgende voorbeeld matcher resulteert in waar op een sensor telemetrie-gebeurtenis met `"Temperature"` als de waarde van het gegevenstype. U kunt meerdere vergelijkingsprogramma voor maken van een door de gebruiker gedefinieerde functie:

```plaintext
POST YOUR_MANAGEMENT_API_URL/matchers
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
  "SpaceId": "YOUR_SPACE_IDENTIFIER"
}
```

| Waarde | Vervangen door |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Welke regio u uw exemplaar wordt gehost op server |

## <a name="create-a-user-defined-function-udf"></a>Een door de gebruiker gedefinieerde functie (UDF's) maken

Nadat de vergelijkingsprogramma voor zijn gemaakt, uploadt u het fragment functie met de volgende **POST** aanroepen:

> [!IMPORTANT]
> - Stel het volgende in de headers `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - De hoofdtekst wordt gedeeltelijk:
>   - Het eerste deel is over de metagegevens die nodig zijn voor de UDF.
>   - Het tweede gedeelte is het JavaScript-logica voor compute.
> - In de **USER_DEFINED_BOUNDARY** sectie, vervangt de **SpaceId** en **Machers** waarden.

```plaintext
POST YOUR_MANAGEMENT_API_URL/userdefinedfunctions with Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"
```

| Parameterwaarde | Vervangen door |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | De naam van een meerdelige inhoud grens |

### <a name="body"></a>Hoofdtekst

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "YOUR_SPACE_IDENTIFIER",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Waarde | Vervangen door |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | De ruimte-id  |
| YOUR_MATCHER_IDENTIFIER | De ID van de matcher die u wilt gebruiken |

### <a name="example-functions"></a>Voorbeeld van de functies

De sensor telemetrie lezen voor de sensor rechtstreeks met het gegevenstype ingesteld **temperatuur**, die is `sensor.DataType`:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

De **telemetrie** parameter wordt aangegeven dat de **SensorId** en **bericht** kenmerken, overeenkomt met een bericht wordt verzonden door een sensor. De **executionContext** parameter wordt aangegeven dat de volgende kenmerken:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

In het volgende voorbeeld melden we een bericht als de sensor telemetrie lezen een vooraf opgegeven drempel wordt overschreden. Als de diagnostische instellingen zijn ingeschakeld op het exemplaar van Azure digitale Twins, worden ook logboeken van de gebruiker gedefinieerde functies doorgestuurd:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

De volgende code wordt een melding geactiveerd als het niveau van de temperatuur boven aan de vooraf gedefinieerde constante:

```JavaScript
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

Voor een meer complexe UDF-codevoorbeeld [controleren op beschikbare plaatsen met een nieuwe lucht UDF](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Een roltoewijzing maken

Er moet een roltoewijzing voor de gebruiker gedefinieerde functie om uit te voeren onder maken. Als we dit niet doet, is er geen de juiste machtigingen om te communiceren met de API Management acties uitvoeren op de graph-objecten. De acties die door de gebruiker gedefinieerde functie uitgevoerd kunnen niet worden uitgesloten van de op rollen gebaseerd toegangsbeheer in de Azure digitale dubbels Management API's. Ze kunnen binnen het bereik worden beperkt door bepaalde functies of bepaalde access control-paden op te geven. Zie voor meer informatie, [op rollen gebaseerd toegangsbeheer](./security-role-based-access-control.md) documentatie.

1. Query voor rollen en haal de ID van de rol die u wilt toewijzen aan de UDF. Door te geven **RoleId**:

    ```plaintext
    GET YOUR_MANAGEMENT_API_URL/system/roles
    ```

1. **Object-id** de UDF-id die eerder is gemaakt.
1. Zoek de waarde van **pad** door het opvragen van uw opslagruimten met `fullpath`.
1. Kopieer de geretourneerde `spacePaths` waarde. U gebruikt die in de volgende code:

    ```plaintext
    GET YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Parameterwaarde | Vervangen door |
    | --- | --- |
    | *YOUR_SPACE_NAME* | De naam van de ruimte die u wilt gebruiken |

1. Plak de geretourneerde `spacePaths` waarde in **pad** een roltoewijzing UDF maken:

    ```plaintext
    POST YOUR_MANAGEMENT_API_URL/roleassignments
    {
      "RoleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "ObjectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "ObjectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "Path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | De waarde van de | Vervangen door |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | De id voor de gewenste rol |
    | YOUR_USER_DEFINED_FUNCTION_ID | De ID voor de UDF die u wilt gebruiken |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | De ID op te geven de UDF-type |
    | YOUR_ACCESS_CONTROL_PATH | De access control-pad |

## <a name="send-telemetry-to-be-processed"></a>Verzenden van telemetrie moeten worden verwerkt

Telemetrie die is gegenereerd door de sensor die worden beschreven in de grafiek wordt het uitvoeren van de gebruiker gedefinieerde functie dat is geüpload. De gegevensverwerker, neemt de telemetrie. Vervolgens wordt een plan uitvoeren voor het aanroepen van de gebruiker gedefinieerde functie gemaakt.

1. Ophalen van de vergelijkingsprogramma voor voor de sensor die lezen afmelden is gegenereerd.
1. Afhankelijk van wat vergelijkingsprogramma voor beoordeling van voltooid, de bijbehorende door de gebruiker gedefinieerde functies worden opgehaald.
1. Elke gebruiker gedefinieerde functie uitgevoerd.

## <a name="client-reference"></a>Referentie voor client

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Deze functie haalt een ruimte-id wordt opgegeven, de ruimte in de grafiek.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | ruimte-id |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Deze functie haalt krijgen een sensor-id, de sensor van de grafiek.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | sensor-id |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Een apparaat-id worden gegeven, haalt deze functie het apparaat in de grafiek.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Apparaat-id |

### <a name="getsensorvaluesensorid-datatype--value"></a>⇒ getSensorValue (sensorId, gegevenstype) `value`

Deze functie een sensor-id en het gegevenstype opgegeven, worden de huidige waarde voor die sensor opgehaald.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | sensor-id |
| *Gegevenstype*  | `string` | gegevens van het sensortype |

### <a name="getspacevaluespaceid-valuename--value"></a>⇒ getSpaceValue (spaceId, valueName) `value`

Deze functie is een ruimte-id en de naam van de opgegeven, haalt de huidige waarde voor die eigenschap ruimte.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | ruimte-id |
| *Waardenaam* | `string` | naam van de eigenschap ruimte |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>⇒ getSensorHistoryValues (sensorId, gegevenstype) `value[]`

Deze functie opgegeven een sensor-id en het gegevenstype, worden de historische waarden voor die sensor opgehaald.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | sensor-id |
| *Gegevenstype* | `string` | gegevens van het sensortype |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>⇒ getSpaceHistoryValues (spaceId, gegevenstype) `value[]`

Deze functie is een ruimte-id en de naam van de opgegeven, haalt de historische waarden voor die eigenschap voor de ruimte.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |
| *Waardenaam* | `string` | naam van de eigenschap ruimte |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Deze functie een ruimte-id wordt opgegeven, worden de onderliggende ruimten voor die bovenliggende ruimte opgehaald.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Een id ruimte worden gegeven, haalt deze functie de onderliggende sensoren voor die bovenliggende ruimte.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Deze functie een ruimte-id wordt opgegeven, worden de onderliggende apparaten voor die bovenliggende ruimte opgehaald.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Een apparaat-id worden gegeven, haalt deze functie de onderliggende sensoren voor dat apparaat bovenliggende.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *apparaat-id* | `guid` | Apparaat-id |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Deze functie een ruimte-id wordt opgegeven, wordt de ruimte van de bovenliggende opgehaald.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | ruimte-id |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Deze functie een sensor-id wordt opgegeven, wordt de ruimte van de bovenliggende opgehaald.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | sensor-id |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Een apparaat-id worden gegeven, haalt deze functie de bovenliggende ruimte.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Apparaat-id |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Deze functie een sensor-id wordt opgegeven, wordt het bovenliggende apparaat opgehaald.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | sensor-id |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>⇒ getSpaceExtendedProperty (spaceId, propertyName) `extendedProperty`

Deze functie haalt krijgen een ruimte-id, de eigenschap en de waarde van de ruimte.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |
| *propertyName* | `string` | naam van de eigenschap ruimte |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>⇒ getSensorExtendedProperty (sensorId, propertyName) `extendedProperty`

Deze functie haalt krijgen een sensor-id, de eigenschap en de waarde van de sensor.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | sensor-id |
| *propertyName* | `string` | naam van de eigenschap sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>⇒ getDeviceExtendedProperty (apparaat-id, propertyName) `extendedProperty`

Deze functie haalt krijgt een apparaat-id, de eigenschap en de waarde van het apparaat.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *apparaat-id* | `guid` | Apparaat-id |
| *propertyName* | `string` | de eigenschap apparaatnaam |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue (sensorId, gegevenstype, waarde)

Deze functie wordt een waarde ingesteld op de sensor-object met het opgegeven gegevenstype.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | sensor-id |
| *Gegevenstype*  | `string` | gegevens van het sensortype |
| *value*  | `string` | Waarde |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue (spaceId, gegevenstype, waarde)

Deze functie stelt een waarde in de ruimte-object met het opgegeven gegevenstype.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |
| *Gegevenstype* | `string` | Gegevenstype |
| *value* | `string` | Waarde |

### <a name="logmessage"></a>log(Message)

Deze functie registreert het volgende bericht in de gebruiker gedefinieerde functie.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *Bericht* | `string` | bericht moeten worden vastgelegd |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification (topologyObjectId, topologyObjectType, nettolading)

Deze functie verzendt een aangepast Meldingsbericht worden verzonden.

**Type**: globale functie

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Graph-object-id. Voorbeelden zijn schijfruimte, de sensor en apparaat-ID.|
| *topologyObjectType*  | `string` | Voorbeelden zijn sensor- en apparaatgegevens.|
| *nettolading*  | `string` | De JSON-nettolading met de melding wordt verzonden. |

## <a name="return-types"></a>Typen retourneren

De volgende modellen beschrijven de geretourneerde objecten uit de voorgaande verwijzing van de client.

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

Deze functie geeft als resultaat van de bovenliggende ruimte van de huidige ruimte.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Deze functie retourneert de onderliggende sensoren van de huidige ruimte.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Deze functie retourneert de onderliggende apparaten van de huidige ruimte.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Deze functie retourneert de uitgebreide eigenschap en de waarde voor de huidige ruimte.

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | naam van de uitgebreide eigenschap |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Deze functie retourneert de waarde van de huidige ruimte.

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *Waardenaam* | `string` | naam van de waarde |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Deze functie retourneert de historische waarden van de huidige ruimte.

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *Waardenaam* | `string` | naam van de waarde |

#### <a name="notifypayload"></a>Notify(Payload)

Deze functie verzendt een melding met de nettolading van de opgegeven.

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *nettolading* | `string` | JSON-nettolading in de melding moeten worden opgenomen |

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

Deze functie geeft als resultaat van de bovenliggende ruimte van het huidige apparaat.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Deze functie retourneert de onderliggende sensoren van het huidige apparaat.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Deze functie retourneert de uitgebreide eigenschap en de waarde voor het huidige apparaat.

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | naam van de uitgebreide eigenschap |

#### <a name="notifypayload"></a>Notify(Payload)

Deze functie verzendt een melding met de nettolading van de opgegeven.

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *nettolading* | `string` | JSON-nettolading in de melding moeten worden opgenomen |

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

Deze functie geeft als resultaat van de bovenliggende ruimte van de huidige sensor.

#### <a name="device--device"></a>Device() ⇒ `device`

Deze functie retourneert het bovenliggende apparaat van de huidige sensor.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Deze functie retourneert de uitgebreide eigenschap en de waarde voor de huidige sensor.

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | naam van de uitgebreide eigenschap |

#### <a name="value--value"></a>Value() ⇒ `value`

Deze functie retourneert de waarde van de huidige sensor.

#### <a name="history--value"></a>History() ⇒ `value[]`

Deze functie retourneert de historische waarden van de huidige sensor.

#### <a name="notifypayload"></a>Notify(Payload)

Deze functie verzendt een melding met de nettolading van de opgegeven.

| Parameter  | Type                | Beschrijving  |
| ------ | ------------------- | ------------ |
| *nettolading* | `string` | JSON-nettolading in de melding moeten worden opgenomen |

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

- Meer informatie over het [digitale dubbels Azure-eindpunten maken](how-to-egress-endpoints.md) voor het verzenden van gebeurtenissen die moeten worden.

- Lees voor meer informatie over Azure digitale dubbels eindpunten [meer informatie over eindpunten](concepts-events-routing.md).
