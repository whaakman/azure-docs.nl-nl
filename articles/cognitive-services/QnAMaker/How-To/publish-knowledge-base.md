---
title: Een kennisdatabase publiceren
titleSuffix: QnA Maker API - Azure Cognitive Services
description: Publiceren van uw knowledge base met QnA Maker API-service is de laatste stap om uw vraag beantwoorden eindpunt beschikbaar maken voor uw knowledge base. Wanneer u een kennisdatabase publiceert, wordt de status van de QnA-inhoud van uw knowledge base verplaatst van de test-index naar een productie-index in Azure search.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/11/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: c65654e7d6b2e66a07116ea9555ed316ace88912
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58091950"
---
# <a name="publish-a-knowledge-base-using-the-qna-maker-api-service-portal"></a>Een kennisdatabase met behulp van de portal QnA Maker API publiceren

Publiceren van uw knowledge base met QnA Maker API-service is de laatste stap om uw vraag beantwoorden eindpunt beschikbaar maken voor uw knowledge base. 

Wanneer u een kennisdatabase publiceren, wordt de status van de vraag en antwoord inhoud van uw knowledge base verplaatst van de test-index naar een productie-index in Azure search.

![Prod test index publiceren](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="publish-a-knowledge-base"></a>Een kennisdatabase publiceren

1. Zodra u klaar met de wijzigingen in uw KB, selecteert u **publiceren** in de bovenste navigatiebalk. U kunt tot het toegewezen aantal knowledge bases publiceren voor de Azure Search. 

    ![Knowledge base publiceren](../media/qnamaker-how-to-publish-kb/publish.png)

2. Selecteer **publiceren** opnieuw het om eindpuntdetails te bekijken die kunnen worden gebruikt in uw toepassing of bot-code.

    ![Is gepubliceerd knowledge base](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de knowledge base, kunt u deze in de QnA Maker portal verwijderen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Analyses te verkrijgen op uw knowledge base](./get-analytics-knowledge-base.md)
