---
title: Blobs toevoegen aan objecten in Azure, digitale dubbels | Microsoft Docs
description: Informatie over het toevoegen van blobs op objecten in digitale dubbels van Azure.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: ffd7d71c33b569b396b9f8babf8105968ee525b9
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263064"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Blobs toevoegen aan objecten in Azure, digitale dubbels

BLOBs zijn niet-gestructureerde representaties van algemene bestandstypen, zoals afbeeldingen en Logboeken. BLOBs bijhouden wat voor soort gegevens die ze geven met behulp van een MIME-type (bijvoorbeeld: ' afbeelding/jpeg') en metagegevens (naam, beschrijving, type, enzovoort).

Azure ondersteunt het digitale dubbels blobs te koppelen aan apparaten, spaties en gebruikers. BLOBs kunnen bestaan uit een profielafbeelding voor een gebruiker, apparaat foto, een video, een kaart, een firmware-zip, JSON-gegevens, een logboek, enzovoort.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-overview"></a>Overzicht van de blobs uploaden

Gedeeltelijk aanvragen kunt u blobs uploaden naar specifieke eindpunten en hun respectieve functionaliteiten.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>De metagegevens van de blob

Naast **Content-Type** en **Content-Disposition**, meerdelige blobaanvragen digitale dubbels Azure de juiste JSON-hoofdtekst moeten opgeven. Welke JSON-hoofdtekst om in te dienen, is afhankelijk van het type HTTP-aanvraagbewerking die wordt uitgevoerd.

De vier belangrijkste JSON schema's zijn:

![JSON-schema][1]

JSON-blob-metagegevens voldoet aan de volgende model:

```JSON
{
    "parentId": "00000000-0000-0000-0000-000000000000",
    "name": "My First Blob",
    "type": "Map",
    "subtype": "GenericMap",
    "description": "A well chosen description",
    "sharing": "None"
  }
```

| Kenmerk | Type | Description |
| --- | --- | --- |
| **parentId** | Reeks | De bovenliggende entiteit te koppelen aan de blob (spaties, apparaten of gebruikers) |
| **De naam** |Reeks | Een human-beschrijvende naam voor de blob |
| **type** | Reeks | Kan geen gebruik van het type van de blob - *type* en *typeId*  |
| **typeId** | Geheel getal | Kan niet worden gebruikt door de ID van het type blob - *type* en *typeId* |
| **subtype** | Reeks | Kan niet worden gebruikt door het subtype van het blob - *subtype* en *subtypeId* |
| **subtypeId** | Geheel getal | Kan niet worden gebruikt door de subtype-ID voor de blob - *subtype* en *subtypeId* |
| **Beschrijving** | Reeks | Aangepaste beschrijving van de blob |
| **delen** | Reeks | Of de blob kan worden gedeeld - enum [`None`, `Tree`, `Global`] |

Metagegevens van de BLOB wordt altijd opgegeven als de eerste chunk met **Content-Type** `application/json` of als een `.json` bestand. Gegevens uit een bestand is opgegeven in de tweede chunk en van alle ondersteunde MIME-type kan zijn.

De Swagger-documentatie beschreven deze model schema's in de volledige details.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Meer informatie over het gebruik van de naslagdocumentatie door te lezen [over het gebruik van Swagger](./how-to-use-swagger.md).

<div id="blobModel"></div>

### <a name="blobs-response-data"></a>Antwoordgegevens voor blobs

Afzonderlijk geretourneerde blobs voldoen aan de volgende JSON-schema:

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "parentId": "00000000-0000-0000-0000-000000000000",
  "type": "string",
  "subtype": "string",
  "typeId": 0,
  "subtypeId": 0,
  "sharing": "None",
  "description": "string",
  "contentInfos": [
    {
      "type": "string",
      "sizeBytes": 0,
      "mD5": "string",
      "version": "string",
      "lastModifiedUtc": "2019-01-12T00:58:08.689Z",
      "metadata": {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    }
  ],
  "fullName": "string",
  "spacePaths": [
    "string"
  ]
}
```

| Kenmerk | Type | Description |
| --- | --- | --- |
| **id** | Reeks | De unieke id voor de blob |
| **De naam** |Reeks | Een human-beschrijvende naam voor de blob |
| **parentId** | Reeks | De bovenliggende entiteit te koppelen aan de blob (spaties, apparaten of gebruikers) |
| **type** | Reeks | Kan geen gebruik van het type van de blob - *type* en *typeId*  |
| **typeId** | Geheel getal | Kan niet worden gebruikt door de ID van het type blob - *type* en *typeId* |
| **subtype** | Reeks | Kan niet worden gebruikt door het subtype van het blob - *subtype* en *subtypeId* |
| **subtypeId** | Geheel getal | Kan niet worden gebruikt door de subtype-ID voor de blob - *subtype* en *subtypeId* |
| **delen** | Reeks | Of de blob kan worden gedeeld - enum [`None`, `Tree`, `Global`] |
| **Beschrijving** | Reeks | Aangepaste beschrijving van de blob |
| **contentInfos** | Matrix | Hiermee geeft u de metagegevens van niet-gestructureerde gegevens met inbegrip van versie |
| **Volledige naam** | Reeks | De volledige naam van de blob |
| **spacePaths** | Reeks | Het pad van de ruimte |

Metagegevens van de BLOB wordt altijd opgegeven als de eerste chunk met **Content-Type** `application/json` of als een `.json` bestand. Gegevens uit een bestand is opgegeven in de tweede chunk en van alle ondersteunde MIME-type kan zijn.

### <a name="blob-multipart-request-examples"></a>Voorbeelden van BLOB-multipart aanvraag

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Als u wilt een tekstbestand als een blob uploaden en deze koppelen aan een spatie, moet u een geverifieerde HTTP POST-aanvraag naar:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs
```

