---
title: Het publiceren van een knowledge base - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Het publiceren van een knowledge base
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: e9dbeacfb0df98c6b8f084c263690c05fe966cdc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345265"
---
# <a name="publish-a-knowledge-base"></a>Een kennisdatabase publiceren

Publiceren van uw knowledge base, is de laatste stap bij het maken van uw knowledge base beschikbaar als een eindpunt voor het beantwoorden van vraag. 

Wanneer u een knowledge base publiceert, wordt de inhoud QnA van de knowledge base van de test-index verplaatst naar een productie-index in Azure search.

![Prod test index publiceren](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

## <a name="publish-a-knowledge-base"></a>Een kennisdatabase publiceren

1. Wanneer u klaar met de wijzigingen in uw KB, selecteren **publiceren** in de bovenste navigatiebalk. U kunt publiceren tot maximaal het toegewezen aantal kennis basissen voor de Azure Search. 

    ![Kennisdatabase publiceren](../media/qnamaker-how-to-publish-kb/publish.png)

2. Selecteer **publiceren** opnieuw het om eindpuntdetails te bekijken die kunnen worden gebruikt in uw toepassing of bot-code.

    ![Kennisdatabase publiceren](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Analytics op uw knowledge base ophalen](./get-analytics-knowledge-base.md)
