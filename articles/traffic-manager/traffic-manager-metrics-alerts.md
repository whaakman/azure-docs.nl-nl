---
title: Metrische gegevens en waarschuwingen in Azure Traffic Manager
description: Dit artikel beschrijft metrische gegevens beschikbaar zijn voor Traffic Manager in Azure.
services: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: 0bce0d407246ceab05c3951dc976884dd6f15b08
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200187"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Traffic Manager-statistieken en -waarschuwingen

Traffic Manager kunt u DNS op basis van de taakverdeling met meerdere methoden voor het doorsturen en controle-opties-eindpunt. In dit artikel beschrijft de metrische gegevens en de bijbehorende waarschuwingen die beschikbaar voor klanten zijn. 

## <a name="metrics-available-in-traffic-manager"></a>Metrische gegevens beschikbaar zijn in Traffic Manager 

Traffic Manager biedt de volgende metrische gegevens op basis van per profiel die klanten gebruiken kunnen om te begrijpen van hun gebruik van Traffic manager en de status van de bijbehorende eindpunten onder dit profiel.  

### <a name="queries-by-endpoint-returned"></a>Query's op eindpunt geretourneerd
Gebruik [met deze metriek](../azure-monitor/platform/metrics-supported.md) om het aantal query's die een Traffic Manager-profiel worden verwerkt in een opgegeven periode weer te geven. U kunt ook de dezelfde informatie weergeven op een eindpunt niveau granulariteit die geeft u inzicht in hoe vaak een eindpunt is geretourneerd in de reacties op query's vanuit Traffic Manager.

In het volgende voorbeeld wordt weergegeven in afbeelding 1 alle reacties van query's waarmee de Traffic Manager-profiel wordt geretourneerd. 

  
![Samengevoegde weergave van alle query 's](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Afbeelding 1: Cumulatieve weergeven met alle query 's*
  
Afbeelding 2 ziet u de dezelfde gegevens, maar deze wordt gesplitst door eindpunten. Als gevolg hiervan, ziet u het volume van de reacties op query's waarin een bepaald eindpunt is geretourneerd.

![Metrische gegevens de Traffic Manager - weergave van queryvolume per eindpunt splitsen](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Afbeelding 2: Weergave splitsen met queryvolume weergegeven per eindpunt geretourneerd*

## <a name="endpoint-status-by-endpoint"></a>Eindpuntstatus door eindpunt
Gebruik [met deze metriek](../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) om te begrijpen van de status van de eindpunten in het profiel. Het duurt twee waarden:
 - Gebruik **1** als het eindpunt actief is.
 - Gebruik **0** als het eindpunt niet actief is.

Deze metrische gegevens kan worden weergegeven als een cumulatieve waarde vertegenwoordigt de status van alle metrische (afbeelding 3), of deze kan worden gesplitst (Zie afbeelding 4) om de status van specifieke eindpunten weer te geven. Als het voormalige, als het aggregatieniveau van de is geselecteerd als **Avg**, de waarde van deze metriek wordt het rekenkundige gemiddelde van de status van alle eindpunten. Bijvoorbeeld, als een profiel twee eindpunten heeft en slechts één in orde is, klikt u vervolgens deze metrische gegevens een waarde heeft van **0,50** zoals wordt weergegeven in afbeelding 3. 


![Metrische gegevens van Traffic Manager - samengestelde weergave van de status van endpoint](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Afbeelding 3: Samengestelde weergave van metrische gegevens eindpunt status: "Avg" aggregatie geselecteerd*


![Metrische gegevens de Traffic Manager - weergave van de status van endpoint splitsen](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Afbeelding 4: Weergave van de status van metrische gegevens over endpoint splitsen*

U kunt deze metrische gegevens via verbruiken [Azure Monitor service](../azure-monitor/platform/metrics-supported.md)van portal [REST-API](https://docs.microsoft.com/rest/api/monitor/), [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), en [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights), of via het gedeelte van de metrische gegevens van Traffic Manager van portal-ervaring.

## <a name="alerts-on-traffic-manager-metrics"></a>Waarschuwingen over metrische gegevens voor Traffic Manager
Naast de verwerking en weergeven van metrische gegevens van Traffic Manager, kunnen Azure Monitor klanten configureren en waarschuwingen die zijn gekoppeld aan deze metrische gegevens ontvangen. U kunt voorwaarden moeten worden voldaan in deze metrische gegevens voor een waarschuwing plaatsvindt, hoe vaak deze voorwaarden hoeven te worden bewaakt en hoe u de waarschuwingen moeten worden verzonden. Zie voor meer informatie, [documentatie van Azure Monitor-waarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure Monitor-service](../azure-monitor/platform/metrics-supported.md)
- Leer hoe u [maken van een grafiek met behulp van Azure Monitor](../azure-monitor/platform/metrics-charts.md#create-a-new-chart)
