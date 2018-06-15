---
title: Metrische gegevens, meldingen en logboeken met diagnostische gegevens voor Azure Batch | Microsoft Docs
description: Registreren en analyseren van diagnostische gebeurtenissen voor Azure Batch-account resources zoals pools en taken.
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 04/05/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: e64d272695c4e47c972df040d1c1c2a63bf3dddd
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31788191"
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Batch metrische gegevens, waarschuwingen en logboeken voor de bewaking en diagnostische evaluatie

In dit artikel wordt uitgelegd hoe u voor het bewaken van een Batch-account gebruikt functies van [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Monitor voor Azure verzamelt [metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en [diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) voor resources in uw Batch-account. Verzamelen en gebruiken deze gegevens op verschillende manieren om uw Batch-account bewaken en onderzoeken van problemen. U kunt ook configureren [metrische waarschuwingen](../monitoring-and-diagnostics/monitoring-overview-alerts.md#alerts-on-azure-monitor-data) zodat u meldingen ontvangen wanneer een metriek een opgegeven waarde heeft bereikt. 

## <a name="batch-metrics"></a>Batch metrische gegevens

Metrische gegevens zijn Azure telemetriegegevens (ook wel prestatiemeteritems) die door uw Azure-resources die worden verbruikt door de Monitor van de Azure-service. Voorbeeld van de metrische gegevens in een Batch-account behoren: groep maken gebeurtenissen, aantal van de knooppunten prioriteit Laag en taak voltooid. 

Zie de [lijst met ondersteunde Batch metrische gegevens](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftbatchbatchaccounts).

Metrische gegevens zijn:

* Standaard ingeschakeld in elke Batch-account zonder aanvullende configuratie
* Elke 1 minuut gegenereerd
* Niet persistent automatisch, maar hebben een rolling geschiedenis van 30 dagen. U kunt metrische gegevens van activiteiten behouden als onderdeel van [Diagnostische logboekregistratie](#work-with-diagnostic-logs).

### <a name="view-metrics"></a>Metrische gegevens weergeven

Metrische gegevens voor uw Batch-account weergeven in de Azure-portal. De **overzicht** pagina voor het account standaard ziet u het knooppunt, core en taak metrische gegevens. 

Alle Batch-account metrische gegevens weergeven: 

1. Klik in de portal op **alle services** > **Batch-accounts**, en klik vervolgens op de naam van uw Batch-account.
2. Onder **bewaking**, klikt u op **metrische gegevens**.
3. Selecteer een of meer van de metrische gegevens. Als u wilt, schakelt u metrische gegevens voor aanvullende resources met behulp van de **abonnementen**, **resourcegroep**, **brontype**, en **Resource** vervolgkeuzelijsten.

    ![Batch metrische gegevens](media/batch-diagnostics/metrics-portal.png)

U haalt metrische gegevens via een programma met de Monitor Azure-API's. Zie bijvoorbeeld [metrische gegevens ophalen van Azure-Monitor met .NET](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/).

## <a name="batch-metric-alerts"></a>Batch metrische waarschuwingen

Configureer desgewenst bijna realtime *metrische waarschuwingen* die wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u toewijst. De waarschuwing genereert een [melding](../monitoring-and-diagnostics/insights-alerts-portal.md) u kiezen wanneer de waarschuwing is 'geactiveerd' (als de drempelwaarde is overschreden en de waarschuwing voorwaarde wordt voldaan) evenals bij het probleem wordt 'opgelost' (wanneer de drempel opnieuw wordt gepasseerd en de voorwaarde is geen meer voldaan aan). 

U wilt bijvoorbeeld een waarschuwing voor metrische configureren wanneer uw met lage prioriteit core-telling op een bepaald niveau valt zodat u de samenstelling van uw pools kunt aanpassen.

Een metriek waarschuwing configureren in de portal:

1. Klik op **Alle services** > **Batch-accounts** en klik vervolgens op de naam van uw Batch-account.
2. Onder **bewaking**, klikt u op **waarschuwing regels** > **metrische waarschuwing toevoegen**.
3. Selecteer een waarde, een meldingsvoorwaarde (zoals een metriek wordt als een bepaalde waarde gedurende een periode overschrijdt) en een of meer meldingen.

U kunt ook een near realtime waarschuwingen met de [REST-API](). Zie voor meer informatie [gebruik van de nieuwere metrische waarschuwingen voor Azure-services in Azure-portal](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md)
## <a name="batch-diagnostics"></a>Batch diagnostische gegevens

Logboeken met diagnostische gegevens bevatten informatie die door Azure-resources met een beschrijving van de werking van elke resource. Voor Batch, kunt u de volgende logboeken verzamelen:

* **Service-logboeken** gebeurtenissen die door de Azure Batch-service tijdens de levensduur van een afzonderlijke Batch-resource, zoals een groep of de taak. 

* **Metrische gegevens** logboeken op het niveau van het account. 

Instellingen voor het verzamelen van diagnostische logboeken zijn standaard niet ingeschakeld. Diagnostische instellingen voor elke Batch-account dat u wilt bewaken expliciet inschakelen.

### <a name="log-destinations"></a>Logboek bestemmingen

Een gebruikelijk scenario is een account Azure opslag als bestemming voor de logboekbestanden selecteren. Voor het opslaan van Logboeken in Azure, opslag, de account te maken voordat u de collectie van logboeken inschakelt. Als u een opslag-account aan uw account Batch gekoppeld, kunt u die account als de bestemming van het logboek. 

Andere optionele bestemmingen voor diagnostische logboeken:

* Stream-Batch diagnostische gebeurtenissen naar een [Azure gebeurtenis Hub](../event-hubs/event-hubs-what-is-event-hubs.md). Event Hubs kunnen miljoenen gebeurtenissen per seconde die u kunt vervolgens transformeren en opslaan met behulp van een realtime-analyseprovider opnemen. 

* Diagnostische logboeken te verzenden [Azure logboek Analytics](../log-analytics/log-analytics-overview.md), kunt u deze in de portal Operations Management Suite (OMS analyseren), of ze voor analyse in Power BI of Excel exporteren.

> [!NOTE]
> U mogelijk extra kosten wordt opgeslagen of diagnostische gegevens met Azure services verwerkt. 
>

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Verzameling van Batch diagnostische logboeken inschakelen

1. Klik in de portal op **alle services** > **Batch-accounts**, en klik vervolgens op de naam van uw Batch-account.
2. Onder **Monitoring**, klikt u op **diagnostische logboeken** > **diagnostics inschakelen**.
3. In **diagnostische instellingen**, typ een naam voor de instelling en kies een bestemming logboek (bestaande opslag account, gebeurtenis Hub of Analytics logboek). Selecteer een of beide **ServiceLog** en **AllMetrics**.

    Wanneer u een opslag-account selecteert, moet u eventueel een bewaarbeleid instellen. Als u een aantal dagen voor handhaving niet opgeeft, wordt tijdens de duur van de opslag account gegevens behouden.

4. Klik op **Opslaan**.

    ![Batch diagnostische gegevens](media/batch-diagnostics/diagnostics-portal.png)

Opnemen van andere opties voor het verzamelen: Azure Monitor in de portal gebruiken om diagnostische instellingen configureren, gebruikt u een [sjabloon Resource Manager](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md), of gebruikt u PowerShell Azure de CLI Azure. Zie [verzamelen en logboekgegevens van uw resources Azure verbruiken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs).


### <a name="access-diagnostics-logs-in-storage"></a>Diagnostische gegevens van Access zich in opslag

Als u Batch diagnostische logboeken op een rekening van opslag archiveert, wordt een opslag container in de opslag gemaakt zodra een bijbehorende gebeurtenis zich voordoet. BLOB's worden gemaakt volgens het patroon van de volgende naamgeving:

```
insights-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/
RESOURCEGROUPS/{resource group name}/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/{batch account name}/y={four-digit numeric year}/
m={two-digit numeric month}/d={two-digit numeric day}/
h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
Voorbeeld:

```
insights-metrics-pt1m/resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/
RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/MYBATCHACCOUNT/y=2018/m=03/d=05/h=22/m=00/PT1H.json
```
Elke PT1H.json blob-bestand bevat opgemaakte JSON gebeurtenissen die hebben plaatsgevonden binnen het uur dat is opgegeven in de blob-URL (bijvoorbeeld, h = 12). Tijdens het huidige uur worden gebeurtenissen toegevoegd aan het bestand PT1H.json wanneer deze zich voordoen. De minuut waarde (m = 00) is altijd 00, aangezien diagnostische gebeurtenissen worden onderverdeeld in afzonderlijke blobs per uur. (Alle tijden zijn UTC).


Zie voor meer informatie over het schema van de diagnostische logboeken in de opslag, [archief Azure diagnoselogboeken](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

Als u de logboeken in uw account opslag via programmering, gebruik de Storage API's. 

### <a name="service-log-events"></a>Service-logboekgebeurtenissen
Azure Batch, Service, Logboeken, als verzameld, bevatten gebeurtenissen die door de service Azure Batch uitgestoten gedurende de levensduur van een afzonderlijke resource Batch als een groep van toepassingen of taak. Elke gebeurtenis door Batch uitgestraalde vastgelegd in JSON-indeling. Dit is bijvoorbeeld de hoofdtekst van een steekproef **groep maken gebeurtenis**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "5",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

De Batch-service verzendt momenteel de volgende Service gebeurtenissen. Deze lijst zijn mogelijk niet volledig, omdat er aanvullende gebeurtenissen zijn toegevoegd sinds dit artikel voor het laatst is bijgewerkt.

| **Service-logboekgebeurtenissen** |
| --- |
| [Groep maken](batch-pool-create-event.md) |
| [Toepassingen verwijderen starten](batch-pool-delete-start-event.md) |
| [Groep verwijderen is voltooid](batch-pool-delete-complete-event.md) |
| [Groep formaat starten](batch-pool-resize-start-event.md) |
| [Groep formaat voltooid](batch-pool-resize-complete-event.md) |
| [Taak starten](batch-task-start-event.md) |
| [De taak is voltooid](batch-task-complete-event.md) |
| [Taak is mislukt](batch-task-fail-event.md) |



## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [Batch-API's en -hulpprogramma's](batch-apis-tools.md) die beschikbaar zijn voor het bouwen van Batch-oplossingen.
* Meer informatie over [Batch bewakingsoplossingen](monitoring-overview.md).
