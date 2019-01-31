---
title: Naslaginformatie Translator Text-API-V3.0
titlesuffix: Azure Cognitive Services
description: Referentiedocumentatie voor de V3.0 Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 4d1c33480e408f892517cde6d42e103b34218f26
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459063"
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

Microsoft Translator is geleverd vanuit meerdere datacenter-locaties. Op dit moment ze bevinden zich op 6 [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions):

* **Amerika:** VS-West 2 en West-Centraal VS 
* **Azië en Stille Oceaan:** Zuidoost-Azië en Korea-Zuid
* **Europa:** Noord-Europa en West-Europa

Aanvragen voor de Microsoft Translator Text-API zijn in de meeste gevallen door het datacenter die zich het dichtst bij de oorsprong van de aanvraag verwerkt. In het geval van een datacenter-fout optreedt, kan de aanvraag worden gerouteerd buiten de regio.

Om af te dwingen de aanvraag moet worden verwerkt door een specifieke datacenter, wijzigt u de globaal eindpunt voor in de API-aanvraag naar de gewenste regionale eindpunt:

|Description|Regio|Basis-URL|
|:--|:--|:--|
|Azure|Wereldwijd|  api.cognitive.microsofttranslator.com|
|Azure|Noord-Amerika|   api-nam.cognitive.microsofttranslator.com|
|Azure|Europa|  api-eur.cognitive.microsofttranslator.com|
|Azure|Azië en Stille Oceaan|    api-apc.cognitive.microsofttranslator.com|


## <a name="authentication"></a>Verificatie

Abonneren op Translator Text-API of [Cognitive Services All-in-one](https://azure.microsoft.com/pricing/details/cognitive-services/) op Microsoft Cognitive Services, en uw abonnement sleutel (beschikbaar in Azure portal) om te verifiëren. 

Er zijn drie kopteksten die u gebruiken kunt om te verifiëren van uw abonnement. Deze tabel bevat wordt beschreven hoe deze worden gebruikt:

|Headers|Description|
|:----|:----|
|OCP-Apim-Subscription-Key|*Gebruiken met Cognitive Services-abonnement als u de geheime sleutel doorgeeft*.<br/>De waarde is de Azure geheime sleutel voor uw abonnement op Translator Text-API.|
|Autorisatie|*Met Cognitive Services-abonnement gebruiken als u een verificatietoken doorgeeft.*<br/>De waarde is het Bearer-token: `Bearer <token>`.|
|Ocp-Apim-Subscription-Region|*Met Cognitive Services All-in-one-abonnement gebruiken als u een alles-in-een geheime sleutel zijn doorgegeven.*<br/>De waarde is de regio van de alles-in-één-abonnement. Deze waarde is optioneel wanneer niet met een alles-in-één-abonnement.|

###  <a name="secret-key"></a>Geheime sleutel
De eerste optie is om te verifiëren met behulp van de `Ocp-Apim-Subscription-Key` header. Voeg de `Ocp-Apim-Subscription-Key: <YOUR_SECRET_KEY>` header op uw aanvraag.

### <a name="authorization-token"></a>Autorisatietoken
U kunt ook de geheime sleutel voor een toegangstoken uitwisselen. Dit token is opgenomen in elke aanvraag als de `Authorization` header. Als u wilt een verificatietoken hebt verkregen, moet u een `POST` aanvraag voor de volgende URL:

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

### <a name="all-in-one-subscription"></a>Alles-in-één abonnement

De laatste optie voor de verificatie is het gebruik van een Cognitive Service alles-in-één abonnement. Hiermee kunt u één geheime sleutel gebruiken om te verifiëren van aanvragen voor meerdere services. 

Wanneer u een alles-in-een geheime sleutel gebruikt, moet u twee verificatieheaders opnemen met uw aanvraag. De eerste de geheime sleutel wordt doorgegeven, de tweede Hiermee geeft u de regio die is gekoppeld aan uw abonnement. 
* `Ocp-Api-Subscription-Key`
* `Ocp-Apim-Subscription-Region`

Als u de geheime sleutel in de query-tekenreeks met de parameter doorgeven `Subscription-Key`, en vervolgens moet u de regio met queryparameter `Subscription-Region`.

Als u een bearer-token hebt gebruikt, moet u het token verkrijgen van het eindpunt regio: `https://<your-region>.api.cognitive.microsoft.com/sts/v1.0/issueToken`.

Beschikbare regio's zijn `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `japaneast`, `northeurope`, `southcentralus`, `southeastasia`, `uksouth`, `westcentralus`, `westeurope`, `westus`, en `westus2`.

De regio is vereist voor de alles-in-één Text-API-abonnement.

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

| Code | Description |
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

