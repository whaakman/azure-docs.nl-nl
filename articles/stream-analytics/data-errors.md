---
title: Azure Stream Analytics diagnostisch logboek gegevensfouten
description: In dit artikel wordt uitgelegd voor de andere invoer en uitvoer gegevensfouten die optreden kunnen bij het gebruik van Azure Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/09/2019
ms.openlocfilehash: b00eb12092838746f4bfe16f00eac55df9224b09
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65607226"
---
# <a name="azure-stream-analytics-data-errors"></a>Fouten met Azure Stream Analytics-gegevens

Wanneer er een verschil in de gegevens die wordt verwerkt door een Azure Stream Analytics-taak, verzendt Stream Analytics een foutgebeurtenis gegevens naar de diagnostische logboeken. Stream Analytics schrijft de gedetailleerde informatie en voorbeelden van gebeurtenissen, naar de logboeken met diagnostische gegevens wanneer er fouten optreden. Een overzicht van deze informatie is ook beschikbaar via de portalmeldingen voor enkele fouten.

In dit artikel bevat een overzicht van de verschillende typen, oorzaken en details van de diagnostische logboeken voor invoer-en uitvoergegevens fouten.

## <a name="diagnostic-log-schema"></a>Diagnostische logboeken schema

Zie [oplossen Azure Stream Analytics met behulp van logboeken met diagnostische gegevens](stream-analytics-job-diagnostic-logs.md#diagnostics-logs-schema) om te zien van het schema voor diagnostische logboeken. De volgende JSON is een van de voorbeeldwaarde voor de **eigenschappen** veld van een diagnostisch logboek voor een fout.

```json
{
    "Source": "InputTelemetryData",
    "Type": "DataError",
    "DataErrorType": "InputDeserializerError.InvalidData",
    "BriefMessage": "Json input stream should either be an array of objects or line separated objects. Found token type: Integer",
    "Message": "Input Message Id: https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt Error: Json input stream should either be an array of objects or line separated objects. Found token type: Integer",
    "ExampleEvents": "[\"1,2\\\\u000d\\\\u000a3,4\\\\u000d\\\\u000a5,6\"]",
    "FromTimestamp": "2019-03-22T22:34:18.5664937Z",
    "ToTimestamp": "2019-03-22T22:34:18.5965248Z",
    "EventCount": 1
}
```

## <a name="input-data-errors"></a>Invoergegevens fouten

### <a name="inputdeserializererrorinvalidcompressiontype"></a>InputDeserializerError.InvalidCompressionType

* Oorzaak: De invoer compressietype geselecteerd komt niet overeen met de gegevens.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Voer de bericht-id. De id is voor Event Hub, de PartitionId, Offset en volgnummer.

**Foutbericht**

```json
"BriefMessage": "Unable to decompress events from resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt'. Please ensure compression setting fits the data being processed."
```

### <a name="inputdeserializererrorinvalidheader"></a>InputDeserializerError.InvalidHeader

* Oorzaak: De koptekst van invoergegevens is ongeldig. Een CSV heeft bijvoorbeeld kolommen met dubbele namen.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Voer de bericht-id. 
   * De nettolading van werkelijke maximaal enkele kilobytes.

**Foutbericht**

```json
"BriefMessage": "Invalid CSV Header for resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt'. Please make sure there are no duplicate field names."
```

### <a name="inputdeserializererrormissingcolumns"></a>InputDeserializerError.MissingColumns

* Oorzaak: De ingevoerde kolommen gedefinieerd met CREATE TABLE of door de TIMESTAMP BY bestaat niet.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Voer de bericht-id. 
   * Namen van de kolommen die ontbreken. 
   * De nettolading van werkelijke maximaal enkele kilobytes.

**Foutberichten**

```json
"BriefMessage": "Could not deserialize the input event(s) from resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt' as Csv. Some possible reasons: 1) Malformed events 2) Input source configured with incorrect serialization format" 
```

```json
"Message": "Missing fields specified in query or in create table. Fields expected:ColumnA Fields found:ColumnB"
```

### <a name="inputdeserializererrortypeconversionerror"></a>InputDeserializerError.TypeConversionError

* Oorzaak: Kan niet converteren van de invoer naar het type dat is opgegeven in de instructie CREATE TABLE.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Voer de bericht-id. 
   * De naam van de kolom en het verwachte type.

**Foutberichten**

```json
"BriefMessage": "Could not deserialize the input event(s) from resource '''https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt ' as Csv. Some possible reasons: 1) Malformed events 2) Input source configured with incorrect serialization format" 
```

```json
"Message": "Unable to convert column: dateColumn to expected type."
```

### <a name="inputdeserializererrorinvaliddata"></a>InputDeserializerError.InvalidData

* Oorzaak: Invoergegevens is niet in de juiste indeling. De invoer is bijvoorbeeld niet geldig JSON.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Voer de bericht-id. 
   * De nettolading van werkelijke maximaal enkele kilobytes.

**Foutberichten**

```json
"BriefMessage": "Json input stream should either be an array of objects or line separated objects. Found token type: String"
```

```json
"Message": "Json input stream should either be an array of objects or line separated objects. Found token type: String"
```

### <a name="invalidinputtimestamp"></a>InvalidInputTimeStamp

* Oorzaak: De waarde van de TIMESTAMP BY-expressie kan niet worden geconverteerd naar datum/tijd.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Voer de bericht-id. 
   * Foutbericht. 
   * De nettolading van werkelijke maximaal enkele kilobytes.

**Foutbericht**

```json
"BriefMessage": "Unable to get timestamp for resource 'https:\\/\\/exampleBlob.blob.core.windows.net\\/inputfolder\\/csv.txt ' due to error 'Cannot convert string to datetime'"
```

### <a name="invalidinputtimestampkey"></a>InvalidInputTimeStampKey

* Oorzaak: De waarde van de TIMESTAMP BY OVER timestampColumn is NULL.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * De nettolading van de werkelijke maximaal enkele kilobytes.

**Foutbericht**

```json
"BriefMessage": "Unable to get value of TIMESTAMP BY OVER COLUMN"
```

### <a name="lateinputevent"></a>LateInputEvent

* Oorzaak: Het verschil tussen de tijd van de toepassing en ontvangsttijd is groter dan laat aankomst tolerantie-venster.
* Er is een portalmelding opgegeven: Nee
* Diagnostisch logboek-niveau: Informatie
* Logboekdetails
   * Tijd van de toepassing en aankomsttijd. 
   * De nettolading van werkelijke maximaal enkele kilobytes.

**Foutbericht**

```json
"BriefMessage": "Input event with application timestamp '2019-01-01' and arrival time '2019-01-02' was sent later than configured tolerance."
```

### <a name="earlyinputevent"></a>EarlyInputEvent

* Oorzaak: Het verschil tussen de tijd van de toepassing en ontvangsttijd is groter dan 5 minuten.
* Er is een portalmelding opgegeven: Nee
* Diagnostisch logboek-niveau: Informatie
* Logboekdetails
   * Tijd van de toepassing en aankomsttijd. 
   * De nettolading van werkelijke maximaal enkele kilobytes.

**Foutbericht**

```json
"BriefMessage": "Input event arrival time '2019-01-01' is earlier than input event application timestamp '2019-01-02' by more than 5 minutes."
```

### <a name="outoforderevent"></a>OutOfOrderEvent

* Oorzaak: Gebeurtenis wordt beschouwd als niet-geordende op basis van het venster niet de juiste volgorde tolerantie is gedefinieerd.
* Er is een portalmelding opgegeven: Nee
* Diagnostisch logboek-niveau: Informatie
* Logboekdetails
   * De nettolading van werkelijke maximaal enkele kilobytes.

**Foutbericht**

```json
"Message": "Out of order event(s) received."
```

## <a name="output-data-errors"></a>Gegevens uitvoerfouten

### <a name="outputdataconversionerrorrequiredcolumnmissing"></a>OutputDataConversionError.RequiredColumnMissing

* Oorzaak: De kolom die is vereist voor de uitvoer bestaat niet. Bijvoorbeeld, een kolom die is gedefinieerd als Azure Table PartitionKey does't bestaan.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * De naam van de kolom en de record-id of een gedeelte van de record.

**Foutbericht**

```json
"Message": "The output record does not contain primary key property: [deviceId] Ensure the query output contains the column [deviceId] with a unique non-empty string less than '255' characters."
```

### <a name="outputdataconversionerrorcolumnnameinvalid"></a>OutputDataConversionError.ColumnNameInvalid

* Oorzaak: De waarde in de kolom voldoen niet aan de uitvoer. Naam van de kolom is bijvoorbeeld niet een geldig Azure table-kolom.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * De naam van de kolom en record-id of een gedeelte van de record.

**Foutbericht**

```json
"Message": "Invalid property name #deviceIdValue. Please refer MSDN for Azure table property naming convention."
```

### <a name="outputdataconversionerrortypeconversionerror"></a>OutputDataConversionError.TypeConversionError

* Oorzaak: Een kolom kan niet worden geconverteerd naar een geldig type in de uitvoer. Bijvoorbeeld, is de waarde van kolom niet compatibel met beperkingen of dat is gedefinieerd in SQL-tabel.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Naam van de kolom.
   * Record-id of een gedeelte van de record.

**Foutbericht**

```json
"Message": "The column [id] value null or its type is invalid. Ensure to provide a unique non-empty string less than '255' characters."
```

### <a name="outputdataconversionerrorrecordexceededsizelimit"></a>OutputDataConversionError.RecordExceededSizeLimit

* Oorzaak: De waarde van het bericht is groter dan de uitvoergrootte van de ondersteunde. Bijvoorbeeld is een record groter dan 1 MB voor een Event Hub-uitvoer.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Record-id of een gedeelte van de record.

**Foutbericht**

```json
"BriefMessage": "Single output event exceeds the maximum message size limit allowed (262144 bytes) by Event Hub."
```

### <a name="outputdataconversionerrorduplicatekey"></a>OutputDataConversionError.DuplicateKey

* Oorzaak: Een record bevat al een kolom met dezelfde naam als een systeemkolom. Bijvoorbeeld, CosmosDB-uitvoer met een kolom met de naam ID als ID-kolom naar een andere kolom is.
* Er is een portalmelding opgegeven: Ja
* Diagnostisch logboek-niveau: Waarschuwing
* Logboekdetails
   * Naam van de kolom.
   * Record-id of een gedeelte van de record.

```json
"BriefMessage": "Column 'devicePartitionKey' is being mapped to multiple columns."
```

## <a name="next-steps"></a>Volgende stappen

* [Azure Stream Analytics oplossen met behulp van logboeken met diagnostische gegevens](stream-analytics-job-diagnostic-logs.md)

* [Informatie over Stream Analytics-taak controleren en bewaken van query 's](stream-analytics-monitoring.md)
