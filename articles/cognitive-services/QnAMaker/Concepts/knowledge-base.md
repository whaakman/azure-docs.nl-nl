---
title: Knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: Een kennisdatabase QnA Maker bestaat uit een reeks vraag/antwoord (QnA)-sleutelparen en optionele metagegevens die zijn gekoppeld aan elk paar QnA.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 022b16669791b9b9cce066b3dd17c70b33569cc0
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955235"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Wat is er een basis voor kennis van QnA Maker?

Een kennisdatabase QnA Maker bestaat uit een reeks vraag/antwoord (QnA)-sleutelparen en optionele metagegevens die zijn gekoppeld aan elk paar QnA.

## <a name="key-knowledge-base-concepts"></a>Kennisbank van belangrijke concepten

* **Vragen over** -een vraag tekst bevat die het dichtst bij de aanvraag voor een gebruiker. 
* **Antwoorden** -antwoord is het antwoord dat wordt geretourneerd als een gebruikersquery met de bijbehorende vraag overeenkomt.  
* **Metagegevens** -metagegevens zijn labels die zijn gekoppeld aan een paar QnA en worden weergegeven als sleutel-waardeparen. Tags voor meta gegevens worden gebruikt voor het filteren van QnA-paren en het beperken van de set waarover overeenkomende query's worden uitgevoerd.

Een enkele QnA, vertegenwoordigd door een numerieke ID van de QnA heeft meerdere varianten van een vraag (alternatieve vragen) dat alle aan een enkel antwoord toewijzen. Daarnaast kunnen aan elk van deze combi natie meerdere meta gegevens velden zijn gekoppeld: één sleutel en één waarde.

![QnA Maker knowledge bases](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Inhoud Knowledge base-indeling

Wanneer u inhoud met opmaak in een knowledge base opnemen, probeert de QnA Maker aan de inhoud naar markdown converteren. Lezen [dit](https://aka.ms/qnamaker-docs-markdown-support) blog om te begrijpen van de markdown indelingen onbegrijpelijk voor de meeste chat-clients.

Velden van de metagegevens bestaan uit de sleutel / waarde-paren, gescheiden door een dubbele punt **(Product: Shredder)** . Zowel de sleutel en de waarde moet alleen tekst. De metagegevenssleutel mag geen spaties bevatten. Meta gegevens bieden ondersteuning voor slechts één waarde per sleutel.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Hoe QnA Maker een gebruikers query verwerkt om het beste antwoord te selecteren

De getrainde en [gepubliceerde](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) QnA Maker Knowledge Base ontvangt een gebruikers query, vanuit een bot of andere client toepassing, op de [GenerateAnswer-API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage). Het volgende diagram illustreert het proces wanneer de query van de gebruiker wordt ontvangen.

![Het volgorde proces voor een gebruikers query](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

Het proces wordt uitgelegd in de volgende tabel:

|Stap|Doel|
|--|--|
|1|De client toepassing verzendt de query van de gebruiker naar de [GenerateAnswer-API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage).|
|2|QnA Maker voor het verwerken van de gebruikers query met taal detectie, spellingen en woord afbreekers.|
|3|Deze voor verwerking wordt gebruikt om de gebruikers query voor de beste Zoek resultaten te wijzigen.|
|4|Deze gewijzigde query wordt verzonden naar Azure search index, waarbij het `top` aantal resultaten wordt ontvangen. Als het juiste antwoord niet in deze resultaten voor komt, verhoogt u `top` de waarde enigszins. Over het algemeen is de waarde `top` 10 voor Works in 90% van de query's.|
|5|QnA Maker past geavanceerde parametrisatie toe om de juistheid van de opgehaalde Azure Search resultaten voor de gebruikers query te bepalen. |
|6|Het getrainde rangorde model gebruikt de functie Score uit stap 5 om de Azure Search resultaten te rangschikken.|
|7|De nieuwe resultaten worden weer gegeven in de rang orde van de client toepassing.|
|||

De functies die worden gebruikt, zijn, maar zijn niet beperkt tot semantiek op woord niveau, urgentie op term niveau in een verzameling en diep opgedane semantische modellen om de gelijkenis en relevantie tussen twee teken reeksen te bepalen.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ontwikkelingslevenscyclus van een kennisdatabase](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Zie ook

[Overzicht van QnA Maker](../Overview/overview.md)
