---
title: Azure DNS-metrische gegevens en waarschuwingen | Microsoft Docs
description: Meer informatie over Azure DNS metrische gegevens en waarschuwingen.
services: dns
documentationcenter: na
author: KumudD
manager: jennoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2018
ms.author: kumud
ms.openlocfilehash: 54c4df446ee5c1bf8d29dd6c33b304f39ce8f1b8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="azure-dns-metrics-and-alerts"></a>Azure DNS-metrische gegevens en waarschuwingen
Azure DNS is een hosting service voor DNS-domeinen die naamomzetting met de Microsoft Azure-infrastructuur biedt. Dit artikel wordt beschreven metrische gegevens en waarschuwingen voor de Azure DNS-service.

## <a name="azure-dns-metrics"></a>Azure DNS-metrische gegevens

Azure DNS voorziet in maatstaven voor klanten om ze te controleren van bepaalde aspecten van hun DNS-updates in de service. Bovendien met metrische gegevens voor Azure DNS, kunt u configureren en waarschuwingen op basis van de voorwaarden van belang zijn ontvangen. De metrische gegevens worden geleverd de [Azure-controleservice](../monitoring-and-diagnostics/index.yml). Azure DNS biedt de volgende metrische gegevens via de Azure-Monitor voor de DNS-zones:

-   QueryVolume
-   RecordSetCount
-   RecordSetCapacityUtilization

U ziet ook de [definitie van deze metrische gegevens](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkdnszones) op de pagina met documentatie van Azure-Monitor.
>[!NOTE]
> Deze metrische gegevens zijn momenteel alleen beschikbaar voor openbare DNS-zones die worden gehost in Azure DNS. Als u particuliere Zones die worden gehost in Azure DNS hebt, wordt deze metrische gegevens niet leveren van gegevens voor deze zones. Bovendien wordt de metrische gegevens en het waarschuwen onderdeel alleen ondersteund in Azure openbare cloud. Ondersteuning voor soevereine clouds volgen op een later tijdstip. 

De gedetailleerd niveau van de dimensie voor deze metrische gegevens is DNS-Zone.

### <a name="query-volume"></a>Query-volume

De *Query Volume* metrische gegevens in Azure DNS bevat het volume van DNS-query's (queryverkeer) dat is ontvangen door Azure DNS voor de DNS-zone. De maateenheid Count is en de aggregatie is het totaal van alle query's die zijn ontvangen via een bepaalde periode. Als u wilt deze metrische gegevens weergeven, selecteert u ervaring in Metrics (preview) explorer op het tabblad Monitor in de Azure portal. Selecteer uw DNS-zone in de vervolgkeuzelijst Resource, selecteer de metriek Query Volume en som selecteren als de aggregatie. Onderstaande schermafbeelding toont een voorbeeld.  Voor meer informatie over de Metrics Explorer optreden en grafieken, Zie [Azure Monitor Metrics Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md).

![Query-volume](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

*Afbeelding: Azure DNS-Query Volume metrische gegevens*

### <a name="record-set-count"></a>Set-aantal records
De *ingesteld aantal records* metriek toont het aantal Recordsets in Azure DNS voor de DNS-zone. Alle Recordsets gedefinieerd in uw zone worden meegeteld. De maateenheid Count is en de aggregatie is het Maximum van alle Recordsets. Als u wilt weergeven met deze metriek, selecteer **metrische gegevens (preview)** explorer-ervaring van de **Monitor** tabblad in de Azure-portal. Selecteer uw DNS-zone van de **Resource** vervolgkeuzelijst, selecteer de **ingesteld aantal records** metrische en selecteert u vervolgens **Max** als de **aggregatie** . Voor meer informatie over de Metrics Explorer optreden en grafieken, Zie [Azure Monitor Metrics Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md). 

![Set-aantal records](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

*Afbeelding: Azure DNS-Record aantal instellen metrische gegevens*


### <a name="record-set-capacity-utilization"></a>Recordset capaciteitsverbruik
De *Record ingesteld gebruik van capaciteit* metrische gegevens in Azure DNS bevat het percentage van het gebruik van uw Recordset capaciteit voor een DNS-Zone. Elke DNS-zone in Azure DNS is onderworpen aan een Recordset limiet die definieert het maximum aantal Recordsets die zijn toegestaan voor de Zone (Zie [DNS-limieten](dns-zones-records.md#limits)). Daarom ziet met deze metriek u hoe sluiten zijn roept de limiet voor de Recordset. Als er 500 Recordsets zijn geconfigureerd voor de DNS-zone en de zone de standaardlimiet voor de Recordset van 5000 heeft, wordt de metriek RecordSetCapacityUtilization bijvoorbeeld de waarde van 10% (die wordt verkregen door tussen 500 door 5000) weergegeven. Is de maateenheid **Percentage** en de **aggregatie** type **maximale**. Als u wilt deze metrische gegevens weergeven, selecteert u ervaring in Metrics (preview) explorer op het tabblad Monitor in de Azure portal. Selecteer uw DNS-zone in de vervolgkeuzelijst Resource, selecteer de metriek Record ingesteld gebruik van capaciteit en Max selecteren als de aggregatie. Onderstaande schermafbeelding toont een voorbeeld. Voor meer informatie over de Metrics Explorer optreden en grafieken, Zie [Azure Monitor Metrics Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md). 

![Set-aantal records](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

*Afbeelding: Azure DNS-Record ingesteld gebruik van capaciteit metrische gegevens*

## <a name="alerts-in-azure-dns"></a>Waarschuwingen in Azure DNS
Monitor voor Azure biedt de mogelijkheid op waarschuwing op basis van beschikbare metrische waarden. De DNS-metrische gegevens zijn beschikbaar in de nieuwe ervaring voor configuratie van de waarschuwing. Zoals beschreven in de [Azure Monitor waarschuwingen documentatie](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md), DNS-Zone selecteren als de bron, kies het signaaltype metrische en configureren van de waarschuwing logica en andere parameters zoals **periode**en **frequentie**. Verder kunt u een [actie groep](../monitoring-and-diagnostics/monitoring-action-groups.md) voor wanneer de waarschuwing voorwaarde wordt voldaan, waarbij de waarschuwing worden geleverd via de gekozen acties. Zie voor meer informatie over het configureren van waarschuwingen voor Azure Monitor metrische gegevens [maken, weergeven en beheren van waarschuwingen via Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). 

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure DNS](dns-overview.md).
