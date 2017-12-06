---
title: Een Azure-Kubernetes-cluster met CoScale bewaken
description: Een cluster Kubernetes in Azure Container Service met behulp van CoScale bewaken
services: container-service
author: fryckbos
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 2d6757397d76b6ca87a45254cb31f34d34a42541
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>Een Azure Container Service Kubernetes-cluster met CoScale bewaken

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

In dit artikel wordt beschreven hoe u voor het implementeren van de [CoScale](https://www.coscale.com/) agent voor het bewaken van alle knooppunten en containers in uw cluster Kubernetes in Azure Container Service. U moet een account met CoScale voor deze configuratie. 


## <a name="about-coscale"></a>Over CoScale 

CoScale is een controle platform waarmee metrische gegevens en gebeurtenissen verzameld uit alle containers in verschillende orchestration-platforms. CoScale biedt volledige stack bewaking voor Kubernetes omgevingen. Biedt visualisaties en analytics voor alle lagen in de stack: de OS, Kubernetes, Docker en toepassingen die worden uitgevoerd binnen uw containers. CoScale biedt diverse ingebouwde bewaking dashboards en biedt ingebouwde afwijkingsdetectie om toe te staan van operators en ontwikkelaars, zodat u snel problemen infrastructuur en de toepassing.

![CoScale gebruikersinterface](./media/container-service-kubernetes-coscale/coscale.png)

Zoals u in dit artikel, kunt u agents installeren op een cluster Kubernetes CoScale uitvoeren als een SaaS-oplossing. Als u voorkomen dat uw gegevens ter plaatse wilt, is CoScale ook beschikbaar voor lokale installatie.


## <a name="prerequisites"></a>Vereisten

U moet eerst [maken van een account CoScale](https://www.coscale.com/free-trial).

In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).

Ook wordt ervan uitgegaan dat u hebt de `az` Azure CLI en `kubectl` hulpprogramma's geïnstalleerd.

U kunt testen, hebt u de `az` hulpprogramma is geïnstalleerd door te voeren:

```azurecli
az --version
```

Als u hebt de `az` hulpprogramma is geïnstalleerd, er zijn instructies [hier](/cli/azure/install-azure-cli).

U kunt testen, hebt u de `kubectl` hulpprogramma is geïnstalleerd door te voeren:

```bash
kubectl version
```

Als u geen `kubectl` geïnstalleerd, u kunt uitvoeren:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a>De CoScale-agent installeren met een DaemonSet
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) worden gebruikt door Kubernetes één exemplaar van een container uitvoeren op elke host in het cluster.
Ze zijn ideaal voor bewaking agenten zoals de CoScale agent uit te voeren.

Nadat u zich bij CoScale aanmelden, gaat u naar de [agent pagina](https://app.coscale.com/) CoScale om agents te installeren op uw cluster met behulp van een DaemonSet. De gebruikersinterface CoScale biedt begeleide configuratiestappen voor het maken van een agent en start de bewaking van uw volledige Kubernetes-cluster.

![De agentconfiguratie coScale](./media/container-service-kubernetes-coscale/installation.png)

Voor het starten van de agent op het cluster, moet u de opgegeven opdracht uitvoeren:

![Start de agent CoScale](./media/container-service-kubernetes-coscale/agent_script.png)

Dat is alles. Zodra de agents actief en werkend zijn, moet u de gegevens in de console zien over een paar minuten. Ga naar de [agent pagina](https://app.coscale.com/) aanvullende configuratiestappen uitvoeren voor een samenvatting van uw cluster, en Zie dashboards, zoals de **Kubernetes cluster overzicht**.

![Overzicht van de cluster Kubernetes](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

De agent CoScale wordt automatisch geïmplementeerd op nieuwe computers in het cluster. De agent wordt automatisch bijgewerkt wanneer een nieuwe versie wordt uitgebracht.


## <a name="next-steps"></a>Volgende stappen

Zie de [CoScale documentatie](http://docs.coscale.com/) en [blog](https://www.coscale.com/blog) voor meer informatie over CoScale bewakingsoplossingen. 

