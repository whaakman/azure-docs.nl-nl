---
title: Gebruik een statisch IP-adres met de Azure Kubernetes-service (AKS) load balancer
description: Meer informatie over het maken en gebruiken van een statisch IP-adres met de Azure Kubernetes service (AKS) load balancer.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 03/04/2019
ms.author: mlearned
ms.openlocfilehash: 9e32715766734bcbb150d70aeed2dc5b06a4bcbb
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "67614473"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Gebruik een statisch openbaar IP-adres met de Azure Kubernetes-service (AKS) load balancer

Het open bare IP-adres dat is toegewezen aan een load balancer bron die is gemaakt met een AKS-cluster, is standaard alleen geldig voor de levens duur van die resource. Als u de Kubernetes-service verwijdert, worden ook de gekoppelde load balancer en het IP-adres verwijderd. Als u een specifiek IP-adres wilt toewijzen of een IP-adres voor opnieuw geïmplementeerde Kubernetes-Services wilt behouden, kunt u een statisch openbaar IP-adres maken en gebruiken.

In dit artikel wordt beschreven hoe u een statisch openbaar IP-adres maakt en dit toewijst aan uw Kubernetes-service.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u beschikt over een bestaand AKS-cluster. Als u een AKS-cluster nodig hebt, raadpleegt u de AKS Quick Start [met behulp van de Azure cli][aks-quickstart-cli] of [met behulp van de Azure Portal][aks-quickstart-portal].

Ook moet de Azure CLI-versie 2.0.59 of hoger zijn geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, raadpleegt u [Azure cli installeren][install-azure-cli].

Momenteel wordt alleen de *basis-IP SKU*ondersteund. Het werk wordt uitgevoerd om de *standaard-IP-resource-* SKU te ondersteunen. Zie [IP-adres typen en toewijzings methoden in azure][ip-sku]voor meer informatie.

## <a name="create-a-static-ip-address"></a>Een statisch IP-adres maken

Wanneer u een statisch openbaar IP-adres maakt voor gebruik met AKS, moet de IP-adres bron worden gemaakt in de **knooppunt** resource groep. Als u de resources wilt scheiden, raadpleegt u de volgende sectie voor het [gebruiken van een statisch IP-adres buiten de resource groep van het knoop punt](#use-a-static-ip-address-outside-of-the-node-resource-group).

Haal eerst de naam van de resource groep van het knoop punt op met de opdracht [AZ AKS show][az-aks-show] en voeg de `--query nodeResourceGroup` query parameter toe. In het volgende voor beeld wordt de resource groep node opgehaald voor de AKS-cluster naam *myAKSCluster* in de naam van de resource groep *myResourceGroup*:

```azurecli-interactive
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Maak nu een statisch openbaar IP-adres met de opdracht [AZ Network Public IP Create][az-network-public-ip-create] . Geef de naam van de resource groep voor het knoop punt op die u hebt verkregen in de vorige opdracht en klik vervolgens op een naam voor de IP-adres bron, zoals *myAKSPublicIP*:

```azurecli-interactive
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

Het IP-adres wordt weer gegeven, zoals wordt weer gegeven in de volgende verkorte voorbeeld uitvoer:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [...]
  }
}
```

U kunt later het open bare IP-adres ophalen met de opdracht [AZ Network Public-IP List][az-network-public-ip-list] . Geef de naam op van de knooppunt resource groep en het open bare IP-adres dat u hebt gemaakt, en voer een query uit voor het *IP* -adres, zoals weer gegeven in het volgende voor beeld:

```azurecli-interactive
$ az network public-ip show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --query ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Een service maken met behulp van het statische IP-adres

Als u een service met het statische open bare IP-adres wilt `loadBalancerIP` maken, voegt u de eigenschap en de waarde van het statische open bare IP-adres toe aan het yaml-manifest. Maak een bestand met `load-balancer-service.yaml` de naam en kopieer de volgende YAML. Geef uw eigen open bare IP-adres op dat u in de vorige stap hebt gemaakt.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

Maak de service en de implementatie met `kubectl apply` behulp van de opdracht.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="use-a-static-ip-address-outside-of-the-node-resource-group"></a>Een statisch IP-adres buiten de resource groep van het knoop punt gebruiken

Met Kubernetes 1,10 of hoger kunt u een statisch IP-adres gebruiken dat buiten de resource groep van het knoop punt is gemaakt. De service-principal die wordt gebruikt door het AKS-cluster moet gedelegeerde machtigingen hebben voor de andere resource groep, zoals wordt weer gegeven in het volgende voor beeld:

```azurecli-interactive
az role assignment create\
    --assignee <SP Client ID> \
    --role "Network Contributor" \
    --scope /subscriptions/<subscription id>/resourceGroups/<resource group name>
```

Als u een IP-adres buiten de resource groep voor het knoop punt wilt gebruiken, voegt u een aantekening toe aan de service definitie. In het volgende voor beeld wordt de aantekening ingesteld op de resource groep met de naam *myResourceGroup*. Geef de naam van uw eigen resource groep op:

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-resource-group: myResourceGroup
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

## <a name="troubleshoot"></a>Problemen oplossen

Als het statische IP-adres dat is gedefinieerd in de eigenschap *loadBalancerIP* van het Kubernetes-service manifest niet bestaat of niet is gemaakt in de resource groep node en er geen aanvullende delegaties zijn geconfigureerd, mislukt het maken van de Load Balancer-service. Als u problemen wilt oplossen, controleert u de gebeurtenissen voor het maken van een service met de opdracht [kubectl beschrijven][kubectl-describe] . Geef de naam van de service op zoals opgegeven in het YAML-manifest, zoals wordt weer gegeven in het volgende voor beeld:

```console
kubectl describe service azure-load-balancer
```

Informatie over de Kubernetes-service resource wordt weer gegeven. De *gebeurtenissen* aan het einde van de volgende voorbeeld uitvoer geven aan dat het door de *gebruiker opgegeven IP-adres niet is gevonden*. Controleer in deze scenario's of u het statische open bare IP-adres in de knooppunt resource groep hebt gemaakt en of het IP-adres dat is opgegeven in het Kubernetes-service manifest juist is.

```
Name:                     azure-load-balancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-load-balancer
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
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-load-balancer: user supplied IP Address 40.121.183.52 was not found
```

## <a name="next-steps"></a>Volgende stappen

Als u meer controle over het netwerk verkeer voor uw toepassingen wilt, kunt u in plaats daarvan [een ingangs controller maken][aks-ingress-basic]. U kunt ook [een ingangs controller met een statisch openbaar IP-adres maken][aks-static-ingress].

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[aks-ingress-basic]: ingress-basic.md
[aks-static-ingress]: ingress-static-ip.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ip-sku]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md#sku
