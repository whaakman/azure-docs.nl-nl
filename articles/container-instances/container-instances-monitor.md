---
title: Containers in Azure Container Instances bewaken
description: Hier vindt u meer informatie over het bewaken van het verbruik van rekenresources zoals CPU en geheugen door uw containers in Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 04/24/2018
ms.author: danlep
ms.openlocfilehash: 950d8b4b5ec1a55e2054039a01d6807915b5c714
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58848230"
---
# <a name="monitor-container-resources-in-azure-container-instances"></a>Containerresources in Azure Container Instances bewaken

Azure Monitor biedt inzicht in de rekenresources die door uw containerinstanties worden gebruikt. Gebruik Azure Monitor voor het bijhouden van het CPU- en geheugenverbruik van de containergroepen en hun containers. Deze gegevens over resourcegebruik helpt u bij het bepalen van de beste instellingen voor de CPU en het geheugen voor de containergroepen.

In dit document wordt besproken hoe CPU- en geheugengebruik voor containerinstanties wordt verzameld met behulp van zowel de Azure Portal als Azure CLI.

> [!IMPORTANT]
> Op dit moment zijn metrische gegevens over resourcegebruik alleen beschikbaar voor Linux-containers.
>

## <a name="available-metrics"></a>Beschikbare metrische gegevens

Azure Monitor biedt metrische gegevens over zowel **CPU**- als **geheugen**gebruik voor Azure Container Instances. Beide metrische gegevens zijn beschikbaar voor een containergroep en afzonderlijke containers.

Metrische gegevens voor CPU worden uitgedrukt in **millicores**. Eén millicore is 1/1000ste van een CPU-kern, dus 500 millicores (of 500 m) vertegenwoordigt een gebruik van 50% van de CPU-kern.

Metrische gegevens voor geheugen worden uitgedrukt in **bytes**.

## <a name="get-metrics---azure-portal"></a>Metrische gegevens ophalen - Azure Portal

Wanneer een containergroep wordt gemaakt, zijn Azure Monitor-gegevens beschikbaar op de Azure Portal. Wanneer u de metrische gegevens van een containergroep wilt zien, selecteert u de resourcegroep en vervolgens de containergroep. Hier ziet u vooraf gemaakte grafieken voor CPU-en geheugengebruik.

![dubbele grafiek][dual-chart]

Als u een containergroep met meerdere containers hebt, gebruikt u een [dimensie] [ monitor-dimension] om metrische gegevens voor elke afzonderlijke container te presenteren. Als u een grafiek wilt maken met metrische gegevens voor elke afzonderlijke container, voert u de volgende stappen uit:

1. Selecteer **Controleren** in het navigatiemenu links.
2. Selecteer een containergroep en een type metrische gegevens (CPU of Geheugen).
3. Selecteer de groene dimensieknop en selecteer vervolgens **Containernaam**.

![dimensie][dimension]

## <a name="get-metrics---azure-cli"></a>Metrische gegevens ophalen - Azure CLI

CPU- en geheugengebruik van containerinstanties kan ook worden verzameld met de Azure CLI. Haal eerst de id van de containergroep op met de volgende opdracht. Vervang `<resource-group>` door de naam van uw resourcegroep en `<container-group>` door de naam van uw containergroep.


```console
CONTAINER_GROUP=$(az container show --resource-group <resource-group> --name <container-group> --query id --output tsv)
```

Gebruik de volgende opdracht om metrische gegevens over **CPU-gebruik** op te halen.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --output table

