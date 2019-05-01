---
title: Containers in Azure Container Instances bewaken
description: Klik hier voor meer informatie over het bewaken van het verbruik van de compute-resources, zoals CPU en geheugen door uw containers in Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 04/24/2019
ms.author: danlep
ms.openlocfilehash: 7b46ea0518038eeb908591b8438acc2a9095242c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64570940"
---
# <a name="monitor-container-resources-in-azure-container-instances"></a>Containerresources in Azure Container Instances bewaken

[Azure Monitor] [ azure-monitoring] biedt inzicht in de rekenresources die worden gebruikt door uw containers-instanties. Deze gegevens over brongebruik helpt u de beste resource-instellingen voor uw containergroepen bepalen. Azure Monitor biedt ook metrische gegevens die netwerkactiviteit in containerexemplaren bijhouden.

In dit document worden verzameld met Azure Monitor metrics voor container instances met behulp van Azure portal en Azure CLI.

> [!IMPORTANT]
> Metrische gegevens van Azure Monitor in Azure Container Instances zijn momenteel in preview en sommige [gelden beperkingen](#preview-limitations). Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

## <a name="preview-limitations"></a>Preview-beperkingen

Azure Monitor metrics zijn op dit moment alleen beschikbaar voor Linux-containers.

## <a name="available-metrics"></a>Beschikbare metrische gegevens

Azure Monitor biedt de volgende [metrische gegevens voor Azure Container Instances][supported-metrics]. Deze metrische gegevens zijn beschikbaar voor een containergroep en afzonderlijke containers.

* **CPU-gebruik** - gemeten in **millicores**. Een millicore is 1/1000th van een CPU-kern, zodat 500 millicores (of 500 miljoen) vertegenwoordigt 50% gebruik van een CPU-kern. Samengevoegd als **Gemiddeld gebruik** over alle cores.

* **Geheugengebruik** - samengevoegd als **bytes gemiddelde**.

* **Ontvangen Bytes Per seconde netwerk** en **netwerk Bytes verzonden Per seconde** - samengevoegd als **gemiddelde aantal bytes per seconde**. 

## <a name="get-metrics---azure-portal"></a>Metrische gegevens ophalen - Azure Portal

Wanneer een containergroep wordt gemaakt, zijn Azure Monitor-gegevens beschikbaar op de Azure Portal. Als u wilt een containergroep metrische gegevens weergeven, gaat u naar de **overzicht** pagina voor de containergroep. Hier kunt u vooraf gemaakte grafieken voor elk van de beschikbare metrische gegevens zien.

![dubbele grafiek][dual-chart]

Gebruik in de containergroep van een met meerdere containers, een [dimensie] [ monitor-dimension] metrische per container presenteren. Als u een grafiek wilt maken met metrische gegevens voor elke afzonderlijke container, voert u de volgende stappen uit:

1. In de **overzicht** pagina, selecteert u een van de grafieken met metrische gegevens, zoals **CPU**. 
1. Selecteer de **toepassen splitsen** en selecteer **containernaam**.

![dimensie][dimension]

## <a name="get-metrics---azure-cli"></a>Metrische gegevens ophalen - Azure CLI

Metrische gegevens voor container instances kunnen ook worden verzameld met de Azure CLI. Haal eerst de id van de containergroep op met de volgende opdracht. Vervang `<resource-group>` door de naam van uw resourcegroep en `<container-group>` door de naam van uw containergroep.


```console
CONTAINER_GROUP=$(az container show --resource-group <resource-group> --name <container-group> --query id --output tsv)
```

Gebruik de volgende opdracht om metrische gegevens over **CPU-gebruik** op te halen.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --output table

Timestamp            Name       Average
-------------------  ---------  ---------
2019-04-23 22:59:00  CPU Usage
2019-04-23 23:00:00  CPU Usage
2019-04-23 23:01:00  CPU Usage  0.0
2019-04-23 23:02:00  CPU Usage  0.0
2019-04-23 23:03:00  CPU Usage  0.5
2019-04-23 23:04:00  CPU Usage  0.5
2019-04-23 23:05:00  CPU Usage  0.5
2019-04-23 23:06:00  CPU Usage  1.0
2019-04-23 23:07:00  CPU Usage  0.5
2019-04-23 23:08:00  CPU Usage  0.5
2019-04-23 23:09:00  CPU Usage  1.0
2019-04-23 23:10:00  CPU Usage  0.5
```

Wijzig de waarde van de `--metric` parameter in de opdracht voor het ophalen van andere [ondersteunde metrische gegevens][supported-metrics]. Gebruik bijvoorbeeld de volgende opdracht om op te halen **geheugen** metrische gegevens over gebruik. 

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --output table

Timestamp            Name          Average
-------------------  ------------  ----------
2019-04-23 22:59:00  Memory Usage
2019-04-23 23:00:00  Memory Usage
2019-04-23 23:01:00  Memory Usage  0.0
2019-04-23 23:02:00  Memory Usage  8859648.0
2019-04-23 23:03:00  Memory Usage  9181184.0
2019-04-23 23:04:00  Memory Usage  9580544.0
2019-04-23 23:05:00  Memory Usage  10280960.0
2019-04-23 23:06:00  Memory Usage  7815168.0
2019-04-23 23:07:00  Memory Usage  7739392.0
2019-04-23 23:08:00  Memory Usage  8212480.0
2019-04-23 23:09:00  Memory Usage  8159232.0
2019-04-23 23:10:00  Memory Usage  8093696.0
```

Voor een groep met meerdere containers, de `containerName` dimensie kan worden toegevoegd om terug te keren metrische gegevens per container.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --dimension containerName --output table

Timestamp            Name          Containername             Average
-------------------  ------------  --------------------  -----------
2019-04-23 22:59:00  Memory Usage  aci-tutorial-app
2019-04-23 23:00:00  Memory Usage  aci-tutorial-app
2019-04-23 23:01:00  Memory Usage  aci-tutorial-app      0.0
2019-04-23 23:02:00  Memory Usage  aci-tutorial-app      16834560.0
2019-04-23 23:03:00  Memory Usage  aci-tutorial-app      17534976.0
2019-04-23 23:04:00  Memory Usage  aci-tutorial-app      18329600.0
2019-04-23 23:05:00  Memory Usage  aci-tutorial-app      19742720.0
2019-04-23 23:06:00  Memory Usage  aci-tutorial-app      14786560.0
2019-04-23 23:07:00  Memory Usage  aci-tutorial-app      14651392.0
2019-04-23 23:08:00  Memory Usage  aci-tutorial-app      15470592.0
2019-04-23 23:09:00  Memory Usage  aci-tutorial-app      15450112.0
2019-04-23 23:10:00  Memory Usage  aci-tutorial-app      15339520.0
2019-04-23 22:59:00  Memory Usage  aci-tutorial-sidecar
2019-04-23 23:00:00  Memory Usage  aci-tutorial-sidecar
2019-04-23 23:01:00  Memory Usage  aci-tutorial-sidecar  0.0
2019-04-23 23:02:00  Memory Usage  aci-tutorial-sidecar  884736.0
2019-04-23 23:03:00  Memory Usage  aci-tutorial-sidecar  827392.0
2019-04-23 23:04:00  Memory Usage  aci-tutorial-sidecar  831488.0
2019-04-23 23:05:00  Memory Usage  aci-tutorial-sidecar  819200.0
2019-04-23 23:06:00  Memory Usage  aci-tutorial-sidecar  843776.0
2019-04-23 23:07:00  Memory Usage  aci-tutorial-sidecar  827392.0
2019-04-23 23:08:00  Memory Usage  aci-tutorial-sidecar  954368.0
2019-04-23 23:09:00  Memory Usage  aci-tutorial-sidecar  868352.0
2019-04-23 23:10:00  Memory Usage  aci-tutorial-sidecar  847872.0
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure Monitoring is te vinden in het [Azure Monitoring-overzicht][azure-monitoring].

Meer informatie over het maken van [metrische waarschuwingen] [ metric-alert] om te worden geïnformeerd wanneer een metriek voor Azure Container Instances een drempelwaarde overschrijdt.

<!-- IMAGES -->
[cpu-chart]: ./media/container-instances-monitor/cpu-multi.png
[dimension]: ./media/container-instances-monitor/dimension.png
[dual-chart]: ./media/container-instances-monitor/metrics.png
[memory-chart]: ./media/container-instances-monitor/memory-multi.png

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-monitoring]: ../azure-monitor/overview.md
[metric-alert]: ..//azure-monitor/platform/alerts-metric.md
[monitor-dimension]: ../azure-monitor/platform/data-platform-metrics.md#multi-dimensional-metrics
[supported-metrics]: ../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups
