---
title: Azure Kubernetes-cluster - Sysdig bewaken
description: Kubernetes-cluster in Azure Container Service met Sysdig bewaken
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: bbf59a35f420b5bbf292fbdaa5a8bbc173e4ee24
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35645782"
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Een Azure Container Service Kubernetes-cluster met Sysdig bewaken

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).

Ook wordt ervan uitgegaan dat u de azure cli en kubectl-hulpprogramma's geïnstalleerd hebt.

U kunt testen als u hebt de `az` hulpprogramma is geïnstalleerd door uit te voeren:

```console
$ az --version
```

Als u geen de `az` hulpprogramma is geïnstalleerd, worden er instructies [hier](https://github.com/azure/azure-cli#installation).

U kunt testen als u hebt de `kubectl` hulpprogramma is geïnstalleerd door uit te voeren:

```console
$ kubectl version
```

Als u geen `kubectl` geïnstalleerd, u kunt uitvoeren:

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a>Sysdig
Sysdig is een externe bewaking als een servicebedrijf dat containers in uw Kubernetes-cluster worden uitgevoerd in Azure kunt bewaken. Sysdig moet een actief Sysdig-account.
U kunt zich registreren voor een account op hun [site](https://app.sysdigcloud.com).

Nadat u bent aangemeld bij de Sysdig-cloudwebsite, klikt u op uw gebruikersnaam. Op de pagina ziet u uw ‘toegangssleutel’. 

![Sysdig API-sleutel](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a>Installatie van de Sysdig-agents naar Kubernetes
Sysdig wordt voor het bewaken van uw containers, een proces uitgevoerd op elke machine met behulp van een Kubernetes `DaemonSet`.
DaemonSets zijn Kubernetes API-objecten die één exemplaar van een container per machine uitvoeren.
Ze zijn ideaal voor het installeren van hulpprogramma's als bewaking van de Sysdig-agent.

Als u wilt de daemonset Sysdig installeren, moet u eerst downloaden [de sjabloon](https://github.com/draios/sysdig-cloud-scripts/tree/master/agent_deploy/kubernetes) van sysdig. Sla dit bestand op als `sysdig-daemonset.yaml`.

Op Linux en OS X kunt u het volgende uitvoeren:

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

In PowerShell:

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Vervolgens bewerkt u het bestand voor het invoegen van uw toegangssleutel die u hebt verkregen via uw Sysdig-account.

Maak ten slotte de DaemonSet:

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>De bewaking weergeven
Eenmaal geïnstalleerd en wordt uitgevoerd, moeten de agents gegevens terug naar de Sysdig pump.  Ga terug naar de [sysdig dashboard](https://app.sysdigcloud.com) en u ziet nu informatie over uw containers.

U kunt ook installeren met Kubernetes-specifieke dashboards via de [wizard Nieuw dashboard](https://app.sysdigcloud.com/#/dashboards/new).
