---
title: Maken van een plan voor continuty voor uw service QnA Maker - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Het maken van een plan voor bedrijfscontinuïteit voor uw service QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: ca6e54b8a8ca8b38e8ef6b1a148f8b2c54bd43da
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345275"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Maken van een plan voor bedrijfscontinuïteit voor uw service QnA Maker

Het primaire doel van het plan voor bedrijfscontinuïteit is het maken van een robuuste knowledge base-eindpunt dat erop toezien dat geen uitvaltijd voor de Bot of het gebruiken van deze toepassing.

![QnA Maker bcp plannen](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Het idee op hoog niveau is zoals wordt weergegeven boven als volgt:

1. Instellen van twee parallel [QnA Maker services](../How-To/set-up-qnamaker-service-azure.md) in [Azure regio's gekoppeld](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).

2. De primaire en secundaire Azure zoekindexen synchroon te houden. Gebruik het voorbeeld github [hier](https://github.com/pchoudhari/QnAMakerBackupRestore) om te zien hoe Azure indexen back-up-herstel.

3. Back-up van de Application Insights met [continue export](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-telemetry).

4. Zodra de primaire en secundaire stapels zijn ingesteld, gebruikt u [traffic manager](https://docs.microsoft.com/en-us/azure/traffic-manager/) voor het configureren van de twee eindpunten en het instellen van een methode voor het doorsturen.

5. U moet een SSL-certificaat voor uw traffic manager-eindpunt te maken. [De SSL-certificaat binden](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-custom-ssl) in uw App-services.

6. Gebruik tot slot de traffic manager-eindpunt in uw Bot of App.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Kies de capaciteit voor uw implementatie QnA Maker](../Tutorials/choosing-capacity-qnamaker-deployment.md)
