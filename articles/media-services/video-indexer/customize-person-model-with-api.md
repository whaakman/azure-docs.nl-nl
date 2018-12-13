---
title: De Video Indexer-API gebruiken voor het aanpassen van een persoon model - Azure
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe het aanpassen van een persoon-model met de Video Indexer-API.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/10/2018
ms.author: anzaman
ms.openlocfilehash: 2b035243e577adbbefa866acbae6243a9e7e9a31
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285072"
---
# <a name="customize-a-person-model-with-the-video-indexer-api"></a>Aanpassen van een persoon-model met de Video Indexer-API

Video Indexer biedt ondersteuning voor gezichtsdetectie en herkenning van beroemdheden voor video-inhoud. De functie van de herkenning van beroemdheden bevat informatie over ongeveer één miljoen gezichten op basis van veelvuldig opgevraagde gegevensbron zoals IMDB Wikipedia en bovenste LinkedIn testteam. Gezichten die niet worden herkend door de functie voor de herkenning van beroemdheden worden gevonden. ze blijven echter naamloze. Nadat u u video naar Video Indexer uploaden en resultaten van de vorige ophalen, kunt u teruggaan en de naam van de gezichten die niet zijn herkend. Wanneer u een gezicht met de naam van een label, het gezicht en de naam toegevoegd naar persoon-model van uw account. Video Indexer herkent vervolgens deze gezicht in uw toekomstige video's en de afgelopen video's.

De Video Indexer-API kunt u gezichten die zijn gedetecteerd in een video bewerken, zoals beschreven in dit onderwerp. U kunt ook de Video Indexer-website, zoals beschreven in [aanpassen persoon model met behulp van de website Video Indexer](customize-person-model-with-api.md).

## <a name="managing-multiple-person-models"></a>Beheren van meerdere modellen van de persoon 

Video Indexer biedt ondersteuning voor meerdere modellen van de persoon per account. Deze functie is momenteel alleen beschikbaar via de Video Indexer-API's.

Als uw account caters naar verschillende use-casescenario's, is het raadzaam om meerdere modellen voor persoon per account te maken. Als uw inhoud is gerelateerd aan sport, kunt u bijvoorbeeld, klikt u vervolgens een afzonderlijke persoon-model voor elke sport (football, basketbal, soccer, enz.) maken. 

Nadat een model is gemaakt, kunt u deze kunt gebruiken door op te geven van de model-ID van een specifieke persoon model bij het uploaden/indexeren of een video opnieuw indexeren. Het specifieke aangepaste model dat de video is gekoppeld aan een nieuw gezicht voor een video training worden bijgewerkt.

Elk account heeft een limiet van 50 persoon modellen. Als u niet de ondersteuning voor meerdere model persoon hoeft, wijs geen een persoon model-ID toe aan uw video bij het uploaden/indexeren of opnieuw indexeren. In dit geval Video Indexer maakt gebruik van het standaardmodel voor de aangepaste persoon in uw account.

## <a name="create-a-new-person-model"></a>Een nieuwe persoon-model maken

Maak een nieuwe persoon-model in het opgegeven account. 

### <a name="request-url"></a>Aanvraag-URL

