---
title: Azure Kubernetes cluster - Sysdig bewaken
description: Bewaking van Kubernetes cluster in Azure Container Service met behulp van Sysdig
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 4ff610f72af4e6a750749009f3cd4b4df632a37f
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Een Azure Container Service Kubernetes-cluster met behulp van Sysdig bewaken

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).

Ook wordt ervan uitgegaan dat u de azure cli en kubectl-hulpprogramma's geïnstalleerd hebt.

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

## <a name="sysdig"></a>Sysdig
Sysdig is een externe controle als een servicebedrijf die containers in uw Kubernetes cluster worden uitgevoerd in Azure kunt bewaken. Sysdig, moet een actieve Sysdig-account.
U kunt aanmelden voor een account op hun [site](https://app.sysdigcloud.com).

Nadat u bent aangemeld bij de Sysdig-cloudwebsite, klikt u op uw gebruikersnaam. Op de pagina ziet u uw ‘toegangssleutel’. 

![Sysdig API-sleutel](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a>Installatie van de agents Sysdig naar Kubernetes
Voor het bewaken van uw containers Sysdig een proces uitvoert op elke machine met behulp van een Kubernetes `DaemonSet`.
DaemonSets zijn Kubernetes API-objecten die één exemplaar van een container per computer worden uitgevoerd.
Ze zijn ideaal voor het installeren van hulpprogramma's zoals de Sysdig-bewakingsagent.

Als u wilt de daemonset Sysdig installeren, moet u eerst downloaden [de sjabloon](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) van sysdig. Sla dit bestand op als `sysdig-daemonset.yaml`.

Op Linux en OS X kunt u het volgende uitvoeren:

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

In PowerShell:

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Naast dit bestand voor het invoegen van uw toegangssleutel die u hebt ontvangen van uw account Sysdig bewerken.

Maak ten slotte de DaemonSet:

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>De bewaking weergeven
Eenmaal geïnstalleerd en wordt uitgevoerd, moeten de agents gegevens terug naar Sysdig pomp.  Ga terug naar de [sysdig dashboard](https://app.sysdigcloud.com) en ziet u informatie over uw containers.

U kunt ook installeren Kubernetes-specifieke dashboards via de [wizard Nieuw dashboard](https://app.sysdigcloud.com/#/dashboards/new).
