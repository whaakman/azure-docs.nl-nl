---
title: Hoge beschikbaarheid en betrouwbaarheid - Azure Scheduler
description: Meer informatie over hoge beschikbaarheid en betrouwbaarheid in Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.topic: article
ms.date: 08/16/2016
ms.openlocfilehash: d647de379972bac317a213e2f8925c0ff8c3372c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947921"
---
# <a name="high-availability-and-reliability-for-azure-scheduler"></a>Hoge beschikbaarheid en betrouwbaarheid voor Azure Scheduler

> [!IMPORTANT]
> [Met Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, is buiten gebruik gesteld. Voor het plannen van taken, [Azure Logic Apps in plaats daarvan probeert](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Azure Scheduler biedt [hoge beschikbaarheid](https://docs.microsoft.com/azure/architecture/guide/pillars#availability) en betrouwbaarheid voor uw taken. Zie voor meer informatie, [SLA voor Scheduler](https://azure.microsoft.com/support/legal/sla/scheduler).

## <a name="high-availability"></a>Hoge beschikbaarheid

Azure Scheduler is [maximaal beschikbaar] en maakt gebruik van zowel geografisch redundante service-implementatie en geo-regionale taak replicatie.

### <a name="geo-redundant-service-deployment"></a>Geografisch redundante service-implementatie

Azure Scheduler is beschikbaar in de Azure-portal voor bijna [elke geografische regio die tegenwoordig wordt ondersteund door Azure](https://azure.microsoft.com/global-infrastructure/regions/#services). Dus als een Azure-datacenter in een gehoste regio niet beschikbaar is, kunt u nog steeds gebruiken Azure Scheduler omdat failover-functionaliteit van de service Scheduler beschikbaar is via een ander datacenter.

### <a name="geo-regional-job-replication"></a>Geo-regionale taak replicatie

Uw eigen taken in Azure Scheduler worden gerepliceerd tussen Azure-regio's. Dus als één regio een storing heeft, Azure Scheduler voert een failover en zorgt ervoor dat de taak wordt uitgevoerd vanaf een ander datacenter in de gekoppelde geografische regio.

Bijvoorbeeld, als u een taak in Zuid-centraal VS maken, repliceert Azure Scheduler automatisch deze taak in Noord-centraal VS. Als er een fout doet zich voor in Zuid-centraal VS, wordt de taak in Azure Scheduler uitgevoerd in Noord-centraal VS. 

![Geo-regionale taak replicatie](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png)

Azure Scheduler maakt ook dat uw gegevens binnen dezelfde, maar naar een grotere geografische regio, blijft het geval een softwarestoring in Azure. U hebt dus geen kopie van uw taken als u wilt dat alleen hoge beschikbaarheid. Azure Scheduler zorgt automatisch voor hoge beschikbaarheid voor uw taken.

## <a name="reliability"></a>Betrouwbaarheid

Azure Scheduler garandeert een eigen hoge beschikbaarheid maar een andere benadering kost aan projecten die door de gebruiker zijn gemaakt. Stel bijvoorbeeld dat uw taak roept een HTTP-eindpunt dat is niet beschikbaar. Azure Scheduler wil nog steeds uw taak is uitgevoerd door te geven u alternatieve manieren voor het afhandelen van fouten: 

* Instellen van beleid voor opnieuw proberen.
* Alternatieve eindpunten instellen.

<a name="retry-policies"></a>

### <a name="retry-policies"></a>Beleid voor opnieuw proberen

Azure Scheduler kunt u beleid voor opnieuw proberen instellen. Als een taak, klikt u vervolgens standaard mislukt pogingen Scheduler de taak vier keer intervallen van 30 seconden. Kunt u dit beleid voor opnieuw proberen uitgebreider toe, zoals 10 keer intervallen van 30 seconden of minder agressief, zoals twee keer op dagelijks interval.

Stel bijvoorbeeld dat u maakt een wekelijkse taak die een HTTP-eindpunt aanroept. Als de HTTP-eindpunt niet beschikbaar is voor een paar uur, wanneer de taak wordt uitgevoerd, wilt u mogelijk niet wachten nog een week voor de taak opnieuw, uit te voeren die wordt uitgevoerd omdat het standaardbeleid voor opnieuw proberen in dit geval werkt niet. Dus als u wilt wijzigen van het standaardproces voor nieuwe pogingen-beleid, zodat nieuwe pogingen is gebeurd, bijvoorbeeld elke drie uur in plaats van elke 30 seconden. 

Zie voor meer informatie over het instellen van een beleid voor opnieuw proberen, [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoints"></a>Alternatieve eindpunten

Als uw Azure Scheduler-taak aanroept die een eindpunt dat is niet bereikbaar is, zelfs nadat u het beleid voor opnieuw proberen, Scheduler gebruikgemaakt van een alternatieve eindpunt dat dergelijke fouten kan worden verwerkt. Dus als u dit eindpunt hebt ingesteld, roept Scheduler dat eindpunt, waardoor uw eigen taken maximaal beschikbaar is wanneer er fouten optreden.

In dit diagram ziet u bijvoorbeeld Scheduler volgt hoe het beleid voor opnieuw proberen bij het aanroepen van een webservice in New York. Als de nieuwe pogingen is mislukt, controleert u Scheduler een eindpunt alternate. Als het eindpunt bestaat, start Scheduler verzenden van aanvragen naar het eindpunt alternate. De hetzelfde beleid voor opnieuw proberen is van toepassing op zowel de oorspronkelijke actie en de alternatieve actie.

![Scheduler-gedrag met beleid voor opnieuw proberen en eindpunt alternate](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png)

Het actietype voor de alternatieve actie kan afwijken van de oorspronkelijke actie. Bijvoorbeeld, hoewel de oorspronkelijke actie een HTTP-eindpunt aanroept, kan de alternatieve actie fouten vastleggen met behulp van een Storage-wachtrij, een Service Bus-wachtrij of een Service Bus-onderwerpactie.

Zie voor meer informatie over het instellen van een eindpunt alternate, [errorAction](scheduler-concepts-terms.md#error-action).

## <a name="see-also"></a>Zie ook

* [Wat is Azure Scheduler?](scheduler-intro.md)
* [Concepten en terminologie entiteitenhiërarchie](scheduler-concepts-terms.md)
* [Complexe schema's en geavanceerde terugkeerpatronen bouwen](scheduler-advanced-complexity.md)
* [Limieten, quota, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)
