---
title: Een cognitieve knowledge base - QnA Maker - Azure-Services testen | Microsoft Docs
description: Test uw knowledge base voordat het wordt gepubliceerd.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: cffb63666edab25e1b3b0739d0e0f2f828600f3a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345312"
---
# <a name="test-your-knowledge-base"></a>Test uw kennisdatabase

Testen van uw kennisdatabase QnA Maker is een belangrijk onderdeel van een iteratief proces voor het verbeteren van de nauwkeurigheid van de antwoorden die wordt geretourneerd. U kunt de knowledge base via een verbeterde chat-interface die ook kunt dat u wijzigingen aanbrengen testen.

## <a name="test-answer-matching"></a>Test antwoorden vergelijken

1.  Toegang tot uw knowledge base door het selecteren van de naam ervan op de **mijn basissen knowledge** pagina.
2.  Selecteer voor toegang tot het paneel Test-dia-out moet **Test** in het bovenpaneel van uw toepassing.

    ![Test voor toegang](../media/qnamaker-how-to-test-kb/access-test.png)

3.  Voer een query in het tekstvak in en selecteer Enter.

4.  De best overeenkomende antwoord van de kennisdatabase wordt geretourneerd als het antwoord.

## <a name="clear-test-panel"></a>Schakel test-Configuratiescherm

Schakel alle ingevoerde test-query's en de resultaten van de test-console, selecteer **beginnen** op de linkerbovenhoek van het paneel Test.

## <a name="close-test-panel"></a>Deelvenster sluiten test

U sluit het deelvenster Test, selecteer de **Test** knop opnieuw. Terwijl het paneel Test geopend is, kunt u de Knowledge Base-inhoud niet bewerken.

## <a name="inspect-score"></a>Score controleren

U inspecteren details van de testresultaten in het deelvenster inspecteren.

1.  Met de Test dia-out Configuratiescherm openen, selecteer **inspecteren** voor meer informatie over de reactie.

    ![Controleren van antwoorden](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Het paneel inspectie wordt weergegeven. Het deelvenster bevat de bovenste score berekenen voor doel, evenals de geïdentificeerde entiteiten. Het paneel geeft het resultaat van de geselecteerde utterance.

## <a name="correct-the-top-scoring-answer"></a>Score berekenen antwoord boven corrigeren

Als boven score berekenen antwoord onjuist is, selecteert u het juiste antwoord in de lijst en selecteer **opslaan en trainen**.

![Test voor toegang](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Alternatieve vragen toevoegen

U kunt alternatieve vormen een van de vragen toevoegen aan een bepaald antwoord. Typ de alternatieve antwoorden in het tekstvak en klikt u op enter om te toe te voegen. Selecteer **opslaan en trainen** voor het opslaan van de updates.

![Test voor toegang](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Voeg een nieuw antwoord

U kunt een nieuw antwoord voor toevoegen als een van de bestaande antwoorden die zijn afgestemd onjuist zijn of het antwoord niet in het knowledge base bestaat (geen goede overeenkomst gevonden in de KB). Voer het nieuwe antwoord op de huidige vraag in het tekstvak en druk op enter om dit te voegen. 

Selecteer **opslaan en trainen** om vast te leggen dit antwoord. Een nieuwe vraag antwoord-set is nu toegevoegd aan uw knowledge base.

![Test voor toegang](../media/qnamaker-how-to-test-kb/add-answer.png)

> [!NOTE]
> Alle bewerkingen van uw knowledge base alleen worden opgeslagen wanneer u op de **opslaan en trainen** knop.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase publiceren](./publish-knowledge-base.md)
