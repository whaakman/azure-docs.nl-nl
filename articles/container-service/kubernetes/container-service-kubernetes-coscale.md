---
title: (AFGESCHAFT) Een Azure Kubernetes-cluster bewaken met CoScale
description: Monitor voor een Kubernetes-cluster in Azure Container Service met CoScale
services: container-service
author: fryckbos
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 895346447e33926dcaa5ca09302f35c9d6636ed9
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "53000227"
---
# <a name="deprecated-monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>(AFGESCHAFT) Een Azure Container Service Kubernetes-cluster bewaken met CoScale

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

In dit artikel laten we zien u over het implementeren van de [CoScale](https://web.archive.org/web/20180317071550/ https://www.coscale.com/) agent voor het bewaken van alle knooppunten en de containers in uw Kubernetes-cluster in Azure Container Service. U hebt een account met CoScale nodig voor deze configuratie. 


## <a name="about-coscale"></a>Over CoScale 

CoScale is een bewakingsplatform zijn dat de metrische gegevens en gebeurtenissen verzameld van alle containers in verschillende orchestration-platforms. CoScale biedt fullstack bewaking voor Kubernetes-omgevingen. Biedt deze visualisaties en analyses voor alle lagen in de stack: de OS-, Kubernetes, Docker en toepassingen die in uw containers worden uitgevoerd. CoScale biedt diverse ingebouwde bewaking dashboards en biedt ingebouwde detectie van anomalieën om toe te staan van operators en ontwikkelaars de infrastructuur en toepassing problemen snel te vinden.

![CoScale UI](./media/container-service-kubernetes-coscale/coscale.png)

Zoals u in dit artikel, kunt u agents installeren op een Kubernetes-cluster uit te voeren CoScale als een SaaS-oplossing. Als u voorkomen dat uw gegevens ter plekke wilt, is CoScale ook beschikbaar voor on-premises installatie.


## <a name="prerequisites"></a>Vereisten

U moet eerst [maken van een account CoScale](https://web.archive.org/web/20170507123133/ https://www.coscale.com/free-trial).

In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).

Ook wordt ervan uitgegaan dat u hebt de `az` Azure CLI en `kubectl` hulpprogramma's zijn geïnstalleerd.

U kunt testen als u hebt de `az` hulpprogramma is geïnstalleerd door uit te voeren:

```azurecli
az --version
```

Als u geen de `az` hulpprogramma is geïnstalleerd, worden er instructies [hier](/cli/azure/install-azure-cli).

U kunt testen als u hebt de `kubectl` hulpprogramma is geïnstalleerd door uit te voeren:

```bash
kubectl version
```

Als u geen `kubectl` geïnstalleerd, u kunt uitvoeren:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a>De CoScale-agent installeren met een DaemonSet
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) worden gebruikt door Kubernetes om uit te voeren van één exemplaar van een container op elke host in het cluster.
Ze zijn ideaal voor het uitvoeren van bewakingsagents, zoals de CoScale-agent.

Nadat u zich aanmeldt bij CoScale, gaat u naar de [agent pagina](https://app.coscale.com/) CoScale om agents te installeren op uw cluster met behulp van een DaemonSet. De gebruikersinterface CoScale biedt begeleide configuratiestappen uit om te maken van een agent en de bewaking van uw volledige Kubernetes-cluster starten.

![Configuratie van de agent coScale](./media/container-service-kubernetes-coscale/installation.png)

Voor het starten van de agent op het cluster, voert u de opgegeven opdracht uit:

![Start de agent CoScale](./media/container-service-kubernetes-coscale/agent_script.png)

Dat is alles. Zodra de agents actief en werkend zijn, ziet u gegevens in de console in een paar minuten. Ga naar de [agent pagina](https://app.coscale.com/) aanvullende configuratiestappen uitvoeren voor een samenvatting van het cluster, en Zie dashboards, zoals de **Kubernetes-cluster overzicht**.

![Overzicht van de Kubernetes-cluster](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

De agent CoScale wordt automatisch geïmplementeerd op nieuwe virtuele machines in het cluster. De agent wordt automatisch bijgewerkt wanneer een nieuwe versie is uitgebracht.


## <a name="next-steps"></a>Volgende stappen

Zie de [CoScale documentatie](https://web.archive.org/web/20180415164304/ http://docs.coscale.com:80/) en [blog](https://web.archive.org/web/20170501021344/ http://www.coscale.com:80/blog) voor meer informatie over CoScale bewakingsoplossingen. 

