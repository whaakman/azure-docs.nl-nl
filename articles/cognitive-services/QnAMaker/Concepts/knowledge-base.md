---
title: Knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: Een kennisdatabase QnA Maker bestaat uit een reeks vraag/antwoord (QnA)-sleutelparen en optionele metagegevens die zijn gekoppeld aan elk paar QnA.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b9562a1686c4de4f4e2ef57a7d91bbf18dce63ef
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447607"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Wat is er een basis voor kennis van QnA Maker?

Een kennisdatabase QnA Maker bestaat uit een reeks vraag/antwoord (QnA)-sleutelparen en optionele metagegevens die zijn gekoppeld aan elk paar QnA.

## <a name="key-knowledge-base-concepts"></a>Kennisbank van belangrijke concepten

* **Vragen over** -een vraag tekst bevat die het dichtst bij de aanvraag voor een gebruiker. 
* **Antwoorden** -antwoord is het antwoord dat wordt geretourneerd als een gebruikersquery met de bijbehorende vraag overeenkomt.  
* **Metagegevens** -metagegevens zijn labels die zijn gekoppeld aan een paar QnA en worden weergegeven als sleutel-waardeparen. METADATA-codes worden gebruikt om te filteren QnA-paren en beperken van de set via welke query's die overeenkomen met wordt uitgevoerd.

Een enkele QnA, vertegenwoordigd door een numerieke ID van de QnA heeft meerdere varianten van een vraag (alternatieve vragen) dat alle aan een enkel antwoord toewijzen. Bovendien elk paar dergelijke meerdere metagegevensvelden die zijn gekoppeld aan deze kan hebben: één sleutel en één waarde.

![QnA Maker knowledge bases](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Inhoud Knowledge base-indeling

Wanneer u inhoud met opmaak in een knowledge base opnemen, probeert de QnA Maker aan de inhoud naar markdown converteren. Lezen [dit](https://aka.ms/qnamaker-docs-markdown-support) blog om te begrijpen van de markdown indelingen onbegrijpelijk voor de meeste chat-clients.

Velden van de metagegevens bestaan uit de sleutel / waarde-paren, gescheiden door een dubbele punt **(Product: Shredder)** . Zowel de sleutel en de waarde moet alleen tekst. De metagegevenssleutel mag geen spaties bevatten. Metagegevens ondersteunt slechts één waarde per sleutel.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Hoe QnA Maker een gebruikersquery selecteren van het beste antwoord verwerkt

De getraind en [gepubliceerd](/quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) QnA Maker-kennisdatabase ontvangt een gebruikersquery van een bot of andere clienttoepassing op de [GenerateAnswer API](/how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api). Het volgende diagram illustreert het proces als de gebruikersquery is ontvangen.

![Het proces classificatie voor een gebruikersquery](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

Het proces wordt uitgelegd in de volgende tabel:

|Stap|Doel|
|--|--|
|1|De clienttoepassing verzendt de gebruikersquery naar de [GenerateAnswer API](/how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api).|
|2|Qna Maker voorverwerking van de gebruikersquery taaldetectie spellingcontroleprogramma en woordafbrekingen.|
|3|Deze vooraf wordt gebruikt om te wijzigen van de query van de gebruiker voor de beste zoekresultaten.|
|4|Deze gewijzigde query wordt verzonden naar Azure Search-Index, ontvangen de `top` aantal resultaten. Als het juiste antwoord niet in deze resultaten, verhoogt u de waarde van `top` iets. In het algemeen een waarde van 10 voor `top` werkt in 90% van query's.|
|5|QnA Maker is van toepassing geavanceerde parametrisatie om de juistheid van de opgehaalde Azure Search-resultaten voor gebruikersquery te bepalen. |
|6|Het getrainde model kerntechnologie maakt gebruik van de functie score, uit stap 5, rangschikken van de Azure Search-resultaten.|
|7|De nieuwe resultaten worden geretourneerd naar de clienttoepassing in gerangschikte volgorde.|
|||

Gebruikt functies omvatten maar zijn niet beperkt tot het word-niveau semantiek, term-niveau van belang zijn in een corpus en uitgebreide geleerde semantische modellen om te bepalen van overeenkomsten en relevantie tussen twee tekenreeksen.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ontwikkelingslevenscyclus van een kennisdatabase](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Zie ook

[Overzicht van QnA Maker](../Overview/overview.md)
