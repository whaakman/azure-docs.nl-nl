---
title: Assets in mediaservices - Azure | Microsoft Docs
description: Dit artikel bevat een uitleg over wat activa zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/01/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 8507d51f0d4d49d89fc24b38ed73df7488261daa
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969572"
---
# <a name="assets"></a>Assets

In Azure Media Services, een [Asset](https://docs.microsoft.com/rest/api/media/assets) digitale bestanden (met inbegrip van de video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden-bestanden) en de metagegevens over deze bestanden bevat. Nadat de digitale bestanden zijn geüpload naar een Asset, kunnen ze worden gebruikt in de Media Services, codering, streaming, inhoud werkstromen analyseren. Zie voor meer informatie de [digitale bestanden uploaden naar activa](#upload-digital-files-into-assets) onderstaande sectie.

Een Asset is toegewezen aan een blobcontainer in de [Azure Storage-account](storage-account-concept.md) en de bestanden in de Asset worden opgeslagen als blok-blobs in die container. Media Services biedt ondersteuning voor Blob-lagen wanneer het account wordt gebruikt voor algemeen gebruik v2 (GPv2)-opslag. U kunt bestanden te verplaatsen met gpv2-Opslagaccounts [koude of Archiefopslag](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Archief** opslag is geschikt voor het archiveren van bronbestanden wanneer u niet meer nodig hebt (bijvoorbeeld, nadat ze zijn gecodeerd).

De **archief** storage-laag wordt alleen aanbevolen voor zeer grote bronbestanden die al zijn gecodeerd en de uitvoer van de codering is geplaatst in een uitvoer-blob-container. De blobs in de uitvoercontainer die u wilt koppelen aan een Asset en om te streamen of te analyseren van uw inhoud moeten aanwezig zijn in een **warm** of **Cool** storage-laag.

## <a name="asset-definition"></a>Asset-definitie

De volgende tabel ziet u de eigenschappen van de Asset en biedt de definities.

|Name|Description|
|---|---|
|id|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|De naam van de resource.|
|properties.alternateId |De alternatieve ID van de Asset.|
|properties.assetId |De Asset-ID.|
|Properties.container |De naam van de asset blob-container.|
|Properties.created |De aanmaakdatum van de Asset.<br/> Er is altijd datum/tijd in UTC-notatie.|
|Properties.Description|De beschrijving van de Asset.|
|properties.lastModified |Datum van de activa van de laatste wijziging. <br/> Er is altijd datum/tijd in UTC-notatie.|
|properties.storageAccountName |De naam van het storage-account.|
|properties.storageEncryptionFormat |De coderingsindeling Asset. Een op None of MediaStorageEncryption.|
|type|Het type van de resource.|

Zie voor een volledige definitie [activa](https://docs.microsoft.com/rest/api/media/assets).

## <a name="upload-digital-files-into-assets"></a>Digitale bestanden uploaden naar activa

Een van de meestvoorkomende werkstromen zijn Media Services is om te uploaden, coderen en streamen van een bestand. In deze sectie geeft een overzicht van de algemene stappen.

1. De Media Services v3-API gebruiken om een nieuwe "tekstinvoer" Asset maken. Deze bewerking wordt een container gemaakt in het opslagaccount dat is gekoppeld aan uw Media Services-account. De API retourneert de containernaam van de (bijvoorbeeld `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Als u een blob-container die u wilt koppelen aan een Asset al hebt, kunt u de containernaam van de opgeven bij het maken van de Asset. Media Services ondersteunt momenteel alleen blobs in de hoofdmap van de container en niet met paden in de bestandsnaam. Dus werkt een container met de naam 'input.mp4'. Een container met de naam van het bestand "videos/inputs/input.mp4" werkt echter niet.

    De Azure CLI kunt u rechtstreeks naar een opslagaccount en container die u in uw abonnement rechten voor het uploaden. <br/>De containernaam moet uniek zijn en volgen richtlijnen voor de naamgeving opslag. De naam van de beschikt niet over de Media Services-Asset-containernaam (Asset-GUID) opmaak volgen. 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Een SAS-URL met machtigingen voor lezen / schrijven die wordt gebruikt voor digitale bestanden uploaden naar de container Asset ophalen. U kunt de API van Media Services naar [de URL van de container asset](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Gebruik de Azure Storage-API's of de SDK's (bijvoorbeeld de [REST API voor Storage](../../storage/common/storage-rest-api-auth.md), [JAVA SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md), of [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) bestanden uploaden naar de container Asset. 
4. Media Services v3 API's gebruiken om een transformatie en een taak voor het verwerken van uw "tekstinvoer" Asset te maken. Zie voor meer informatie, [transformaties en taken](transform-concept.md).
5. De inhoud van de ' uitvoerasset ' Stream.

> [!TIP]
> Voor een volledig .NET-voorbeeld laat zien hoe u: maken van de Asset, een beschrijfbare SAS-URL ophalen van de Asset-container in de opslag, het bestand uploaden naar de container in storage met behulp van de SAS-URL, Zie [de Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Een nieuwe asset maken

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Zie voor een voorbeeld van de REST de [een Asset te maken met REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) voorbeeld.

Het voorbeeld laat zien hoe maken de **aanvraagtekst** waarin u nuttige informatie, zoals beschrijving, de containernaam van de, storage-account en andere informatie kunt opgeven.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Zie voor een compleet voorbeeld [de Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md). In Media Services v3, kan ook invoer van een taak van HTTPS-URL's worden gemaakt (Zie [de Taakinvoer van een maken van een HTTPS-URL](job-input-from-http-how-to.md)).

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Media Services ondersteunt de volgende OData-queryopties voor activa: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

Beschrijving van de operator:

* EQ = gelijk zijn aan
* Ne = niet gelijk zijn aan
* Ge = groter dan of gelijk aan
* Le = kleiner dan of gelijk aan
* Gt = groter dan
* Lt = minder dan

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de Asset-eigenschappen: 

|Name|Filteren|Bestellen|
|---|---|---|
|id|||
|naam|Ondersteunt: Eq, Gt, Lt|Ondersteunt: Oplopend of aflopend|
|properties.alternateId |Ondersteunt: EQ||
|properties.assetId |Ondersteunt: EQ||
|Properties.container |||
|Properties.created|Ondersteunt: Eq, Gt, Lt| Ondersteunt: Oplopend of aflopend|
|Properties.Description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|type|||

De volgende C#-voorbeeld kunt u filteren op de aanmaakdatum:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de paginagrootte 1000.

> [!TIP]
> U moet de volgende koppeling altijd gebruiken om inventariseren van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina.

Als een query-antwoord veel items bevat, retourneert de service een "\@odata.nextLink" eigenschap om de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. U kunt het formaat van de pagina niet configureren. 

Als Assets zijn gemaakt of verwijderd, wanneer de verzameling worden er pagina's, worden de wijzigingen worden doorgevoerd in de geretourneerde resultaten (als deze wijzigingen in het gedeelte van de verzameling die niet zijn gedownload zijn). 

#### <a name="c-example"></a>C#-voorbeeld

De volgende C#-voorbeeld laat zien hoe om te inventariseren alle activa in het account.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

#### <a name="rest-example"></a>REST-voorbeeld

Houd rekening met het volgende voorbeeld van waar $skiptoken wordt gebruikt. Zorg ervoor dat u vervangen *amstestaccount* met uw accountnaam en stel de *api-versie* waarde naar de nieuwste versie.

Als u een lijst van Assets als volgt:

```
GET  https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

U wilt teruggaan een antwoord ongeveer als volgt uit:

```
HTTP/1.1 200 OK
 
{
"value":[
{
"name":"Asset 0","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 0","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-5a4f-470a-9d81-6037d7c23eff","created":"2018-12-11T22:12:44.98Z","lastModified":"2018-12-11T22:15:48.003Z","container":"asset-98d07299-5a4f-470a-9d81-6037d7c23eff","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
},
// lots more assets
{
"name":"Asset 517","id":"/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 517","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-912e-447b-a1ed-0f723913b20d","created":"2018-12-11T22:14:08.473Z","lastModified":"2018-12-11T22:19:29.657Z","container":"asset-fd05a503-912e-447b-a1ed-0f723913b20d","storageAccountName":"amsdevc1stoaccount11","storageEncryptionFormat":"None"
}
}
],"@odata.nextLink":"https:// management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517"
}
```

U zou de volgende pagina vervolgens aanvragen door een get-aanvraag verzenden voor:

```
https://management.azure.com/subscriptions/00000000-3761-485c-81bb-c50b291ce214/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

Zie voor meer voorbeelden van REST [activa - lijst](https://docs.microsoft.com/rest/api/media/assets/list)

## <a name="storage-side-encryption"></a>Versleuteling van opslag aan de serverzijde

Ter bescherming van uw activa in rust, moeten de activa van de versleuteling van opslag aan de serverzijde worden versleuteld. De volgende tabel ziet u de werking van de versleuteling van opslag aan de serverzijde in Media Services:

|Optie voor opslagversleuteling|Description|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services-Storage-versleuteling|AES-256-codering, sleutel beheerd door Media Services|Ondersteund<sup>(1)</sup>|Niet ondersteund<sup>(2)</sup>|
|[Storage Service Encryption voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Versleuteling op de server die worden aangeboden door Azure Storage, sleutel beheerd door Azure of door de klant|Ondersteund|Ondersteund|
|[Client-Side-versleuteling van opslag](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Clientversleuteling die worden aangeboden door Azure-opslag, beheerd door de klant in Key Vault sleutel|Niet ondersteund|Niet ondersteund|

<sup>1</sup> terwijl Media Services biedt ondersteuning voor de verwerking van inhoud in de wissen/zonder enige vorm van versleuteling, als dit dus niet aanbevolen.

<sup>2</sup> in Media Services v3, versleuteling van opslag (AES-256-codering) wordt alleen ondersteund voor achterwaartse compatibiliteit bij uw activa zijn gemaakt met Media Services v2. Dit betekent dat v3 werkt met bestaande opslag versleuteld activa, maar staat niet toe dat het maken van nieuwe labels.

## <a name="next-steps"></a>Volgende stappen

[Een bestand streamen](stream-files-dotnet-quickstart.md)

[Verschillen tussen de Media Services v2 en v3](migrate-from-v2-to-v3.md)
