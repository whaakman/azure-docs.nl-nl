---
title: De Video Indexer-API's gebruiken voor het aanpassen van een taalmodel - Azure
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe een taalmodel met Video Indexer-API's aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: anzaman
ms.openlocfilehash: c2c722331c95e72bae5605606564a2083e2802e3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58075029"
---
# <a name="customize-a-language-model-with-the-video-indexer-apis"></a>Een taalmodel met Video Indexer-API's aanpassen

Video Indexer kunt u aangepaste taalmodellen voor het aanpassen van spraakherkenning door te uploaden aanpassing van woordenlijsten tekst, namelijk de tekst uit het domein waarvan u de engine om aan te passen dat wilt aan de woordenlijst maken. Nadat u uw model te trainen, kunt u nieuwe woorden die voorkomen in de aanpassing van woordenlijsten tekst wordt herkend. 

Zie voor een gedetailleerd overzicht en aanbevolen procedures voor het aangepaste taalmodellen [aanpassen van een taalmodel met Video Indexer](customize-language-model-overview.md).

U kunt de Video Indexer-API's maken en bewerken van aangepaste taalmodellen in uw account, zoals beschreven in dit onderwerp. U kunt ook de website, zoals beschreven in [aanpassen taalmodel met behulp van de website Video Indexer](customize-language-model-with-api.md).

## <a name="create-a-language-model"></a>Een taalmodel maken

De volgende opdracht maakt een nieuwe aangepaste taalmodel in het opgegeven account. U kunt bestanden voor het taalmodel in deze aanroep uploaden. U kunt ook hier het taalmodel maken en uploaden van bestanden voor het model later door het taalmodel wordt bijgewerkt.

> [!NOTE]
> U moet nog steeds het model te trainen met de ingeschakelde bestanden voor het model voor meer informatie over de inhoud van de bestanden. Er zijn instructies voor het trainen van een taal in de volgende sectie.

### <a name="request-url"></a>Aanvraag-URL

Dit is een POST-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?name={name}&accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X POST "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language?accessToken={accessToken}&modelName={modelName}&language={language}"

--data-ascii "{body}" 
```

[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/Create-PersonModel?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|
|modelName|string|Ja|De naam van het taalmodel|
|language|string|Ja|De taal van het taalmodel. <br/>De **taal** parameter moet de taal in de indeling van de BCP-47 van 'taal tag-gebied' krijgen (bijvoorbeeld: 'en-US'). Ondersteunde talen zijn Engels (en-US), Duits (nl-nl), Spaans (es-SP), Arabisch (ar-bijvoorbeeld), Frans (fr-FR), Hindi (High-Hallo), Italiaans (it-IT), Japans (ja-JP), Portugees (pt-BR), Russisch (ru-RU/s) en Chinees (zh-CN).  |

### <a name="request-body"></a>Aanvraagbody

Als u wilt uploaden van bestanden worden toegevoegd aan het taalmodel, moet u de bestanden in de hoofdtekst met formuliergegevens naast het opgeven van de waarden voor de bovenstaande vereiste parameters uploaden. Er zijn twee manieren om dit te doen: 

1. Sleutel wordt de naam en waarde worden de txt-bestand
2. Sleutel is de naam en waarde is een URL naar een txt-bestand

### <a name="response"></a>Antwoord

Het antwoord bevat metagegevens voor de zojuist gemaakte taalmodel samen met de metagegevens op elk van de indeling van de voorbeeld-JSON-uitvoer bestanden van het model.

```json
{
    "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
    "name": "TestModel",
    "language": "En-US",
    "state": "None",
    "languageModelId": "00000000-0000-0000-0000-000000000000",
    "files": [
    {
        "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.6733333"
    },
    {
        "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
        "name": "worldfile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.86"
    }
    ]
}

