---
title: Wat is Project URL Preview?
titlesuffix: Azure Cognitive Services
description: Inleiding tot Project URL Preview.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: url-preview
ms.topic: overview
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: e3791c897722c9bbf50f58c126d7e4e573b2096f
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465811"
---
# <a name="what-is-project-url-preview"></a>Wat is Project URL Preview?
Het eindpunt van URL Preview gebruikt een URL-queryparameter en retourneert een JSON-antwoord met de naam van de doelresource, een korte beschrijving en een koppeling naar een afbeelding die moet worden weergegeven in een voorbeeldvenster. Het antwoord bevat ook de [isFamilyFriendly](url-preview-reference.md#query-parameters)-vlag, die aangeeft of de URL erotische inhoud, geplagieerde inhoud of andere illegale inhoud bevat. 

Om de resultaten van het URL-voorbeeld te verkrijgen, dient u een GET-aanvraag in en neemt u de header *Ocp-Apim-Subscription-Key* op met een geldig token:  
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```
Het antwoord: 
````
HTTP Headers:
BingAPIs-TraceId: 3CC74C94769440C0851D9DF0869FCE7F
BingAPIs-SessionId: 52219085A6364692958C9C83983A0DBA
X-MSEdge-ClientID: 13D44DC2DE946B4B0F25460CDF036AD6
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 3CC74C94769440C0851D9DF0869FCE7F Ref B: CO1EDGE0315 Ref C: 2018-04-11T18:47:40Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https:\/\/swiftkey.com\/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https:\/\/swiftkey.com\/images\/og\/default.jpg"
  }
}

````
## <a name="scenarios"></a>Scenario's 

De URL Preview-API biedt ondersteuning voor korte beschrijvingen van webresources. Ontwikkelaars gebruiken deze voor het bouwen van rijke voorbeeldervaringen.  Gebruikers kunnen webpagina's, nieuws, blogs, forums, enzovoort delen of van een bladwijzer voorzien. Deze API kan ook worden gebruikt voor het goedkeuren van inhoud.    

Toepassingen gebruiken URL Preview om webaanvragen te verzenden naar het eindpunt, waarbij een query wordt toegewezen die de URL moet weergeven.  Het JSON-antwoord bevat de informatie over het voorbeeld: naam, beschrijving van de resource, de *familyFriendly*-vlag en koppelingen die toegang geven tot een representatieve afbeelding en de volledige resource online. 

## <a name="terms-of-use"></a>Gebruiksvoorwaarden
Gebruik de gegevens van Project URL Preview alleen om voorbeelden weer te geven van codefragmenten en miniatuurafbeeldingen die zijn gekoppeld aan hun bronsites, in door de eindgebruiker geïnitieerde functies voor het delen van URL's op social media, chatbots of vergelijkbare aanbiedingen. Het is niet toegestaan om gegevens die u van Project URL Preview ontvangt, te kopiëren, op te slaan of in de cache op te slaan. Respecteer alle verzoeken om voorbeelden uit te schakelen die u ontvangt van website- of inhoudseigenaren.

U, of een derde partij die namens u handelt, mag geen gegevens van de URL Preview-API gebruiken, behouden, opslaan, in een cache plaatsen, delen of distribueren voor test-, ontwikkelings-, trainings- of distributiedoeleinden, noch voor het beschikbaar maken van een niet-Microsoft-service of -functie. 

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor C#](csharp.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor JavaScript](javascript.md)
- [Snelstart voor Node](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
