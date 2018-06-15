---
title: Een Azure Kubernetes Service (AKS) interne load balancer maken
description: Een interne load balancer gebruiken met Azure Kubernetes Service (AKS).
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 3/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a1a5d6d455086ee34767e92b277936840717bcd6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33933770"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Een interne load balancer gebruiken met Azure Kubernetes Service (AKS)

Interne load balancing maakt een service Kubernetes toegankelijk voor toepassingen die worden uitgevoerd in hetzelfde virtuele netwerk als het cluster Kubernetes. Dit documentgegevens maken van een interne load balancer met Azure Kubernetes Service (AKS).

## <a name="create-internal-load-balancer"></a>Interne load balancer maken

Voor het maken van een interne load balancer, het maken van een servicemanifest van het servicetype `LoadBalancer` en de `azure-load-balancer-internal` aantekening zoals in het volgende voorbeeld.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Zodra geïmplementeerd, wordt een Azure load balancer gemaakt en beschikbaar gesteld in hetzelfde virtuele netwerk als het cluster AKS.

![Afbeelding van AKS interne load balancer](media/internal-lb/internal-lb.png)

Wanneer bij het ophalen van de service details, het IP-adres in de `EXTERNAL-IP` kolom is het IP-adres van de interne load balancer.

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Geef een IP-adres

Als u gebruiken van een specifiek IP-adres met de interne load balancer wilt, voegt u de `loadBalancerIP` eigenschap in op de load balancer-specificaties. Het opgegeven IP-adres moet zich in hetzelfde subnet als het cluster AKS bevinden en moet niet worden toegewezen aan een resource.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Bij het ophalen van de details van de service het IP-adres op de `EXTERNAL-IP` moet overeenkomen met het opgegeven IP-adres.

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="delete-the-load-balancer"></a>De load balancer verwijderen

Wanneer alle services met behulp van de interne load balancer is verwijderd, wordt de load balancer zelf ook verwijderd.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Kubernetes services op de [Kubernetes services documentatie][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/