---
title: Gebruik een statisch IP-adres met de Azure Kubernetes Service (AKS) load balancer
description: Gebruik een statisch IP-adres met de Azure Kubernetes Service (AKS) load balancer.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 2/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c250ef3520079f58eea2362212d861fdb134e1af
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="use-a-static-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Gebruik een statisch IP-adres met de Azure Kubernetes Service (AKS) load balancer

In sommige gevallen, zoals wanneer het Azure Kubernetes Service (AKS) load balancer opnieuw gemaakt wordt of Kubernetes services met een type Load Balancer opnieuw gemaakt worden, het openbare IP-adres van de service Kubernetes kan worden gewijzigd. Dit documentgegevens voor het configureren van een statisch IP-adres voor uw Kubernetes-services.

## <a name="create-static-ip-address"></a>Statische IP-adres maken

Maak een statische openbare IP-adres voor de service Kubernetes. Het IP-adres moet worden gemaakt in de resourcegroep die automatisch gemaakt tijdens de implementatie van het cluster is. Zie voor informatie over de verschillende AKS resourcegroepen en identificeren van de resourcegroep automatisch gemaakt, de [AKS Veelgestelde vragen over][aks-faq-resource-group].

Gebruik de [az netwerk openbare IP-adres maken] [ az-network-public-ip-create] opdracht voor het maken van het IP-adres.

```azurecli-interactive
az network public-ip create --resource-group MC_myResourceGRoup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static
```

Let op het IP-adres.

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    "ipConfiguration": null,
    "ipTags": [],
    "location": "eastus",
    "name": "myAKSPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Static",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "56ec8760-a3b8-4aeb-a89d-42e68d2cbc8c",
    "sku": {
      "name": "Basic"
    },
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses",
    "zones": null
  }
````

 Indien nodig, het adres kan worden opgehaald met de [az netwerk openbare ip-lijst] [ az-network-public-ip-list] opdracht.

```azurecli-interactive
az network public-ip list --resource-group MC_myResourceGRoup_myAKSCluster_eastus --query [0].ipAddress --output tsv
```

```console
40.121.183.52
```

## <a name="create-service-with-ip-address"></a>Service met IP-adres maken

Zodra het statische IP-adres is ingericht, een service Kubernetes kan worden gemaakt met de `loadBalancerIP` eigenschap en een waarde van het statische IP-adres.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="troubleshooting"></a>Problemen oplossen

Als het statische IP-adres niet gemaakt is of is gemaakt in de verkeerde resourcegroep, mislukt het maken van de service. Als u wilt oplossen, retourneren gebeurtenissen van de service maken met de [kubectl beschrijven] [ kubectl-describe] opdracht.

```azurecli-interactive
kubectl describe service azure-vote-front
```

```console
Name:                     azure-vote-front
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-vote-front
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-vote-front: user supplied IP Address 40.121.183.52 was not found
```

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az_network_public_ip_create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az_network_public_ip_list
