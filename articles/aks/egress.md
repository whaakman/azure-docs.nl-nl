---
title: Geaccepteerde uitgaande verkeer vanuit Azure Kubernetes Service (AKS)-cluster
description: Geaccepteerde uitgaande verkeer van een cluster met Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/23/2018
ms.author: iainfou
ms.openlocfilehash: 2394b61fb84a2f22af036f35819b87074a1dbd2d
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100142"
---
# <a name="azure-kubernetes-service-aks-egress"></a>Uitgaande van Azure Kubernetes Service (AKS)

Standaard wordt het adres van de uitgaande van een cluster met Azure Kubernetes Service (AKS) willekeurig toegewezen. Deze configuratie is niet ideaal wanneer die behoefte hebben aan een IP-adres voor toegang tot externe services identificeren. Dit document details over het maken en onderhouden van een uitgaande statisch toegewezen IP-adres in een cluster AKS.

## <a name="egress-overview"></a>Uitgaande-overzicht

Uitgaand verkeer van een cluster AKS volgt Azure Load Balancer-conventies, die worden beschreven [hier][outbound-connections]. Voordat de eerste Kubernetes-service van het type `LoadBalancer` is gemaakt, de agent knooppunten maken geen deel uit van alle toepassingen die Azure Load Balancer. In deze configuratie zijn de knooppunten zonder een instantieniveau openbare IP-adres. Azure zet de stroom uitgaande naar een openbare IP-adres die niet kunnen worden geconfigureerd of deterministisch.

Eenmaal een Kubernetes-service van het type `LoadBalancer` is gemaakt, agent knooppunten worden toegevoegd aan een Load Balancer van Azure-toepassingen. Voor uitgaande verkeersstroom omgezet Azure in het eerste openbare IP-adres geconfigureerd op de load balancer.

## <a name="create-a-static-public-ip"></a>Een statisch openbaar IP-adres maken

Maken van een statisch IP-adres wilt willekeurige IP-adressen voorkomen dat wordt gebruikt, en zorg ervoor dat de load balancer gebruikmaakt van dit adres. Het IP-adres moet worden gemaakt in de AKS **knooppunt** resourcegroep.

Ophalen van de naam van de resourcegroep met de [az resource weergeven] [ az-resource-show] opdracht. De naam van een resourcegroep en de naam is afgestemd op uw omgeving bijwerken.

```
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Gebruik vervolgens de [az netwerk openbare ip-maken] [ public-ip-create] opdracht voor het maken van een statische openbare IP-adres. Werk de naam van de resourcegroep overeenkomen met de naam gatherred in de vorige stap.

```console
$ az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static --query publicIp.ipAddress -o table

Result
-------------
23.101.128.81
```

## <a name="create-a-service-with-the-static-ip"></a>Maken van een service met het statische IP-adres

Nu dat u een IP-adres hebt, een service Kubernetes maken met het type `LoadBalancer` en het IP-adres toewijzen aan de service.

Maak een bestand met de naam `egress-service.yaml` en kopieer de volgende YAML. Het IP-adres voor uw omgeving bijwerken.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-egress
spec:
  loadBalancerIP: 23.101.128.81
  type: LoadBalancer
  ports:
  - port: 8080
```

De service en de implementatie met de `kubectl apply` opdracht.

```console
$ kubectl apply -f egress-service.yaml

service "aks-egress" created
```

Maken van deze service configureert een nieuwe frontend-IP-op de Azure Load Balancer. Als u nog geen andere IP-adressen is geconfigureerd, klikt u vervolgens **alle** uitgaande verkeer moet nu dit adres gebruiken. Als meerdere adressen zijn geconfigureerd op de Azure Load Balancer, uitgaande maakt gebruik van het eerste IP-adres op dat load balancer.

## <a name="verify-egress-address"></a>Controleer of uitgaande adres

Om te verifiëren dat het openbare IP-adres wordt gebruikt, gebruikt u een service, zoals `checkip.dyndns.org`.

Start en koppelen aan een schil:

```console
$ kubectl run -it --rm aks-ip --image=debian
```

Installeer zo nodig curl in de container:

```console
$ apt-get update && apt-get install curl -y
```

CURL `checkip.dyndns.org`, dat resulteert in het uitgaande IP-adres:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

U ziet dat het IP-adres overeenkomt met het statische IP-adres dat is gekoppeld aan de Azure load balancer.

## <a name="ingress-controller"></a>Ingress-controller

Om te voorkomen dat meerdere openbare IP-adressen op de Azure Load Balancer behouden, kunt u overwegen een domeincontroller inkomend. Ingress-controllers bieden voordelen zoals taakverdeling, SSL/TLS-beëindiging, ondersteuning voor URI herschreven en upstream SSL/TLS-versleuteling. Zie voor meer informatie over ingress-domeincontrollers in AKS de [NGINX configureren inkomend domeincontroller in een cluster AKS] [ ingress-aks-cluster] handleiding.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de software die in dit document wordt gedemonstreerd.

- [Helm CLI][helm-cli-install]
- [NGINX ingress-controller][nginx-ingress]
- [Azure Load Balancer uitgaande verbindingen][outbound-connections]

<!-- LINKS - internal -->
[az-resource-show]: /cli/azure/resource#az-resource-show
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[create-aks-cluster]: ./kubernetes-walkthrough.md
[helm-cli-install]: ./kubernetes-helm.md#install-helm-cli
[ingress-aks-cluster]: ./ingress.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create

<!-- LINKS - external -->
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
