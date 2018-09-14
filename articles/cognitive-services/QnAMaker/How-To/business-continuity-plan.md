---
title: Plan voor bedrijfscontinuïteit - QnA Maker
titleSuffix: Azure Cognitive Services
description: Het primaire doel van het plan voor bedrijfscontinuïteit is het maken van een robuuste knowledge base-eindpunt, wat dat zonder uitvaltijd voor de Bot of de toepassing die wordt verbruikt.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: c65ef07f3644004c6e453f6a19035115df9d5764
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573974"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Een plan voor bedrijfscontinuïteit voor uw QnA Maker-service maken

Het primaire doel van het plan voor bedrijfscontinuïteit is het maken van een robuuste knowledge base-eindpunt, wat dat zonder uitvaltijd voor de Bot of de toepassing die wordt verbruikt.

![QnA Maker bcp-plan](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Het idee op hoog niveau is zoals voorgesteld hierboven als volgt:

1. Instellen van twee parallelle [QnA Maker services](../How-To/set-up-qnamaker-service-azure.md) in [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. De primaire en secundaire Azure search-indexen gesynchroniseerd te houden. Gebruik het voorbeeld github [hier](https://github.com/pchoudhari/QnAMakerBackupRestore) om te zien hoe u back-up-restore Azure indexen.

3. Back-up van de Application Insights met [continue export](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Zodra de primaire en secundaire-stacks zijn ingesteld, gebruikt u [traffic manager](https://docs.microsoft.com/azure/traffic-manager/) voor het configureren van de twee eindpunten en routeringsmethode instellen.

5. U moet een SSL-certificaat voor uw traffic manager-eindpunt maken. [De SSL-certificaat binden](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) in uw App-services.

6. Gebruik tot slot de traffic manager-eindpunt in uw Bot of App.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Capaciteit voor uw implementatie QnA Maker kiezen](../Tutorials/choosing-capacity-qnamaker-deployment.md)