```

## <a name="train-a-language-model"></a>Een taalmodel te trainen

De volgende opdracht traint een aangepast taalmodel in het opgegeven account met de inhoud van de bestanden die zijn geüpload naar en ingeschakeld in het taalmodel. 

> [!NOTE]
> Eerst moet u het taalmodel maken en uploaden van bestanden. Bij het maken van het taalmodel of door het taalmodel wordt bijgewerkt, kunt u bestanden uploaden. 

### <a name="request-url"></a>Aanvraag-URL

Dit is een PUT-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Train?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X PUT "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Train?accessToken={accessToken}"
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5bac3cf761779a6c2ab27?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountID|string|Ja|Unieke id voor de account|
|modelId|string|Ja|De taal model-id (gegenereerd wanneer het taalmodel wordt gemaakt).|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat metagegevens voor het nieuwe getrainde taalmodel, samen met de metagegevens op elk van de indeling van de voorbeeld-JSON-uitvoer bestanden van het model.

```json
{
    "id": "41464adf-e432-42b1-8e09-f52905d7e29d",
    "name": "TestModel",
    "language": "En-US",
    "state": "Waiting",
    "languageModelId": "531e5745-681d-4e1d-b124-12e5ab57a891",
    "files": [
    {
        "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
        "name": "RenamedFile",
        "enable": false,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.5233333"
    },
    {
        "id": "9ac35b4b-1381-49c4-9fe4-8234bfdd0f50",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.68"
    }
    ]
}
```

Vervolgens moet u gebruiken de **id** waarde van het taalmodel voor de **linguisticModelId** parameter wanneer [uploaden van video's naar index](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) en voor de  **languageModelId** parameter wanneer [indexeren van een video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?).

## <a name="delete-a-language-model"></a>Een taalmodel verwijderen

De volgende opdracht verwijdert een aangepast taalmodel van het opgegeven account. Een video die is met behulp van het verwijderde taalmodel blijft de dezelfde index totdat u de video opnieuw indexeren. Als u de video opnieuw indexeren, kunt u een nieuwe taalmodel toewijzen aan de video. Video Indexer wordt gebruik anders de standaardmodel opnieuw indexeren van de video.

### <a name="request-url"></a>Aanvraag-URL

Dit is een DELETE-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X DELETE "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}"
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5ba53782606e91f65be9d?).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountID|string|Ja|Unieke id voor de account|
|modelId|string|Ja|De taal model-id (gegenereerd wanneer het taalmodel wordt gemaakt).|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Er is geen geretourneerde inhoud wanneer het taalmodel is verwijderd.

## <a name="update-a-language-model"></a>Een taalmodel bijwerken

De volgende opdracht werkt een aangepast taalmodel voor een persoon in het opgegeven account.

> [!NOTE]
> U moet het taalmodel al hebt gemaakt. U kunt deze aanroep inschakelen of uitschakelen van alle bestanden onder het model, de naam van het taalmodel bijwerken en uploaden van bestanden worden toegevoegd aan het taalmodel.

### <a name="request-url"></a>Aanvraag-URL

Dit is een PUT-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}[&modelName][&enable]
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X PUT "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}?modelName={string}&enable={string}"

--data-ascii "{body}" 
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5b3ce85f4684240bdb78f?).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountID|string|Ja|Unieke id voor de account|
|modelId|string|Ja|De taal model-id (gegenereerd wanneer het taalmodel wordt gemaakt).|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|
|modelName|string|Nee|Nieuwe naam die u tot het model geven kunt|
|inschakelen|booleaans|Nee|Kiezen of alle bestanden onder dit model worden ingeschakeld (true) of uitgeschakeld (ONWAAR)|

### <a name="request-body"></a>Aanvraagbody

Als u wilt uploaden van bestanden worden toegevoegd aan het taalmodel, moet u de bestanden in de hoofdtekst met formuliergegevens naast het opgeven van de waarden voor de bovenstaande vereiste parameters uploaden. Er zijn twee manieren om dit te doen: 

1. Sleutel wordt de naam en waarde worden de txt-bestand
2. Sleutel is de naam en waarde is een URL naar een txt-bestand

### <a name="response"></a>Antwoord

Het antwoord bevat metagegevens voor het nieuwe getrainde taalmodel, samen met de metagegevens op elk van de indeling van de voorbeeld-JSON-uitvoer bestanden van het model.

```json
{
    "id": "41464adf-e432-42b1-8e09-f52905d7e29d",
    "name": "TestModel",
    "language": "En-US",
    "state": "Waiting",
    "languageModelId": "531e5745-681d-4e1d-b124-12e5ab57a891",
    "files": [
    {
        "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
        "name": "RenamedFile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.5233333"
    },
    {
        "id": "9ac35b4b-1381-49c4-9fe4-8234bfdd0f50",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-27T20:10:10.68"
    }
    ]
}
```
U kunt de **id** van de bestanden geretourneerd hier om te downloaden van de inhoud van het bestand.

## <a name="update-a-file-from-a-language-model"></a>Een bestand uit een taalmodel bijwerken

De volgende opdracht kunt u de naam van de update en **inschakelen** status van een bestand in een aangepast taalmodel in het opgegeven account.

### <a name="request-url"></a>Aanvraag-URL

Dit is een PUT-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}[&fileName][&enable]
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X PUT "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}?fileName={string}&enable={string}"
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5b60547f33c1c2b2d1375?).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountId|string|Ja|Unieke id voor de account|
|modelId|string|Ja|Id van het taalmodel waarin het bestand (gegenereerd wanneer het taalmodel wordt gemaakt).|
|fileId|string|Ja|Id van het bestand dat moet worden bijgewerkt (gegenereerd wanneer het bestand geüpload bij het maken of bijwerken van het taalmodel is)|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|
|fileName|string|Nee|Naam van de bestandsnaam om te werken|
|inschakelen|booleaans|Nee|Bijwerken of dit bestand is (true) of uitgeschakeld (ONWAAR) in het taalmodel|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat metagegevens voor het bestand dat u de volgende de indeling van het voorbeeld van JSON-uitvoer onderstaande bijgewerkt.

```json
{
  "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
  "name": "RenamedFile",
  "enable": false,
  "creator": "John Doe",
  "creationTime": "2018-04-27T20:10:10.5233333"
}
```
U kunt de **id** van het bestand geretourneerd hier om te downloaden van de inhoud van het bestand.

## <a name="get-a-specific-language-model"></a>Ophalen van een specifieke taalmodel

De volgende opdracht retourneert informatie over de opgegeven taalmodel in het opgegeven account, zoals taal en de bestanden die zich in het taalmodel. 

### <a name="request-url"></a>Aanvraag-URL

Dit is een GET-aanvraag.
```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}?accessToken={accessToken}"
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5b4fcbd9b437d27d53f16).

