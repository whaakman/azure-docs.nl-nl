---
title: Resource en de sleutel-management - QnA Maker
titleSuffix: Azure Cognitive Services
description: Uw service QnA Maker omgaat met twee soorten sleutels, abonnementssleutels en eindpunt sleutels.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b0b82f37b76a6bca6d84a05bc48b7ae9986fb76b
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967684"
---
# <a name="how-to-manage-keys-in-qna-maker"></a>Over het beheren van sleutels in QnA Maker

Uw service QnA Maker omgaat met twee soorten sleutels, **abonnementssleutels** en **eindpunt sleutels**.

![Sleutelbeheer](../media/qnamaker-how-to-key-management/key-management.png)

1. **Abonnements sleutels**: Deze sleutels worden gebruikt voor toegang tot de Api's van de [QnA Maker Management-service](https://go.microsoft.com/fwlink/?linkid=2092179). Met deze Api's kunt u uw kennis database bewerken.  

2. **Eindpunt sleutels**: Deze sleutels worden gebruikt om toegang te krijgen tot het eind punt van de Knowledge Base om een antwoord te krijgen op de vraag van een gebruiker. Normaal gesp roken gebruikt u dit eind punt in uw chat bot of client toepassings code die de QnA Maker-service verbruikt.
 
## <a name="subscription-keys"></a>Abonnementssleutels
U kunt weergeven en opnieuw instellen van uw abonnementssleutels vanuit de Azure-portal waar u de QnA Maker-resource hebt gemaakt. 
1. Ga naar de QnA Maker-resource in Azure portal.

    ![Lijst met resources voor QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Ga naar **sleutels**.

    ![Abonnementssleutel](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Eindpunt-sleutels

Eindpunt-sleutels kunnen worden beheerd vanuit de [QnA Maker portal](https://qnamaker.ai).

1. Meld u aan bij de [QnA Maker-Portal](https://qnamaker.ai), ga naar uw profiel en klik vervolgens op service- **instellingen**.

    ![Eindpuntsleutel](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Weergeven of opnieuw instellen van uw sleutels.

    ![belangrijkste endpoint-beheerder](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Als u denkt dat ze zijn aangetast, vernieuwt u uw sleutels. Hiervoor kunnen eventueel bijbehorende wijzigingen in de client toepassing of bot code worden vereist.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase maken in een andere taal](./language-knowledge-base.md)
