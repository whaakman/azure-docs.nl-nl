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
ms.openlocfilehash: 6f679536d69f700fd6678eb3bbbb869e42439cde
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2018
ms.locfileid: "51853350"
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
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```
De foutcode is een 6-cijferige numerieke combineren het 3-cijferige HTTP-statuscode gevolgd door een getal 3 cijfers en verder categoriseren van de fout. Veelvoorkomende foutcodes zijn:

| Code | Beschrijving |
|:----|:-----|
| 400000| Een van de invoer van de aanvraag is niet geldig.|
| 400001| De bereikparameter '' is ongeldig.|
| 400002| De parameter 'categorie' is ongeldig.|
| 400003| Een taal-id is ongeldig of ontbreekt.|
| 400004| Een aanduiding van de doel-script ('naar script') is ongeldig of ontbreekt.|
| 400005| Een invoertekst is ongeldig of ontbreekt.|
| 400006| De combinatie van taal en het script is niet geldig.|
| 400018| Een aanduiding van de bron-script ("van script') is ongeldig of ontbreekt.|
| 400019| Een van de opgegeven taal wordt niet ondersteund.|
| 400020| Een van de elementen in de matrix van invoertekst is niet geldig.|
| 400021| De API-versie-parameter is ongeldig of ontbreekt.|
| 400023| Een van de combinatie van de opgegeven taal is niet geldig.|
| 400035| De source-taal ('van' veld) is niet geldig.|
| 400036| De doeltaal ("voor" veld) is ongeldig of ontbreekt.|
| 400042| Een van de opties opgegeven ('Opties') is niet geldig.|
| 400043| De client traceer-ID (ClientTraceId veld of de header X-ClientTranceId) is ongeldig of ontbreekt.|
| 400050| De ingevoerde tekst is te lang.|
| 400064| De parameter 'translation' is ongeldig of ontbreekt.|
| 400070| Het aantal doel-scripts (ToScript parameter) komt niet overeen met het aantal doel talen (parameter).|
| 400071| De waarde is niet geldig voor teksttype.|
| 400072| De matrix van de ingevoerde tekst heeft te veel elementen.|
| 400073| De scriptparameter is niet geldig.|
| 400074| De hoofdtekst van de aanvraag is geen geldige JSON.|
| 400075| De combinatie van taal paar en categorie is niet geldig.|
| 400077| Grootte van de maximale aanvraag is overschreden.|
| 400079| Het aangepaste systeem aangevraagd voor de omzetting van van en naar taal bestaat niet.|
| 401000| De aanvraag is niet toegestaan omdat de referenties zijn ontbreekt of is ongeldig.|
| 401015| "De opgegeven referenties zijn voor de spraak-API. Deze aanvraag moet referenties voor de Text-API. Gebruik een abonnement op Translator Text-API."|
| 403000| De bewerking is niet toegestaan.|
| 403001| De bewerking is niet toegestaan omdat het abonnement het gratis quotum is overschreden.|
| 405000| De aanvraagmethode wordt niet ondersteund voor de aangevraagde resource.|
| 408001| De aangepaste vertaalsysteem aangevraagd is nog niet beschikbaar. Probeer het opnieuw over een paar minuten.|
| 415000| De header Content-Type is ongeldig of ontbreekt.|
| 429000, 429001, 429002| De server heeft de aanvraag geweigerd omdat de client te veel aanvragen verzendt. Verminder de frequentie van aanvragen om te voorkomen dat beperking.|
| 500000| Er is een onverwachte fout opgetreden. Als de fout zich blijft voordoen, die rapporteren aan de datum/tijd van de fout, aanvraag-id van response-header X-RequestId en client-id van de aanvraagheader X-ClientTraceId.|
| 503000| Service is tijdelijk niet beschikbaar. Probeer het opnieuw. Als de fout zich blijft voordoen, die rapporteren aan de datum/tijd van de fout, aanvraag-id van response-header X-RequestId en client-id van de aanvraagheader X-ClientTraceId.|