Timestamp            Name              Average
-------------------  ------------  -----------
2018-04-22 04:39:00  CPU Usage
2018-04-22 04:40:00  CPU Usage
2018-04-22 04:41:00  CPU Usage
2018-04-22 04:42:00  CPU Usage
2018-04-22 04:43:00  CPU Usage      0.375
2018-04-22 04:44:00  CPU Usage      0.875
2018-04-22 04:45:00  CPU Usage      1
2018-04-22 04:46:00  CPU Usage      3.625
2018-04-22 04:47:00  CPU Usage      1.5
2018-04-22 04:48:00  CPU Usage      2.75
2018-04-22 04:49:00  CPU Usage      1.625
2018-04-22 04:50:00  CPU Usage      0.625
2018-04-22 04:51:00  CPU Usage      0.5
2018-04-22 04:52:00  CPU Usage      0.5
2018-04-22 04:53:00  CPU Usage      0.5
```

En gebruik de volgende opdracht om metrische gegevens over **geheugengebruik** op te halen.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --output table

Timestamp            Name              Average
-------------------  ------------  -----------
2018-04-22 04:38:00  Memory Usage
2018-04-22 04:39:00  Memory Usage
2018-04-22 04:40:00  Memory Usage
2018-04-22 04:41:00  Memory Usage
2018-04-22 04:42:00  Memory Usage  6.76915e+06
2018-04-22 04:43:00  Memory Usage  9.22061e+06
2018-04-22 04:44:00  Memory Usage  9.83552e+06
2018-04-22 04:45:00  Memory Usage  8.42906e+06
2018-04-22 04:46:00  Memory Usage  8.39526e+06
2018-04-22 04:47:00  Memory Usage  8.88013e+06
2018-04-22 04:48:00  Memory Usage  8.89293e+06
2018-04-22 04:49:00  Memory Usage  9.2073e+06
2018-04-22 04:50:00  Memory Usage  9.36243e+06
2018-04-22 04:51:00  Memory Usage  9.30509e+06
2018-04-22 04:52:00  Memory Usage  9.2416e+06
2018-04-22 04:53:00  Memory Usage  9.1008e+06
```

Voor een groep met meerdere containers kan de dimensie `containerName` worden toegevoegd om deze gegevens per container te retourneren.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --dimension containerName --output table

Timestamp            Name          Containername             Average
-------------------  ------------  --------------------  -----------
2018-04-22 17:03:00  Memory Usage  aci-tutorial-app      1.95338e+07
2018-04-22 17:04:00  Memory Usage  aci-tutorial-app      1.93096e+07
2018-04-22 17:05:00  Memory Usage  aci-tutorial-app      1.91488e+07
2018-04-22 17:06:00  Memory Usage  aci-tutorial-app      1.94335e+07
2018-04-22 17:07:00  Memory Usage  aci-tutorial-app      1.97714e+07
2018-04-22 17:08:00  Memory Usage  aci-tutorial-app      1.96178e+07
2018-04-22 17:09:00  Memory Usage  aci-tutorial-app      1.93434e+07
2018-04-22 17:10:00  Memory Usage  aci-tutorial-app      1.92614e+07
2018-04-22 17:11:00  Memory Usage  aci-tutorial-app      1.90659e+07
2018-04-22 16:12:00  Memory Usage  aci-tutorial-sidecar  1.35373e+06
2018-04-22 16:13:00  Memory Usage  aci-tutorial-sidecar  1.28614e+06
2018-04-22 16:14:00  Memory Usage  aci-tutorial-sidecar  1.31379e+06
2018-04-22 16:15:00  Memory Usage  aci-tutorial-sidecar  1.29536e+06
2018-04-22 16:16:00  Memory Usage  aci-tutorial-sidecar  1.38138e+06
2018-04-22 16:17:00  Memory Usage  aci-tutorial-sidecar  1.41312e+06
2018-04-22 16:18:00  Memory Usage  aci-tutorial-sidecar  1.49914e+06
2018-04-22 16:19:00  Memory Usage  aci-tutorial-sidecar  1.43565e+06
2018-04-22 16:20:00  Memory Usage  aci-tutorial-sidecar  1.408e+06
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure Monitoring is te vinden in het [Azure Monitoring-overzicht][azure-monitoring].

<!-- IMAGES -->
[cpu-chart]: ./media/container-instances-monitor/cpu-multi.png
[dimension]: ./media/container-instances-monitor/dimension.png
[dual-chart]: ./media/container-instances-monitor/metrics.png
[memory-chart]: ./media/container-instances-monitor/memory-multi.png

<!-- LINKS - Internal -->
[azure-monitoring]: ../monitoring-and-diagnostics/monitoring-overview.md
[monitor-dimension]: ../azure-monitor/platform/data-platform-metrics.md#multi-dimensional-metrics
