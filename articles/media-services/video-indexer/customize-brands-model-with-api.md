---
title: Azure Video Indexer gebruiken voor het aanpassen van merken model
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe Azure Video Indexer gebruiken om aan te passen merken model.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/03/2018
ms.author: anzaman
ms.openlocfilehash: 5591c36a13291432f2d790b4c83e98270b4bc035
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284869"
---
# <a name="customize-a-brands-model-with-the-video-indexer-api"></a>Aanpassen van een model merken met Video Indexer-API

Video Indexer biedt ondersteuning voor detectie merk van spraak en tekst visual tijdens het indexeren en indexeren van video en audio-inhoud. De mogelijkheid voor het detecteren van merk vermeldingen van producten, services, identificeert en bedrijven voorgesteld door de Bing merken database. Bijvoorbeeld, als Microsoft wordt vermeld in een video of audio-inhoud of als deze weergegeven in de visual tekst in een video wordt, Video Indexer wordt dit gedetecteerd als een onderdeel in de inhoud. Een aangepast merken-model kunt u merken dat moeten deel uitmaken van het model die mogelijk niet in de database van de Bing merken opnemen en uitsluiten van bepaalde merken wordt gedetecteerd.

Zie voor een gedetailleerd overzicht [overzicht](customize-brands-model-overview.md).

U kunt de Video Indexer-API's gebruiken om te maken, gebruiken en bewerken van aangepaste merken modellen gedetecteerd in een video, zoals beschreven in dit onderwerp. U kunt ook de Video Indexer-website, zoals beschreven in [aanpassen merken model met behulp van de website Video Indexer](customize-brands-model-with-api.md).

## <a name="create-a-brand"></a>Maken van een merk

Hiermee maakt u een nieuwe aangepaste merk en voegt deze toe aan het aangepaste merken model voor het opgegeven account. 

### <a name="request-url"></a>Aanvraag-URL

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands?accessToken={accessToken}
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-Brand).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Naast deze parameters moet u een aanvraag hoofdtekst van de JSON-object dat bevat informatie over de nieuwe huisstijl in de indeling van het volgende voorbeeld te volgen.

```json
{
  "name": "Example",
  "enabled": true,
  "tags": ["Tag1", "Tag2"],
  "description": "This is an example",
  "referenceUrl": "https://en.wikipedia.org/wiki/Example"
}
```

Instellen van **ingeschakeld** naar de plaatst waar het merk in de *opnemen* lijst voor Video Indexer om te detecteren. Instellen van **ingeschakeld** op false plaatst u het merk in de *uitsluiten* aanbieden, zodat deze wordt niet gedetecteerd door Video Indexer.

De **referenceUrl** waarde mag geen websites verwijzing voor het merk, zoals een koppeling naar de Wikipedia-pagina.

De **tags** waarde is een lijst met tags voor het merk. Deze wordt weergegeven in van het merk *categorie* veld in de Video Indexer-website. Bijvoorbeeld, worden het merk 'Azure' gemarkeerd of aangemerkt als 'Cloud'.

### <a name="response"></a>Antwoord

Het antwoord bevat informatie over het merk die u zojuist hebt gemaakt na de indeling van het voorbeeld hieronder.

```json
{
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "id": 97974,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-25T14:59:52.7433333",
  "enabled": true,
  "description": "This is an example",
  "tags": [
    "Tag1",
    "Tag2"
  ]
}
```

## <a name="delete-a-brand"></a>Een onderdeel verwijderen

Hiermee verwijdert u een onderdeel van het aangepaste merken model voor het opgegeven account. Het account is opgegeven in de **accountId** parameter. Nadat is aangeroepen, het merk niet langer de *opnemen* of *uitsluiten* lijsten merken.

### <a name="request-url"></a>Aanvraag-URL

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands/{id}?accessToken={accessToken}
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Delete-Brand?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|id|geheel getal|Ja|Het merk-id (die is gegenereerd toen het merk werd gemaakt)|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Er is geen geretourneerde inhoud wanneer het merk is verwijderd.

## <a name="get-a-specific-brand"></a>Een specifiek merk ophalen

Hiermee kunt u zoeken naar de details van een merk in de aangepaste merken model voor het opgegeven account met behulp van het merk-id. 

### <a name="request-url"></a>Aanvraag-URL

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands?accessToken={accessToken}
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brand?).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|id|geheel getal|Ja|Het merk-ID (die is gegenereerd toen het merk werd gemaakt)|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat informatie over het merk dat u hebt gezocht (met behulp van merk-ID) de indeling van het volgende voorbeeld te volgen.

```json
{
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "id": 128846,
  "name": "Example",
  "accountId": "SampleAccountId",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-01-06T13:51:38.3666667",
  "lastModified": "2018-01-11T13:51:38.3666667",
  "enabled": true,
  "description": "This is an example",
  "tags": [
    "Tag1",
    "Tag2"
  ]
}
```

> [!NOTE]
> **ingeschakeld** wordt ingesteld op **waar** geeft aan dat het merk zich in de *opnemen* lijst voor Video Indexer om te detecteren, en **ingeschakeld** wordt de waarde false geeft aan dat de merk is in de *uitsluiten* aanbieden, zodat deze wordt niet gedetecteerd door Video Indexer.

## <a name="update-a-specific-brand"></a>Bijwerken van een specifiek merk

Hiermee kunt u zoeken naar de details van een merk in de aangepaste merken model voor het opgegeven account met behulp van het merk-ID. 

