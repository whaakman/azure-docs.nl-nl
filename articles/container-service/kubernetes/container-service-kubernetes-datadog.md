---
title: Azure Kubernetes cluster met Datadog bewaken
description: Bewaking van Kubernetes cluster in Azure Container Service met behulp van Datadog
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 96bbd88f163939b58263f2f3a94b7b4d90f85736
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Een Azure Container Service-cluster met DataDog bewaken

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).

Ook wordt ervan uitgegaan dat u hebt de `az` Azure cli en `kubectl` hulpprogramma's geïnstalleerd.

U kunt testen, hebt u de `az` hulpprogramma is geïnstalleerd door te voeren:

```console
$ az --version
```

Als u hebt de `az` hulpprogramma is geïnstalleerd, er zijn instructies [hier](https://github.com/azure/azure-cli#installation).

U kunt testen, hebt u de `kubectl` hulpprogramma is geïnstalleerd door te voeren:

```console
$ kubectl version
```

Als u geen `kubectl` geïnstalleerd, u kunt uitvoeren:

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Datadog is een controleservice die bewakingsgegevens moeten worden verzameld uit uw containers in uw Azure Container Service-cluster. Datadog heeft een Docker-integratie Dashboard waarin u de specifieke metrische gegevens binnen uw containers kunt zien. Metrische gegevens die afkomstig zijn van uw containers worden ingedeeld op basis van CPU, geheugen, netwerk en i/o. Datadog splitst metrische gegevens in containers en afbeeldingen.

U moet eerst [een account maken](https://www.datadoghq.com/lpg/)

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>De Datadog-Agent installeren met een DaemonSet
DaemonSets worden gebruikt door Kubernetes één exemplaar van een container uitvoeren op elke host in het cluster.
Ze zijn ideaal voor bewaking agenten uit te voeren.

Nadat u zich bij Datadog hebt aangemeld, kunt u volgen de [Datadog instructies](https://app.datadoghq.com/account/settings#agent/kubernetes) Datadog om agents te installeren op uw cluster met behulp van een DaemonSet.

## <a name="conclusion"></a>Conclusie
Dat is alles. Wanneer de agents actief en werkend zijn ziet u gegevens in de console in een paar minuten. Ga naar de geïntegreerde [kubernetes dashboard](https://app.datadoghq.com/screen/integration/kubernetes) voor een overzicht van het cluster.
