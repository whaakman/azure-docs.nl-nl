---
title: Knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: Een kennisdatabase QnA Maker bestaat uit een reeks vraag/antwoord (QnA)-sleutelparen en optionele metagegevens die zijn gekoppeld aan elk paar QnA.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/18/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: d036b93bc043f74dedae24455ca0a43cd808a936
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227111"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Wat is er een basis voor kennis van QnA Maker?

Een kennisdatabase QnA Maker bestaat uit een reeks vraag/antwoord (QnA)-sleutelparen en optionele metagegevens die zijn gekoppeld aan elk paar QnA.

## <a name="key-knowledge-base-concepts"></a>Kennisbank van belangrijke concepten

* **Vragen over** -een vraag tekst bevat die het dichtst bij de aanvraag voor een gebruiker. 
* **Antwoorden** -antwoord is het antwoord dat wordt geretourneerd als een gebruikersquery met de bijbehorende vraag overeenkomt.  
* **Metagegevens** -metagegevens zijn labels die zijn gekoppeld aan een paar QnA en worden weergegeven als sleutel-waardeparen. METADATA-codes worden gebruikt om te filteren QnA-paren en beperken van de set via welke query's die overeenkomen met wordt uitgevoerd.

Een enkele QnA, vertegenwoordigd door een numerieke ID van de QnA heeft meerdere varianten van een vraag (alternatieve vragen) dat alle aan een enkel antwoord toewijzen. Deze bladwijzers kan bovendien meerdere metagegevensvelden die zijn gekoppeld aan deze hebben.

![QnA Maker knowledge bases](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Inhoud Knowledge base-indeling

Wanneer u inhoud met opmaak in een knowledge base opnemen, probeert de QnA Maker aan de inhoud naar markdown converteren. Lezen [dit](https://aka.ms/qnamaker-docs-markdown-support) blog om te begrijpen van de markdown indelingen onbegrijpelijk voor de meeste chat-clients.

Velden van de metagegevens bestaan uit de sleutel / waarde-paren, gescheiden door een dubbele punt **(Product: Shredder)**. Zowel de sleutel en de waarde moet alleen tekst. De metagegevenssleutel mag geen spaties bevatten.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ontwikkelingslevenscyclus van een kennisdatabase](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Zie ook

[Overzicht van QnA Maker](../Overview/overview.md)
