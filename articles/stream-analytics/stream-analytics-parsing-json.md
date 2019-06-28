---
title: Het parseren van JSON en AVRO in Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u werken met complexe gegevenstypen zoals matrices, JSON, opgemaakt CSV-gegevens.
services: stream-analytics
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: daf5b97e4ac586f89e5964ee16ee73c86f59b01d
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329364"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Parseren van JSON en Avro-gegevens in Azure Stream Analytics

Azure Stream Analytics ondersteuning voor verwerking van gebeurtenissen in de opmaak van CSV, JSON en Avro-gegevens. JSON- en Avro-gegevens kunnen worden onderverdeeld en bevatten enkele complexe typen zoals geneste objecten (records) en -matrices. 




## <a name="record-data-types"></a>Record-gegevenstypen
Record-gegevenstypen worden gebruikt om weer te geven van JSON en Avro matrices wanneer bijbehorende indelingen worden gebruikt in de invoer-gegevensstromen. Deze voorbeelden ziet u een voorbeeld van de sensor die invoer gebeurtenissen in JSON-indeling wordt gelezen. Hier volgt een voorbeeld van een enkelvoudige gebeurtenis:

```json
{
    "DeviceId" : "12345",
    "Location" :
    {
        "Lat": 47,
        "Long": 122
    },
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99,
        "SensorMetadata" : 
        {
        "Manufacturer":"ABC",
        "Version":"1.2.45"
        }
    }
}
```


### <a name="access-nested-fields-in-known-schema"></a>Toegang tot het geneste velden in bekende schema
Gebruik een puntnotering (.) eenvoudig toegang tot geneste velden rechtstreeks vanuit uw query. Deze query wordt bijvoorbeeld de breedtegraad en lengtegraad coördinaten onder de locatie-eigenschap in de voorgaande JSON-gegevens geselecteerd. De puntnotatie kan worden gebruikt om te navigeren van meerdere niveaus, zoals hieronder wordt weergegeven.

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long,
    SensorReadings.SensorMetadata.Version
FROM input
```

### <a name="select-all-properties"></a>Alle eigenschappen selecteren
U kunt de eigenschappen van het gebruik van een geneste record selecteren ' *' Jokertekens. Kijk een naar het volgende voorbeeld:

```SQL
SELECT input.Location.*
FROM input
```

Het resultaat is:

```json
{
    "Lat" : 47,
    "Long" : 122
}
```


### <a name="access-nested-fields-when-property-name-is-a-variable"></a>Toegang tot het geneste velden als naam van de eigenschap een variabele is
Gebruik de [GetRecordPropertyValue](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue) als naam van de eigenschap is een variabele. 

Denk bijvoorbeeld aan dat een voorbeeld van de gegevensstroom moet worden samengevoegd met verwijzing met gegevensdrempels voor elk apparaat sensor. Hieronder ziet u een fragment van dergelijke referentiegegevens.

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 75
}
```

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName
FROM input      -- stream input
JOIN thresholds -- reference data input
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
    -- the where statement selects the property value coming from the reference data
```

### <a name="convert-record-fields-into-separate-events"></a>Recordvelden converteren naar afzonderlijke gebeurtenissen
Als u wilt converteren recordvelden in afzonderlijke gebeurtenissen, gebruikt u de [toepassen](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) operator samen met de [GetRecordProperties](https://docs.microsoft.com/stream-analytics-query/getrecordproperties-azure-stream-analytics) functie. Bijvoorbeeld, als het vorige voorbeeld had meerdere records voor SensorReading, kan de volgende query kan worden gebruikt om op te halen ze in verschillende gebeurtenissen:

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```



## <a name="array-data-types"></a>Matrix-gegevenstypen

Matrix-gegevenstypen zijn een geordende reeks waarden. Sommige normale bewerkingen op basis van matrixwaarden worden hieronder beschreven. Deze voorbeelden wordt ervan uitgegaan dat de invoer gebeurtenissen hebben een eigenschap met de naam 'arrayField' dat wil zeggen een matrix-gegevenstype.

Deze voorbeelden gebruiken de functies [GetArrayElement](https://docs.microsoft.com/stream-analytics-query/getarrayelement-azure-stream-analytics), [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics), [GetArrayLength](https://docs.microsoft.com/stream-analytics-query/getarraylength-azure-stream-analytics), en de [toepassen](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) operator.

### <a name="working-with-a-specific-array-element"></a>Werken met een specifieke matrixelement
Selecteer matrixelement op een opgegeven index (waarbij u het eerste matrixelement):

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

### <a name="select-array-length"></a>Matrixlengte van de selecteren

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

### <a name="convert-array-elements-into-separate-events"></a>Matrixelementen converteren naar afzonderlijke gebeurtenissen
Selecteer alle matrixelement als afzonderlijke gebeurtenissen. De [toepassen](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) operator samen met de [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics) ingebouwde functie haalt alle matrixelementen als afzonderlijke gebeurtenissen:

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```


## <a name="see-also"></a>Zie ook
[Gegevenstypen in Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)
