---
title: Veld-verwijzingen in Azure Search indexeerfuncties
description: Azure Search-indexeerfunctie veldtoewijzingen voor verschillen in de veldnamen en gegevensweergaven configureren
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 0325a4de-0190-4dd5-a64d-4e56601d973b
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/30/2017
ms.author: eugenesh
ms.openlocfilehash: 3f2ead208ea1525489a40d1fb637da47cd8a9b24
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="field-mappings-in-azure-search-indexers"></a>Veld-verwijzingen in Azure Search indexeerfuncties
Wanneer u Azure Search indexeerfuncties, kunt u zelf zo nu en dan vinden in situaties waarin de invoergegevens behoorlijk het schema van de doelindex komt niet overeen. In deze gevallen kunt u **veld toewijzingen** voor het transformeren van uw gegevens in de gewenste vorm.

Sommige situaties waarin het veldtoewijzingen handig:

* De gegevensbron bevat een veld `_id`, maar Azure Search mag geen veldnamen die beginnen met een onderstrepingsteken. Een veldtoewijzing kunt u een veld ' naam '.
* Wilt u verschillende velden in de index met dezelfde gegevens brongegevens, bijvoorbeeld omdat u wilt toepassen van andere analyzers deze velden invullen. Veldtoewijzingen kunnen u een gegevensbronveld ' vertakken'.
* U moet naar Base64 coderen of uw gegevens worden ontsleuteld. Veldtoewijzingen ondersteuning voor diverse **toewijzing functies**, waaronder functies voor Base64 coderen en decoderen.   

## <a name="setting-up-field-mappings"></a>Instellen van veld-verwijzingen
U kunt veldtoewijzingen toevoegen bij het maken van een nieuw indexeerfunctie met de [indexeerfunctie maken](https://msdn.microsoft.com/library/azure/dn946899.aspx) API. U kunt beheren veldtoewijzingen op een indexering indexeerfunctie met behulp van de [Update indexeerfunctie](https://msdn.microsoft.com/library/azure/dn946892.aspx) API.

Een veldtoewijzing bestaat uit drie delen:

1. Een `sourceFieldName`, die staat voor een veld in de gegevensbron. Deze eigenschap is vereist.
2. Een optionele `targetFieldName`, die staat voor een veld in uw search-index. Als u dit weglaat, wordt de dezelfde naam als in de gegevensbron wordt gebruikt.
3. Een optionele `mappingFunction`, die kunt transformeert u uw gegevens met behulp van een van de vooraf gedefinieerde functies. De volledige lijst met functies is [hieronder](#mappingFunctions).

Velden toewijzingen worden toegevoegd aan de `fieldMappings` matrix op de definitie van de indexeerfunctie.

Dit is hoe u aankan verschillen in veldnamen:

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

Een indexeerfunctie kan meerdere veldtoewijzingen hebben. Hier volgt een voorbeeld van hoe u kunt "vertakken' een veld:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" },
]
```

> [!NOTE]
> Niet-hoofdlettergevoelige vergelijking Azure Search gebruikt voor het veld en functie naamomzetting in veld-verwijzingen. Dit is handig (u hoeft het hoofdlettergebruik direct), maar het betekent dat de gegevensbron en index kan geen velden hebben die alleen geval verschillen.  
>
>

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>Veld toewijzing functies
Deze functies worden momenteel ondersteund:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

## <a name="base64encode"></a>base64Encode
Voert *URL-safe* Base64-codering van de invoertekenreeks. Wordt ervan uitgegaan dat de invoer UTF-8 is-codering.

### <a name="sample-use-case---document-key-lookup"></a>Voorbeeld gebruiksvoorbeeld - document opzoeken van een sleutel
Alleen URL-safe tekens kunnen worden weergegeven in een Azure Search documentsleutel (omdat klanten moeten kunnen voor het oplossen van het document met behulp van de [Lookup API](https://docs.microsoft.com/rest/api/searchservice/lookup-document), bijvoorbeeld). Als uw gegevens URL onveilige tekens bevat en u gebruiken wilt voor het vullen van een sleutelveld in uw search-index, moet u deze functie gebruiken. Zodra de sleutel is gecodeerd, kunt u het decoderen van base64 om op te halen van de oorspronkelijke waarde. Zie voor meer informatie de [base64-codering en decodering](#base64details) sectie.

#### <a name="example"></a>Voorbeeld
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : { "name" : "base64Encode" }
  }]
```

### <a name="sample-use-case---retrieve-original-key"></a>Voorbeeld gebruiksvoorbeeld - de oorspronkelijke sleutel ophalen
U hebt een blob-indexeerfunctie die indexen blobs met de metagegevens van de blob pad als de documentsleutel. Bij het ophalen van de gecodeerde documentsleutel, die u wilt decoderen van het pad en de blob te downloaden.

#### <a name="example"></a>Voorbeeld
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "SourceKey",
    "targetFieldName" : "IndexKey",
    "mappingFunction" : { "name" : "base64Encode", "parameters" : { "useHttpServerUtilityUrlTokenEncode" : false } }
  }]
```

Als u niet nodig hebt om te zoeken documenten met sleutels en ook niet nodig hebt tot decoderen van de gecodeerde inhoud, kunt u zojuist hebt laten uit `parameters` voor de functie voor toewijzing, die standaard `useHttpServerUtilityUrlTokenEncode` naar `true`. Raadpleeg anders [base64 details](#base64details) sectie om te bepalen welke instellingen u wilt gebruiken.

<a name="base64DecodeFunction"></a>

## <a name="base64decode"></a>base64Decode
Voert het decoderen van Base64-tekenreeks van de invoer. De invoer wordt ervan uitgegaan dat een *URL-safe* Base64-gecodeerde tekenreeks.

### <a name="sample-use-case"></a>Voorbeeld gebruiksvoorbeeld
BLOB aangepaste metagegevenswaarden moet ASCII-codering. U kunt een Base64-codering gebruiken om aan te duiden willekeurige UTF-8-tekenreeksen in de blob aangepaste metagegevens. U kunt echter om zoeken af te stemmen, deze functie gebruiken om de gecodeerde gegevens terug in 'reguliere' tekenreeksen wanneer uw search-index te vullen.

#### <a name="example"></a>Voorbeeld
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode", "parameters" : { "useHttpServerUtilityUrlTokenDecode" : false } }
  }]
```

