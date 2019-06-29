---
title: Samenwerken aan knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker kunnen meerdere mensen samenwerken aan een knowledge base. Deze functie is beschikbaar met de op rollen gebaseerd toegangsbeheer.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: 757b791f632e6d7ecb15a0a40384061f0658a6ff
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447516"
---
# <a name="collaborate-on-your-knowledge-base"></a>Samenwerken aan uw knowledge base

QnA Maker kunnen meerdere mensen samenwerken aan een knowledge base. Deze functie is beschikbaar met de Azure [Role-Based Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure). 

De volgende stappen uitvoeren om de QnA Maker-service delen met iemand:

1. Meld u aan bij Azure portal en Ga naar de QnA Maker-resource.

    ![Lijst met resources voor QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Ga naar de **Access Control (IAM)** tabblad.

    ![QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Selecteer **Toevoegen**.

    ![QnA Maker IAM toevoegen](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Selecteer de **eigenaar** of de **Inzender** rol. U kunt geen alleen-lezen toegang tot en met Role-Based Access Control verlenen. Rol van eigenaar en Inzender heeft lees-/ schrijftoegang recht op QnA Maker-service.

    ![QnA Maker IAM toevoegen rol](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Voer het e-mailbericht dat u wilt delen met, en klik op save.

    ![QnA Maker IAM toevoegen e-mailadres](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Nu als de persoon die u hebt gedeeld de QnA Maker-service met, meldt zich aan bij de [QnA Maker portal](https://qnamaker.ai) zien ze alle knowledge bases in die service.

Let op: u kunt een bepaalde knowledge base in een QnA Maker-service niet delen. Als u meer gedetailleerd toegangsbeheer wilt, kunt u overwegen uw knowledge bases distribueren naar verschillende QnA Maker-services.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase testen](./test-knowledge-base.md)