### <a name="request-parameters-and-request-body"></a>Parameters van de aanvraag en de hoofdtekst van de aanvraag

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountID|string|Ja|Unieke id voor de account|
|modelId|string|Ja|De taal model-id (gegenereerd wanneer het taalmodel wordt gemaakt).|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat metagegevens voor de opgegeven taalmodel, samen met de metagegevens op elk van de indeling van het voorbeeld van JSON-uitvoer onderstaande bestanden van het model.

```json
{
    "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
    "name": "TestModel",
    "language": "En-US",
    "state": "None",
    "languageModelId": "00000000-0000-0000-0000-000000000000",
    "files": [
    {
        "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
        "name": "hellofile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.6733333"
    },
    {
        "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
        "name": "worldfile",
        "enable": true,
        "creator": "John Doe",
        "creationTime": "2018-04-28T11:55:34.86"
    }
    ]
}
```

U kunt de **id** van het bestand geretourneerd hier om te downloaden van de inhoud van het bestand.

## <a name="get-all-the-language-models"></a>Alle taalmodellen ophalen

De volgende opdracht retourneert alle van de aangepaste taalmodellen in het opgegeven account in een lijst.

### <a name="request-url"></a>Aanvraag-URL

Dit is een aanvraag ophalen.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language?accessToken={accessToken}"
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5b4979e6ecbd30faa6f75?).

### <a name="request-parameters"></a>Aanvraagparameters

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountID|string|Ja|Unieke id voor de account|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat een lijst van alle modellen van de taal in uw account en elk van de metagegevens en de bestanden die de indeling van het voorbeeld van JSON-uitvoer onderstaande te volgen.

```json
[
    {
        "id": "dfae5745-6f1d-4edd-b224-42e1ab57a891",
        "name": "TestModel",
        "language": "En-US",
        "state": "None",
        "languageModelId": "00000000-0000-0000-0000-000000000000",
        "files": [
        {
            "id": "25be7c0e-b6a6-4f48-b981-497e920a0bc9",
            "name": "hellofile",
            "enable": true,
            "creator": "John Doe",
            "creationTime": "2018-04-28T11:55:34.6733333"
        },
        {
            "id": "33025f5b-2354-485e-a50c-4e6b76345ca7",
            "name": "worldfile",
            "enable": true,
            "creator": "John Doe",
            "creationTime": "2018-04-28T11:55:34.86"
        }
        ]
    },
    {
        "id": "dfae5745-6f1d-4edd-b224-42e1ab57a892",
        "name": "AnotherTestModel",
        "language": "En-US",
        "state": "None",
        "languageModelId": "00000000-0000-0000-0000-000000000001",
        "files": []
    }
]
```

