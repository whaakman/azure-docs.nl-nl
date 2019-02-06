---
title: Filteren, bestellen, voor het wisselbestand van entiteiten in Azure Media Services - Azure | Microsoft Docs
description: Dit artikel worden besproken filteren, bestellen, voor het wisselbestand van Azure Media Services-entiteiten.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/24/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 4c6e3281bd2b37b60c8d165c6c3152e970a5ce32
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745093"
---
# <a name="filtering-ordering-paging-of-media-services-entities"></a>Filters, bestellen, wisselbestand van Media Services-entiteiten

## <a name="overview"></a>Overzicht

Media Services ondersteunt de volgende OData-queryopties voor Media Services v3-entiteiten: 

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

Eigenschappen van entiteiten die van het type datum/tijd zijn altijd in UTC-notatie.

## <a name="page-results"></a>Resultaten van de pagina

Als een query-antwoord veel items bevat, retourneert de service een "\@odata.nextLink" eigenschap om de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. U kunt het formaat van de pagina niet configureren. Formaat van de pagina is afhankelijk van het type entiteit, lees de afzonderlijke secties die volgen voor meer informatie.

Als entiteiten zijn gemaakt of verwijderd, wanneer de verzameling worden er pagina's, worden de wijzigingen worden doorgevoerd in de geretourneerde resultaten (als deze wijzigingen in het gedeelte van de verzameling die niet zijn gedownload zijn). 

> [!TIP]
> U moet de volgende koppeling altijd gebruiken om inventariseren van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina.

## <a name="assets"></a>Assets

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe de filters en opties bestellen kunnen worden toegepast op de [Asset](https://docs.microsoft.com/rest/api/media/assets) eigenschappen: 

|Name|Filteren|Bestellen|
|---|---|---|
|id|||
|naam|eq, gt, lt| Oplopend of aflopend|
|properties.alternateId |eq||
|properties.assetId |eq||
|Properties.container |||
|Properties.created| eq, gt, lt| Oplopend of aflopend|
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

## <a name="content-key-policies"></a>Beleid voor inhoudssleutels

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de [Inhoudbeleidsregels sleutel](https://docs.microsoft.com/rest/api/media/contentkeypolicies) eigenschappen: 

|Name|Filteren|Bestellen|
|---|---|---|
|id|||
|naam|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.created |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.Description |Eq, ne, ge, le, gt, lt||
|properties.lastModified|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.Options |||
|properties.policyId|eq, ne||
|type|||

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de grootte van 10.

De volgende C# voorbeeld laat zien hoe om te inventariseren door alle **Inhoudbeleidsregels sleutel** in het account.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [inhoud sleutelbeleid - lijst](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)

## <a name="jobs"></a>Taken

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de [taken](https://docs.microsoft.com/rest/api/media/jobs) eigenschappen: 

| Name    | Filteren                        | Bestellen |
|---------|-------------------------------|-------|
| naam                    | eq            | Oplopend of aflopend|
| properties.state        | eq, ne        |                         |
| Properties.created      | gt, ge, lt, le| Oplopend of aflopend|
| properties.lastModified | gt, ge, lt, le | Oplopend of aflopend| 


### <a name="pagination"></a>Paginering

Taken paginering wordt ondersteund in Media Services v3.

De volgende C# voorbeeld laat zien hoe om te inventariseren door de taken in het account.

```csharp            
List<string> jobsToDelete = new List<string>();
var pageOfJobs = client.Jobs.List(config.ResourceGroup, config.AccountName, "Encode");

bool exit;
do
{
    foreach (Job j in pageOfJobs)
    {
        jobsToDelete.Add(j.Name);
    }

    if (pageOfJobs.NextPageLink != null)
    {
        pageOfJobs = client.Jobs.ListNext(pageOfJobs.NextPageLink);
        exit = false;
    }
    else
    {
        exit = true;
    }
}
while (!exit);

```

Zie voor voorbeelden van REST [taken - lijst](https://docs.microsoft.com/rest/api/media/jobs/list)

## <a name="streaming-locators"></a>Streaming-locators

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de StreamingLocator-eigenschappen: 

|Name|Filteren|Bestellen|
|---|---|---|
|id |||
|naam|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.alternativeMediaId  |||
|properties.assetName   |||
|properties.contentKeys |||
|Properties.created |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.defaultContentKeyPolicyName |||
|properties.endTime |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.startTime   |||
|properties.streamingLocatorId  |||
|properties.streamingPolicyName |||
|type   |||

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de grootte van 10.

De volgende C#-voorbeeld laat zien hoe om te inventariseren alle StreamingLocators in het account.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [Streaming-Locators - lijst](https://docs.microsoft.com/rest/api/media/streaminglocators/list)

## <a name="streaming-policies"></a>Beleid voor streaming

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de StreamingPolicy-eigenschappen: 

|Name|Filteren|Bestellen|
|---|---|---|
|id|||
|naam|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|Properties.created |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|type|||

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de grootte van 10.

De volgende C#-voorbeeld laat zien hoe om te inventariseren alle StreamingPolicies in het account.

```csharp
var firstPage = await MediaServicesArmClient.StreamingPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [Streaming beleid - lijst](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)


## <a name="transform"></a>Transformeren

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de [transformeert](https://docs.microsoft.com/rest/api/media/transforms) eigenschappen: 

| Name    | Filteren                        | Bestellen |
|---------|-------------------------------|-------|
| naam                    | eq            | Oplopend of aflopend|
| Properties.created      | gt, ge, lt, le| Oplopend of aflopend|
| properties.lastModified | gt, ge, lt, le | Oplopend of aflopend|

## <a name="next-steps"></a>Volgende stappen

[Een bestand streamen](stream-files-dotnet-quickstart.md)
