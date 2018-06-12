---
title: Metrische gegevens en waarschuwingen in Azure Traffic Manager | Microsoft Docs
description: In dit artikel beschrijft metrische gegevens beschikbaar voor Traffic Manager in Azure.
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: 424782be2d814df6d598591198b5005fb494d3da
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303752"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Traffic Manager metrische gegevens en waarschuwingen

Traffic Manager biedt u DNS-gebaseerde taakverdeling waaronder meerdere Routering methoden en eindpunt controle-opties. In dit artikel beschrijft de metrische gegevens en de bijbehorende waarschuwingen die aan klanten beschikbaar zijn. 

## <a name="metrics-available-in-traffic-manager"></a>Metrische gegevens beschikbaar zijn in Traffic Manager 

Traffic Manager biedt de volgende metrische gegevens per profiel op basis van een die kan worden gebruikt door klanten om hun gebruik van Traffic manager en de status van hun eindpunten onder dat profiel te begrijpen.  

### <a name="queries-by-endpoint-returned"></a>Query's door eindpunt geretourneerd
Gebruik [met deze metriek](../monitoring-and-diagnostics/monitoring-supported-metrics.md) om het aantal query's dat is verwerkt door een Traffic Manager-profiel gedurende een bepaalde periode weer te geven. U kunt ook dezelfde informatie weergeven in een eindpunt niveau samenvattingen die geeft u inzicht in hoe vaak een eindpunt in de query-antwoorden van Traffic Manager is geretourneerd.

In het volgende voorbeeld wordt weergegeven in afbeelding 1 alle queryantwoorden op de die zijn geretourneerd door de Traffic Manager-profiel. 

  
![Metrische gegevens voor het Traffic Manager - samengevoegde weergave van alle query 's](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Afbeelding 1: Cumulatieve weergave met alle query 's*
  
Afbeelding 2 ziet u de dezelfde gegevens, is echter gesplitst door eindpunten. Als gevolg hiervan, ziet u het volume van de reacties op query's waarin een specifieke eindpunt is geretourneerd.

![Metrische gegevens Traffic Manager - weergave van query-volume per eindpunt splitsen](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Afbeelding 2: Gesplitste weergave met query volume weergegeven per eindpunt geretourneerd*

## <a name="endpoint-status-by-endpoint"></a>Status endpoint door eindpunt
Gebruik [met deze metriek](../monitoring-and-diagnostics/monitoring-supported-metrics.md) om te begrijpen van de status van de eindpunten in het profiel. Het duurt twee waarden:
 - Gebruik **1** als het eindpunt is actief.
 - Gebruik **0** als het eindpunt niet actief is.

Met deze metriek kan worden weergegeven als een cumulatieve waarde die de status van alle de metrische gegevens (afbeelding 3) aangeeft of het kan worden gesplitst (Zie afbeelding 4) om de status van specifieke eindpunten te geven. In het geval van het eerste als het aggregatieniveau van de is geselecteerd als **Gem**, de waarde van deze metriek wordt het rekenkundige gemiddelde van de status van alle eindpunten. Bijvoorbeeld, als een profiel twee eindpunten heeft en slechts één in orde is, met deze metriek heeft een waarde van **0,50** zoals in afbeelding 3. 


![Metrische gegevens voor het Traffic Manager - samengestelde weergave van endpoint-status](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Afbeelding 3: Samengestelde weergave van eindpunt status metriek – 'Gem' aggregatie is geselecteerd*


![Metrische gegevens Traffic Manager - weergave van de status van endpoint splitsen](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Afbeelding 4: Weergave van eindpunt status metrische gegevens splitsen*

U kunt deze metrische gegevens via verbruiken [Azure Monitor-service](../monitoring-and-diagnostics/monitoring-supported-metrics.md)van portal [REST-API](https://docs.microsoft.com/rest/api/monitor/), [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), en [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights), of via de sectie metrische gegevens van de portal van Traffic Manager-ervaring.

## <a name="alerts-on-traffic-manager-metrics"></a>Waarschuwingen op het Traffic Manager metrische gegevens
Naast de verwerking en metrische gegevens van Traffic Manager weergeven, kunnen Monitor Azure klanten configureren en waarschuwingen die zijn gekoppeld aan deze metrische gegevens ontvangen. U kunt voorwaarden moeten worden voldaan in deze metrische gegevens voor een waarschuwing moet worden vertoond, hoe vaak deze voorwaarden moeten worden bewaakt en hoe de waarschuwingen naar u worden verzonden. Zie voor meer informatie [Azure Monitor waarschuwingen documentatie](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure Monitor-service](../monitoring-and-diagnostics/monitoring-supported-metrics.md)
- Meer informatie over hoe [een nieuwe grafiek met behulp van Azure-Monitor maken](../monitoring-and-diagnostics/monitoring-metric-charts.md#how-do-i-create-a-new-chart)