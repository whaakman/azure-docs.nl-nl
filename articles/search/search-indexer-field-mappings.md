---
title: Veldtoewijzingen voor automatisch indexeren met indexeerfuncties - Azure Search
description: Azure Search-indexeerfunctie veldtoewijzingen voor verschillen in de veldnamen en gegevensweergaven configureren.
ms.date: 05/02/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 3546e342b535a122ea4ed3f844cd5e28a76d551a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147796"
---
# <a name="field-mappings-and-transformations-using-azure-search-indexers"></a>Veldtoewijzingen en transformaties met behulp van Azure Search-indexeerfuncties

Wanneer u Azure Search-indexeerfuncties, vindt u ook dat de ingevoerde gegevens vrij komt niet overeen met het schema van de doelindex. In deze gevallen kunt u **veldtoewijzingen** om te zetten om uw gegevens tijdens het indexeren.

Sommige situaties waarin veldtoewijzingen handig:

* Uw gegevensbron biedt een veld met de naam `_id`, maar Azure Search is niet toegestaan veldnamen die met een onderstrepingsteken beginnen. De veldtoewijzing van een kunt u effectief de naam van een veld wijzigen.
* U wilt om verschillende velden in de index van de dezelfde gegevens van gegevensbronnen te vullen. U wilt bijvoorbeeld verschillende analyzers van toepassing op deze velden.
* U wilt voor het vullen van een indexveld met gegevens van meer dan één gegevensbron en de gegevensbronnen van elk ander veldnamen gebruiken.
* U moet naar Base64 coderen of decoderen van uw gegevens. Veldtoewijzingen ondersteunen verschillende **toewijzing functies**, waaronder functies voor Base64 coderen en decoderen.

> [!NOTE]
> De functie voor het veld van Azure Search-indexeerfuncties biedt een eenvoudige manier om gegevensvelden toewijzen aan indexvelden, met een aantal opties voor gegevensconversie. Complexere gegevens mogelijk vooraf verwerken om te zetten om deze in een formulier die gemakkelijk te index.
>
> Microsoft Azure Data Factory is een krachtige cloud-gebaseerde oplossing voor het importeren en transformeren van gegevens. U kunt ook code om brongegevens te transformeren voordat het indexeren schrijven. Zie voor meer codevoorbeelden, [relationele gegevens modelleren](search-example-adventureworks-modeling.md) en [Model met meerdere niveaus facetten](search-example-adventureworks-multilevel-faceting.md).
>

## <a name="set-up-field-mappings"></a>Veldtoewijzingen instellen

De veldtoewijzing van een bestaat uit drie onderdelen:

1. Een `sourceFieldName`, die staat voor een veld in de gegevensbron. Deze eigenschap is vereist.
2. Een optionele `targetFieldName`, die een veld in uw search-index vertegenwoordigt. Als u dit weglaat, wordt de dezelfde naam als in de gegevensbron wordt gebruikt.
3. Een optionele `mappingFunction`, die kunt gebruiken om uw gegevens met behulp van een van verschillende vooraf gedefinieerde functies. De volledige lijst van functies is [hieronder](#mappingFunctions).

Veldtoewijzingen worden toegevoegd aan de `fieldMappings` matrix van de definitie van de indexeerfunctie.

## <a name="map-fields-using-the-rest-api"></a>Velden toewijzen met behulp van de REST-API

U kunt veldtoewijzingen toevoegen bij het maken van een nieuwe indexeerfunctie met de [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-Indexer) API-aanvraag. U kunt de veldtoewijzingen van een bestaande indexeerfunctie met beheren de [Update indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/update-indexer) API-aanvraag.

Dit is bijvoorbeeld het veld van een gegevensbron toewijzen aan een doelveld met een andere naam:

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
}
```

Het veld van een gegevensbron kan in meerdere veldtoewijzingen worden verwezen. Het volgende voorbeeld laat zien hoe een veld, de dezelfde bronveld kopiëren naar twee verschillende indexvelden een ' fork ':

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }
]
```

> [!NOTE]
> Azure Search maakt gebruik van niet-hoofdlettergevoelige vergelijking op te lossen de namen van het veld en functies in veldtoewijzingen. Dit is handig (u hebt niet meteen alle het hoofdlettergebruik), maar betekent dit dat uw gegevensbron of de index kan geen velden hebben die alleen per aanvraag verschillen.  
>
>

## <a name="map-fields-using-the-net-sdk"></a>Velden toewijzen met de .NET SDK

U veldtoewijzingen definiëren in de .NET-SDK met de [FieldMapping](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.fieldmapping) klasse de eigenschappen heeft `SourceFieldName` en `TargetFieldName`, en een optionele `MappingFunction` verwijzing.

