---
title: Samen werken op Knowledge Base-QnA Maker
titleSuffix: Azure Cognitive Services
description: Met QnA Maker kunnen meerdere personen samen werken aan een Knowledge Base. Deze functie wordt meegeleverd met de op rollen gebaseerde Access Control van Azure.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: diberry
ms.openlocfilehash: 9c5398ff7cb31698db3d4a798b6a082f9e74b99b
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955139"
---
# <a name="collaborate-on-your-knowledge-base"></a>Samen werken aan uw Knowledge Base

Met QnA Maker kunnen meerdere personen samen werken aan een Knowledge Base. Deze functie wordt meegeleverd met de [op rollen gebaseerde Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)van Azure. 

Voer de volgende stappen uit om uw QnA Maker-service te delen met iemand:

1. Meld u aan bij de Azure Portal en ga naar uw QnA Maker-resource.

    ![Lijst met resources voor QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Ga naar het tabblad **Access Control (IAM)** .

    ![QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Selecteer **Toevoegen**.

    ![QnA Maker IAM toevoegen](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Selecteer de rol **eigenaar** of **Inzender** . U kunt alleen-lezen toegang verlenen via Access Control op basis van rollen. De rol eigenaar en Inzender heeft lees-/schrijftoegang tot QnA Maker-service.

    ![QnA Maker IAM-rol toevoegen](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Voer het e-mail adres in waarmee u wilt delen en druk op opslaan.

    ![QnA Maker IAM add e-mail](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Wanneer de persoon met wie u uw QnA Maker-service hebt gedeeld, zich aanmeldt bij de [QnA Maker-Portal](https://qnamaker.ai) , kunnen ze alle kennissen in die service zien.

Houd er rekening mee dat u een bepaalde Knowledge Base niet kunt delen in een QnA Maker-service. Als u meer nauw keuriger toegangs beheer wilt, kunt u uw kennis grondslagen distribueren over verschillende QnA Maker Services.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennis database testen](./test-knowledge-base.md)
