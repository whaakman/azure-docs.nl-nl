---
title: Instellen van een service voor QnA Maker - QnA Maker
titleSuffix: Azure Cognitive Services
description: Voordat u een QnA Maker knowledge bases maken kunt, moet u eerst een QnA Maker-service in Azure instellen. Iedereen met autorisatie voor het maken van nieuwe resources in een abonnement kunt een QnA Maker-service instellen.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/02/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 90e7abf229e0ee1ca396150fee0c1f0970d257aa
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58876398"
---
# <a name="create-a-qna-maker-service"></a>Een QnA Maker-service maken

Voordat u een QnA Maker knowledge bases maken kunt, moet u eerst een QnA Maker-service in Azure instellen. Iedereen met autorisatie voor het maken van nieuwe resources in een abonnement kunt een QnA Maker-service instellen.

## <a name="create-a-new-service"></a>Een nieuwe service maken

Deze procedure implementeert u een paar Azure-resources. Deze resources worden samen de knowledge base-inhoud beheren en bieden mogelijkheden beantwoorden van vraag al een eindpunt.

1. Meld u aan bij [Azure Portal](<https://portal.azure.com>).

1. Selecteer **nieuwe resource toevoegen**, en typ 'qna maker' in het zoekvak en selecteer de QnA Maker-resource

    ![Maak een nieuwe QnA Maker-service: nieuwe resource toevoegen](../media/qnamaker-how-to-setup-service/create-new-resource.png)

1. Selecteer **maken** na het lezen van de voorwaarden en bepalingen.

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. In **QnA Maker**, selecteer de juiste lagen en regio's.

    ![Een nieuwe QnA Maker-service - prijscategorie en regio's maken](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Vul de **naam** met een unieke naam voor deze QnA Maker-service. Deze naam identificeert ook de QnA Maker-eindpunt waarnaar uw knowledge bases gekoppeld worden.
    * Kies de **abonnement** in die de QnA Maker-resource wordt geïmplementeerd.
    * Selecteer de **Management prijscategorie** voor de QnA Maker management services (portal en beheer-API's). Zie [hier](https://aka.ms/qnamaker-pricing) voor meer informatie over de prijzen van de SKU's.
    * Maak een nieuwe **resourcegroep** (aanbevolen) of gebruik een bestaande resourcegroep waarin u kunt deze resource QnA Maker implementeren. QnA Maker maakt verschillende Azure-resources; Wanneer u maakt u gemakkelijk kunt vinden, een resourcegroep voor het opslaan van deze resources, beheren en verwijderen van deze resources door de naam van de resourcegroep.
    * Kies de **zoeken prijscategorie** van de Azure Search-service. Als u de gratis laag-optie grijs weergegeven, betekent dat u hebt al een gratis Azure Search-laag in uw abonnement. In dat geval moet u eerst de laag Basic Azure Search. Zie de details van prijzen van Azure search [hier](https://azure.microsoft.com/pricing/details/search/).
    * Kies de **zoeklocatie** waar u Azure Search-gegevens om te worden geïmplementeerd. Beperkingen in waar de gegevens van de klant moet worden opgeslagen, wordt de locatie die u voor Azure Search kiest informeren.
    * Geef een naam aan uw appservice in **appnaam**.
    * Standaard wordt de appservice standaard naar de prijscategorie standard (S1). U kunt het abonnement wijzigen nadat de is gemaakt. Zie meer informatie over prijzen voor App service [hier](https://azure.microsoft.com/pricing/details/app-service/).
    * Kies de **locatie van de Website** waarop de App-Service wordt geïmplementeerd.

        > [!NOTE]
        > De locatie van de zoekopdracht kan afwijken van de locatie van de Website.

    * Kies of u om in te schakelen **Application Insights** of niet. Als **Application Insights** is ingeschakeld, QnA Maker telemetrie verzamelt op verkeer, chatlogs en fouten.
    * Kies de **App insights-locatie** waarop Application Insights-resource wordt geïmplementeerd.
    * Voor kosten besparen metingen, kunt u [delen](upgrade-qnamaker-service.md?#share-existing-services-with-qna-maker) sommige, maar niet alle Azure-resources die zijn gemaakt voor QnA Maker. 

1. Zodra alle velden zijn gevalideerd, kunt u selecteren **maken** implementatie te starten van deze services in uw abonnement. Het duurt een paar minuten om te voltooien.

1. Wanneer de implementatie is voltooid, ziet u de volgende bronnen in uw abonnement gemaakt.

    ![De resource gemaakt in een nieuwe QnA Maker-service](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="region-of-management-service"></a>De regio van de management-service

De management-service van de QnA Maker, alleen gebruikt voor de portal en voor de verwerking van de eerste data is alleen beschikbaar in VS-West. Er zijn geen gegevens van de klant wordt opgeslagen in deze service VS-West.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een knowledge base maken](../Quickstarts/create-publish-knowledge-base.md)
