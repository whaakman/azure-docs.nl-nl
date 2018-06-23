---
title: Samenwerken aan uw knowledge base - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Hoe samenwerken aan uw QnA Maker-kB
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e18d656236276595fc5186a6656349bf28974ead
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345264"
---
# <a name="collaborate-on-your-knowledge-base"></a>Samenwerken aan uw kennisdatabase

QnA Maker kunnen meerdere mensen samenwerken aan een knowledge base. Deze functie wordt geleverd met de Azure [toegangsbeheer op basis van rollen](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure). 

Voer de volgende stappen uit om uw service QnA Maker delen met iemand:

1. Aanmelden bij de Azure-portal en gaat u naar uw QnA Maker-resource.

    ![Lijst met resources QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Ga naar de **Access Control (IAM)** tabblad.

    ![QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Selecteer **Toevoegen**.

    ![QnA Maker IAM toevoegen](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Selecteer de **eigenaar** of de **Inzender** rol.

    ![QnA Maker IAM toevoegen rol](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Voer het e-mailbericht dat u wilt delen en drukt u op opslaan.

    ![QnA Maker IAM toevoegen e-mail](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Nu als de persoon die u uw service QnA Maker met gedeeld, zich aanmeldt bij de [QnA Maker portal](https://qnamaker.ai) zien ze de kennis bases in die service.

Denk eraan dat u een bepaalde knowledge base in een QnA Maker-service niet delen. Als u meer gedetailleerd toegangsbeheer wilt, kunt u uw basissen kennis over verschillende QnA Maker services verdeeld.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een knowledge base testen](./test-knowledge-base.md)
