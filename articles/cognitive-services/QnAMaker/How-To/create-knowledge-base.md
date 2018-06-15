---
title: Het maken van een cognitieve knowledge base - QnA Maker - Azure-Services | Microsoft Docs
description: Het maken van een knowledge base
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 93b64948ecc52feeb0f862f2b76ea898dce2333a
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345925"
---
# <a name="create-a-knowledge-base"></a>Een kennisdatabase maken

QnA Maker kunt u heel eenvoudig om vrij te geven uw bestaande gegevensbronnen voor het maken van een knowledge base. U kunt maken van een nieuwe QnA Maker knowledge base van veelgestelde vragen over pagina's, producten-handleidingen, gestructureerde documenten of redactioneel toe te voegen.

## <a name="steps"></a>Stappen

1. Om te beginnen, meld u aan bij naar de [QnA Maker portal](https://qnamaker.ai) met uw azure-referenties en klik op **nieuwe service maken**.

    ![KB maken ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Als u nog geen een QnA Maker-service hebt gemaakt, selecteert u **maken van een service QnA**. Kies anders een QnA Maker-service van de vervolgkeuzelijsten in stap 2. Selecteer de QnA Maker-service die als host voor de Knowledge Base fungeert.

    ![Setup Writer-service](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

3. Voer de volgende informatie om te kunnen maken van de knowledge base.

    ![Set gegevensbronnen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Geef uw service een **naam.** Dubbele namen worden ondersteund en speciale tekens worden ook ondersteund.
    - Plak in URL's die worden opgehaald uit. Zie voor meer informatie over de soorten gegevensbronnen ondersteund [hier](../Concepts/data-sources-supported.md).
    - U kunt ook bestanden waaruit gegevens worden geëxtraheerd uploaden. Zie de [prijsgegevens](https://aka.ms/qnamaker-pricing
) om te zien hoeveel documenten kunnen toevoegen.
    - Als u handmatig toe te voegen QnAs wilt, kunt u overslaan bestanden koppelen.

4. Selecteer **Maken**.

    ![KB maken](../media/qnamaker-how-to-create-kb/create-kb.png)

5. Het duurt enkele minuten duren voordat de gegevens moet worden geëxtraheerd.

    ![Uitpakken](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

6. Als uw Knowledge Base heeft gemaakt, wordt u omgeleid naar de **kennisdatabase** pagina.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een knowledge base importeren](../Tutorials/migrate-knowledge-base.md)
