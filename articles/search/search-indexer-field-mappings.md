---
title: Veldtoewijzingen voor automatisch indexeren met indexeerfuncties - Azure Search
description: Azure Search-indexeerfunctie veldtoewijzingen voor verschillen in de veldnamen en gegevensweergaven configureren.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 94a7274c0e950661d118ad5421e7c763fe555434
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312694"
---
# <a name="field-mappings-in-azure-search-indexers"></a>Veldtoewijzingen in indexeerfuncties in Azure Search
Bij het gebruik van Azure Search-indexeerfuncties vindt van tijd tot tijd u uzelf in situaties waarin de invoergegevens niet helemaal overeen met het schema van de doelindex. In deze gevallen kunt u **veldtoewijzingen** het transformeren van uw gegevens in de gewenste vorm.

Sommige situaties waarin veldtoewijzingen handig:

* Uw gegevensbron biedt een veld `_id`, maar Azure Search is niet toegestaan veldnamen die beginnen met een onderstrepingsteken. De veldtoewijzing van een kunt u een veld 'naam'.
* U wilt vullen van verschillende velden in de index met de dezelfde gegevens brongegevens, bijvoorbeeld omdat u wilt toepassen van andere analyzers aan deze velden. Veldtoewijzingen kunnen u 'splitsen' van het veld voor een gegevensbron.
* U moet naar Base64 coderen of decoderen van uw gegevens. Veldtoewijzingen ondersteunen verschillende **toewijzing functies**, waaronder functies voor Base64 coderen en decoderen.   

