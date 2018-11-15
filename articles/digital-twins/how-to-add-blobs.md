---
title: Blobs toevoegen aan objecten in Azure, digitale dubbels | Microsoft Docs
description: Inzicht krijgen in hoe u blobs toevoegen aan objecten in Azure, digitale dubbels
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adgera
ms.openlocfilehash: 0929a4a63eee35d21723c980887d6b4217898682
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688133"
---
# <a name="how-to-add-blobs-to-objects-in-azure-digital-twins"></a>Blobs toevoegen aan objecten in Azure, digitale dubbels

BLOBs zijn niet-gestructureerde representaties van algemene bestandstypen (zoals foto's en Logboeken). BLOBs bijhouden van wat voor soort gegevens die ze geven met behulp van een MIME-type (bijvoorbeeld: ' afbeelding/jpeg') en metagegevens (naam, beschrijving, type, enz.).

Azure van digitale dubbels biedt ondersteuning voor Blobs te koppelen aan apparaten, spaties en gebruikers. BLOBs kunnen bestaan uit een profielfoto voor een gebruiker, een apparaat foto, een video, een kaart of een logboek.

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat:
> * Dat wordt uw exemplaar is correct geconfigureerd voor het ontvangen van API Management-aanvragen.
> * Die hebt u goed geverifieerd met behulp van een REST-client van uw keuze.

## <a name="uploading-blobs-an-overview"></a>Blobs uploaden: een overzicht

Gedeeltelijk aanvragen worden gebruikt voor het uploaden van Blobs naar specifieke eindpunten en hun respectieve functionaliteiten.

> [!IMPORTANT]
> Gedeeltelijk aanvragen vereist drie essentiële typen gegevens. Voor Azure van digitale dubbels:
> * Een **Content-Type** header:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`.
> * Een **Content-Disposition**: `form-data; name="metadata"`.
> * De inhoud van het bestand te uploaden.
>
> De exacte **Content-Type** en **Content-Disposition** kan variëren afhankelijk van het scenario voor gebruik.

Elke aanvraag multipart is onderverdeeld in verschillende segmenten. Gedeeltelijk aanvragen bij de Azure digitale dubbels Management API's worden onderverdeeld in **twee** (**2**) dergelijke onderdelen:

1. Het eerste deel is vereist en Blob-metagegevens, zoals een bijbehorende MIME-type per bevat de **Content-Type** en **Content-Disposition** hierboven.

1. Het tweede gedeelte bevat de werkelijke inhoud van een Blob (de ongestructureerde inhoud van het bestand).  

Geen van de twee onderdelen zijn vereist voor **PATCH** aanvragen. Beide zijn vereist voor **POST** of bewerkingen voor maken.

### <a name="blob-metadata"></a>De metagegevens van de blob

Naast **Content-Type** en **Content-Disposition**, meerdelige aanvragen moeten ook de juiste JSON-hoofdtekst opgeven. Welke JSON-hoofdtekst om in te dienen, is afhankelijk van het soort bewerking HTTP-aanvraag die wordt uitgevoerd.

Er zijn de vier belangrijkste JSON schema's gebruikt:

![Ruimte blobs][1]

Deze model schema's vindt u in de volledige details in de opgegeven Swagger-documentatie.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

Meer informatie over het gebruik van de opgegeven verwijzing documentatie door te lezen [over het gebruik van Swagger](./how-to-use-swagger.md).

### <a name="examples"></a>Voorbeelden

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Een POST-aanvraag die een tekstbestand als een Blob geüpload en associeert deze met een spatie maken:

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

| Parameterwaarde | Vervangen door |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | De naam van een meerdelige inhoud grens |

Een .NET-implementatie van de dezelfde Blob-upload hieronder vindt u met behulp van de klasse [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent):

```csharp
//Supply your metaData in a suitable format
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

Hieronder is wordt een overzicht van core eindpunten en hun specifieke functies geboden.

### <a name="devices"></a>Apparaten

BLOBs kunnen worden gekoppeld aan apparaten. De afbeelding hieronder (waarin de naslagdocumentatie over de Swagger voor uw beheer-API's worden weergegeven) Hiermee geeft u de apparaat-gerelateerde API-eindpunten voor gebruik van de Blob en eventueel vereiste padparameters om door te geven in deze:

![Apparaat-blobs][2]

Als u wilt bijwerken of een Blob maken en koppelen van de Blob aan een apparaat, bijvoorbeeld een PATCH-aanvraag gemaakt op:

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste Blob-ID |

Geslaagde aanvragen wordt een DeviceBlob JSON-object in het antwoord geretourneerd. DeviceBlobs voldoen aan de volgende JSON-schema:

| Kenmerk | Type | Beschrijving | Voorbeelden |
| --- | --- | --- | --- |
| **DeviceBlobType** | Reeks | De categorie van een Blob die kan worden gekoppeld aan een apparaat | `Model` en `Specification` |
| **DeviceBlobSubtype*** | Reeks | Een Blob subcategorie dat is meer gedetailleerde dan DeviceBlobType | `PhysicalModel`, `LogicalModel`, `KitSpecification`, en `FunctionalSpecification` |

> [!TIP]
> Gebruik de bovenstaande tabel om te verwerken is de geretourneerde aanvraaggegevens.

### <a name="spaces"></a>Opslagruimten

BLOBs kunnen ook worden gekoppeld aan opslagruimten. De onderstaande afbeelding geeft een lijst van alle ruimte API-eindpunten die verantwoordelijk is voor het verwerken van Blobs, samen met eventuele padparameters om door te geven in deze:

![Ruimte blobs][3]

Bijvoorbeeld, als u wilt terugkeren van een Blob die is gekoppeld aan een spatie, moet u een GET-aanvraag naar:

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste Blob-ID |

Die PATCH-aanvraag verzendt naar hetzelfde eindpunt kunt u een beschrijving van de metagegevens bijwerken en maak een nieuwe versie van de Blob. De HTTP-aanvraag is gemaakt met behulp van de PATCH-methode, samen met eventuele vereiste metagegevens en meerdelige formuliergegevens.

Voltooide bewerkingen retourneert een SpaceBlob die voldoet aan het volgende schema en kan worden gebruikt om de geretourneerde gegevens gebruiken:

| Kenmerk | Type | Beschrijving | Voorbeelden |
| --- | --- | --- | --- |
| **SpaceBlobType** | Reeks | De categorie van een Blob die kan worden gekoppeld aan een spatie | `Map` en `Image` |
| **SpaceBlobSubtype** | Reeks | Een Blob subcategorie dat is meer gedetailleerde dan SpaceBlobType | `GenericMap`, `ElectricalMap`, `SatelliteMap`, en `WayfindingMap` |

### <a name="users"></a>Gebruikers

BLOBs kunnen worden gekoppeld aan de gebruiker-modellen (te koppelen van een profielfoto). De onderstaande afbeelding ziet u relevante gebruikers API-eindpunten en alle vereiste padparameters, zoals een `id`:

![Gebruiker-blobs][4]

Bijvoorbeeld, om op te halen van een Blob die is gekoppeld aan een gebruiker, moet u een GET-aanvraag met alle vereiste formuliergegevens aan:

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_BLOB_ID* | De gewenste Blob-ID |

Geretourneerde JSON (UserBlobs) zal voldoen aan de volgende JSON-modellen:

| Kenmerk | Type | Beschrijving | Voorbeelden |
| --- | --- | --- | --- |
| **UserBlobType** | Reeks | De categorie van een Blob die kan worden gekoppeld aan een gebruiker | `Image` en `Video` |
| **UserBlobSubtype** |  Reeks | Een Blob subcategorie dat is meer gedetailleerde dan UserBlobType | `ProfessionalImage`, `VacationImage`, en `CommercialVideo` |

## <a name="common-errors"></a>Algemene fouten

Inclusief niet de juiste headerinformatie:

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over de opgegeven Azure digitale dubbele Swagger naslagdocumentatie [het gebruik van digitale dubbele Swagger](how-to-use-swagger.md).

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
