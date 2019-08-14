---
title: Niet-Engelse Knowledge Base-QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker ondersteunt Knowledge Base-inhoud in veel talen. Elke QnA Maker-service moet echter voor één taal worden gereserveerd. De eerste Knowledge Base die als doel heeft gericht op een bepaalde QnA Maker service, stelt de taal van die service in.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: 26792246267ced6d9fff50fe4fea11cc8d280d6a
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966677"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Taal ondersteuning van Knowledge Base-inhoud voor QnA Maker
QnA Maker ondersteunt Knowledge Base-inhoud in veel talen. Elke QnA Maker-service moet echter voor één taal worden gereserveerd. De eerste Knowledge Base die als doel heeft gericht op een bepaalde QnA Maker service, stelt de taal van die service in. [Hier](../Overview/languages-supported.md) vindt u een lijst met ondersteunde talen.

De taal wordt automatisch herkend door de inhoud van de gegevens bronnen die worden geëxtraheerd. Zodra u een nieuwe QnA Maker-service en een nieuwe Knowledge Base in die service hebt gemaakt, kunt u controleren of de taal juist is ingesteld.

1. Navigeer naar [Azure Portal](https://portal.azure.com/).

2. Selecteer **resource groepen** en navigeer naar de resource groep waar de QnA Maker-service is geïmplementeerd en selecteer de **Azure Search** resource.

    ![Azure Search resource selecteren](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. Selecteer de **testkb** -index. Deze Azure Search index is altijd de eerste die wordt gemaakt en bevat de opgeslagen inhoud van alle Knowledge bases in die service. 

    ![De test KB selecteren](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. Selecteer de sectie **velden** met de details van de testkb.

    ![Selecteer velden](../media/qnamaker-how-to-language-kb/selectfields.png)

5. Schakel het selectie vakje voor **analyse** in om de taal details te bekijken.

    ![Analyzer selecteren](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. U moet nagaan of de analyse functie is ingesteld op een bepaalde taal. Deze taal is automatisch gedetecteerd tijdens de stap voor het maken van de Knowledge Base. Deze taal kan niet worden gewijzigd nadat de resource is gemaakt.

    ![Geselecteerde analyse functie](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een QnA-bot maken met Azure Bot Service](../Tutorials/create-qna-bot.md)
