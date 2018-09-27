---
title: Gebruik een statisch IP-adres aan de load balancer van Azure Kubernetes Service (AKS)
description: Informatie over het maken en gebruiken van een statisch IP-adres met de Azure Kubernetes Service (AKS) load balancer.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: a7e592e9911c596f2cf74724e73c469ed616e5f0
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391335"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Gebruik een statisch openbaar IP-adres aan de load balancer van Azure Kubernetes Service (AKS)

Het openbare IP-adres toegewezen aan een load balancer-resource die zijn gemaakt door een AKS-cluster is standaard alleen geldig voor de levensduur van die resource. Als u de Kubernetes-service verwijdert, worden ook de bijbehorende load balancer en het IP-adres verwijderd. Als u wilt toewijzen van een specifiek IP-adres of een IP-adres voor Kubernetes-services opnieuw geïmplementeerde behouden, kunt u maken en een statisch openbaar IP-adres.

Dit artikel ziet u hoe u een statisch openbaar IP-adres maakt en deze toewijzen aan uw Kubernetes-service.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

U ook moet de Azure CLI versie 2.0.46 of later geïnstalleerd en geconfigureerd. Voer `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, raadpleegt u [Azure CLI installeren] [install-azure-cli].

## <a name="create-a-static-ip-address"></a>Een statisch IP-adres maken

Wanneer u een statisch openbaar IP-adres voor gebruik met AKS maakt, de IP-adresresource moet worden gemaakt in de **knooppunt** resourcegroep. De naam van de resource met de [az aks show] [ az-aks-show] opdracht en voeg de `--query nodeResourceGroup` queryparameter. Het volgende voorbeeld wordt de resourcegroep van het knooppunt voor de naam van het AKS-cluster *myAKSCluster* in naam van de resourcegroep *myResourceGroup*:

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Maak nu een statisch openbaar IP-adres met de [az network public-ip maken] [ az-network-public-ip-create] opdracht. Geef de naam van de resourcegroep knooppunt verkregen in de vorige opdracht, en vervolgens een naam op voor het IP-adres betrekking op resource, zoals *myAKSPublicIP*:

```azurecli
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

Het IP-adres wordt weergegeven, zoals wordt weergegeven in de volgende verkorte voorbeeld-uitvoer:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [..]
  }
````

U kunt later ophalen voor het openbare IP-adres met de [az network public-IP-lijst] [ az-network-public-ip-list] opdracht. Geef de naam van de resourcegroep van het knooppunt en klik vervolgens op te vragen voor de *ipAddress* zoals wordt weergegeven in het volgende voorbeeld:

```azurecli
$ az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Een service maken met het statische IP-adres

Toevoegen voor het maken van een service met het statische IP-adres, de `loadBalancerIP` eigenschap en de waarde van de statische IP-adres in het manifest YAML, zoals wordt weergegeven in het volgende voorbeeld:

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

## <a name="troubleshoot"></a>Problemen oplossen

Als het statische IP-adres die zijn gedefinieerd in de *loadBalancerIP* eigenschap van het Kubernetes-servicemanifest bestaat niet of is niet gemaakt in de resourcegroep van het knooppunt, mislukt het maken van de load balancer-service. Als u wilt oplossen, bekijk de gebeurtenissen van de service maken met de [kubectl beschrijven] [ kubectl-describe] opdracht. Geef de naam van de service die zijn opgegeven in het manifest YAML, zoals wordt weergegeven in het volgende voorbeeld:

```console
kubectl describe service azure-vote-front
```

Informatie over de resource voor de Kubernetes-service wordt weergegeven. De *gebeurtenissen* aan het einde van de volgende voorbeelduitvoer geven aan dat de *gebruiker opgegeven IP-adres is niet gevonden*. Controleer of dat u de statische openbare IP-adres hebt gemaakt in de resourcegroep van het knooppunt en of het IP-adres dat is opgegeven in het manifest van de Kubernetes-service juist is in deze scenario's.

```
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

## <a name="next-steps"></a>Volgende stappen

Als u meer controle wilt over het netwerkverkeer voor uw toepassingen, kunt u in plaats daarvan [maken van een controller voor binnenkomend verkeer][aks-ingress-basic]. U kunt ook [maken van een controller voor binnenkomend verkeer met een statisch openbaar IP-adres][aks-static-ingress].

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
