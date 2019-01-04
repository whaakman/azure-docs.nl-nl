---
title: Blobs toevoegen aan objecten in Azure, digitale dubbels | Microsoft Docs
description: Informatie over het toevoegen van blobs op objecten in digitale dubbels van Azure.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 6bb1709d10a406d88378189cd68b9a36abed2c8d
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017563"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>Blobs toevoegen aan objecten in Azure, digitale dubbels

BLOBs zijn niet-gestructureerde representaties van algemene bestandstypen, zoals afbeeldingen en Logboeken. BLOBs bijhouden van wat voor soort gegevens die ze geven met behulp van een MIME-type (bijvoorbeeld: ' afbeelding/jpeg') en metagegevens (naam, beschrijving, type, enzovoort).

Azure ondersteunt het digitale dubbels blobs te koppelen aan apparaten, spaties en gebruikers. BLOBs kunnen bestaan uit een profielafbeelding voor een gebruiker, een apparaat foto, een video, een kaart of een logboek.

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-an-overview"></a>Blobs uploaden: een overzicht

Gedeeltelijk aanvragen kunt u blobs uploaden naar specifieke eindpunten en hun respectieve functionaliteiten.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>De metagegevens van de blob

Naast **Content-Type** en **Content-Disposition**, meerdelige aanvragen moeten de juiste JSON-hoofdtekst opgeven. Welke JSON-hoofdtekst om in te dienen, is afhankelijk van het type HTTP-aanvraagbewerking die wordt uitgevoerd.

De vier belangrijkste JSON schema's zijn:

![JSON-schema][1]

De Swagger-documentatie beschreven deze model schema's in de volledige details.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Meer informatie over het gebruik van de naslagdocumentatie door te lezen [over het gebruik van Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Voorbeelden

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Om een **POST** -aanvraag die een tekstbestand als een blob geüpload en associeert deze met een spatie:

```plaintext
POST YOUR_MANAGEMENT_API_URL/spaces/blobs HTTP/1.1
Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"

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

var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

## <a name="api-endpoints"></a>API-eindpunten

De volgende secties beschrijven de core blob-gerelateerde API-eindpunten en hun functies.

### <a name="devices"></a>Apparaten

U kunt blobs koppelen aan apparaten. De volgende afbeelding toont de Swagger-referentiedocumentatie voor de beheer-API's. Het geeft API-eindpunten met betrekking tot apparaat voor gebruik van de blob en eventueel vereiste padparameters om door te geven in deze.

![Apparaat-blobs][2]

Bijvoorbeeld, als u wilt bijwerken of een blob maken en koppelen van de blob aan een apparaat, moet u een **PATCH** aanvragen:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste blob-ID |

Geslaagde aanvragen retourneren een **DeviceBlob** JSON-object in het antwoord. **DeviceBlob** objecten voldoen aan de volgende JSON-schema:

| Kenmerk | Type | Description | Voorbeelden |
| --- | --- | --- | --- |
| **DeviceBlobType** | Reeks | De categorie van een blob die kan worden gekoppeld aan een apparaat | `Model` en `Specification` |
| **DeviceBlobSubtype** | Reeks | Een blob subcategorie dat is meer dan specifieke **DeviceBlobType** | `PhysicalModel`, `LogicalModel`, `KitSpecification`, en `FunctionalSpecification` |

> [!TIP]
> Gebruik de voorgaande tabel om te verwerken is de geretourneerde aanvraaggegevens.

### <a name="spaces"></a>Spaties

U kunt ook blobs koppelen naar spaties. De volgende afbeelding geeft een lijst van alle ruimte API-eindpunten die verantwoordelijk is voor het verwerken van blobs. Het bevat ook een padparameters om door te geven in deze eindpunten.

![Ruimte blobs][3]

Bijvoorbeeld, als u wilt terugkeren van een blob die is gekoppeld aan een spatie, moet u een **ophalen** aanvragen:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste blob-ID |

Maken van een **PATCH** aanvraag voor hetzelfde eindpunt kunt u een beschrijving van de metagegevens van update en maak een nieuwe versie van de blob. De HTTP-aanvraag wordt gedaan via de **PATCH** methode, samen met eventuele vereiste metagegevens en meerdelige formuliergegevens.

Voltooide bewerkingen retourneren een **SpaceBlob** -object dat aan het volgende schema voldoet. U kunt deze gebruiken voor geretourneerde gegevens.

| Kenmerk | Type | Description | Voorbeelden |
| --- | --- | --- | --- |
| **SpaceBlobType** | Reeks | De categorie van een blob die kan worden gekoppeld aan een spatie | `Map` en `Image` |
| **SpaceBlobSubtype** | Reeks | Een blob subcategorie dat is meer dan specifieke **SpaceBlobType** | `GenericMap`, `ElectricalMap`, `SatelliteMap`, en `WayfindingMap` |

### <a name="users"></a>Gebruikers

U kunt blobs koppelen aan de gebruiker-modellen (bijvoorbeeld om te koppelen van een profielfoto). De volgende afbeelding ziet u relevante gebruiker API-eindpunten en alle vereiste padparameters, zoals `id`:

![Gebruiker-blobs][4]

Bijvoorbeeld, om op te halen van een blob die is gekoppeld aan een gebruiker, moet u een **ophalen** aanvraag met gegevens die vereiste formulier:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste blob-ID |

De geretourneerde JSON (**UserBlob** objecten) voldoet aan de volgende JSON-modellen:

| Kenmerk | Type | Description | Voorbeelden |
| --- | --- | --- | --- |
| **UserBlobType** | Reeks | De categorie van een blob die kan worden gekoppeld aan een gebruiker | `Image` en `Video` |
| **UserBlobSubtype** |  Reeks | Een blob subcategorie dat is meer dan specifieke **UserBlobType** | `ProfessionalImage`, `VacationImage`, en `CommercialVideo` |

## <a name="common-errors"></a>Algemene fouten

Een veelvoorkomende fout, moet u niet de juiste headerinformatie:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over het Swagger-naslagdocumentatie voor Azure digitale dubbels [gebruik Azure digitale dubbele Swagger-](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