Als u een geen opgeeft `parameters`, klikt u vervolgens de standaardwaarde van `useHttpServerUtilityUrlTokenDecode` is `true`. Zie [base64 details](#base64details) sectie om te bepalen welke instellingen u wilt gebruiken.

<a name="base64details"></a>

### <a name="details-of-base64-encoding-and-decoding"></a>Details van base64-codering en decodering
Azure Search ondersteunt twee base64-coderingen: HttpServerUtility URL token en URL-safe base64-codering zonder opvulling. U moet dezelfde codering gebruiken als de toewijzing van functies als u wilt coderen van een documentsleutel voor het uiterlijk van, coderen een waarde om te worden gedecodeerd door de indexeerfunctie of een veld dat is gecodeerd door de indexeerfunctie worden ontsleuteld.

Als u .NET Framework gebruiken, kunt u instellen `useHttpServerUtilityUrlTokenEncode` en `useHttpServerUtilityUrlTokenDecode` naar `true`, voor het coderen en decoderen van respectievelijk. Vervolgens `base64Encode` gedraagt zich als [HttpServerUtility.UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) en `base64Decode` gedraagt zich als [HttpServerUtility.UrlTokenDecode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokendecode.aspx).

Als u .NET Framework niet gebruikt, wordt u moet instellen `useHttpServerUtilityUrlTokenEncode` en `useHttpServerUtilityUrlTokenDecode` naar `false`. Afhankelijk van de bibliotheek die u gebruikt de base64 coderen en decoderen hulpprogramma functies kunnen afwijken van de Azure Search.

De volgende tabel vergelijkt verschillende base64-coderingen van de tekenreeks `00>00?00`. Toepassen om te bepalen de vereiste extra verwerking (indien aanwezig) voor uw base64-functies, de tapewisselaar coderen functie op de tekenreeks `00>00?00` en vergelijkt u de uitvoer met de verwachte uitvoer `MDA-MDA_MDA`.

| Encoding | Base64-gecodeerd uitvoer | Extra verwerking na bibliotheek codering | Extra verwerking voordat het decoderen van de bibliotheek |
| --- | --- | --- | --- |
| Base64 met opvulling | `MDA+MDA/MDA=` | URL-safe tekens gebruiken en opvulling verwijderen | Gebruik standaard base64-tekens en opvulling toevoegen |
| Base64 zonder opvulling | `MDA+MDA/MDA` | URL-safe tekens gebruiken | Gebruik standaard base64-tekens |
| URL-safe base64 met opvulling | `MDA-MDA_MDA=` | Opvulling verwijderen | Opvulling toevoegen |
| URL-safe base64 zonder opvulling | `MDA-MDA_MDA` | Geen | Geen |

<a name="extractTokenAtPositionFunction"></a>

## <a name="extracttokenatposition"></a>extractTokenAtPosition
Een string-veld met het opgegeven scheidingsteken splitst en neemt over van het token van de opgegeven positie in de resulterende splitsen.

Bijvoorbeeld, als de invoer is `Jane Doe`, wordt de `delimiter` is `" "`(spatie) en de `position` 0 is, het resultaat is `Jane`; als de `position` 1, is het resultaat is `Doe`. Als de positie naar een token dat niet bestaat verwijst, wordt er een fout geretourneerd.

### <a name="sample-use-case"></a>Voorbeeld gebruiksvoorbeeld
De gegevensbron bevat een `PersonName` veld en u wilt deze index als twee aparte `FirstName` en `LastName` velden. U kunt deze functie gebruiken voor het scheiden van de invoer met een spatie als scheidingsteken.

### <a name="parameters"></a>Parameters
* `delimiter`: een tekenreeks die moet worden gebruikt als scheidingsteken wanneer de invoerreeks splitsen.
* `position`: een geheel getal op nul gebaseerde positie van het token om op te halen nadat de invoertekenreeks is gesplitst.    

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
Zet een tekenreeks die is opgemaakt als een JSON-matrix met tekenreeksen in een tekenreeksmatrix die kan worden gebruikt voor het vullen van een `Collection(Edm.String)` veld in de index.

Bijvoorbeeld, als de invoertekenreeks is `["red", "white", "blue"]`, klikt u vervolgens het doelveld van het type `Collection(Edm.String)` wordt gevuld met de drie waarden `red`, `white`, en `blue`. Voor invoerwaarden kunnen niet worden geparseerd als matrices van JSON-tekenreeks, een fout geretourneerd.

### <a name="sample-use-case"></a>Voorbeeld gebruiksvoorbeeld
Azure SQL-database beschikt niet over een ingebouwde gegevenstype dat is natuurlijk is toegewezen aan `Collection(Edm.String)` velden in Azure Search. Indeling van de brongegevens als een tekenreeksmatrix JSON om in te vullen tekenreeksvelden verzameling, en deze functie wilt gebruiken.

### <a name="example"></a>Voorbeeld
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```


## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search te verbeteren
Als u functieaanvragen of suggesties voor verbeteringen hebt, kunt contact met ons op onze [UserVoice site](https://feedback.azure.com/forums/263029-azure-search/).
