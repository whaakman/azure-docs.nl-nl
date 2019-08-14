---
title: Instellen van een service voor QnA Maker - QnA Maker
titleSuffix: Azure Cognitive Services
description: Voordat u een QnA Maker knowledge bases maken kunt, moet u eerst een QnA Maker-service in Azure instellen. Iedereen met autorisatie voor het maken van nieuwe resources in een abonnement kunt een QnA Maker-service instellen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: f30b55eda4a02cfb3e961c0019128e4fe686cf53
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967672"
---
# <a name="create-a-qna-maker-service"></a>Een QnA Maker-service maken

Voordat u een QnA Maker knowledge bases maken kunt, moet u eerst een QnA Maker-service in Azure instellen. Iedereen met autorisatie voor het maken van nieuwe resources in een abonnement kunt een QnA Maker-service instellen.

## <a name="create-a-new-service"></a>Een nieuwe service maken

Met deze procedure implementeert u enkele Azure-resources. Deze resources worden samen de knowledge base-inhoud beheren en bieden mogelijkheden beantwoorden van vraag al een eindpunt.

1. Meld u aan bij de Azure Portal en [Maak een QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) -resource.

1. Selecteer **maken** nadat u de voor waarden hebt gelezen.

    ![Een nieuwe QnA Maker-service maken](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. In **QnA Maker**, selecteer de juiste lagen en regio's.

    ![Een nieuwe QnA Maker-service - prijscategorie en regio's maken](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Vul de **naam** met een unieke naam voor deze QnA Maker-service. Deze naam identificeert ook de QnA Maker-eindpunt waarnaar uw knowledge bases gekoppeld worden.
    * Kies de **abonnement** in die de QnA Maker-resource wordt geïmplementeerd.
    * Selecteer de **prijs categorie** voor de QnA Maker-beheer Services (Portal-en beheer-api's). Zie [hier](https://aka.ms/qnamaker-pricing) voor meer informatie over de prijzen van de SKU's.
    * Maak een nieuwe **resourcegroep** (aanbevolen) of gebruik een bestaande resourcegroep waarin u kunt deze resource QnA Maker implementeren. QnA Maker maakt verschillende Azure-resources. Wanneer u een resource groep maakt om deze resources te bevatten, kunt u deze resources eenvoudig vinden, beheren en verwijderen op basis van de naam van de resource groep.
    * Selecteer een **locatie voor de resource groep**.
    * Kies de **zoeken prijscategorie** van de Azure Search-service. Als u de gratis laag-optie grijs weergegeven, betekent dat u hebt al een gratis Azure Search-laag in uw abonnement. In dat geval moet u eerst de laag Basic Azure Search. Zie de details van prijzen van Azure search [hier](https://azure.microsoft.com/pricing/details/search/).
    * Kies de **zoeklocatie** waar u Azure Search-gegevens om te worden geïmplementeerd. Beperkingen in waar de gegevens van de klant moet worden opgeslagen, wordt de locatie die u voor Azure Search kiest informeren.
    * Geef een naam aan uw appservice in **appnaam**.
    * Standaard wordt de appservice standaard naar de prijscategorie standard (S1). U kunt het abonnement wijzigen nadat de is gemaakt. Zie meer informatie over prijzen voor App service [hier](https://azure.microsoft.com/pricing/details/app-service/).
    * Kies de **locatie van de Website** waarop de App-Service wordt geïmplementeerd.

        > [!NOTE]
        > De locatie van de zoekopdracht kan afwijken van de locatie van de Website.

    * Kies of u om in te schakelen **Application Insights** of niet. Als **Application Insights** is ingeschakeld, QnA Maker telemetrie verzamelt op verkeer, chatlogs en fouten.
    * Kies de **App insights-locatie** waarop Application Insights-resource wordt geïmplementeerd.
    * Voor kosten besparingen kunt u enkele, maar niet alle Azure-resources die zijn gemaakt voor QnA Maker [delen](upgrade-qnamaker-service.md?#share-existing-services-with-qna-maker) . 

1. Wanneer alle velden zijn gevalideerd, kunt u **maken** selecteren om de implementatie van deze services in uw abonnement te starten. Het duurt een paar minuten om te voltooien.

1. Wanneer de implementatie is voltooid, ziet u de volgende bronnen in uw abonnement gemaakt.

    ![De resource gemaakt in een nieuwe QnA Maker-service](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="region-of-management-service"></a>Regio van beheer service

De beheer service van QnA Maker, die alleen wordt gebruikt voor de portal & voor initiële gegevens verwerking, is alleen beschikbaar in VS-West. Er worden geen klant gegevens opgeslagen in deze service vs-West.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Maken en een kennisdatabase publiceren](../Quickstarts/create-publish-knowledge-base.md)