## <a name="delete-a-file-from-a-language-model"></a>Een bestand verwijderen uit een taalmodel

De volgende opdracht verwijdert het opgegeven bestand uit de opgegeven taalmodel in het opgegeven account. 

### <a name="request-url"></a>Aanvraag-URL

Dit is een DELETE-aanvraag.
```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X DELETE "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}"
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5b57b5de51e64ee52242e).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountID|string|Ja|Unieke id voor de account|
|modelId|string|Ja|Id van het taalmodel waarin het bestand (gegenereerd wanneer het taalmodel wordt gemaakt).|
|fileId|string|Ja|Id van het bestand dat moet worden bijgewerkt (gegenereerd wanneer het bestand geüpload bij het maken of bijwerken van het taalmodel is)|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Er is geen geretourneerde inhoud wanneer het bestand wordt verwijderd uit het taalmodel is.

## <a name="get-metadata-on-a-file-from-a-language-model"></a>Metagegevens ophalen voor een bestand van een taalmodel

Dit retourneert de inhoud van en de metagegevens op het opgegeven bestand van de gekozen taalmodel in de uw account.

### <a name="request-url"></a>Aanvraag-URL

Dit is een GET-aanvraag.

```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/PersonModels?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.
```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}?accessToken={accessToken}"
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5b73f9e6416d7a9965b42).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountID|string|Ja|Unieke id voor de account|
|modelId|string|Ja|Id van het taalmodel waarin het bestand (gegenereerd wanneer het taalmodel wordt gemaakt).|
|fileId|string|Ja|Id van het bestand dat moet worden bijgewerkt (gegenereerd wanneer het bestand geüpload bij het maken of bijwerken van het taalmodel is)|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord bevat de inhoud en metagegevens van het bestand in JSON-indeling, ongeveer als volgt uit:

```json
{
    "content": "hello\r\nworld",
    "id": "84fcf1ac-1952-48f3-b372-18f768eedf83",
    "name": "Hello",
    "enable": true,
    "creator": "John Doe",
    "creationTime": "2018-04-27T20:10:10.5233333"
}
```

> [!NOTE]
> De inhoud van dit voorbeeld van een bestand zijn de woorden "Hallo" en de wereld"in twee afzonderlijke regels.

## <a name="download-a-file-from-a-language-model"></a>Een bestand downloaden van een taalmodel

De volgende opdracht wordt een tekstbestand met de inhoud van het opgegeven bestand uit de opgegeven taalmodel in het opgegeven account gedownload. Dit bestand moet overeenkomen met de inhoud van het tekstbestand dat oorspronkelijk is geüpload.

### <a name="request-url"></a>Aanvraag-URL
```
https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}/download?accessToken={accessToken}
```

Hieronder vindt u de aanvraag in Curl.

```curl
curl -v -X GET "https://api.videoindexer.ai/{location}/Accounts/{accountId}/Customization/Language/{modelId}/Files/{fileId}/download?accessToken={accessToken}"
```
 
[Zie de vereiste parameters en testen met behulp van de Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/docs/services/operations/operations/5ae5b99e522cb47bd9679122).

### <a name="request-parameters"></a>Aanvraagparameters 

|**Naam**|**Type**|**Vereist**|**Beschrijving**|
|---|---|---|---|
|location|string|Ja|De Azure-regio waarnaar u de oproep worden gerouteerd. Zie voor meer informatie, [Azure-regio's en Video Indexer](regions.md).|
|accountID|string|Ja|Unieke id voor de account|
|modelId|string|Ja|Id van het taalmodel waarin het bestand (gegenereerd wanneer het taalmodel wordt gemaakt).|
|fileId|string|Ja|Id van het bestand dat moet worden bijgewerkt (gegenereerd wanneer het bestand geüpload bij het maken of bijwerken van het taalmodel is)|
|accessToken|string|Ja|Toegangstoken (moet van het bereik [Account toegangstoken](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token?)) voor verificatie op basis van de aanroep. Toegangstokens verloopt binnen 1 uur.|

### <a name="request-body"></a>Aanvraagbody 

Er is geen verdere aanvraagtekst vereist voor deze aanroep.

### <a name="response"></a>Antwoord

Het antwoord is het downloaden van een tekstbestand met de inhoud van het bestand in de JSON-indeling. 

## <a name="next-steps"></a>Volgende stappen

[Taalmodel via de website aanpassen](customize-language-model-with-website.md)
