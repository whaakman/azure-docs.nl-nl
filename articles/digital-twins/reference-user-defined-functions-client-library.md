---
title: Azure digitale dubbels zelfgedefinieerde functies naslaginformatie over-clientbibliotheek | Microsoft Docs
description: Azure digitale dubbels zelfgedefinieerde functies naslaginformatie over-clientbibliotheek.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: article
ms.date: 12/27/2018
ms.author: alinast
ms.custom: seodec18
ms.openlocfilehash: d309765e6c4b0b0cc8a7e92977134d4135a69a13
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53995483"
---
# <a name="user-defined-functions-client-library-reference"></a>Naslaginformatie over de gebruiker gedefinieerde functies-clientbibliotheek

Dit document bevat referentie-informatie voor de clientbibliotheek van Azure digitale dubbels door gebruiker gedefinieerde functies.

## <a name="helper-methods"></a>Help-methoden

De clientbibliotheek definieert helpermethoden voor veelgebruikte bewerkingen.

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Deze functie haalt een ruimte-id wordt opgegeven, de ruimte in de grafiek.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | ruimte-id |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Deze functie haalt krijgen een sensor-id, de sensor van de grafiek.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | sensor-id |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Een apparaat-id worden gegeven, haalt deze functie het apparaat in de grafiek.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Apparaat-id |

### <a name="getsensorvaluesensorid-datatype--value"></a>⇒ getSensorValue (sensorId, gegevenstype) `value`

Deze functie een sensor-id en het gegevenstype opgegeven, worden de huidige waarde voor die sensor opgehaald.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | sensor-id |
| *Gegevenstype*  | `string` | gegevens van het sensortype |

### <a name="getspacevaluespaceid-valuename--value"></a>⇒ getSpaceValue (spaceId, valueName) `value`

Deze functie is een ruimte-id en de naam van de opgegeven, haalt de huidige waarde voor die eigenschap ruimte.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | ruimte-id |
| *Waardenaam* | `string` | naam van de eigenschap ruimte |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>⇒ getSensorHistoryValues (sensorId, gegevenstype) `value[]`

Deze functie opgegeven een sensor-id en het gegevenstype, worden de historische waarden voor die sensor opgehaald.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | sensor-id |
| *Gegevenstype* | `string` | gegevens van het sensortype |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>⇒ getSpaceHistoryValues (spaceId, gegevenstype) `value[]`

Deze functie is een ruimte-id en de naam van de opgegeven, haalt de historische waarden voor die eigenschap voor de ruimte.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |
| *Waardenaam* | `string` | naam van de eigenschap ruimte |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Deze functie een ruimte-id wordt opgegeven, worden de onderliggende ruimten voor die bovenliggende ruimte opgehaald.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Een id ruimte worden gegeven, haalt deze functie de onderliggende sensoren voor die bovenliggende ruimte.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Deze functie een ruimte-id wordt opgegeven, worden de onderliggende apparaten voor die bovenliggende ruimte opgehaald.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Een apparaat-id worden gegeven, haalt deze functie de onderliggende sensoren voor dat apparaat bovenliggende.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *apparaat-id* | `guid` | Apparaat-id |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Deze functie een ruimte-id wordt opgegeven, wordt de ruimte van de bovenliggende opgehaald.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | ruimte-id |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Deze functie een sensor-id wordt opgegeven, wordt de ruimte van de bovenliggende opgehaald.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | sensor-id |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Een apparaat-id worden gegeven, haalt deze functie de bovenliggende ruimte.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Apparaat-id |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Deze functie een sensor-id wordt opgegeven, wordt het bovenliggende apparaat opgehaald.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | sensor-id |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>⇒ getSpaceExtendedProperty (spaceId, propertyName) `extendedProperty`

Deze functie haalt krijgen een ruimte-id, de eigenschap en de waarde van de ruimte.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |
| *propertyName* | `string` | naam van de eigenschap ruimte |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>⇒ getSensorExtendedProperty (sensorId, propertyName) `extendedProperty`

Deze functie haalt krijgen een sensor-id, de eigenschap en de waarde van de sensor.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | sensor-id |
| *propertyName* | `string` | naam van de eigenschap sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>⇒ getDeviceExtendedProperty (apparaat-id, propertyName) `extendedProperty`

Deze functie haalt krijgt een apparaat-id, de eigenschap en de waarde van het apparaat.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *apparaat-id* | `guid` | Apparaat-id |
| *propertyName* | `string` | de eigenschap apparaatnaam |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue (sensorId, gegevenstype, waarde)

Deze functie wordt een waarde ingesteld op de sensor-object met het opgegeven gegevenstype.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | sensor-id |
| *Gegevenstype*  | `string` | gegevens van het sensortype |
| *value*  | `string` | Waarde |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue (spaceId, gegevenstype, waarde)

Deze functie stelt een waarde in de ruimte-object met het opgegeven gegevenstype.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | ruimte-id |
| *Gegevenstype* | `string` | Gegevenstype |
| *value* | `string` | Waarde |

### <a name="logmessage"></a>log(Message)

