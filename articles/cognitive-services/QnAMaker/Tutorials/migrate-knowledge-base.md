---
title: Migreren van preview knowledge bases - Qna Maker
titleSuffix: Azure Cognitive Services
description: Het importeren van een kennisdatabase
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: a06a04ba992c8d7e9691e4838d38faaafd48de7a
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48853909"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migreren van een kennisdatabase met behulp van exporteren / importeren
QnA Maker algemene beschikbaarheid aangekondigd op 7 mei 2018 op de \\\build\ conferentie. QnA Maker-algemene beschikbaarheid is een nieuwe architectuur die is gebouwd op Azure. Knowledge bases die zijn gemaakt met QnA Maker gratis Preview-versie moet worden gemigreerd naar de QnA Maker algemene beschikbaarheid. QnA Maker Preview wordt in November 2018 afgeschaft. Zie voor meer informatie over de wijzigingen in de QnA Maker-algemene beschikbaarheid, de aankondiging in de QnA Maker GA [blogbericht](https://aka.ms/qnamakerga-blog).

QnA Maker is nu een [prijsmodel](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/qna-maker/).

Vereisten
> [!div class="checklist"]
> * Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
> * Instellen van een nieuwe [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker-preview-portal"></a>Een kennisdatabase migreren van de QnA Maker Preview-portal
1. Navigeer naar [QnA Maker Preview-portal](https://aka.ms/qnamaker-old-portal
) en klikt u op **mijn services**.
2. Selecteer de knowledge base die u migreren wilt door te klikken op het pictogram voor bewerken.

    ![Kennisdatabase bewerken](../media/qnamaker-how-to-migrate-kb/preview-editkb.png)

3. Klik op **downloaden kennisdatabase** voor het downloaden van een .tsv-bestand met de inhoud van uw knowledge base - vragen antwoorden, metagegevens, en de namen van de gegevensbron uit dat deze zijn geëxtraheerd.

    ![Kennisdatabase downloaden](../media/qnamaker-how-to-migrate-kb/preview-download.png)

4. Meld u aan bij naar de [QnA Maker portal](https://qnamaker.ai) met uw azure-referenties en klik op **maken van een kennisdatabase**.
    
5. Als u nog geen een QnA Maker-service hebt gemaakt, selecteert u **maken van een service QnA**. Kies anders een QnA Maker-service in de vervolgkeuzelijsten in stap 2. Selecteer de QnA Maker-service die als voor de Knowledge Base host.

    ![QnA-service instellen](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

6. Een lege kennisdatabase maken 

    ![Set-gegevensbronnen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Geef de service een **naam.** Dubbele namen worden ondersteund en speciale tekens worden ook ondersteund.
    - Overslaan uploaden van bestanden of URL's die u wilt gebruikmaken van de gegevens van uw voorbeeld knowledge base. Nu maakt u een lege knowledge base.

7. Selecteer **Maken**.

    ![KB maken](../media/qnamaker-how-to-create-kb/create-kb.png)

8. In deze nieuwe Knowledge base, opent u de **instellingen** tabblad en klik op **importeren knowledge base**. Deze invoer van de vragen, antwoorden en metagegevens en behoudt de namen van gegevensbronnen waaruit deze zijn geëxtraheerd.

   ![Import-kennisdatabase](../media/qnamaker-how-to-migrate-kb/Import.png)

9. **Test** de nieuwe knowledge base met behulp van het paneel Test. Meer informatie over het [uw knowledge base test](../How-To/test-knowledge-base.md).
10. **Publiceren** de knowledge base. Meer informatie over het [kennisbank publiceren](../How-To/publish-knowledge-base.md).
11. Het eindpunt hieronder gebruiken in uw toepassing of bot-code. Hier te zien hoe u [maken van een QnA bot](../Tutorials/create-qna-bot.md).

    ![QnA Maker-waarden](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

Op dit moment worden alle de knowledge base-inhoud - vragen, antwoorden en metagegevens, samen met de namen van de bronbestanden en de URL's, geïmporteerd naar de nieuwe knowledge base. 

## <a name="chatlogs-and-alterations"></a>Chatlogs en wijzigingen
Wijzigingen (synoniemen) worden niet automatisch geïmporteerd. Gebruik de [V2 API's](https://aka.ms/qnamaker-v2-apis) voor het exporteren van de wijzigingen in de preview-stack en de [V4-API's](https://aka.ms/qnamaker-v4-apis) moet worden vervangen in de nieuwe stack.

Er is geen manier voor het migreren van chatlogs, omdat de nieuwe stack Application Insights gebruikt voor het opslaan van chatlogs. U kunt echter de chatlogs van downloaden de [preview-portal](https://aka.ms/qnamaker-old-portal).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase bewerken](../How-To/edit-knowledge-base.md)
