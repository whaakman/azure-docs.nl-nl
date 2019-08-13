---
title: Levenscyclus van het knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker leert beste in een iteratief cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van eindpunt query's.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 97a4673be2a611149806855e792c5bf1f7a0942a
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955178"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Levenscyclus van de Knowledge base in QnA Maker
QnA Maker leert beste in een iteratief cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van eindpunt query's. 

![Ontwerpcyclus](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Het maken van een kennisdatabase QnA Maker
QnA Maker knowledge base (KB)-eindpunt biedt een best-none-match-antwoord op een gebruikersquery op basis van de inhoud van de KB. Het maken van een kennisdatabase is een eenmalige actie voor het instellen van een opslagplaats voor inhoud van de vragen, antwoorden en gekoppelde metagegevens. Een kennisdatabase kunnen worden gemaakt met het verkennen van de bestaande inhoud zoals veelgestelde vragen over pagina's, producthandleidingen of gestructureerde Q-A-paren. Meer informatie over het [maken van een kennisdatabase](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Testen en bijwerken van de knowledge base

De knowledge base is klaar voor testen wanneer het wordt gevuld met inhoud, redactioneel of via automatische extractie. Interactieve tests kunnen worden uitgevoerd in de QnA Maker portal via het deel venster **testen** door algemene gebruikers query's in te voeren en te controleren of de antwoorden die zijn geretourneerd met de juiste reactie en voldoende betrouw bare Score zijn. 

* **Zo kunt u lage betrouwbaarheids scores herstellen**: alternatieve vragen toevoegen. 
* **Wanneer een query niet de [standaard reactie](confidence-score.md#change-default-answer)retourneert**: nieuwe antwoorden toevoegen aan de juiste vraag. 

Deze lus van test-update gaat door totdat u tevreden met de resultaten bent. Meer informatie over het [uw knowledge base test](../How-To/test-knowledge-base.md).

Gebruik voor grote kb's geautomatiseerde tests met de [generateAnswer-API](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) en de `isTest` eigenschap Body die de `test` Knowledge Base doorzoekt in plaats van de gepubliceerde kennis database. 

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

Elk van deze knowledge bases kan worden gericht voor het testen van afzonderlijk. Met behulp van de api's kunt u de test versie van de Knowledge Base `isTest` richten op de eigenschap Body in de generateAnswer-aanroep.

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