### <a name="request-url"></a>Aanvraag-URL

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands/{id}?accessToken={accessToken}
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brand?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|id|geheel getal|Ja|Het merk-ID (die is gegenereerd toen het merk werd gemaakt)|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Naast deze parameters moet u een JSON-object voor aanvraag hoofdtekst bevat informatie over het merk die u wilt bijwerken nadat de indeling van het voorbeeld hieronder bijgewerkt.

```json
{
  "name": "Example",
  "enabled": false,
  "tags": ["Tag1", "NewTag2"],
  "description": "This is an update example",
  "referenceUrl": "https://en.wikipedia.org/wiki/Example",
  "lastModifierUserName": "SampleUserName",
  "created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-28T15:52:22.3413983",
}
```

> [!NOTE]
> In dit voorbeeld het merk dat is gemaakt in de hoofdtekst van de voorbeeld-aanvraag in de **maken van een merk** sectie hier wordt bijgewerkt met een nieuw label en de nieuwe beschrijving. De **ingeschakeld** waarde is ook gewijzigd op false om plaatst deze in de *uitsluiten* lijst.

### <a name="response"></a>Antwoord

Het antwoord bevat de bijgewerkte informatie over het merk op dat u hebt bijgewerkt na de indeling van het voorbeeld hieronder.

```json
{
  "referenceUrl": null,
  "id": 97974,
  "name": "Example",
  "accountId": "SampeAccountId",
  "lastModifierUserName": "SampleUserName",
  "Created": "2018-04-25T14:59:52.7433333",
  "lastModified": "2018-04-25T15:37:50.67",
  "enabled": false,
  "description": "This is an update example",
  "tags": [
    "Tag1",
    "NewTag2"
  ]
}
```

## <a name="get-all-of-the-brands"></a>Alle van de merken die

Dit retourneert alle van de merken die in het aangepaste merken model voor het opgegeven account, ongeacht of het merk is bedoeld om te worden de *opnemen* of *uitsluiten* merken lijst.

### <a name="request-url"></a>Aanvraag-URL

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands?accessToken={accessToken}
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat een lijst van alle de merken die in uw account en elk van de gegevens na de indeling van het voorbeeld hieronder.

```json
[
    {
        "ReferenceUrl": null,
        "id": 97974,
        "name": "Example",
        "accountId": "AccountId",
        "lastModifierUserName": "UserName",
        "Created": "2018-04-25T14:59:52.7433333",
        "LastModified": "2018-04-25T14:59:52.7433333",
        "enabled": true,
        "description": "This is an example",
        "tags": ["Tag1", "Tag2"]
    },
        {
        "ReferenceUrl": null,
        "id": 97975,
        "name": "Example2",
        "accountId": "AccountId",
        "lastModifierUserName": "UserName",
        "Created": "2018-04-26T14:59:52.7433333",
        "LastModified": "2018-04-26T14:59:52.7433333",
        "enabled": false,
        "description": "This is another example",
        "tags": ["Tag1", "Tag2"]
    },
]
```

> [!NOTE]
> Het merk met de naam *voorbeeld* is in de *opnemen* lijst voor Video Indexer om te detecteren en het merk met de naam *voorbeeld2* is in de *uitsluiten* lijst , zodat deze wordt niet gedetecteerd door Video Indexer.

## <a name="get-brands-model-settings"></a>Instellingen voor het model van merken ophalen

Hiermee wordt de modelinstellingen merken in het opgegeven account. Instellingen voor het model merken vertegenwoordigen of detectie uit de database van Bing merken of niet is ingeschakeld. Als u Bing merken niet zijn ingeschakeld, detecteert Video Indexer alleen merken uit de aangepaste merken model van het opgegeven account.

### <a name="request-url"></a>Aanvraag-URL

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Brands?accessToken={accessToken}
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Brands).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord geeft aan of Bing merken zijn ingeschakeld op de volgende de indeling van het voorbeeld hieronder.

```json
{
  "state": true,
  "useBuiltIn": true
}
```

> [!NOTE]
> **useBuiltIn** wordt ingesteld op true vertegenwoordigt die Bing merken zijn ingeschakeld. Als *useBuiltin* is ingesteld op false, Bing merken zijn uitgeschakeld. De **status** waarde kan worden genegeerd als deze is afgeschaft.

## <a name="update-brands-model-settings"></a>Instellingen voor merken model bijwerken

Hiermee wordt de instellingen voor het merken-model in het opgegeven account bijgewerkt. Instellingen voor het model merken vertegenwoordigen of detectie uit de database van Bing merken of niet is ingeschakeld. Als u Bing merken niet zijn ingeschakeld, detecteert Video Indexer alleen merken uit de aangepaste merken model van het opgegeven account.

### <a name="request-url"></a>Aanvraag-URL: 
```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/BrandsModelSettings?accessToken={accessToken}
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Update-Brands-Model-Settings?).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Naast deze parameters moet u een aanvraag hoofdtekst van de JSON-object dat bevat informatie over de nieuwe huisstijl in de indeling van het volgende voorbeeld te volgen.

```json
{
    "useBuiltIn":true
}
```

> [!NOTE]
> **useBuiltIn** wordt ingesteld op true vertegenwoordigt die Bing merken zijn ingeschakeld. Als *useBuiltin* is ingesteld op false, Bing merken zijn uitgeschakeld.

### <a name="response"></a>Antwoord

Er is geen geretourneerde inhoud wanneer de instelling voor het model van merken is bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

[Merken model met behulp van website aanpassen](customize-brands-model-with-website.md)
