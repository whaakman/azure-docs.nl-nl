---
title: Metrische gegevens weergeven met Azure Monitor
description: In dit artikel laat zien hoe om te controleren metrische gegevens met de Azure portal grafieken en de Azure CLI.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: d331dc4eb0c6668426e1ab1a01a1dd1dcebe0c85
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795832"
---
# <a name="monitor-media-services-metrics"></a>Controleer metrische gegevens voor mediaservices 

[Azure Monitor](../../azure-monitor/overview.md) kunt u metrische gegevens controleren en tot diagnostische logboeken waarmee u inzicht in hoe uw toepassingen worden uitgevoerd. Voor een gedetailleerde beschrijving van deze functie en om te zien waarom u wilt met behulp van Logboeken voor het metrische en diagnostische gegevens van Azure Media Services, Zie [Media-Services controleren metrische gegevens en logboeken met diagnostische gegevens](media-services-metrics-diagnostic-logs.md).

Azure Monitor biedt verschillende manieren om te communiceren met metrische gegevens, zoals ze in het portaal grafieken, toegang hebben tot deze via de REST-API of uitvoeren van deze query's met behulp van CLI. In dit artikel laat zien hoe om te controleren metrische gegevens met de Azure portal grafieken en de Azure CLI.

## <a name="prerequisites"></a>Vereisten

- [Een Media Services-account maken](create-account-cli-how-to.md)
- Beoordeling [Media-Services controleren metrische gegevens en logboeken met diagnostische gegevens](media-services-metrics-diagnostic-logs.md)

## <a name="view-metrics-in-azure-portal"></a>Metrische gegevens weergeven in Azure portal

1. Meld u aan bij Azure Portal op https://portal.azure.com.
1. Navigeer naar uw Azure Media Services-account en selecteer **metrische gegevens**.
1. Klik op de **RESOURCE** vak en selecteert u de resource waarvoor u wilt bewaken, metrische gegevens. 

    De **selecteert u een resource** venster wordt weergegeven aan de rechterkant de lijst van beschikbare resources voor u. In dit geval ziet u 

    * &lt;Media Services-accountnaam&gt;
    * &lt;Media Services-accountnaam&gt;/&lt;streaming-eindpuntnaam&gt;
    * &lt;De naam van opslagaccount&gt;

    Selecteer de resource en druk op **toepassen**. Zie voor meer informatie over ondersteunde resources en metrische gegevens [mediaservices Monitor metrics](media-services-metrics-diagnostic-logs.md).
 
    ![Metrische gegevens](media/media-services-metrics/metrics02.png)
    
    > [!NOTE]
    > Als u wilt overschakelen tussen resources waarvoor u wilt bewaken, metrische gegevens, klikt u op de **RESOURCE** vak opnieuw en Herhaal deze stap.
1. (Optioneel) Geef de grafiek, een naam (de naam van de door het potlood aan de bovenkant op bewerken).
1. Metrische gegevens die u wilt weergeven, toevoegen.

    ![Metrische gegevens](media/media-services-metrics/metrics03.png)
1. U kunt uw grafiek vastmaken aan uw dashboard.

## <a name="view-metrics-with-azure-cli"></a>Metrische gegevens weergeven met Azure CLI

Als u 'Uitgaande' metrische gegevens met CLI, moet u het volgende uitvoeren `az monitor metrics` CLI-opdracht:

```cli
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Als u andere metrische gegevens, vervangen door "Uitgaande" de naam van de meetwaarde waarin u geïnteresseerd bent.

## <a name="see-also"></a>Zie ook

* [Azure Monitor Metrics](../../azure-monitor/platform/data-platform.md)
* [Maken, weergeven en beheren van metrische waarschuwingen met behulp van Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

## <a name="next-steps"></a>Volgende stappen

[Diagnostische logboeken](media-services-diagnostic-logs-howto.md)
