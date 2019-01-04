---
title: Het testen van een knowledge base - QnA Maker
titlesuffix: Azure Cognitive Services
description: Testen van uw kennisdatabase QnA Maker is een belangrijk onderdeel van een iteratief proces voor het verbeteren van de nauwkeurigheid van de antwoorden die worden geretourneerd. De knowledge base via een verbeterde chat-interface waarmee ook kunt dat u wijzigingen aanbrengen, kunt u testen.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 12/17/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 2ad8d4fa04801996364755d1e0270a6509912679
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53581943"
---
# <a name="test-your-knowledge-base-interactively-in-qna-maker"></a>Test uw knowledge base interactief in QnA Maker

Testen van uw kennisdatabase QnA Maker is een belangrijk onderdeel van een iteratief proces voor het verbeteren van de nauwkeurigheid van de antwoorden die worden geretourneerd. De knowledge base via een verbeterde chat-interface waarmee ook kunt dat u wijzigingen aanbrengen, kunt u testen.

## <a name="test-answer-matching"></a>Test antwoorden vergelijken

1.  Toegang tot uw knowledge base door het selecteren van de naam ervan op de **mijn knowledge bases** pagina.
2.  Voor toegang tot het deelvenster van de dia-out Test, selecteer **Test** in het bovenste gedeelte van uw toepassing.

    ![Test-Toegangsvenster](../media/qnamaker-how-to-test-kb/access-test.png)

3.  Voer een query in het tekstvak in en selecteer Enter.

4.  De best overeenkomende antwoord in de kennisdatabase wordt geretourneerd als antwoord.

## <a name="clear-test-panel"></a>Deelvenster wissen testen

Schakel alle ingevoerde test-query's en de resultaten van de testconsole, selecteert u **beginnen** in de linkerbovenhoek van het paneel Test.

## <a name="close-test-panel"></a>Deelvenster sluiten testen

Als u wilt het Test-deelvenster sluiten, selecteert u de **Test** knop opnieuw. Tijdens de Test-deelvenster geopend is, kunt u de Knowledge Base-inhoud niet bewerken.

## <a name="inspect-score"></a>Controleren van score

U inspecteren details van de testresultaten in het deelvenster inspecteren.

1.  Met behulp van de Test dia-out deelvenster geopend, selecteert u **inspecteren** voor meer informatie over de respons.

    ![Antwoorden controleren](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Het deelvenster controle wordt weergegeven. Het deelvenster bevat de scoring-doel, evenals de geïdentificeerde entiteiten boven. Het deelvenster toont het resultaat van de geselecteerde utterance.

## <a name="correct-the-top-scoring-answer"></a>Corrigeer de bovenkant scoring-antwoord

Als de bovenkant scoring-antwoord onjuist is, selecteert u het juiste antwoord in de lijst en selecteer **opslaan en trainen**.

![Corrigeer de bovenkant scoring-antwoord](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Alternatieve vragen toevoegen

U kunt andere vormen van een vraag toevoegen aan een bepaald antwoord. Typ de alternatieve vindt u antwoorden op in het tekstvak in en klikt u op enter om te toe te voegen. Selecteer **opslaan en trainen** voor het opslaan van de updates.

![Alternatieve vragen toevoegen](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Een nieuw antwoord toevoegen

U kunt een nieuw antwoord toevoegen als een van de bestaande antwoorden die zijn afgestemd onjuist zijn of het antwoord niet in het knowledge base bestaat (geen goede overeenkomst gevonden in de KB). De nieuwe antwoord op de huidige vraag invoeren in het tekstvak in en druk op enter om deze te voegen. 

Selecteer **opslaan en trainen** om vast te leggen dit antwoord. Een nieuwe set met vraag-antwoord is nu is toegevoegd aan uw knowledge base.

![Nieuwe vraag en antwoord-paar toevoegen](../media/qnamaker-how-to-test-kb/add-answer.png)

> [!NOTE]
> Alleen alle wijzigingen aan uw knowledge base opgeslagen wanneer u op de **opslaan en trainen** knop.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase publiceren](./publish-knowledge-base.md)
