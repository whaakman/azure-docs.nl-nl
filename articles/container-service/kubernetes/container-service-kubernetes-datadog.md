---
title: Monitor Azure Kubernetes cluster met Datadog | Microsoft Docs
description: Bewaking van Kubernetes cluster in Azure Container Service met behulp van Datadog
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 40b34457447a8f80d8cdf77579750e0c42df22d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Een Azure Container Service-cluster met DataDog bewaken

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
