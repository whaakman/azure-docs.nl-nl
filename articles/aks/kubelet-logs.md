---
title: Kubelet-logboeken ophalen uit Azure Kubernetes Service (AKS)
description: Kubelet logboeken ophalen uit Azure Kubernetes Service (AKS) clusterknooppunten
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0467be7e91fdbf4685fc41a375ea86a503e26009
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="get-kubelet-logs-from-azure-kubernetes-service-aks-cluster-nodes"></a>Kubelet logboeken ophalen uit Azure Kubernetes Service (AKS) clusterknooppunten

In sommige gevallen moet u wellicht kubelet logboeken ophalen uit een Azure Kubernetes Service (AKS)-knooppunt voor het oplossen van problemen. In dit document worden één optie voor het ophalen van deze logboeken.

## <a name="create-an-ssh-connection"></a>Maken van een SSH-verbinding

Maak eerst een SSH-verbinding met het knooppunt waarop u wilt pull-kubelet Logboeken. Deze bewerking wordt beschreven in de [SSH in Azure Kubernetes Service (AKS) clusterknooppunten] [ aks-ssh] document.

## <a name="get-kubelet-logs"></a>Kubelet-logboeken ophalen

U hebt één keer verbonden met het knooppunt, voer de volgende opdracht voor het ophalen van de logboeken kubelet.

```azurecli-interactive
docker logs $(docker ps -a | grep "hyperkube kubele" | awk -F ' ' '{print $1}')
```

Voorbeeld van uitvoer:

```console
2018-03-05 00:04:00.883195 I | proto: duplicate proto type registered: google.protobuf.Any
2018-03-05 00:04:00.883242 I | proto: duplicate proto type registered: google.protobuf.Duration
2018-03-05 00:04:00.883253 I | proto: duplicate proto type registered: google.protobuf.Timestamp
Flag --non-masquerade-cidr has been deprecated, will be removed in a future version
Flag --non-masquerade-cidr has been deprecated, will be removed in a future version
I0305 00:04:00.978560    8021 feature_gate.go:144] feature gates: map[Accelerators:true]
I0305 00:04:00.978996    8021 azure.go:174] azure: using client_id+client_secret to retrieve access token
I0305 00:04:00.979041    8021 server.go:439] Successfully initialized cloud provider: "azure" from the config file: "/etc/kubernetes/azure.json"
I0305 00:04:00.979058    8021 server.go:740] cloud provider determined current node name to be aks-nodepool1-42032720-0
```

<!-- LINKS - internal -->
[aks-ssh]: aks-ssh.md