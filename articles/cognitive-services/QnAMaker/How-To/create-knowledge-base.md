---
title: Maak een knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: Chit chat toe te voegen aan uw bot kunt u meer beschrijvende en aantrekkelijke. QnA Maker kunt u eenvoudig een vooraf gevulde set van de bovenste chit-chat, toevoegen aan uw KB. Dit kan een beginpunt voor van uw bot chit-chat en bespaart u tijd en kosten van het schrijven van deze helemaal.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 66705a0afdcdb04fe49bea0bfc03286df3611071
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543355"
---
# <a name="create-a-knowledge-base"></a>Een kennisdatabase maken

QnA Maker is het zeer eenvoudig om onboarding uw bestaande gegevensbronnen te maken van een kennisdatabase. U kunt een nieuwe QnA Maker-kennisdatabase maken van de veelgestelde vragen over pagina's, producten handleidingen, gestructureerde documenten of redactioneel toe te voegen.

## <a name="steps"></a>Stappen

1. Als u wilt beginnen, meld u aan bij naar de [QnA Maker portal](https://qnamaker.ai) met uw azure-referenties en klik op **maken van nieuwe service**.

    ![KB maken ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Als u nog geen een QnA Maker-service hebt gemaakt, selecteert u **maken van een service QnA**. Kies anders een QnA Maker-service in de vervolgkeuzelijsten in stap 2. Selecteer de QnA Maker-service die als voor de Knowledge Base host.

    ![QnA-service instellen](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

3. Voer de volgende informatie om te kunnen maken van de knowledge base.

    ![Set-gegevensbronnen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Geef de service een **naam.** Dubbele namen worden ondersteund en speciale tekens worden ook ondersteund.
    - Plak in URL's die worden geëxtraheerd uit. Zie voor meer informatie over de verschillende typen gegevensbronnen ondersteund [hier](../Concepts/data-sources-supported.md).
    - U kunt ook uploadbestanden waaruit gegevens worden opgehaald. Zie de [informatie over de prijzen](https://aka.ms/qnamaker-pricing
) om te zien hoeveel documenten kunnen toevoegen.
    - Als u handmatig toevoegen van vragen en antwoorden supereenvoudig wilt, kunt u overslaan bestanden koppelen.

4. Selecteer **Maken**.

    ![KB maken](../media/qnamaker-how-to-create-kb/create-kb.png)

5. Het duurt enkele minuten duren voordat gegevens worden geëxtraheerd.

    ![Extractie](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

6. Als uw Knowledge Base is gemaakt, wordt u omgeleid naar de **Knowledge base** pagina.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase importeren](../Tutorials/migrate-knowledge-base.md)
