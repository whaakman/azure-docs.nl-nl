---
title: Levenscyclus van de ontwikkeling van een knowledge base - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Levenscyclus van de ontwikkeling van een knowledge base
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: 9ecdd2c7823eed145621b214690eff7681065507
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "35345949"
---
# <a name="knowledge-base-lifecycle"></a>Knowledge base-levenscyclus
QnA Maker leert best in een herhalende cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van de endpoint-query's. 

![Cyclus ontwerpen](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Maken van een QnA Maker knowledge base
QnA Maker knowledge base (KB)-eindpunt zorgt voor een optimale antwoord op een gebruikersquery op basis van de inhoud van de KB. Maken van een knowledge base is een eenmalige bewerking voor het instellen van een opslagplaats van vragen, antwoorden en gekoppelde metagegevens voor inhoud. Een knowledge base kunnen worden gemaakt door het verkennen van de bestaande inhoud zoals veelgestelde vragen over pagina's, producthandleidingen of gestructureerde Q-A-paren. Meer informatie over hoe [maken van een knowledge base](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Testen en bijwerken van de knowledge base
De knowledge base is gereed voor het testen van zodra deze is gevuld met inhoud, redactioneel of via automatische extractie. Testen kunt u doen via de **Test** deelvenster door te voeren algemene gebruikersquery en controleren of de antwoorden geretourneerd naar verwachting zijn en met voldoende vertrouwen score. U kunt andere vragen op te lossen lage vertrouwen scores toevoegen. U kunt ook nieuwe antwoorden toevoegen wanneer een query retourneert 'geen overeenkomst gevonden in de KB' default antwoord. Deze nauwe lus van update van de test wordt vervolgd totdat u tevreden met de resultaten bent. Meer informatie over hoe [uw knowledge base test](../How-To/test-knowledge-base.md).

Voor grote kB kan de tests worden geautomatiseerd door middel van generateAnswer API's. 

## <a name="publish-the-knowledge-base"></a>De kennisdatabase publiceren
Wanneer u klaar bent de knowledge base test, kunt u deze publiceren. De nieuwste versie van de geteste kennisdatabase naar een specifieke Azure Search-index die pushes publiceren de **gepubliceerd** knowledge base. Deze maakt ook een eindpunt dat kan worden aangeroepen in uw toepassing of chat bot.

Op deze manier eventuele wijzigingen in de testversie van de knowledge base hebben geen invloed op de gepubliceerde versie die mogelijk live in een productietoepassing.

Elk van deze kennis kan worden gericht voor het testen van afzonderlijk. Met de API's, kunt u de testversie van de kennisdatabase met richt `isTest=true` vlag in de aanroep van generateAnswer.

Meer informatie over hoe [publiceren van uw knowledge base](../How-To/publish-knowledge-base.md).

## <a name="monitor-usage"></a>Gebruik bewaken
Als u de logboeken chat van uw service aanmelden, moet u Application Insights inschakelen wanneer u [maken van uw service QnA Maker](../How-To/set-up-qnamaker-service-azure.md).

U kunt verschillende analyse van uw gebruik van de service. Meer informatie over het gebruiken van application insights voor [analytics voor uw service QnA Maker](../How-To/get-analytics-knowledge-base.md).

Op basis van wat u van uw analyses leert, zorg juiste [updates voor uw knowledge base](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Score vertrouwen](./confidence-score.md)

## <a name="see-also"></a>Zie ook 

[Kennisdatabase](./knowledge-base.md)
[QnA Maker-overzicht](../Overview/overview.md)