## <a name="setting-up-field-mappings"></a>Instellen van veldtoewijzingen
U kunt veldtoewijzingen toevoegen bij het maken van een nieuwe indexeerfunctie met de [indexeerfunctie maken](https://msdn.microsoft.com/library/azure/dn946899.aspx) API. U kunt beheren veldtoewijzingen op een indexering indexeerfunctie met behulp van de [Update indexeerfunctie](https://msdn.microsoft.com/library/azure/dn946892.aspx) API.

De veldtoewijzing van een bestaat uit drie onderdelen:

1. Een `sourceFieldName`, die staat voor een veld in de gegevensbron. Deze eigenschap is vereist.
2. Een optionele `targetFieldName`, die een veld in uw search-index vertegenwoordigt. Als u dit weglaat, wordt de dezelfde naam als in de gegevensbron wordt gebruikt.
3. Een optionele `mappingFunction`, die kunt gebruiken om uw gegevens met behulp van een van verschillende vooraf gedefinieerde functies. De volledige lijst van functies is [hieronder](#mappingFunctions).

Velden toewijzingen worden toegevoegd aan de `fieldMappings` matrix op de definitie van de indexeerfunctie.

Dit is hoe u aankan verschillen in de namen van velden:

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

Een indexeerfunctie kan meerdere veldtoewijzingen hebben. Hier volgt een voorbeeld van hoe u kunt "splitsen" een veld:

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

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Veld toewijzing van functies
Deze functies worden momenteel ondersteund:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

## <a name="base64encode"></a>base64Encode
Voert *URL-safe* Base64-codering van de ingevoerde tekenreeks. Wordt ervan uitgegaan dat de invoer UTF-8 is-codering.

### <a name="sample-use-case---document-key-lookup"></a>Voorbeeld van use-case - document opzoeken van een sleutel
Alleen URL-safe tekens kunnen worden weergegeven in een Azure Search-documentsleutel (omdat klanten moeten kunnen adres het document met behulp van de [Lookup API](https://docs.microsoft.com/rest/api/searchservice/lookup-document), bijvoorbeeld). Als uw gegevens URL onveilige tekens bevat en u wilt gebruiken voor het vullen van een sleutelveld in uw search-index, moet u deze functie gebruiken. Nadat de sleutel is gecodeerd, kunt u met base64 decoderen om op te halen van de oorspronkelijke waarde. Zie voor meer informatie, de [base64 coderen en decoderen](#base64details) sectie.

#### <a name="example"></a>Voorbeeld
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : { "name" : "base64Encode" }
  }]
```

### <a name="sample-use-case---retrieve-original-key"></a>Voorbeeld van een use-case - oorspronkelijke sleutel ophalen
Hebt u een blob-indexeerfunctie die indexen blobs met de metagegevens van de blob-pad als de documentsleutel. Bij het ophalen van de gecodeerde documentsleutel, die u wilt decoderen van het pad en de blob te downloaden.

#### <a name="example"></a>Voorbeeld
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : { "name" : "base64Encode", "parameters" : { "useHttpServerUtilityUrlTokenEncode" : false } }
  }]
 ```

Als u niet wilt opzoeken van documenten door sleutels en ook geen moet moet worden gedecodeerd de gecodeerde inhoud, u kunt alleen weglaat `parameters` voor de toewijzing-functie die standaard `useHttpServerUtilityUrlTokenEncode` naar `true`. Raadpleeg anders [base64 details](#base64details) sectie om te bepalen welke instellingen u wilt gebruiken.

<a name="base64DecodeFunction"></a>

## <a name="base64decode"></a>base64Decode
Voert het decoderen van Base64 van de ingevoerde tekenreeks. De invoer wordt ervan uitgegaan dat een *URL-safe* Base64-gecodeerde tekenreeks.

### <a name="sample-use-case"></a>Voorbeeld van use-case
BLOB aangepaste metagegevenswaarden moet ASCII-codering. U kunt een Base64-codering gebruiken om weer te geven van willekeurige UTF-8-tekenreeksen in aangepaste metagegevens van de blob. Echter om search af te stemmen, kunt u deze functie gebruiken om de gecodeerde gegevens terug in "normale" tekenreeksen tijdens het vullen van uw search-index.

#### <a name="example"></a>Voorbeeld
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode", "parameters" : { "useHttpServerUtilityUrlTokenDecode" : false } }
  }]
```

Als u geen dat een opgeeft `parameters`, klikt u vervolgens de standaardwaarde van `useHttpServerUtilityUrlTokenDecode` is `true`. Zie [base64 details](#base64details) sectie om te bepalen welke instellingen u wilt gebruiken.

<a name="base64details"></a>

### <a name="details-of-base64-encoding-and-decoding"></a>Details van base64 coderen en decoderen
Azure Search ondersteunt twee base64-codering: HttpServerUtility URL-token en URL-safe base64-codering zonder opvulling. U moet de dezelfde codering gebruiken als de toewijzing van functies als u wilt coderen van een documentsleutel voor het zoeken van, een waarde die moet worden gedecodeerd door de indexeerfunctie coderen of een veld door de indexeerfunctie gecodeerde decoderen.

Als `useHttpServerUtilityUrlTokenEncode` of `useHttpServerUtilityUrlTokenDecode` parameters voor het coderen en decoderen van respectievelijk zijn ingesteld op `true`, klikt u vervolgens `base64Encode` gedraagt zich als [HttpServerUtility.UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) en `base64Decode` gedraagt zich als [HttpServerUtility.UrlTokenDecode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx).

Als u niet het volledige .NET Framework gebruikt (dat wil zeggen, u gebruikmaakt van .NET Core of andere programmeeromgeving) om te produceren de sleutelwaarden om Azure Search-gedrag te emuleren, moet u vervolgens ingesteld `useHttpServerUtilityUrlTokenEncode` en `useHttpServerUtilityUrlTokenDecode` naar `false`. Afhankelijk van de bibliotheek die u gebruikt, de met base64 coderen en decoderen hulpprogramma functies kunnen afwijken van de Azure Search.

De volgende tabel vergelijkt verschillende base64-codering van de tekenreeks `00>00?00`. Om te bepalen de extra verwerking die vereist (indien aanwezig) voor uw base64-functies, van toepassing uw bibliotheek gecodeerd functie op de tekenreeks `00>00?00` en vergelijkt u de uitvoer met de verwachte uitvoer `MDA-MDA_MDA`.

| Encoding | Met base64 coderen uitvoer | Extra verwerking na bibliotheek codering | Als u meer te worden verwerkt voordat het decoderen van bibliotheek |
| --- | --- | --- | --- |
| Met base64 met opvulling | `MDA+MDA/MDA=` | URL-safe tekens gebruiken en verwijderen van opvulling | Gebruik standaard base64-tekens en opvulling toevoegen |
| Met base64 zonder opvulling | `MDA+MDA/MDA` | URL-safe tekens gebruiken | Standard base64-tekens gebruiken |
| URL-safe base64 met opvulling | `MDA-MDA_MDA=` | Opvulling verwijderen | Opvulling toevoegen |
| URL-safe base64 zonder opvulling | `MDA-MDA_MDA` | Geen | Geen |

<a name="extractTokenAtPositionFunction"></a>

## <a name="extracttokenatposition"></a>extractTokenAtPosition
Hiermee wordt een tekenreeksveld met behulp van het opgegeven scheidingsteken en kiest het token op de opgegeven positie in de resulterende splitsen.

Bijvoorbeeld, als de invoer is `Jane Doe`, wordt de `delimiter` is `" "`(spatie) en de `position` 0 is, is het resultaat `Jane`; als de `position` 1, is het resultaat is `Doe`. Als de positie naar een token die niet bestaat verwijst, wordt er een fout geretourneerd.

### <a name="sample-use-case"></a>Voorbeeld van use-case
De gegevensbron bevat een `PersonName` veld, en u wilt deze index als twee afzonderlijke `FirstName` en `LastName` velden. Deze functie kunt u de invoer met behulp van de spatie als scheidingsteken gesplitst.

### <a name="parameters"></a>Parameters
* `delimiter`: een tekenreeks die moet worden gebruikt als scheidingsteken als de ingevoerde tekenreeks te splitsen.
* `position`: een geheel getal op nul gebaseerde positie van het token om op te halen nadat de invoertekenreeks wordt gesplitst.    

### <a name="example"></a>Voorbeeld
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

## <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection
Zet een tekenreeks die is opgemaakt als een JSON-matrix van tekenreeksen in een string-matrix die kan worden gebruikt voor het vullen van een `Collection(Edm.String)` veld in de index.

Bijvoorbeeld, als de invoertekenreeks is `["red", "white", "blue"]`, klikt u vervolgens het doelveld van het type `Collection(Edm.String)` wordt gevuld met de drie waarden `red`, `white`, en `blue`. Voor de invoerwaarden die kunnen niet worden geparseerd als matrices van JSON-tekenreeks, een fout geretourneerd.

### <a name="sample-use-case"></a>Voorbeeld van use-case
Azure SQL database beschikt niet over een ingebouwde gegevenstype dat op een natuurlijke manier worden toegewezen aan `Collection(Edm.String)` velden in Azure Search. Tekenreeks verzameling velden te vullen, uw gegevens als een tekenreeksmatrix JSON-indeling en deze functie wilt gebruiken.

### <a name="example"></a>Voorbeeld
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```


## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search verbeteren
Als u functieverzoeken heeft of suggesties voor verbeteringen hebt, neem contact met ons opnemen op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).
