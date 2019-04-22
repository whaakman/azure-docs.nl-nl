---
title: Levenscyclus van het knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker leert beste in een iteratief cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van eindpunt query's.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/16/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 3f78b8a2566137d596f4ab3f083e1d14289365c3
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684018"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Levenscyclus van de Knowledge base in QnA Maker
QnA Maker leert beste in een iteratief cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van eindpunt query's. 

![Ontwerpcyclus](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Het maken van een kennisdatabase QnA Maker
QnA Maker knowledge base (KB)-eindpunt biedt een best-none-match-antwoord op een gebruikersquery op basis van de inhoud van de KB. Het maken van een kennisdatabase is een eenmalige actie voor het instellen van een opslagplaats voor inhoud van de vragen, antwoorden en gekoppelde metagegevens. Een kennisdatabase kunnen worden gemaakt met het verkennen van de bestaande inhoud zoals veelgestelde vragen over pagina's, producthandleidingen of gestructureerde Q-A-paren. Meer informatie over het [maken van een kennisdatabase](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Testen en bijwerken van de knowledge base

De knowledge base is klaar voor testen wanneer het wordt gevuld met inhoud, redactioneel of via automatische extractie. Interactieve testen kan worden uitgevoerd in de QnA Maker portal via de **Test** Configuratiescherm door te voeren van de algemene query's van gebruikers en verifiëren dat het antwoord geretourneerd met de juiste reactie en voldoende betrouwbaarheidsscore. 

* **Om op te lossen lage vertrouwen scores**: alternatieve vragen toevoegen. 
* **Wanneer een query onjuist retourneert de [Standaardantwoord](confidence-score.md#change-default-answer)**: nieuwe antwoorden op de juiste vraag toevoegen. 

Deze lus van test-update gaat door totdat u tevreden met de resultaten bent. Meer informatie over het [uw knowledge base test](../How-To/test-knowledge-base.md).

Voor grote kB's, gebruik automatisch testen met de [generateAnswer API](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) en de `isTest` body eigenschap welke query's de `test` knowledge base in plaats van de gepubliceerde knowledge base. 

```json
{
  "question": "example question",
  "top": 3,
  "userId": "Default",
  "isTest": true
}
```

## <a name="publish-the-knowledge-base"></a>De knowledge base publiceren
Zodra u klaar bent met testen van de knowledge base, kunt u deze publiceert. De nieuwste versie van de geteste knowledge base voor een specifieke Azure Search-index vertegenwoordigt pushes publiceren de **gepubliceerd** knowledge base. Hiermee wordt ook een eindpunt gemaakt dat kan worden aangeroepen in uw toepassing of chatbot.

Op deze manier alle wijzigingen aan de testversie van de knowledge base niet van invloed op de gepubliceerde versie die mogelijk live in een productietoepassing.

Elk van deze knowledge bases kan worden gericht voor het testen van afzonderlijk. Met de API's, kunt u de testversie van de knowledge base met richten `isTest` eigenschap in de aanroep van generateAnswer hoofdtekst.

Meer informatie over het [kennisbank publiceren](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).

## <a name="monitor-usage"></a>Gebruik bewaken
Als u zich de chatlogs van uw service, moet u Application Insights inschakelen wanneer u [uw QnA Maker-service maken](../How-To/set-up-qnamaker-service-azure.md).

U kunt verschillende analyse van uw gebruik van de service ophalen. Meer informatie over hoe u application insights gebruiken om op te halen [analytics voor uw service QnA Maker](../How-To/get-analytics-knowledge-base.md).

Op basis van wat u van uw analyses leert, Controleer juiste [updates voor uw knowledge base](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Betrouwbaarheidsscore](./confidence-score.md)

## <a name="see-also"></a>Zie ook 

[Knowledge base](./knowledge-base.md)
[QnA Maker-overzicht](../Overview/overview.md)
