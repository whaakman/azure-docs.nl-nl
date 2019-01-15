---
title: Plan voor bedrijfscontinuïteit - QnA Maker
titleSuffix: Azure Cognitive Services
description: Het primaire doel van het plan voor bedrijfscontinuïteit is het maken van een robuuste knowledge base-eindpunt, wat dat zonder uitvaltijd voor de Bot of de toepassing die wordt verbruikt.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/14/2018
ms.author: tulasim
ms.openlocfilehash: 28d2e9ce16106a1995702bd908825d9aa75f06bd
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263863"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Een plan voor bedrijfscontinuïteit voor uw QnA Maker-service maken

Het primaire doel van het plan voor bedrijfscontinuïteit is het maken van een robuuste knowledge base-eindpunt, wat dat zonder uitvaltijd voor de Bot of de toepassing die wordt verbruikt.

![QnA Maker bcp-plan](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Het idee op hoog niveau is zoals voorgesteld hierboven als volgt:

1. Instellen van twee parallelle [QnA Maker services](../How-To/set-up-qnamaker-service-azure.md) in [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. De primaire en secundaire Azure search-indexen gesynchroniseerd te houden. Gebruik het voorbeeld GitHub [hier](https://github.com/pchoudhari/QnAMakerBackupRestore) om te zien hoe u back-up-restore Azure indexen.

3. Back-up van de Application Insights met [continue export](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Zodra de primaire en secundaire-stacks zijn ingesteld, gebruikt u [traffic manager](https://docs.microsoft.com/azure/traffic-manager/) voor het configureren van de twee eindpunten en routeringsmethode instellen.

5. U moet een SSL-certificaat voor uw traffic manager-eindpunt maken. [De SSL-certificaat binden](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) in uw App-services.

6. Gebruik tot slot de traffic manager-eindpunt in uw Bot of App.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Capaciteit voor uw implementatie QnA Maker kiezen](../Tutorials/choosing-capacity-qnamaker-deployment.md)
