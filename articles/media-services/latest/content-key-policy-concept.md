---
title: Beleid voor sleutels in Azure mediaservices Content | Microsoft Docs
description: Dit artikel bevat een uitleg over wat inhoudsbeleid van de sleutels zijn, en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 355e5576dca72cbcabb30e9f8f09f69509dae92a
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381060"
---
# <a name="content-key-policies"></a>Beleid voor inhoudssleutels

U kunt Azure Media Services gebruiken voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via opslag, verwerking en levering. Met Media Services, kunt u uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen leveren: Microsoft PlayReady en Google Widevine Apple FairPlay. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients.

In Azure Media Services v3 kunt beleid voor inhoud sleutels u om op te geven hoe de inhoudssleutel wordt geleverd als u wilt beëindigen van clients via het onderdeel voor de levering van Media Services-sleutel. Zie voor meer informatie, [overzicht van de beveiliging van inhoud](content-protection-overview.md).

## <a name="contentkeypolicies-definition"></a>ContentKeyPolicies definitie

De volgende tabel ziet u de eigenschappen van de ContentKeyPolicy en biedt de definities.

|Naam|Type|Beschrijving|
|---|---|---|
|id|tekenreeks|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|tekenreeks|De naam van de resource.|
|Properties.created |tekenreeks|De aanmaakdatum van het beleid|
|Properties.Description |tekenreeks|Een beschrijving voor het beleid.|
|properties.lastModified    |tekenreeks|De datum van laatste wijziging van het beleid|
|Properties.Options |ContentKeyPolicyOption]|De sleutel beleidsopties.|
|properties.policyId    |tekenreeks|De verouderde beleids-ID.|
|type   |tekenreeks|Het type van de resource.|

Zie voor de definitie van de volledige [Inhoudbeleidsregels sleutel](https://docs.microsoft.com/rest/api/media/contentkeypolicies).

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Media Services ondersteunt de volgende OData-queryopties voor ContentKeyPolicies: 

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

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de StreamingPolicy-eigenschappen: 

|Naam|Filteren|Bestellen|
|---|---|---|
|id|||
|naam|Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.created |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.Description |Eq, ne, ge, le, gt, lt||
|properties.lastModified    |Eq, ne, ge, le, gt, lt|Oplopend of aflopend|
|Properties.Options |||
|properties.policyId    |Eq, ne||
|type   |||

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de grootte van 10.

> [!TIP]
> U moet de volgende koppeling altijd gebruiken om inventariseren van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina.

Als een query-antwoord veel items bevat, retourneert de service een "\@odata.nextLink" eigenschap om de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. U kunt het formaat van de pagina niet configureren. 

Als StreamingPolicy worden gemaakt of verwijderd, wanneer de verzameling worden er pagina's, worden de wijzigingen doorgevoerd in de geretourneerde resultaten (als deze wijzigingen zijn in het gedeelte van de verzameling die niet zijn gedownload.) 

De volgende C#-voorbeeld laat zien hoe om te inventariseren alle ContentKeyPolicies in het account.

```csharp
var firstPage = await MediaServicesArmClient.ContentKeyPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.ContentKeyPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [inhoud sleutelbeleid - lijst](https://docs.microsoft.com/rest/api/media/contentkeypolicies/contentkeypolicies_list)

## <a name="next-steps"></a>Volgende stappen

[Gebruik dynamische AES-128-versleuteling en de sleutelleveringsservice](protect-with-aes128.md)

[Gebruik DRM dynamische versleuteling en licentie leveringsservice voor](protect-with-drm.md)