U kunt veldtoewijzingen opgeven bij het maken van de indexeerfunctie of later door rechtstreeks de `Indexer.FieldMappings` eigenschap.

De volgende C# voorbeeld wordt de veldtoewijzingen ingesteld bij het maken van een indexeerfunctie.

```csharp
  List<FieldMapping> map = new List<FieldMapping> {
    // removes a leading underscore from a field name
    new FieldMapping("_custId", "custId"),
    // URL-encodes a field for use as the index key
    new FieldMapping("docPath", "docId", FieldMappingFunction.Base64Encode() )
  };

  Indexer sqlIndexer = new Indexer(
    name: "azure-sql-indexer",
    dataSourceName: sqlDataSource.Name,
    targetIndexName: index.Name,
    fieldMappings: map,
    schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

  await searchService.Indexers.CreateOrUpdateAsync(indexer);
```

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Veld toewijzing van functies

Een veld toewijzing functie transformeert de inhoud van een veld voordat deze wordt opgeslagen in de index. De volgende functies van de toewijzing wordt momenteel ondersteund:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

### <a name="base64encode-function"></a>de functie base64Encode

Voert *URL-safe* Base64-codering van de ingevoerde tekenreeks. Wordt ervan uitgegaan dat de invoer UTF-8 is-codering.

#### <a name="example---document-key-lookup"></a>Voorbeeld - document opzoeken van een sleutel

Alleen URL-safe tekens kunnen worden weergegeven in een Azure Search-documentsleutel (omdat klanten moeten kunnen adres het document met behulp van de [Lookup API](https://docs.microsoft.com/rest/api/searchservice/lookup-document) ). Als het bronveld voor uw sleutel-URL-onveilige tekens bevat, kunt u de `base64Encode` functie omzetten in op tijd te indexeren.

Wanneer u de gecodeerde sleutel tijdens het zoeken ophalen, kunt u vervolgens gebruiken de `base64Decode` functie voor het ophalen van de waarde van de oorspronkelijke sleutel, en wordt deze gebruikt om op te halen van de brondocument.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : {
      "name" : "base64Encode",
      "parameters" : { "useHttpServerUtilityUrlTokenEncode" : false }
    }
  }]
 ```

Als u niet een eigenschap van de parameters voor de functie voor toewijzing opgeeft, wordt de waarde standaard `{"useHttpServerUtilityUrlTokenEncode" : true}`.

Azure Search ondersteunt twee verschillende Base64-codering. U moet dezelfde parameters als coderen en decoderen van hetzelfde veld gebruiken. Zie voor meer informatie, [base64 coderingsopties](#base64details) om te bepalen welke parameters moeten worden gebruikt.

<a name="base64DecodeFunction"></a>

### <a name="base64decode-function"></a>de functie base64Decode

Voert het decoderen van Base64 van de ingevoerde tekenreeks. De invoer wordt ervan uitgegaan dat een *URL-safe* Base64-gecodeerde tekenreeks.

#### <a name="example---decode-blob-metadata-or-urls"></a>Voorbeeld: metagegevens van de blob of URL's decoderen

Uw gegevens mogelijk met Base64 gecodeerde tekenreeksen, zoals de blob-metagegevens tekenreeksen of web-URL's, die u wilt doorzoekbaar als tekst zonder opmaak bevatten. U kunt de `base64Decode` functie met de gecodeerde gegevens terug in gewone tekenreeksen tijdens het vullen van uw search-index.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { 
      "name" : "base64Decode", 
      "parameters" : { "useHttpServerUtilityUrlTokenDecode" : false }
    }
  }]
```

Als u een eigenschap parameters niet opgeeft, wordt de waarde standaard `{"useHttpServerUtilityUrlTokenEncode" : true}`.

