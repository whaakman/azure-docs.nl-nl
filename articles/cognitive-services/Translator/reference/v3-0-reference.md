---
title: Naslaginformatie Translator Text-API-V3.0
titlesuffix: Azure Cognitive Services
description: Referentiedocumentatie voor de V3.0 Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9282d8af30cbfb3346394bcd71510faf8d8c8a21
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129383"
---
# <a name="translator-text-api-v30"></a>Translator Text-API v3.0

## <a name="whats-new"></a>Wat is nieuw?

Versie 3 van de Translator Text-API biedt een moderne JSON gebaseerde Web-API. Het verbetert de gebruiksvriendelijkheid en prestaties door te consolideren van bestaande functies in minder bewerkingen en het beschikt over nieuwe functies.

 * Vele converteren van tekst in één taal van één script naar een ander script.
 * De vertaling voor meerdere talen in één aanvraag.
 * Taaldetectie, vertaling en vele in één aanvraag.
 * Woordenlijst voor alternatieve vertalingen opzoeken van een term zoeken naar back-vertalingen en voorbeelden die laten zien termen die in de context worden gebruikt.
 * Taal meer informatieve resultaten.

## <a name="base-urls"></a>Basis-URL 's

Tekst-API v3.0 is beschikbaar in de volgende cloud:

| Beschrijving | Regio | Basis-URL                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Wereldwijd | API.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Verificatie

Abonneren op Translator Text-API in Microsoft Cognitive Services en uw abonnementssleutel (beschikbaar in Azure portal) gebruiken om te verifiëren. 

De eenvoudigste manier is om door te geven van uw Azure-geheime sleutel naar de Translator-service met behulp van aanvraagheader `Ocp-Apim-Subscription-Key`.

Een alternatief is het gebruik van de geheime sleutel van een verificatietoken ophalen van de service voor beveiligingstokens. Vervolgens kunt u het Autorisatietoken doorgeven aan de Translator service met behulp de `Authorization` aanvraagheader. Als u wilt een verificatietoken hebt verkregen, moet u een `POST` aanvraag voor de volgende URL:

| Omgeving     | Service-URL webverificatie                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Hier vindt u voorbeeld verzoeken voor het verkrijgen van een token krijgen een geheime sleutel:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Een geslaagde aanvraag retourneert het gecodeerde toegangstoken als tekst zonder opmaak in de hoofdtekst van de reactie. De geldige token wordt doorgegeven aan de Translator-service als een bearer-token in de autorisatie.

```
Authorization: Bearer <Base64-access_token>
```

Een verificatietoken is geldig voor 10 minuten. Het token moet opnieuw worden gebruikt bij het maken van meerdere aanroepen naar de Translator-API's. Echter, als uw programma aanvragen naar de API van Translator gedurende een lange periode maakt, vervolgens uw programma moet aanvragen een nieuw toegangstoken op gezette tijden (bijvoorbeeld elke 8 minuten).

Om samen te vatten, bevat een clientaanvraag naar de API van Translator één autorisatie-header die is overgenomen uit de volgende tabel:

<table width="100%">
  <th width="30%">Headers</th>
  <th>Beschrijving</th>
  <tr>
    <td>OCP-Apim-Subscription-Key</td>
    <td>*Gebruiken met Cognitive Services-abonnement als u de geheime sleutel doorgeeft*.<br/>De waarde is de Azure geheime sleutel voor uw abonnement op Translator Text-API.</td>
  </tr>
  <tr>
    <td>Autorisatie</td>
    <td>*Met Cognitive Services-abonnement gebruiken als u een verificatietoken doorgeeft.*<br/>De waarde is het Bearer-token: ' Bearer <token>'.</td>
  </tr>
</table> 

## <a name="errors"></a>Fouten

Een reactie van de standaardfout is een JSON-object met de naam/waarde-paar met de naam `error`. De waarde is ook een JSON-object met eigenschappen:

  * `code`: Een server gedefinieerde foutcode.

  * `message`: Een tekenreeks die een leesbare weergave van de fout.

Een klant met een gratis proefabonnement zou bijvoorbeeld het volgende foutbericht krijgt, zodra het gratis quotum is bereikt:

```
{
  "error": {
    "code":403000,
    "message":"The subscription has exceeded its free quota."
    }
}
```
