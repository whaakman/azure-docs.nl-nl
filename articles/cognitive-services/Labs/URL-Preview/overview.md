---
title: Wat is Project URL Preview? -Cognitieve Microsoft-Services | Microsoft Docs
description: Inleiding tot het Project URL-Preview.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 6b486e0ab4092bef4fe829a5f166311a572a2900
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345276"
---
# <a name="what-is-project-url-preview"></a>Wat is Project URL Preview?
Het eindpunt-URL Preview neemt een URL-queryparameter en retourneert een JSON-antwoord met de naam van de doelresource, een korte beschrijving en een koppeling naar een afbeelding weergegeven in een voorbeeld. Het antwoord bevat ook de [isFamilyFriendly](url-preview-reference.md#query-parameters) vlag die aangeeft of de URL ongeldige inhoud voor volwassenen, piraterij of andere bevat. 

Als u de voorbeeldresultaten URL, een GET-aanvraag indienen en bevatten de *Ocp-Apim-Subscription-Key* -koptekst met een geldig token:  
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

De URL Preview-API biedt ondersteuning voor korte beschrijvingen van bronnen op het Web. Ontwikkelaars deze gebruiken voor het bouwen van uitgebreide preview-ervaring.  Gebruikers kunnen delen of maak een bladwijzer webpagina's, nieuws, blogs, forums, enzovoort. Deze API kan ook worden gebruikt voor inhoud toezicht.    

Toepassingen gebruiken URL Preview webaanvragen verzenden naar het eindpunt met een query die is toegewezen aan de URL voor de preview.  De JSON-antwoord bevat de informatie preview: naam, beschrijving van de resource *familyFriendly* vlag en koppelingen die toegang tot een representatieve installatiekopie en de volledige resource online bieden. 

## <a name="terms-of-use"></a>Gebruiksvoorwaarden
Gebruik alleen de gegevens van Project URL Preview weer te geven preview codefragmenten en miniatuurafbeeldingen hyperlink voor hun bronsites in een eindgebruiker geïnitieerde URL delen op sociale media, chat bot of vergelijkbare aanbiedingen. D kopiëren, opslaan of alle gegevens die u van Project URL Preview ontvangt in de cache. Voldoen aan de verzoeken van voorbeelden die kunnen worden weergegeven van de website of inhoud eigenaar van uitschakelen.

U of een derde partij namens u mogelijk niet gebruiken, behouden, opslaan, cache, delen, of distribueren van alle gegevens van de URL-Preview-API voor testdoeleinden, ontwikkelen, training, distribueren of het beschikbaar maken van een niet-Microsoft-service of functie. 

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen
- [Quick Start C#](csharp.md)
- [Java-Quick Start](java-quickstart.md)
- [JavaScript Quick Start](javascript.md)
- [Knooppunt Quick Start](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
