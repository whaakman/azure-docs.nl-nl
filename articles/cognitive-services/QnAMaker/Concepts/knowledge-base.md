---
title: Knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: Een kennisdatabase QnA Maker bestaat uit een reeks vraag/antwoord (QnA)-sleutelparen en optionele metagegevens die zijn gekoppeld aan elk paar QnA.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: cb3426a960a6644b3ae149f02055cdb083febca7
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47040758"
---
# <a name="knowledge-base"></a>Knowledge base

Een kennisdatabase QnA Maker bestaat uit een reeks vraag/antwoord (QnA)-sleutelparen en optionele metagegevens die zijn gekoppeld aan elk paar QnA.

## <a name="key-knowledge-base-concepts"></a>Kennisbank van belangrijke concepten

* **Vragen over** -een vraag tekst bevat die het dichtst bij de aanvraag voor een gebruiker. 
* **Antwoorden** -antwoord is het antwoord dat wordt geretourneerd als een gebruikersquery met de bijbehorende vraag overeenkomt.  
* **Metagegevens** -metagegevens zijn labels die zijn gekoppeld aan een paar QnA en worden weergegeven als sleutel-waardeparen. Metagegevens worden gebruikt om te filteren QnA-paren en beperken van de set via welke query's die overeenkomen met wordt uitgevoerd.

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