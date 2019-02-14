---
title: De Tekstanalyse-API aanroepen
titlesuffix: Azure Cognitive Services
description: Meer informatie over het aanroepen van de Text Analytics REST-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 52c5cb640bfb861fb2da52ee711fe3955a169bcf
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56244025"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Over het aanroepen van de Text Analytics REST-API

Aanroepen naar de **Tekstanalyse-API** zijn HTTP POST/GET-aanroepen, die u in elke taal kan formuleren. In dit artikel gebruiken we REST en [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) ter illustratie van de belangrijkste concepten.

Elke aanvraag moet uw toegangssleutel en een HTTP-eindpunt bevatten. Het eindpunt Hiermee geeft u de regio die u hebt gekozen tijdens het aanmelden van de service-URL en een bron die wordt gebruikt voor de aanvraag: `sentiment`, `keyphrases`, `languages`, en `entities`. 

Intrekken Text Analytics is staatloos, zodat er geen gegevensassets die u zijn wilt beheren. De tekst is geüpload, na ontvangst, geanalyseerd en resultaten direct worden geretourneerd naar de aanroepende toepassing.

> [!Tip]
> Voor eenmalige-aanroepen om te zien hoe de API werkt, kunt u een POST-aanvragen verzenden vanuit de ingebouwde **API testconsole**, beschikbaar op elke [API-document pagina](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6). Er zijn geen instellingen en de enige vereisten zijn een toegangssleutel en de JSON-documenten in de aanvraag plakken. 

## <a name="prerequisites"></a>Vereisten

Hebt u een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met de Tekstanalyse-API, en de [eindpunt en de toegangssleutel](text-analytics-how-to-access-key.md) die voor u wordt gegenereerd wanneer u zich aanmeldt voor Cognitive Services. 

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>JSON-schema-definitie

Invoer moet JSON in onbewerkte ongestructureerde tekst. XML wordt niet ondersteund. Het schema is eenvoudig, die bestaat uit de elementen die worden beschreven in de volgende lijst. 

U kunt dezelfde documenten voor alle Text Analytics-bewerkingen op dit moment indienen: sentiment, sleuteluitdrukkingen, taaldetectie en entiteits-id. (Het schema is waarschijnlijk varieert voor elke analyse in de toekomst.)

| Element | Geldige waarden | Vereist? | Gebruik |
|---------|--------------|-----------|-------|
|`id` |Het gegevenstype tekenreeks is, maar in de praktijk document-id's zijn meestal gehele getallen. | Vereist | De id's die u verstrekt structuur van de uitvoer wordt gebruikt door het systeem. Taalcodes, sleuteltermen en gevoel scores worden gegenereerd voor elke-ID in de aanvraag.|
|`text` | Ongestructureerde onbewerkte tekst, maximaal 5000 tekens. | Vereist | Voor de taaldetectie, kan tekst worden uitgedrukt in elke taal. Voor sentimentanalyse, sleuteltermextractie en entiteits-id, de tekst moet zich in een [ondersteunde taal](../text-analytics-supported-languages.md). |
|`language` | 2-teken [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) code voor een [ondersteunde taal](../text-analytics-supported-languages.md) | Varieert | Vereist voor sentimentanalyse, sleuteltermextractie en entiteiten koppelen; Dit is optioneel voor taaldetectie. Er is geen fout als u deze wordt uitgesloten, maar de analyse is zonder verzwakt. De taalcode die moet overeenkomen met de `text` u bieden. |

Zie voor meer informatie over de limieten [Text Analytics-overzicht > gegevenslimieten](../overview.md#data-limits). 

## <a name="set-up-a-request-in-postman"></a>Instellen van een aanvraag in Postman

De service accepteert aanvragen van maximaal 1 MB in grootte. Als u Postman (of een ander hulpprogramma voor Web-API-test), het eindpunt instellen om op te nemen van de resource die u wilt gebruiken en de toegangssleutel in een aanvraagheader bevatten. Elke bewerking vereist dat u de juiste resource aan het eindpunt toevoegen. 

1. In Postman:

   + Kies **Post** als het aanvraagtype.
   + Plak in het eindpunt dat u hebt gekopieerd uit de portal-pagina.
   + Een resource toevoegen.

  Resource-eindpunt zijn als volgt te werk (uw regio kan verschillen):

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`

2. Stel de drie aanvraagheaders:

   + `Ocp-Apim-Subscription-Key`: uw toegangssleutel hebt verkregen via Azure portal.
   + `Content-Type`: application/json.
   + `Accept`: application/json.

  Uw aanvraag moet eruitzien als op de volgende schermafbeelding, ervan uitgaande dat het een **/keyPhrases** resource.

   ![Schermafbeelding van de aanvraag met het eindpunt en -koppen](../media/postman-request-keyphrase-1.png)

4. Klik op **hoofdtekst** en kies **onbewerkte** voor de indeling.

   ![Schermafbeelding van de aanvraag met de hoofdtekst van de instellingen](../media/postman-request-body-raw.png)

5. In enkele JSON-documenten in een indeling die geldig is voor de beoogde analyse te plakken. Zie de volgende onderwerpen voor meer informatie over een bepaalde analyse:

  + [Taaldetectie](text-analytics-how-to-language-detection.md)  
  + [Sleuteltermextractie](text-analytics-how-to-keyword-extraction.md)  
  + [Sentimentanalyse](text-analytics-how-to-sentiment-analysis.md)  
  + [Herkenning van entiteit (Preview)](text-analytics-how-to-entity-linking.md)  


6. Klik op **verzenden** dien de aanvraag. U kunt maximaal 100 aanvragen per minuut indienen. 

  In Postman, wordt het antwoord weergegeven in het volgende venster omlaag, als een enkel JSON-document met een item voor elk document-ID opgegeven in de aanvraag.

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Taal detecteren](text-analytics-how-to-language-detection.md)
