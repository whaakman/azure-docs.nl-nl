---
title: Gebruik een statisch IP-adres aan de load balancer van Azure Kubernetes Service (AKS)
description: Informatie over het maken en gebruiken van een statisch IP-adres met de Azure Kubernetes Service (AKS) load balancer.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: f1507bc2aebcd29feea7480761cd1b4949439583
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994484"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Gebruik een statisch openbaar IP-adres aan de load balancer van Azure Kubernetes Service (AKS)

Het openbare IP-adres toegewezen aan een load balancer-resource die zijn gemaakt door een AKS-cluster is standaard alleen geldig voor de levensduur van die resource. Als u de Kubernetes-service verwijdert, worden ook de bijbehorende load balancer en het IP-adres verwijderd. Als u wilt toewijzen van een specifiek IP-adres of een IP-adres voor Kubernetes-services opnieuw geïmplementeerde behouden, kunt u maken en een statisch openbaar IP-adres.

Dit artikel ziet u hoe u een statisch openbaar IP-adres maakt en deze toewijzen aan uw Kubernetes-service.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

Ook moet de Azure CLI-versie 2.0.46 of later zijn geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

Momenteel worden alleen standaard IP-SKU wordt ondersteund. Werk wordt uitgevoerd voor de ondersteuning van standaard IP-adressen.

## <a name="create-a-static-ip-address"></a>Een statisch IP-adres maken

Wanneer u een statisch openbaar IP-adres voor gebruik met AKS maakt, de IP-adresresource moet worden gemaakt in de **knooppunt** resourcegroep. Als u wilt de resources wilt scheiden, Zie [gebruikt een statisch IP-adres buiten de resourcegroep knooppunt](#use-a-static-ip-address-outside-of-the-node-resource-group).

Ophalen van knooppuntnaam van de resourcegroep met de [az aks show] [ az-aks-show] opdracht en voeg de `--query nodeResourceGroup` queryparameter. Het volgende voorbeeld wordt de resourcegroep van het knooppunt voor de naam van het AKS-cluster *myAKSCluster* in naam van de resourcegroep *myResourceGroup*:

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
    [...]
  }
```

U kunt later ophalen voor het openbare IP-adres met de [az network public-IP-lijst] [ az-network-public-ip-list] opdracht. Geef de naam van de resourcegroep knooppunt en het openbare IP-adres die u hebt gemaakt en de query voor de *ipAddress* zoals wordt weergegeven in het volgende voorbeeld:

```azurecli
$ az network public-ip show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --query ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Een service maken met het statische IP-adres

Toevoegen voor het maken van een service met de statische openbare IP-adres, de `loadBalancerIP` eigenschap en de waarde van de statische openbare IP-adres in het manifest YAML. Maak een bestand met de naam `load-balancer-service.yaml` en kopieer de volgende YAML. Geef uw eigen openbare IP-adres in de vorige stap hebt gemaakt.

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

Maken van de service en de implementatie met de `kubectl apply` opdracht.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="use-a-static-ip-address-outside-of-the-node-resource-group"></a>Gebruik een statisch IP-adres buiten de resourcegroep van knooppunt

Met Kubernetes 1.10 of hoger, kunt u een statisch IP-adres dat buiten de resourcegroep van het knooppunt wordt gemaakt. De service-principal die worden gebruikt door het AKS-cluster moet zijn gemachtigd om de andere resourcegroep, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli
az role assignment create\
    --assignee <SP Client ID> \
    --role "Network Contributor" \
    --scope /subscriptions/<subscription id>/resourceGroups/<resource group name>
```

Voor het gebruik van een IP-adres buiten de resourcegroep van het knooppunt, moet u een aantekening toevoegen aan de definitie van de Service. Het volgende voorbeeld wordt de aantekening aan de resourcegroep met de naam *myResourceGroup*. Geef uw eigen Resourcegroepnaam:

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

Als het statische IP-adres die zijn gedefinieerd in de *loadBalancerIP* eigenschap van het Kubernetes-servicemanifest bestaat niet of is niet gemaakt in de resourcegroep van het knooppunt, mislukt het maken van de load balancer-service. Als u wilt oplossen, bekijk de gebeurtenissen van de service maken met de [kubectl beschrijven] [ kubectl-describe] opdracht. Geef de naam van de service die zijn opgegeven in het manifest YAML, zoals wordt weergegeven in het volgende voorbeeld:

```console
kubectl describe service azure-load-balancer
```

Informatie over de resource voor de Kubernetes-service wordt weergegeven. De *gebeurtenissen* aan het einde van de volgende voorbeelduitvoer geven aan dat de *gebruiker opgegeven IP-adres is niet gevonden*. Controleer of dat u de statische openbare IP-adres hebt gemaakt in de resourcegroep van het knooppunt en of het IP-adres dat is opgegeven in het manifest van de Kubernetes-service juist is in deze scenario's.

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
[install-azure-cli]: /cli/azure/install-azure-cli
