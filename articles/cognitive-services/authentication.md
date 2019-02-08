---
title: Authentication
titleSuffix: Cognitive Services - Azure
description: 'Er zijn drie manieren om te verifiëren van een aanvraag voor een bron voor Azure Cognitive Services: een abonnementssleutel, een bearer-token of een abonnement op meerdere services. In dit artikel leert u over elke methode en hoe u een aanvraag.'
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: erhopf
ms.openlocfilehash: f724bba5acdda20d31d067b850634178a0650cf7
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859741"
---
# <a name="authenticate-requests-to-azure-cognitive-services"></a>Verifiëren van aanvragen voor Azure Cognitive Services

Elke aanvraag aan een Azure Cognitive Service moet een verificatieheader bevatten. Deze header doorgeeft aan een abonnementssleutel of token, dat wordt gebruikt voor het valideren van uw abonnement voor een service of de groep van services. In dit artikel leert u over drie manieren om een aanvraag en de vereisten voor elk te verifiëren.

* [Verifiëren met een enkele service-abonnementssleutel](#authenticate-with-a-single-service-subscription-key)
* [Verifiëren met een abonnementssleutel met meerdere service](#authenticate-with-a-multi-service-subscription-key)
* [Verifiëren met een token](#authenticate-with-an-authentication-token)

## <a name="prerequisites"></a>Vereisten

Voordat u een aanvraag indienen, moet u een Azure-account en een Azure Cognitive Services-abonnement. Als u al een account hebt, gaat u verder en gaat u verder met de volgende sectie. Als u geen account hebt, hebben we een handleiding waarmee u binnen minuten opzet: [Een Cognitive Services-account maken voor Azure](cognitive-services-apis-create-account.md).

## <a name="authentication-headers"></a>Verificatieheaders

Laten we snel bekijken de verificatieheaders die beschikbaar zijn voor gebruik met Azure Cognitive Services.

| Header | Description |
|--------|-------------|
| OCP-Apim-Subscription-Key | Deze header gebruiken om te verifiëren met een abonnementssleutel voor een bepaalde service of een abonnementssleutel met meerdere service. |
| Ocp-Apim-Subscription-Region | Deze header is alleen vereist als u een abonnementssleutel met meerdere service-met de [Translator Text-API](./Translator/reference/v3-0-reference.md). Gebruik deze header de regio van het abonnement op te geven. |
| Autorisatie | Gebruik deze header als u een verificatietoken. De stappen voor het uitvoeren van een token exchange worden beschreven in de volgende secties. De opgegeven waarde volgende indeling: `Bearer <TOKEN>`. |

## <a name="authenticate-with-a-single-service-subscription-key"></a>Verifiëren met een enkele service-abonnementssleutel

De eerste optie is voor een aanvraag voor verificatie met een abonnementssleutel voor een bepaalde service, zoals Translator tekst. De sleutels zijn beschikbaar in de Azure-portal voor elke bron die u hebt gemaakt. Als u een abonnementssleutel wilt verifiëren van een aanvraag, deze moet worden doorgegeven als de `Ocp-Apim-Subscription-Key` header.

Deze voorbeeldaanvragen ziet u hoe u de `Ocp-Apim-Subscription-Key` header. Houd er rekening mee bij het gebruik van dit voorbeeld moet u een geldig abonnement-sleutel bevatten.

Dit is een voorbeeld-aanroep naar de Bing webzoekopdrachten-API:
```cURL
curl -X GET 'https://api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Dit is een voorbeeld-aanroep naar de Translator Text-API:
```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

De volgende video ziet u een Cognitive Services-sleutel gebruikt. 

## <a name="authenticate-with-a-multi-service-subscription-key"></a>Verifiëren met een abonnementssleutel met meerdere service

>[!WARNING]
> Op dit moment is deze services **niet** ondersteuning voor meerdere services sleutels: QnA Maker, Speech Services en Custom Vision.

Deze optie gebruikt ook een abonnementssleutel om aanvragen te verifiëren. Het belangrijkste verschil is dat de abonnementssleutel van een is niet gekoppeld aan een bepaalde service, in plaats daarvan één sleutel kan worden gebruikt voor het verifiëren van aanvragen voor meerdere Cognitive Services. Zie [prijzen voor Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) voor informatie over regionale beschikbaarheid, ondersteunde functies en prijzen.

De abonnementssleutel is opgegeven in elke aanvraag als de `Ocp-Apim-Subscription-Key` header.

[![Belangrijkste demonstratie meerdere service-abonnement voor Cognitive Services](./media/index/single-key-demonstration-video.png)](https://www.youtube.com/watch?v=psHtA1p7Cas&feature=youtu.be)

### <a name="supported-regions"></a>Ondersteunde regio’s

Bij het gebruik van de abonnementssleutel met meerdere service-om te vragen naar `api.cognitive.microsoft.com`, moet u de regio in de URL opnemen. Bijvoorbeeld: `westus.api.cognitive.microsoft.com`.

Als u meerdere services abonnementssleutel voor de Translator Text-API, moet u de regio van het abonnement met de `Ocp-Apim-Subscription-Region` header.

Meerdere service-verificatie wordt ondersteund in deze regio's:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |


### <a name="sample-requests"></a>Van voorbeeldaanvragen

Dit is een voorbeeld-aanroep naar de Bing webzoekopdrachten-API:

```cURL
curl -X GET 'https://YOUR-REGION.api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Dit is een voorbeeld-aanroep naar de Translator Text-API:

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Ocp-Apim-Subscription-Region: YOUR_SUBSCRIPTION_REGION' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="authenticate-with-an-authentication-token"></a>Verifiëren met een verificatietoken

Sommige Azure Cognitive Services accepteren en in sommige gevallen vereisen een verificatietoken. Deze services worden op dit moment verificatietokens ondersteunen:

* Tekstomzettings-API
* Speech Services: Spraak-naar-tekst REST-API
* Speech Services: Text to Speech REST-API

>[!NOTE]
> QnA Maker ook de autorisatie-header wordt gebruikt, maar vereist een eindpuntsleutel. Zie voor meer informatie, [QnA Maker: Antwoord ophalen uit knowledge base](./qnamaker/quickstarts/get-answer-from-kb-using-curl.md).

>[!WARNING]
> De services die ondersteuning bieden voor verificatietokens kunnen worden gewijzigd na verloop van tijd Controleer de API-verwijzing voor een service voordat u deze verificatiemethode.

Beide service enkele en meerdere services abonnementssleutels kunnen worden uitgewisseld voor verificatietokens. Verificatietokens zijn geldig voor 10 minuten.

Verificatietokens worden opgenomen in een aanvraag als de `Authorization` header. De opgegeven waarde voor de token moet worden voorafgegaan door `Bearer`, bijvoorbeeld: `Bearer YOUR_AUTH_TOKEN`.

### <a name="sample-requests"></a>Van voorbeeldaanvragen

Deze URL gebruiken voor het uitwisselen van een enkele service-abonnementssleutel voor een verificatietoken: `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`.

```cURL
curl -v -X POST \
"https://api.cognitive.microsoft.com/sts/v1.0/issueToken" \
-H "Content-type: application/x-www-form-urlencoded" \
-H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

Wanneer u de abonnementssleutel van een op meerdere services gebruikt, moet u een specifieke regio-eindpunt voor de token exchange. Deze URL gebruiken voor het uitwisselen van een abonnementssleutel met meerdere service-voor een verificatietoken: `https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken`.

Deze meerdere services-regio's ondersteunen token exchange:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |

```cURL
curl -v -X POST \
"https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
-H "Content-type: application/x-www-form-urlencoded" \
-H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

Nadat u een verificatietoken hebt ontvangen, moet u doorgeeft in elke aanvraag als de `Authorization` header. Dit is een voorbeeld-aanroep naar de Translator Text-API:

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Authorization: Bearer YOUR_AUTH_TOKEN' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="see-also"></a>Zie ook

* [Wat zijn cognitieve services?](welcome.md)
* [Prijsinformatie cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/)
* [Een account maken](cognitive-services-apis-create-account.md)
