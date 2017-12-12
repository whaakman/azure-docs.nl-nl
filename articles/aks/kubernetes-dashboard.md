---
title: Cluster met een webgebruikersinterface Azure Kubernetes beheren
description: Gebruik het dashboard Kubernetes in AKS
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ca828dab7bdb47e41596be2717598cfe828953ca
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="kubernetes-dashboard-with-azure-container-service-aks"></a>Kubernetes dashboard met Azure Container Service (AKS)

De Azure CLI kunnen worden gebruikt voor het starten van het Kubernetes Dashboard. Dit document wordt begeleid bij het dashboard Kubernetes beginnen met de Azure CLI en bepaalde bewerkingen basic dashboard ook wordt uitgelegd. Voor meer informatie over het dashboard Kubernetes Zie [Kubernetes Web UI Dashboard][kubernetes-dashboard].

## <a name="before-you-begin"></a>Voordat u begint

Bij de stappen die in dit document worden uiteengezet, wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en een kubectl-verbinding met het cluster hebt gemaakt. Als u deze items moet weergeven, de [AKS Quick Start][aks-quickstart].

Ook moet de Azure CLI-versie 2.0.21 of later zijn geïnstalleerd en geconfigureerd. Voer az --version uit om de versie te zoeken. Als u wilt installeren of upgraden, Zie [Azure CLI installeren][install-azure-cli].

## <a name="start-kubernetes-dashboard"></a>Start Kubernetes dashboard

Gebruik de `az aks browse` opdracht waarmee het dashboard Kubernetes wordt gestart. Wanneer deze opdracht wordt uitgevoerd, vervangt u de naam van de resource group en het cluster.

```azurecli
az aks browse --resource-group myResourceGroup --name myK8SCluster
```

Met deze opdracht maakt een proxy tussen uw ontwikkelsysteem en de API Kubernetes en opent een webbrowser naar het dashboard Kubernetes.

## <a name="run-an-application"></a>Een toepassing uitvoeren

Klik in het dashboard Kubernetes op de **maken** knop in het bovenste rechtervenster. Geef de naam op voor de implementatie `nginx` en voer `nginx:latest` voor de naam van de installatiekopieën. Onder **Service**, selecteer **externe** en voer `80` voor de poort en de doelpoort.

Wanneer u klaar bent, klikt u op **implementeren** de implementatie te maken.

![Dialoogvenster maken Kubernetes Service](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>De toepassing weergeven

Status van de toepassing kan worden weergegeven op het dashboard Kubernetes. Zodra de toepassing wordt uitgevoerd, heeft elk onderdeel een groen selectievakje ernaast.

![Kubernetes gehele product](./media/container-service-kubernetes-ui/complete-deployment.png)

Als u meer informatie over het gehele product van toepassing, klikt u op **gehele product** in het menu links en selecteer vervolgens de **NGINX** schil. Hier ziet u schil-specifieke informatie zoals resourceverbruik.

![Kubernetes Resources](./media/container-service-kubernetes-ui/running-pods.png)

Voor het IP-adres van de toepassing, klikt u op **Services** in het menu links en selecteer vervolgens de **NGINX** service.

![nginx weergeven](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>De toepassing bewerken

Naast het maken en weergeven van toepassingen, kan het dashboard Kubernetes worden gebruikt om te bewerken en bijwerken van implementaties van toepassingen.

Een implementatie bewerken, klikt u op **implementaties** in het menu links en selecteer vervolgens de **NGINX** implementatie. Tot slot selecteert **bewerken** in de bovenste navigatiebalk voor rechts.

![Kubernetes bewerken](./media/container-service-kubernetes-ui/view-deployment.png)

Zoek de `spec.replica` -waarde moet 1 zijn, wijzigt u deze waarde in 3. Het aantal replica's van de NGINX-implementatie wordt hierbij verhoogd van 1 tot 3.

Selecteer **Update** als u klaar bent.

![Kubernetes bewerken](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het dashboard Kubernetes de Kubernetes-documentatie.

> [!div class="nextstepaction"]
> [Kubernetes Web UI-Dashboard][kubernetes-dashboard]

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli