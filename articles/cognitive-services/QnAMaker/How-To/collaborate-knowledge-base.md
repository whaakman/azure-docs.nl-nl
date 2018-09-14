---
title: Samenwerken aan knowledge base - Qna Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker kunnen meerdere mensen samenwerken aan een knowledge base. Deze functie is beschikbaar met de op rollen gebaseerd toegangsbeheer.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 52cac0d73dd75f400c9777b9cc9c0b3ca101df58
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579799"
---
# <a name="collaborate-on-your-knowledge-base"></a>Samenwerken aan uw knowledge base

QnA Maker kunnen meerdere mensen samenwerken aan een knowledge base. Deze functie is beschikbaar met de Azure [Role-Based Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure). 

De volgende stappen uitvoeren om de QnA Maker-service delen met iemand:

1. Meld u aan bij de Azure-portal en Ga naar de QnA Maker-resource.

    ![Lijst met resources voor QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Ga naar de **Access Control (IAM)** tabblad.

    ![QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Selecteer **Toevoegen**.

    ![QnA Maker IAM toevoegen](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Selecteer de **eigenaar** of de **Inzender** rol.

    ![QnA Maker IAM toevoegen rol](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Voer het e-mailbericht dat u wilt delen met, en klik op save.

    ![QnA Maker IAM toevoegen e-mailadres](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Nu als de persoon die u hebt gedeeld de QnA Maker-service met, meldt zich aan bij de [QnA Maker portal](https://qnamaker.ai) zien ze alle knowledge bases in die service.

Let op: u kunt een bepaalde knowledge base in een QnA Maker-service niet delen. Als u meer gedetailleerd toegangsbeheer wilt, kunt u overwegen uw knowledge bases distribueren naar verschillende QnA Maker-services.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase testen](./test-knowledge-base.md)
