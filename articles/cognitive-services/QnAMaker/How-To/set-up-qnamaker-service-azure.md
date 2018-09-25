---
title: Instellen van een service voor QnA Maker - QnA Maker
titleSuffix: Azure Cognitive Services
description: Voordat u een QnA Maker knowledge bases maken kunt, moet u eerst een QnA Maker-service in Azure instellen. Iedereen met autorisatie voor het maken van nieuwe resources in een abonnement kunt een QnA Maker-service instellen.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 87bcd23983c1d973f7e90caf806092c909b7b9e4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032543"
---
# <a name="create-a-qna-maker-service"></a>Een QnA Maker-service maken

Voordat u een QnA Maker knowledge bases maken kunt, moet u eerst een QnA Maker-service in Azure instellen. Iedereen met autorisatie voor het maken van nieuwe resources in een abonnement kunt een QnA Maker-service instellen.

Deze instellingen implementeert een aantal Azure-resources. Deze resources worden samen de knowledge base-inhoud beheren en bieden mogelijkheden beantwoorden van vraag al een eindpunt.

1. Meld u aan bij [Azure Portal](<https://portal.azure.com>).

2.  Klik op **nieuwe resource toevoegen**, en typ 'qna maker' in het zoekvak en selecteer de QnA Maker-resource

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/create-new-resource.png)

3.  Klik op **maken** na het lezen van de voorwaarden en bepalingen.

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

4. In **QnA Maker**, selecteer de juiste lagen en regio's.

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Vul de **naam** met een unieke naam voor deze QnA Maker-service. Deze naam identificeert ook de QnA Maker-eindpunt waarnaar uw knowledge bases gekoppeld worden.
    * Kies de **abonnement** in die de QnA Maker-resource wordt geïmplementeerd.
    * Selecteer de **Management prijscategorie** voor de QnA Maker management services (portal en beheer-API's). Zie [hier](https://aka.ms/qnamaker-pricing) voor meer informatie over de prijzen van de SKU's.
    * Maak een nieuwe **resourcegroep** (aanbevolen) of gebruik een bestaande resourcegroep waarin u kunt deze resource QnA Maker implementeren.
    * Kies de **zoeken prijscategorie** van de Azure Search-service. Als u de gratis laag-optie grijs weergegeven, betekent dat u hebt al een gratis Azure Search-laag in uw abonnement. In dat geval moet u eerst de laag Basic Azure Search. Zie de details van prijzen van Azure search [hier](https://azure.microsoft.com/en-us/pricing/details/search/).
    * Kies de **zoeklocatie** waar u Azure Search-gegevens om te worden geïmplementeerd. Beperkingen in waar de gegevens van de klant moet worden opgeslagen, wordt de locatie die u voor Azure Search kiest informeren.
    * Geef een naam aan uw appservice in **appnaam**.
    * Standaard wordt de appservice standaard naar de prijscategorie standard (S1). U kunt het abonnement wijzigen nadat de is gemaakt. Zie meer informatie over prijzen voor App service [hier](https://azure.microsoft.com/en-in/pricing/details/app-service/).
    * Kies de **locatie van de Website** waarop de App-Service wordt geïmplementeerd.

        > [!NOTE]
        > De locatie van de zoekopdracht kan afwijken van de locatie van de Website.

    * Kies of u om in te schakelen **Application Insights** of niet. Als **Application Insights** is ingeschakeld, QnA Maker telemetrie verzamelt op verkeer, chatlogs en fouten.
    * Kies de **App insights-locatie** waarop Application Insights-resource wordt geïmplementeerd.

5. Zodra alle velden zijn gevalideerd, kunt u klikken op **maken** implementatie te starten van deze services in uw abonnement. Het duurt een paar minuten om te voltooien.

6.  Wanneer de implementatie is voltooid, ziet u de volgende bronnen in uw abonnement gemaakt.

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Maken en een kennisdatabase publiceren](../Quickstarts/create-publish-knowledge-base.md)