Deze functie registreert het volgende bericht in de gebruiker gedefinieerde functie.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *Bericht* | `string` | bericht moeten worden vastgelegd |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification (topologyObjectId, topologyObjectType, nettolading)

Deze functie verzendt een aangepast Meldingsbericht worden verzonden.

**Type**: globale functie

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Graph-object-id. Voorbeelden zijn schijfruimte, de sensor en apparaat-ID.|
| *topologyObjectType*  | `string` | Voorbeelden zijn sensor- en apparaatgegevens.|
| *nettolading*  | `string` | De JSON-nettolading met de melding wordt verzonden. |

## <a name="return-types"></a>Typen retourneren

De antwoord-modellen geretourneerd door de client verwijzing Help-methoden worden hieronder beschreven.

### <a name="space"></a>Ruimte

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "Space",
  "friendlyName": "Conference Room",
  "typeId": 0,
  "parentSpaceId": "00000000-0000-0000-0000-000000000001",
  "subtypeId": 0
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

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | naam van de uitgebreide eigenschap |

#### <a name="valuevaluename--value"></a>Value(valueName) ⇒ `value`

Deze functie retourneert de waarde van de huidige ruimte.

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *Waardenaam* | `string` | naam van de waarde |

#### <a name="historyvaluename--value"></a>History(valueName) ⇒ `value[]`

Deze functie retourneert de historische waarden van de huidige ruimte.

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *Waardenaam* | `string` | naam van de waarde |

#### <a name="notifypayload"></a>Notify(Payload)

Deze functie verzendt een melding met de nettolading van de opgegeven.

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *nettolading* | `string` | JSON-nettolading in de melding moeten worden opgenomen |

### <a name="device"></a>Apparaat

```JSON
{
  "id": "00000000-0000-0000-0000-000000000002",
  "name": "Device",
  "friendlyName": "Temperature Sensing Device",
  "description": "This device contains a sensor that captures temperature readings.",
  "type": "None",
  "subtype": "None",
  "typeId": 0,
  "subtypeId": 0,
  "hardwareId": "ABC123",
  "gatewayId": "ABC",
  "spaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Apparaatmethoden

#### <a name="parent--space"></a>Parent() ⇒ `space`

Deze functie geeft als resultaat van de bovenliggende ruimte van het huidige apparaat.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Deze functie retourneert de onderliggende sensoren van het huidige apparaat.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Deze functie retourneert de uitgebreide eigenschap en de waarde voor het huidige apparaat.

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | naam van de uitgebreide eigenschap |

#### <a name="notifypayload"></a>Notify(Payload)

Deze functie verzendt een melding met de nettolading van de opgegeven.

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *nettolading* | `string` | JSON-nettolading in de melding moeten worden opgenomen |

### <a name="sensor"></a>Sensor

```JSON
{
  "id": "00000000-0000-0000-0000-000000000003",
  "port": "30",
  "pollRate": 3600,
  "dataType": "Temperature",
  "dataSubtype": "None",
  "type": "Classic",
  "portType": "None",
  "dataUnitType": "FahrenheitTemperature",
  "spaceId": "00000000-0000-0000-0000-000000000000",
  "deviceId": "00000000-0000-0000-0000-000000000001",
  "portTypeId": 0,
  "dataUnitTypeId": 0,
  "dataTypeId": 0,
  "dataSubtypeId": 0,
  "typeId": 0  
}
```

### <a name="sensor-methods"></a>Sensor-methoden

#### <a name="space--space"></a>Space() ⇒ `space`

Deze functie geeft als resultaat van de bovenliggende ruimte van de huidige sensor.

#### <a name="device--device"></a>Device() ⇒ `device`

Deze functie retourneert het bovenliggende apparaat van de huidige sensor.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Deze functie retourneert de uitgebreide eigenschap en de waarde voor de huidige sensor.

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | naam van de uitgebreide eigenschap |

#### <a name="value--value"></a>Value() ⇒ `value`

Deze functie retourneert de waarde van de huidige sensor.

#### <a name="history--value"></a>History() ⇒ `value[]`

Deze functie retourneert de historische waarden van de huidige sensor.

#### <a name="notifypayload"></a>Notify(Payload)

Deze functie verzendt een melding met de nettolading van de opgegeven.

| Parameter  | Type                | Description  |
| ------ | ------------------- | ------------ |
| *nettolading* | `string` | JSON-nettolading in de melding moeten worden opgenomen |

### <a name="value"></a>Waarde

```JSON
{
  "dataType": "Temperature",
  "value": "70",
  "createdTime": ""
}
```

### <a name="extended-property"></a>Uitgebreide eigenschap

```JSON
{
  "name": "OccupancyStatus",
  "value": "Occupied"
}
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [digitale dubbels van Azure door de gebruiker gedefinieerde functies](./concepts-user-defined-functions.md).

- Informatie over [over het maken van de gebruiker gedefinieerde functies](./how-to-user-defined-functions.md).

- Informatie over [fouten opsporen in de gebruiker gedefinieerde functies](./how-to-diagnose-user-defined-functions.md).
