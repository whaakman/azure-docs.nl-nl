---
title: Het importeren van een knowledge base - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Het importeren van een knowledge base
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: ce8f98f9bdb37d5f326e942fe5b5e815e5272c56
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345926"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migreren van een kennisdatabase met exporteren / importeren
QnA Maker algemene beschikbaarheid aangekondigd op 7 mei 2018 op de \\\build\ conferentie. QnA Maker GA heeft een nieuwe architectuur is gebaseerd op Azure. Kennis-databases die zijn gemaakt met QnA Maker gratis Preview moet worden gemigreerd naar QnA Maker algemene beschikbaarheid. QnA Maker Preview in November 2018 afgeschaft. Zie voor meer informatie over de wijzigingen in QnA Maker GA de aankondiging QnA Maker GA [blogbericht](https://aka.ms/qnamakerga-blog).

QnA Maker heeft nu een [prijzen model](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/qna-maker/).

Vereisten
> [!div class="checklist"]
> * Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
> * Instellen van een nieuwe [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker-preview-portal"></a>Migreren van een knowledge base van QnA Maker Preview-portal
1. Navigeer naar [QnA Maker Preview-portal](https://aka.ms/qnamaker-old-portal
) en klik op **mijn services**.
2. Selecteer de kennisdatabase die u migreren wilt door te klikken op het bewerkingspictogram.

    ![Kennisdatabase bewerken](../media/qnamaker-how-to-migrate-kb/preview-editkb.png)

3. Klik op **downloaden kennisdatabase** voor het downloaden van een .tsv-bestand met de inhoud van de knowledge base - vragen, antwoorden, metagegevens en de namen van de gegevensbron uit die deze zijn geëxtraheerd.

    ![Kennisdatabase downloaden](../media/qnamaker-how-to-migrate-kb/preview-download.png)

4. Meld u aan bij naar de [QnA Maker portal](https://qnamaker.ai) met uw azure-referenties en klik op **nieuwe service maken**.

    ![KB maken ](../media/qnamaker-how-to-create-kb/create-new-service.png)
    
5. Als u nog geen een QnA Maker-service hebt gemaakt, selecteert u **maken van een service QnA**. Kies anders een QnA Maker-service van de vervolgkeuzelijsten in stap 2. Selecteer de QnA Maker-service die als host voor de Knowledge Base fungeert.

    ![Setup Writer-service](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

6. Maken van een leeg kennisdatabase 

    ![Set gegevensbronnen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Geef uw service een **naam.** Dubbele namen worden ondersteund en speciale tekens worden ook ondersteund.
    - Uploaden van bestanden of URL's overslaan als u wilt gebruiken de gegevens van uw Preview knowledge base. U maakt nu een lege knowledge base.

7. Selecteer **Maken**.

    ![KB maken](../media/qnamaker-how-to-create-kb/create-kb.png)

8. In deze nieuwe Knowledge base, opent u de **instellingen** tabblad en klik op **importeren kennisdatabase**. Hiermee importeert de vragen, antwoorden en metagegevens en behoudt de namen van gegevensbronnen van waaruit ze zijn uitgepakt.

   ![Kennisdatabase importeren](../media/qnamaker-how-to-migrate-kb/Import.png)

9. **Test** de nieuwe knowledge base in het deelvenster Test. Meer informatie over hoe [uw knowledge base test](../How-To/test-knowledge-base.md).
10. **Publiceren** de knowledge base. Meer informatie over hoe [publiceren van uw knowledge base](../How-To/publish-knowledge-base.md).
11. Het eindpunt hieronder gebruiken in uw toepassing of bot-code. Hier ziet hoe [maken van een bot QnA](../Tutorials/create-qna-bot.md).

    ![QnA Maker waarden](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

Op dit moment alle kennisdatabase inhoud - vragen, antwoorden en metagegevens, samen met de namen van de bronbestanden en de URL's, zijn geïmporteerd naar de nieuwe knowledge base. 

## <a name="chatlogs-and-alterations"></a>Chatlogs en -veranderingen
Veranderingen (synoniemen) worden niet automatisch geïmporteerd. Gebruik de [V2 API's](https://aka.ms/qnamaker-v2-apis) de wijzigingen van de preview-stack exporteren en de [V4-API's](https://aka.ms/qnamaker-v4-apis) vervangen in de nieuwe-stack.

Er is geen manier voor het migreren van chatlogs, omdat de nieuwe stack Application Insights voor het opslaan van chatlogs gebruikt. U kunt echter de chatlogs van downloaden de [preview-portal](https://aka.ms/qnamaker-old-portal).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een knowledge base bewerken](../How-To/edit-knowledge-base.md)
