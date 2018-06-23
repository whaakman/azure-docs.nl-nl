---
title: Het maken van een niet-Engelse knowledge base - QnA Maker - cognitieve Azure-Services | Microsoft Docs
description: Het maken van een niet-Engelse knowledge base.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 3fbd590229044af0daa60968fd8d556d539a58c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345297"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Taalondersteuning voor QnA Maker van knowledge base-inhoud
QnA Maker ondersteunt knowledge base-inhoud in vele talen. Elke QnA Maker-service moet echter worden gereserveerd voor één taal. De eerste knowledge base gemaakt die gericht is op een bepaalde QnA Maker-service wordt ingesteld voor de taal van die service. Zie [hier](../Overview/languages-supported.md) voor de lijst met ondersteunde talen.

De taal van de inhoud van de gegevensbronnen die zijn geëxtraheerd automatisch herkend. Wanneer u een nieuwe QnA Maker-Service en een nieuwe Knowledge Base in die service maakt, kunt u controleren of de taal correct is ingesteld.

1. Navigeer naar de [Azure-Portal](https://portal.azure.com/).

2. Selecteer **resourcegroepen** en navigeer naar de resourcegroep waar de QnA Maker-service geïmplementeerd en selecteer is de **Azure Search** resource.

    ![Selecteer Azure Search-resource](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. Selecteer de **testkb** index. Deze Azure Search-index is altijd de eerste gemaakt en bevat de opgeslagen inhoud van de kennis bases in die service. 

    ![Selecteer de Test KB](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. Selecteer **velden** sectie worden de details testkb.

    ![Velden selecteren](../media/qnamaker-how-to-language-kb/selectfields.png)

5. Schakel het selectievakje voor **Analyzer** om taaldetails te bekijken.

    ![Selecteer Analyzer](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. U ziet dat de Analyzer is ingesteld op een bepaalde taal. Deze taal is automatisch gedetecteerd tijdens de stap voor het maken van knowledge base. Deze taal kan niet worden gewijzigd zodra de resource is gemaakt.

    ![Geselecteerde Analyzer](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een bot QnA maken met Azure Bot Service](../Tutorials/create-qna-bot.md)
