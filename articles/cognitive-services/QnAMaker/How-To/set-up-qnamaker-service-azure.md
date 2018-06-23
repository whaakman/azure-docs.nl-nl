---
title: Het instellen van een service QnA Maker - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Het instellen van een QnA Maker-service
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: ce452dd686529e017b4eae4717eadb044b389409
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345303"
---
# <a name="create-a-qna-maker-service"></a>Een QnA Maker-service maken

Voordat u kunt geen kennis QnA Maker databases maken kunt, moet u eerst een QnA Maker-service in Azure instellen. Iedereen met autorisatie voor het maken van nieuwe resources in een abonnement kunt instellen wanneer er een QnA Maker-service.

Deze instellingen implementeert een paar Azure-resources. Deze resources worden samen het knowledge base-inhoud beheren en bieden mogelijkheden voor het beantwoorden van vraag al een eindpunt.

1. Meld u aan bij [Azure Portal](<https://portal.azure.com>).

2.  Klik op **nieuwe resource toevoegen**, typ 'qna maker' in de zoekopdracht en selecteer de QnA Maker-resource

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/create-new-resource.png)

3.  Klik op **maken** na het lezen van de voorwaarden en bepalingen.

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

4. In **QnA Maker**, selecteer de juiste lagen en regio's.

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Vul de **naam** met een unieke naam om deze service QnA Maker te identificeren. Deze naam identificeert ook het QnA Maker-eindpunt waarmee uw kennis basissen gekoppeld worden.
    * Kies de **abonnement** in de resource QnA Maker zullen worden geïmplementeerd.
    * Selecteer de **Management prijscategorie** voor QnA Maker management services (portal en API's). Zie [hier](https://aka.ms/qnamaker-pricing) voor meer informatie over de prijzen van de SKU's.
    * Maak een nieuwe **resourcegroep** (aanbevolen) of gebruik een bestaande waarin deze resource QnA Maker implementeren.
    * Kies de **zoeken prijscategorie** van de Azure Search-service. Als u de laag gratis optie grijs wordt weergegeven, betekent dit hebt u al een gratis Azure Search-laag geïmplementeerd in uw abonnement. In dat geval moet u beginnen met de laag Basic Azure Search. Details van de Azure search-prijzen [hier](https://azure.microsoft.com/en-us/pricing/details/search/).
    * Kies de **zoeklocatie** waar u gegevens van de Azure Search te worden geïmplementeerd. Beperkingen in waar de gegevens van de klant moet worden opgeslagen, wordt de locatie die u voor Azure Search kiest informeren.
    * Geef een naam aan uw App service in **appnaam**.
    * Standaard wordt de App service standaard naar de prijscategorie standard (S1). U kunt het plan wijzigen nadat is gemaakt. Meer details van de prijzen van App service [hier](https://azure.microsoft.com/en-in/pricing/details/app-service/).
    * Kies de **Website locatie** waarop de App-Service wordt geïmplementeerd.

        > [!NOTE]
        > De locatie van de zoekopdracht kan afwijken van de locatie van de Website.

    * Kies of u wilt inschakelen **Application Insights** of niet. Als **Application Insights** is ingeschakeld, verzamelt QnA Maker telemetrie op verkeer, chat-logboeken en fouten.
    * Kies de **App insights locatie** waar Application Insights-resource wordt geïmplementeerd.

5. Zodra alle velden zijn geverifieerd, kunt u klikken op **maken** implementatie van deze services in uw abonnement te starten. Het duurt enkele minuten om te voltooien.

6.  Als de implementatie is voltooid, ziet u de volgende bronnen in uw abonnement hebt gemaakt.

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Maken en publiceren van een knowledge base](../Quickstarts/create-publish-knowledge-base.md)