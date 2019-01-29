---
title: Statisch IP-adres voor uitgaand verkeer in Azure Kubernetes Service (AKS)
description: Meer informatie over het maken en gebruiken van een statisch openbaar IP-adres voor uitgaande verkeer in een cluster Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: aeffe172fd422f18e2828c5274e9a2ed13cc546a
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103357"
---
# <a name="use-a-static-public-ip-address-for-egress-traffic-in-azure-kubernetes-service-aks"></a>Gebruik een statisch openbaar IP-adres voor uitgaand verkeer in Azure Kubernetes Service (AKS)

De uitgaande IP-adres uit een cluster Azure Kubernetes Service (AKS) is standaard willekeurig toegewezen. Deze configuratie is niet ideaal als u wilt bijvoorbeeld een IP-adres voor toegang tot externe services, identificeren. In plaats daarvan moet u mogelijk een statisch IP-adres dat in de whitelist voor toegang tot de service kan worden toegewezen.

Dit artikel leest u hoe het maken en gebruiken van een statisch openbaar IP-adres voor gebruik met uitgaande verkeer in een AKS-cluster.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

Ook moet de Azure CLI-versie 2.0.46 of later zijn geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u de Azure CLI wilt installeren of upgraden, raadpleegt u  [Azure CLI installeren][install-azure-cli].

## <a name="egress-traffic-overview"></a>Overzicht van uitgaand verkeer

Uitgaand verkeer van een AKS-cluster volgt [Azure Load Balancer conventies][outbound-connections]. Voordat u de eerste Kubernetes-service van het type `LoadBalancer` is gemaakt, de agent knooppunten in een AKS-cluster maken geen deel uit van een Azure Load Balancer-pool. In deze configuratie worden hebben de knooppunten geen instantieniveau openbaar IP-adres. De uitgaande stroom moet een openbaar IP-bronadres die niet kunnen worden geconfigureerd of deterministische wordt omgezet in Azure.

Zodra een Kubernetes-service van het type `LoadBalancer` is gemaakt, agent knooppunten worden toegevoegd aan een groep van Azure Load Balancer. Voor uitgaande stroom, Azure wordt omgezet in het eerste openbare IP-adres geconfigureerd op de load balancer. Dit openbare IP-adres is alleen geldig voor de levensduur van die resource. Als u de service voor Kubernetes LoadBalancer verwijdert, worden ook de bijbehorende load balancer en het IP-adres verwijderd. Als u wilt toewijzen van een specifiek IP-adres of een IP-adres voor Kubernetes-services opnieuw geïmplementeerde behouden, kunt u maken en een statisch openbaar IP-adres.

## <a name="create-a-static-public-ip"></a>Een statisch openbaar IP-adres maken

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
```

U kunt later ophalen voor het openbare IP-adres met de [az network public-IP-lijst] [ az-network-public-ip-list] opdracht. Geef de naam van de resourcegroep van het knooppunt en klik vervolgens op te vragen voor de *ipAddress* zoals wordt weergegeven in het volgende voorbeeld:

```azurecli
$ az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-with-the-static-ip"></a>Een service maken met het statische IP-adres

Toevoegen voor het maken van een service met de statische openbare IP-adres, de `loadBalancerIP` eigenschap en de waarde van de statische openbare IP-adres in het manifest YAML. Maak een bestand met de naam `egress-service.yaml` en kopieer de volgende YAML. Geef uw eigen openbare IP-adres in de vorige stap hebt gemaakt.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-egress
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
```

Maken van de service en de implementatie met de `kubectl apply` opdracht.

```console
kubectl apply -f egress-service.yaml
```

Deze service wordt een nieuwe frontend-IP-adres geconfigureerd op de Azure Load Balancer. Als u nog geen andere IP-adressen geconfigureerd, klikt u vervolgens **alle** uitgaande verkeer moet nu gebruiken voor dit adres. Als meerdere adressen zijn geconfigureerd op de Azure Load Balancer, uitgaand verkeer maakt gebruik van het eerste IP-adres op dat load balancer.

## <a name="verify-egress-address"></a>Adres van uitgaand verkeer controleren

Om te controleren of de statische openbare IP-adres wordt gebruikt, kunt u DNS-resultaat service zoals `checkip.dyndns.org`.

Starten en te koppelen aan een eenvoudige *Debian* pod:

```console
kubectl run -it --rm aks-ip --image=debian
```

Gebruiken voor toegang tot een website vanuit de container, `apt-get` voor het installeren van `curl` naar de container.

```console
apt-get update && apt-get install curl -y
```

Curl nu gebruiken voor toegang tot de *checkip.dyndns.org* site. De uitgaande IP-adres wordt weergegeven, zoals weergegeven in de volgende voorbeelduitvoer. Dit IP-adres overeenkomt met het statische openbare IP-adres hebt gemaakt en gedefinieerd voor de Load Balancer-service:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

## <a name="next-steps"></a>Volgende stappen

U kunt in plaats daarvan een controller voor binnenkomend verkeer om te voorkomen dat meerdere openbare IP-adressen op de Azure Load Balancer onderhouden, gebruiken. Inkomend verkeer controllers biedt extra voordelen, zoals SSL/TLS-beëindiging, ondersteuning voor URI regeneraties en upstream SSL/TLS-versleuteling. Zie voor meer informatie, [een eenvoudige ingangscontroller in AKS maakt][ingress-aks-cluster].

<!-- LINKS - internal -->
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[azure-cli-install]: /cli/azure/install-azure-cli
[ingress-aks-cluster]: ./ingress-basic.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli