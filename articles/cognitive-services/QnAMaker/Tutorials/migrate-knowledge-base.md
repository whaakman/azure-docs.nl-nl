---
title: Migreren van knowledge bases - QnA Maker
titleSuffix: Azure Cognitive Services
description: Migreren van een kennisdatabase vereist exporteren uit een knowledge base en vervolgens importeren in een andere.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/08/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 8ff3c497372a761bd8a02ae81bc897c8ee297bd0
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794868"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migreren van een kennisdatabase met behulp van exporteren / importeren

Migreren van een kennisdatabase vereist exporteren uit een knowledge base en vervolgens importeren in een andere. 

## <a name="prerequisites"></a>Vereisten

* Maak een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.
* Instellen van een nieuwe [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Een kennisdatabase migreren van QnA Maker
1. Aanmelden bij [QnA Maker portal](https://qnamaker.ai).
1. Selecteer de knowledge base die u wilt migreren.

1. Op de **instellingen** weergeeft, schakelt **exporteren kennisdatabase** voor het downloaden van een .tsv-bestand met de inhoud van uw knowledge base - vragen antwoorden, metagegevens, en de namen van de gegevensbron van dat ze zijn geëxtraheerd.

1. Selecteer **maken van een kennisdatabase** maakt een lege knowledge base in het menu bovenaan. 

    ![Set-gegevensbronnen](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Geef de service een **naam.** Dubbele namen worden ondersteund en speciale tekens worden ook ondersteund.

1. Selecteer **Maken**.

    ![KB maken](../media/qnamaker-how-to-create-kb/create-kb.png)

1. In deze nieuwe knowledge base, opent u de **instellingen** tabblad en selecteer **importeren knowledge base**. Deze invoer van de vragen, antwoorden en metagegevens en behoudt de namen van gegevensbronnen waaruit deze zijn geëxtraheerd.

   ![Import-kennisdatabase](../media/qnamaker-how-to-migrate-kb/Import.png)

1. **Test** de nieuwe knowledge base met behulp van het paneel Test. Meer informatie over het [uw knowledge base test](../How-To/test-knowledge-base.md).
1. **Publiceren** de knowledge base. Meer informatie over het [kennisbank publiceren](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).
1. Gebruik het eindpunt in uw toepassing of bot-code. Hier te zien hoe u [maken van een QnA bot](../Tutorials/create-qna-bot.md).

    ![QnA Maker-waarden](../media/qnamaker-how-to-migrate-kb/qnamaker-settings-kbid-key.png)

    Op dit moment worden alle de knowledge base-inhoud - vragen, antwoorden en metagegevens, samen met de namen van de bronbestanden en de URL's, geïmporteerd naar de nieuwe knowledge base. 

## <a name="chat-logs-and-alterations"></a>Chat-logboeken en -veranderingen
Niet-hoofdlettergevoelige wijzigingen (synoniemen) worden niet automatisch geïmporteerd. Gebruik de [V4-API's](https://go.microsoft.com/fwlink/?linkid=2092179) verplaatsen van de wijzigingen in de nieuwe knowledge base.

Er is geen manier voor het migreren van chatlogs, omdat de nieuwe knowledge base Application Insights gebruikt voor het opslaan van chatlogs. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase bewerken](../How-To/edit-knowledge-base.md)
