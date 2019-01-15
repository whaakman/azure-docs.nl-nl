---
title: Niet-Engelse knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker biedt ondersteuning voor knowledge base-inhoud in vele talen. Elke QnA Maker-service moet echter worden gereserveerd voor één taal. De eerste knowledge base gemaakt die gericht is op een bepaalde QnA Maker-service Hiermee stelt u de taal van de betreffende service.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.openlocfilehash: e785abe09bba83f4ee69e7fd676422e22b44b391
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54261737"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Taalondersteuning van knowledge base-inhoud voor QnA Maker
QnA Maker biedt ondersteuning voor knowledge base-inhoud in vele talen. Elke QnA Maker-service moet echter worden gereserveerd voor één taal. De eerste knowledge base gemaakt die gericht is op een bepaalde QnA Maker-service Hiermee stelt u de taal van de betreffende service. Zie [hier](../Overview/languages-supported.md) voor een lijst van ondersteunde talen.

De taal wordt automatisch herkend van de inhoud van de gegevensbronnen die worden geëxtraheerd. Als u een nieuwe QnA Maker-Service en een nieuwe Knowledge Base in die service maakt, kunt u controleren of de taal correct is ingesteld.

1. Navigeer naar de [Azure-Portal](https://portal.azure.com/).

2. Selecteer **resourcegroepen** en navigeer naar de resourcegroep waar de QnA Maker-service geïmplementeerd en selecteer is de **Azure Search** resource.

    ![Azure Search-resource selecteren](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. Selecteer de **testkb** index. Deze Azure Search-index is altijd het eerste certificaat gemaakt en bevat de opgeslagen inhoud van alle knowledge bases in die service. 

    ![Selecteer de Test KB](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. Selecteer **velden** sectie worden de details testkb.

    ![Velden selecteren](../media/qnamaker-how-to-language-kb/selectfields.png)

5. Schakel het selectievakje voor **Analyzer** om taaldetails te bekijken.

    ![Selecteer Analyzer](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. U ziet dat de Analyzer is ingesteld op een bepaalde taal. Deze taal is automatisch gedetecteerd tijdens de stap knowledge base. Deze taal kan niet worden gewijzigd nadat de resource is gemaakt.

    ![Geselecteerde Analyzer](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een QnA bot maken met Azure Bot Service](../Tutorials/create-qna-bot.md)
