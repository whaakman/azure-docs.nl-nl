---
title: Sleutelbeheer - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Het beheren van uw sleutels QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b402187f4949dac34fa476648c81b980ba3efc96
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345306"
---
# <a name="key-management"></a>Sleutelbeheer

Uw service QnA Maker omgaat met twee soorten sleutels, **abonnement sleutels** en **eindpunt sleutels**.

![Sleutelbeheer](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonnement sleutels**: deze sleutels worden gebruikt voor toegang tot de [QnA Maker management-service API's](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff). Deze API's kunt u verschillende CRUD-bewerkingen voor uw knowledge base uitvoeren.  

2. **Eindpunt sleutels**: deze sleutels worden gebruikt voor toegang tot het eindpunt knowledge base voor een antwoord voor een gebruiker vraag. Doorgaans gebruikt u dit eindpunt in uw chat bot/App-code die de service QnA Maker verbruikt.
 
## <a name="subscription-keys"></a>Abonnement-sleutels
U kunt weergeven en opnieuw instellen van uw sleutels abonnement vanuit de Azure-portal waar u de resource QnA Maker hebt gemaakt. 
1. Ga naar de QnA Maker-bron in de Azure portal.

    ![Lijst met resources QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Ga naar **sleutels**.

    ![abonnementssleutel](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Sleutels voor eindpunt

Eindpunt sleutels kunnen worden beheerd vanaf de [QnA Maker portal](https://qnamaker.ai).

1. Meld u aan bij de [QnA Maker portal](https://qnamaker.ai), en Ga naar **sleutels beheren**.

    ![eindpuntsleutel](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Weergeven of opnieuw instellen van uw sleutels.

    ![de belangrijkste manager eindpunt](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Uw sleutels vernieuwen als u denkt dat ermee is geknoeid. Hiervoor moet mogelijk de bijbehorende wijzigingen aan uw App/Bot-code.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een knowledge base in een andere taal maken](./language-knowledge-base.md)