Azure Search ondersteunt twee verschillende Base64-codering. U moet dezelfde parameters als coderen en decoderen van hetzelfde veld gebruiken. Zie voor meer informatie, [base64 coderingsopties](#base64details) om te bepalen welke parameters moeten worden gebruikt.

<a name="base64details"></a>

#### <a name="base64-encoding-options"></a>Base64-codering opties

Azure Search ondersteunt twee verschillende Base64-codering: **HttpServerUtility URL token**, en **URL-safe Base64-codering zonder opvulling**. Een tekenreeks is die wordt base64-gecodeerd tijdens het indexeren moet later worden ontsleuteld met de dezelfde coderingsopties, anders het resultaat niet overeen met de oorspronkelijke.

Als de `useHttpServerUtilityUrlTokenEncode` of `useHttpServerUtilityUrlTokenDecode` parameters voor het coderen en decoderen van respectievelijk zijn ingesteld op `true`, klikt u vervolgens `base64Encode` gedraagt zich als [HttpServerUtility.UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) en `base64Decode` gedraagt zich als [HttpServerUtility.UrlTokenDecode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx).

Als u niet het volledige .NET Framework gebruikt (dat wil zeggen, u gebruikmaakt van .NET Core of een andere framework) om te produceren de sleutelwaarden om Azure Search-gedrag te emuleren, moet u vervolgens ingesteld `useHttpServerUtilityUrlTokenEncode` en `useHttpServerUtilityUrlTokenDecode` naar `false`. De base64-codering en decodering van functies, afhankelijk van de bibliotheek die u gebruikt, kunnen verschillen van de die worden gebruikt door Azure Search.

De volgende tabel vergelijkt verschillende base64-codering van de tekenreeks `00>00?00`. Om te bepalen de extra verwerking die vereist (indien aanwezig) voor uw base64-functies, van toepassing uw bibliotheek gecodeerd functie op de tekenreeks `00>00?00` en vergelijkt u de uitvoer met de verwachte uitvoer `MDA-MDA_MDA`.

| Encoding | Met base64 coderen uitvoer | Extra verwerking na bibliotheek codering | Als u meer te worden verwerkt voordat het decoderen van bibliotheek |
| --- | --- | --- | --- |
| Met base64 met opvulling | `MDA+MDA/MDA=` | URL-safe tekens gebruiken en verwijderen van opvulling | Gebruik standaard base64-tekens en opvulling toevoegen |
| Met base64 zonder opvulling | `MDA+MDA/MDA` | URL-safe tekens gebruiken | Standard base64-tekens gebruiken |
| URL-safe base64 met opvulling | `MDA-MDA_MDA=` | Opvulling verwijderen | Opvulling toevoegen |
| URL-safe base64 zonder opvulling | `MDA-MDA_MDA` | Geen | Geen |

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition-function"></a>de functie extractTokenAtPosition

Hiermee wordt een tekenreeksveld met behulp van het opgegeven scheidingsteken en kiest het token op de opgegeven positie in de resulterende splitsen.

Deze functie maakt gebruik van de volgende parameters:

* `delimiter`: een tekenreeks die moet worden gebruikt als scheidingsteken als de ingevoerde tekenreeks te splitsen.
* `position`: een geheel getal op nul gebaseerde positie van het token om op te halen nadat de invoertekenreeks wordt gesplitst.

Bijvoorbeeld, als de invoer is `Jane Doe`, wordt de `delimiter` is `" "`(spatie) en de `position` 0 is, is het resultaat `Jane`; als de `position` 1, is het resultaat is `Doe`. Als de positie naar een token die niet bestaat verwijst, wordt er een fout geretourneerd.

#### <a name="example---extract-a-name"></a>Voorbeeld: extract een naam

De gegevensbron bevat een `PersonName` veld, en u wilt deze index als twee afzonderlijke `FirstName` en `LastName` velden. Deze functie kunt u de invoer met behulp van de spatie als scheidingsteken gesplitst.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } }
  },
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } }
  }]
```

<a name="jsonArrayToStringCollectionFunction"></a>

### <a name="jsonarraytostringcollection-function"></a>jsonArrayToStringCollection function

Zet een tekenreeks die is opgemaakt als een JSON-matrix van tekenreeksen in een string-matrix die kan worden gebruikt voor het vullen van een `Collection(Edm.String)` veld in de index.

Bijvoorbeeld, als de invoertekenreeks is `["red", "white", "blue"]`, klikt u vervolgens het doelveld van het type `Collection(Edm.String)` wordt gevuld met de drie waarden `red`, `white`, en `blue`. Voor de invoerwaarden die kunnen niet worden geparseerd als matrices van JSON-tekenreeks, een fout geretourneerd.

#### <a name="example---populate-collection-from-relational-data"></a>Voorbeeld - verzameling van relationele gegevens vullen

Azure SQL Database beschikt niet over een ingebouwde gegevenstype dat op een natuurlijke manier worden toegewezen aan `Collection(Edm.String)` velden in Azure Search. Om in te vullen tekenreeksvelden verzameling, kunt u uw brongegevens als een tekenreeksmatrix JSON vooraf verwerken en vervolgens met de `jsonArrayToStringCollection` functie toewijzen.

```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "tags", 
    "mappingFunction" : { "name" : "jsonArrayToStringCollection" }
  }]
```

Zie voor een gedetailleerd voorbeeld waarmee relationele gegevens worden getransformeerd in index verzameling velden, [relationele gegevens modelleren](search-example-adventureworks-modeling.md).
