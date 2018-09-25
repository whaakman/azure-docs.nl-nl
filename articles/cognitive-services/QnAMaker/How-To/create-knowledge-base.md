---
title: Maak een knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: Chit chat toe te voegen aan uw bot kunt u meer beschrijvende en aantrekkelijke. QnA Maker kunt u eenvoudig een vooraf gevulde set van de bovenste chit-chat, toevoegen aan uw KB. Dit kan een beginpunt voor van uw bot chit-chat en bespaart u tijd en kosten van het schrijven van deze helemaal.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 274c2289c75f44c5a1c8dd3799612a23f46a6d67
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037691"
---
# <a name="create-a-knowledge-base"></a>Een kennisdatabase maken

QnA Maker kunt u eenvoudig uw bestaande gegevensbronnen voegen bij het maken van een kennisdatabase. U kunt een nieuwe QnA Maker-kennisdatabase maken van de volgende documenttypen:

<!-- added for scanability -->
* Veelgestelde vragen over pagina 's
* Producten handleidingen
* Gestructureerde documenten

## <a name="steps"></a>Stappen

1. Meld u aan bij naar de [QnA Maker portal](https://qnamaker.ai) met uw Azure-referenties in en selecteer **maken van nieuwe service**.

    ![KB maken ](../media/qnamaker-how-to-create-kb/create-new-service.png)

2. Als u nog geen een QnA Maker-service hebt gemaakt, selecteert u **maken van een service QnA**. 

3. Selecteer uw Azure-tenant, de naam van de Azure-abonnement en de naam van de Azure-resource die is gekoppeld aan de QnA Maker-service in de lijsten in **stap 2** in de portal voor QnA Maker. Selecteer de Azure QnA Maker-service die als voor de Knowledge Base host.

    ![QnA-service instellen](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

4. Voer de naam van uw knowledge base en de gegevensbronnen voor de nieuwe knowledge base.

    ![Set-gegevensbronnen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Geef de service een **naam.** Dubbele namen en speciale tekens worden ondersteund.
    - URL's voor gegevens die u uitgepakte wilt toevoegen. Zie voor meer informatie over de verschillende typen gegevensbronnen ondersteund [hier](../Concepts/data-sources-supported.md).
    - Uploaden van bestanden voor gegevens die u opgehaald wilt. Zie de [informatie over de prijzen](https://aka.ms/qnamaker-pricing) om te zien hoeveel documenten kunnen toevoegen.
    - Als u vragen en antwoorden supereenvoudig handmatig toevoegen wilt, kunt u overslaan **stap 4** weergegeven in de vorige afbeelding.

5. Voeg **Chit chat** aan uw KB. Kies chit chat-ondersteuning voor uw bot toevoegen door te kiezen uit een van de 3 vooraf gedefinieerde wensen. 

    <!-- TBD: add back in when chit chat how-to is merged
    ![Add chit-chat to KB ](../media/qnamaker-how-to-chitchat/create-kb-chit-chat.png)
    -->

6. Selecteer **maken uw KB**.

    ![KB maken](../media/qnamaker-how-to-create-kb/create-kb.png)

7. Het duurt enkele minuten duren voordat gegevens worden geëxtraheerd.

    ![Extractie](../media/qnamaker-how-to-create-kb/hang-tight-extraction.png)

8. Wanneer uw Knowledge Base is gemaakt, wordt u omgeleid naar de **Knowledge base** pagina.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Chit chat persoonlijke toevoegen](./chit-chat-knowledge-base.md)
