---
title: Microsoft Translator tekst API V3.0 verwijzing | Microsoft Docs
description: Documentatie voor de API V3.0 Microsoft Translator tekst.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: cfaa9584e833b137b417d9074fbfcf606eb21388
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344847"
---
#<a name="translator-text-api-v30"></a>Conversieprogramma tekst API v3.0

## <a name="whats-new"></a>Wat is nieuw?

Versie 3 van de Microsoft Translator tekst API biedt een moderne JSON gebaseerde Web-API. De bruikbaarheid verbetert en prestaties van de door de consolidatie van bestaande functies in minder bewerkingen en het beschikt over nieuwe functies.

 * Transliteration tekst in één taal converteren van een script naar een ander script.
 * De vertaling naar meerdere talen in één aanvraag.
 * Taal wordt gedetecteerd, NAT en transliteration in een aanvraag.
 * Woordenlijst voor lookup alternatieve vertalingen van een term, om back-vertalingen en voorbeelden weergegeven van termen die in de context worden gebruikt.
 * Meer informatieve resultaten taal.

## <a name="base-urls"></a>Basis-URL 's

Tekst API v3.0 is beschikbaar in de volgende cloud:

| Beschrijving | Regio | Basis-URL                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Wereldwijd | API.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Verificatie

Abonneren op conversieprogramma tekst API in Microsoft cognitieve Services en uw abonnementssleutel (beschikbaar in de Azure-portal) gebruiken om te verifiëren. 

De eenvoudigste manier is uw Azure geheime sleutel doorgeven aan de NAT-service met behulp van de aanvraag-header `Ocp-Apim-Subscription-Key`.

Een alternatief is het gebruik van uw geheime sleutel van een verificatietoken ophalen van de service voor beveiligingstokens. Vervolgens moet u het verificatietoken doorgegeven met het conversieprogramma service via de `Authorization` aanvraag-header. Voor een verificatietoken controleert een `POST` aanvraag naar de volgende URL:

| Omgeving     | Service-URL voor webverificatie                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Dit zijn voorbeeld verzoeken voor het verkrijgen van een token krijgen een geheime sleutel:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

De aanvraag is gelukt retourneert het gecodeerde toegangstoken als tekst zonder opmaak in de hoofdtekst van het antwoord. Het token geldig is doorgegeven aan de NAT-service als bearer-token in de autorisatie.

```
Authorization: Bearer <Base64-access_token>
```

Een verificatietoken is geldig voor 10 minuten. Het token moet opnieuw worden gebruikt bij meerdere roept de API-conversieprogramma's. Echter, als u uw programma aanvragen aan de API-vertaler gedurende een lange periode, vervolgens uw programma moet aanvragen een nieuw toegangstoken op gezette tijden (bijvoorbeeld elke 8 minuten).

Om samen te vatten, bevat een clientaanvraag naar de API-vertaler één autorisatie-header die afkomstig zijn uit de volgende tabel:

<table width="100%">
  <th width="30%">Headers</th>
  <th>Beschrijving</th>
  <tr>
    <td>OCP-Apim-Subscription-Key</td>
    <td>*Gebruikt met abonnement cognitieve Services als u uw geheime sleutel doorgeeft*.<br/>De waarde is de Azure geheime sleutel voor uw abonnement aan conversieprogramma tekst API.</td>
  </tr>
  <tr>
    <td>Autorisatie</td>
    <td>*Gebruiken met abonnement cognitieve Services als u geen verificatietoken doorgeeft.*<br/>De waarde is de Bearer-token: ' Bearer <token>'.</td>
  </tr>
</table> 

## <a name="errors"></a>Fouten

Een antwoord van de standaardfout is een JSON-object met de naam/waarde-paar met de naam `error`. De waarde is ook een JSON-object met eigenschappen:

  * `code`: Een server gedefinieerde foutcode.

  * `message`: Een tekenreeks die een leesbare weergave van de fout geeft.

Een klant een gratis proefabonnement zou bijvoorbeeld de volgende fout ontvangen wanneer de beschikbare quota is verbruikt:

```
{
  "error": {
    "code":403000,
    "message":"The subscription has exceeded its free quota."
    }
}
```
