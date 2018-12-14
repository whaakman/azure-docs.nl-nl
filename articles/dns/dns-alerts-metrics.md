---
title: Azure DNS-statistieken en -waarschuwingen | Microsoft Docs
description: Meer informatie over Azure DNS-metrische gegevens en waarschuwingen.
services: dns
documentationcenter: na
author: vhorne
manager: jennoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2018
ms.author: victorh
ms.openlocfilehash: 707a23cbd04db9a0420c7776a24eb568a3483b0b
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53385853"
---
# <a name="azure-dns-metrics-and-alerts"></a>Azure DNS-metrische gegevens en waarschuwingen
Azure DNS is een hostingservice voor DNS-domeinen die naamomzetting met de Microsoft Azure-infrastructuur biedt. Dit artikel wordt beschreven voor metrische gegevens en waarschuwingen voor de Azure DNS-service.

## <a name="azure-dns-metrics"></a>Metrische gegevens van Azure DNS

Azure DNS biedt metrische gegevens voor klanten in te schakelen voor het bewaken van specifieke aspecten van hun DNS-labels die worden gehost in de service. Bovendien met Azure DNS-statistieken, kunt u configureren en waarschuwingen op basis van de voorwaarden die van belang zijn ontvangen. De metrische gegevens worden geleverd via de [Azure Monitor service](../azure-monitor/index.yml). Azure DNS biedt de volgende metrische gegevens via Azure Monitor voor uw DNS-zones:

-   QueryVolume
-   RecordSetCount
-   RecordSetCapacityUtilization

U ziet ook de [definitie van deze metrische gegevens](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkdnszones) op de pagina van de documentatie van Azure Monitor.
>[!NOTE]
> Deze metrische gegevens zijn op dit moment alleen beschikbaar voor openbare DNS-zones die worden gehost in Azure DNS. Hebt u Private Zones die worden gehost in Azure DNS, bieden deze metrische gegevens niet voor deze zones. Bovendien is de metrische gegevens en waarschuwingen functie alleen ondersteund in Azure openbare cloud. Ondersteuning voor onafhankelijke clouds volgt op een later tijdstip. 

De gedetailleerde niveau van de dimensie voor deze metrische gegevens is DNS-Zone.

### <a name="query-volume"></a>Queryvolume

De *queryvolume* metrische gegevens in Azure DNS bevat het volume van DNS-query's (queryverkeer) die wordt ontvangen door Azure DNS voor de DNS-zone. De maateenheid Count is en de aggregatie is het totaal van alle query's ontvangen gedurende een bepaalde periode. Wilt deze metrische gegevens weergeven, selecteert u metrische gegevens (preview) explorer ervaring op het tabblad Monitor in Azure portal. Uw DNS-zone in de vervolgkeuzelijst Resource selecteren, selecteert u de Query Volume metrische gegevens en som selecteren als de aggregatie. In de schermafbeelding hieronder ziet u een voorbeeld.  Voor meer informatie over de Metrics Explorer-ervaring en grafieken, Zie [Azure Monitor Metrics Explorer](../azure-monitor/platform/metrics-charts.md).

![Queryvolume](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

*Afbeelding: Metrische gegevens van Azure DNS-Query Volume*

### <a name="record-set-count"></a>Set-aantal records
De *Record ingesteld aantal* metriek wordt het aantal Recordsets in Azure DNS voor de DNS-zone. Alle Recordsets gedefinieerd in uw zone worden geteld. De maateenheid Count is en de aggregatie is het Maximum van alle Recordsets. Als u wilt deze metrische gegevens weergeven, selecteert u **metrische gegevens (preview)** explorer-ervaring van de **Monitor** tabblad in de Azure-portal. Selecteer uw DNS-zone van de **Resource** vervolgkeuzelijst, selecteer de **ingesteld aantal records** metrische gegevens en selecteer vervolgens **Max** als de **aggregatie** . Voor meer informatie over de Metrics Explorer-ervaring en grafieken, Zie [Azure Monitor Metrics Explorer](../azure-monitor/platform/metrics-charts.md). 

![Set-aantal records](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

*Afbeelding: Azure DNS-Record ingesteld aantal metrische gegevens*


### <a name="record-set-capacity-utilization"></a>Gebruik van capaciteit van de recordset
De *Record ingesteld capaciteitsverbruik* metrische gegevens in Azure DNS bevat het percentage van het gebruik van de capaciteit van de Recordset voor een DNS-Zone. Elke DNS-zone in Azure DNS is onderworpen aan de grens van een Recordset dat definieert het maximum aantal Recordsets die zijn toegestaan voor de Zone (Zie [DNS limieten](dns-zones-records.md#limits)). Daarom kan ziet deze metrische gegevens u hoe dicht zijn de Recordset limiet hebt bereikt. Bijvoorbeeld, als u geconfigureerd voor uw DNS-zone-500 Recordsets hebt en heeft de standaardlimiet voor de Recordset van 5000 van de zone, toont de metriek RecordSetCapacityUtilization de waarde van 10% (die wordt verkregen door tussen 500 door 5000). De maateenheid is **Percentage** en de **aggregatie** type **maximale**. Wilt deze metrische gegevens weergeven, selecteert u metrische gegevens (preview) explorer ervaring op het tabblad Monitor in Azure portal. Selecteer uw DNS-zone in de vervolgkeuzelijst Resource, het verbruik van de Record ingesteld opslagcapaciteit metrische waarde selecteren en Max selecteren als de aggregatie. In de schermafbeelding hieronder ziet u een voorbeeld. Voor meer informatie over de Metrics Explorer-ervaring en grafieken, Zie [Azure Monitor Metrics Explorer](../azure-monitor/platform/metrics-charts.md). 

![Set-aantal records](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

*Afbeelding: Metrische gegevens van Azure capaciteitsverbruik voor instellen van DNS-Record*

## <a name="alerts-in-azure-dns"></a>Waarschuwingen in Azure DNS
Azure Monitor biedt de mogelijkheid voor de waarschuwing op basis van beschikbare metrische waarden. De DNS-metrische gegevens zijn beschikbaar in de nieuwe ervaring voor configuratie van de waarschuwing. Zoals beschreven in de [documentatie van Azure Monitor-waarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md), DNS-Zone selecteren als de resource, kies het signaaltype metrische en configureer de waarschuwingslogica en andere parameters, zoals **periode**en **frequentie**. U kunt verder te definiëren een [actiegroep](../azure-monitor/platform/action-groups.md) voor wanneer de waarschuwing voorwaarde wordt voldaan, waarbij de waarschuwing wordt geleverd via de gekozen acties. Zie voor meer informatie over het configureren van waarschuwingen voor metrische gegevens van Azure Monitor [maken, weergeven en beheren van waarschuwingen via Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). 

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure DNS](dns-overview.md).