Dit is een POST-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?name={name}&accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X POST "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?name={name}&accessToken={accessToken}"
```

[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-PersonModel?).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|naam|string|Ja|De naam voor het model van de persoon|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat dat de naam en een gegenereerd model-ID van de persoon die u zojuist hebt gemaakt na de indeling van het voorbeeld hieronder model.

```json
{
    "id": "227654b4-912c-4b92-ba4f-641d488e3720",
    "name": "Example Person Model"
}
```

Vervolgens moet u gebruiken de **id** waarde voor de **personModelId** parameter wanneer [uploaden van video's naar index](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) of [indexeren van een video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?).

## <a name="delete-a-person-model"></a>Het model van een persoon verwijderen

Een aangepast model van de persoon verwijderen uit het opgegeven account. 

Zodra het model van de persoon die is verwijderd, wordt de index van uw huidige video's die de verwijderde model ongewijzigd blijven totdat u ze opnieuw indexeren. Na het opnieuw indexeren, wordt de gezichten met de naam in het model verwijderd niet herkend door Video Indexer in uw huidige video's die zijn geïndexeerd met dat model; Deze gezichten wordt echter nog steeds worden gedetecteerd. Uw huidige video's die zijn geïndexeerd met behulp van de verwijderde model wordt nu van uw account standaard persoon model gebruiken. Als gezichten uit het model verwijderd worden ook de naam in van uw account standaardmodel, blijft deze gezichten worden herkend in de video's.

### <a name="request-url"></a>Aanvraag-URL

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels/{id}?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.
```curl
curl -v -X DELETE "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels/{id}?accessToken={accessToken}"
```

[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-PersonModel?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|id|string|Ja|De persoon model-id (gegenereerd wanneer het model van de persoon die is gemaakt).|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Er is geen geretourneerde inhoud wanneer het model van de persoon die is verwijderd.

## <a name="get-all-person-models"></a>Ophalen van alle modellen van de persoon

Alle persoon modellen ophalen in het opgegeven account. 

### <a name="request-call"></a>Aanroep van aanvraag

Dit is een GET-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?accessToken={accessToken}"
```

[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-PersonModels?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat een lijst van alle modellen persoon in uw account (met inbegrip van het model van de persoon die standaard in het opgegeven account) en al hun namen en -id's de indeling van het volgende voorbeeld te volgen.

```json
[
    {
        "id": "59f9c326-b141-4515-abe7-7d822518571f",
        "name": "Default"
    }, 
    {
        "id": "9ef2632d-310a-4510-92e1-cc70ae0230d4",
        "name": "Test"
    }
]
```

U kunt kiezen welk model u gebruiken voor een video wilt met behulp van de **id** waarde van het model van de persoon die voor de **personModelId** parameter wanneer [uploaden van video's naar index](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) of [indexeren van een video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?).

## <a name="update-a-face"></a>Bijwerken van een gezicht

Met deze opdracht kunt u een gezicht in de video met een naam op met de ID van de video en ID van het gezicht bijwerken. Vervolgens werkt het model persoon die de video is gekoppeld aan bij het uploaden/indexeren of opnieuw indexeren. Als er geen model persoon is toegewezen, werkt het standaardmodel voor personen van het account. 

Wanneer dit gebeurt, wordt de instanties van de dezelfde gezicht in de andere huidige video's delen van het model met dezelfde persoon gedetecteerd. Herkenning van het gezicht in de andere huidige video's kan enige tijd in te voeren, omdat dit een batchproces duren.

U kunt een gezicht die Video Indexer wordt herkend als een beroemdheden met een nieuwe naam bijwerken. De nieuwe naam die u toewijst voorrang krijgt boven de ingebouwde beroemdheden-herkenning.

### <a name="request-call"></a>Aanroep van aanvraag

Dit is een POST-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Videos/{videoId}/Index/Faces/{faceId}?accessToken={accessToken}&newName={newName}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X PUT "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Videos/{videoId}/Index/Faces/{faceId}?accessToken={accessToken}&newName={newName}"
```

[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-face?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|videoId|string|Ja|De id voor de video waarin het gezicht die u wilt bijwerken wordt weergegeven. Dit wordt gemaakt wanneer de video is geüpload en geïndexeerd.|
|faceId|geheel getal|Ja|De id voor het gezicht die zullen worden bijgewerkt. U kunt de face id ophalen uit de video-index|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|
|naam|string|Ja|Nieuwe naam van het gezicht met bijwerken.|

Namen uniek zijn voor persoon modellen, dus als u twee verschillende gezichten in dezelfde persoon hetzelfde model **naam** parameterwaarde, Video Indexer weergaven van de gezichten als dezelfde persoon en ze nadat u uw video opnieuw indexeren convergeert wel ligt. 

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Er is geen geretourneerde inhoud wanneer het gezicht is bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

[Persoon model met behulp van de Video Indexer-website aanpassen](customize-person-model-with-website.md)