Met de volgende tekst:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "ParentId": "54213cf5-285f-e611-80c3-000d3a320e1e",
  "Name": "My First Blob",
  "Type": "Map",
  "SubType": "GenericMap",
  "Description": "A well chosen description",
  "Sharing": "None"
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="myblob.txt"
Content-Type: text/plain

This is my blob content. In this case, some text, but I could also be uploading a picture, an JSON file, a firmware zip, etc.

--USER_DEFINED_BOUNDARY--
```

| Waarde | Vervangen door |
| --- | --- |
| USER_DEFINED_BOUNDARY | De naam van een meerdelige inhoud grens |

De volgende code is een .NET-implementatie van de dezelfde blob-upload, met behulp van de klasse [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

//MY_BLOB.txt is the String representation of your text file
var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

Ten slotte [cURL](https://curl.haxx.se/) kunnen gebruikers aanvragen meerdelige formulier aanbrengen op dezelfde manier:

![Apparaat-blobs][5]

```bash
curl
 -X POST "YOUR_MANAGEMENT_API_URL/spaces/blobs"
 -H "Authorization: Bearer YOUR_TOKEN"
 -H "Accept: application/json"
 -H "Content-Type: multipart/form-data"
 -F "meta={\"ParentId\": \"YOUR_SPACE_ID\",\"Name\":\"My CURL Blob",\"Type\":\"Map\",\"SubType\":\"GenericMap\",\"Description\": \"A well chosen description\", \"Sharing\": \"None\"};type=application/json"
 -F "text=PATH_TO_FILE;type=text/plain"
```

| Waarde | Vervangen door |
| --- | --- |
| YOUR_TOKEN | Uw geldige OAuth 2.0-token |
| YOUR_SPACE_ID | De ID van de ruimte moet het koppelen van de blob met |
| PATH_TO_FILE | Het pad naar het tekstbestand |

Een geslaagde bericht retourneert de ID van de nieuwe de blob (eerder zijn besproken in het rood).

## <a name="api-endpoints"></a>API-eindpunten

De volgende secties beschrijven de core blob-gerelateerde API-eindpunten en hun functies.

### <a name="devices"></a>Apparaten

U kunt blobs koppelen aan apparaten. De volgende afbeelding toont de Swagger-referentiedocumentatie voor de beheer-API's. Het geeft API-eindpunten met betrekking tot apparaat voor gebruik van de blob en eventueel vereiste padparameters om door te geven in deze.

![Apparaat-blobs][2]

Bijvoorbeeld, als u wilt bijwerken of een blob maken en koppelen van de blob aan een apparaat, moet u een geverifieerde vullen van de HTTP-aanvraag naar:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste blob-ID |

Geslaagde aanvragen retourneert een JSON-object als [die eerder zijn beschreven](#blobModel).

### <a name="spaces"></a>Spaties

U kunt ook blobs koppelen naar spaties. De volgende afbeelding geeft een lijst van alle ruimte API-eindpunten die verantwoordelijk is voor het verwerken van blobs. Het bevat ook een padparameters om door te geven in deze eindpunten.

![Ruimte blobs][3]

Bijvoorbeeld, als u wilt terugkeren van een blob die is gekoppeld aan een spatie, moet u een geverifieerde HTTP GET-aanvraag naar:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste blob-ID |

Geslaagde aanvragen retourneert een JSON-object als [die eerder zijn beschreven](#blobModel).

Een PATCH-aanvraag naar hetzelfde eindpunt updates van metagegevens beschrijvingen en versies van de blob maakt. De HTTP-aanvraag wordt gedaan via de PATCH-methode, samen met eventuele vereiste metagegevens en meerdelige formuliergegevens.

### <a name="users"></a>Gebruikers

U kunt blobs koppelen aan de gebruiker-modellen (bijvoorbeeld om te koppelen van een profielfoto). De volgende afbeelding ziet u relevante gebruiker API-eindpunten en alle vereiste padparameters, zoals `id`:

![Gebruiker-blobs][4]

Bijvoorbeeld, om op te halen van een blob die is gekoppeld aan een gebruiker, moet u een geverifieerde HTTP GET-aanvraag met gegevens die vereiste formulier:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste blob-ID |

Geslaagde aanvragen retourneert een JSON-object als [die eerder zijn beschreven](#blobModel).

## <a name="common-errors"></a>Algemene fouten

Een veelvoorkomende fout omvat niet de juiste headerinformatie te verstrekken:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

U kunt deze fout oplossen, controleren of de totale aanvraag een geschikte heeft **Content-Type** header:

* `multipart/mixed`
* `multipart/form-data`

Controleer ook of elk meerdelige segment heeft een bijbehorende **Content-Type** indien nodig.

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over het Swagger-naslagdocumentatie voor Azure digitale dubbels [gebruik Azure digitale dubbele Swagger-](how-to-use-swagger.md).

- Als u wilt uploaden blobs via Postman, lezen [Postman configureren](./how-to-configure-postman.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
[5]: media/how-to-add-blobs/curl.PNG