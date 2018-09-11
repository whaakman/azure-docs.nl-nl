---
title: Lijst met toegestane adressen uitgaande verkeer vanuit het cluster Azure Kubernetes Service (AKS)
description: Lijst met toegestane adressen uitgaande verkeer vanuit een cluster Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/23/2018
ms.author: iainfou
ms.openlocfilehash: e2793a72fcbc20b79bdd564e331426fedf1ae34b
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347797"
---
# <a name="azure-kubernetes-service-aks-egress"></a>Azure Kubernetes Service (AKS) uitgaand verkeer

Adres van uitgaand verkeer van een cluster Azure Kubernetes Service (AKS) is standaard willekeurig toegewezen. Deze configuratie is niet ideaal wanneer u een IP-adres voor toegang tot externe services identificeren. Dit document wordt uitgelegd hoe maken en onderhouden van een statisch toegewezen uitgaande IP-adres in een AKS-cluster.

## <a name="egress-overview"></a>Overzicht van uitgaand verkeer

Uitgaand verkeer van een AKS-cluster Azure Load Balancer-conventies, die worden beschreven volgt [hier][outbound-connections]. Voordat u de eerste Kubernetes-service van het type `LoadBalancer` is gemaakt, de agent knooppunten hebben geen deel uitmaken van een Azure Load Balancer-pool. In deze configuratie worden zijn de knooppunten zonder een instantieniveau openbaar IP-adres. De uitgaande stroom moet een openbaar IP-bronadres die niet kunnen worden geconfigureerd of deterministische wordt omgezet in Azure.

Zodra een Kubernetes-service van het type `LoadBalancer` is gemaakt, agent knooppunten worden toegevoegd aan een groep van Azure Load Balancer. Voor uitgaande stroom, Azure wordt omgezet in het eerste openbare IP-adres geconfigureerd op de load balancer.

## <a name="create-a-static-public-ip"></a>Een statisch openbaar IP-adres maken

Om te voorkomen dat willekeurige IP-adressen worden gebruikt, maakt u een statisch IP-adres en controleer of dat de load balancer gebruikt dit adres. Het IP-adres moet worden gemaakt in de AKS **knooppunt** resourcegroep.

De naam van de resource met de [az resource show] [ az-resource-show] opdracht. De naam van de resourcegroep en de naam van het cluster zodat deze overeenkomen met uw omgeving bijwerken.

```
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Gebruik vervolgens de [az network public-ip maken] [ public-ip-create] opdracht voor het maken van een statisch openbaar IP-adres. Werk de naam van de resourcegroep zodat deze overeenkomen met de naam gatherred in de vorige stap.

```console
$ az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static --query publicIp.ipAddress -o table

Result
-------------
23.101.128.81
```

## <a name="create-a-service-with-the-static-ip"></a>Een service maken met het statische IP-adres

Nu dat u een IP-adres hebt, een Kubernetes-service maken met het type `LoadBalancer` en het IP-adres toewijzen aan de service.

Maak een bestand met de naam `egress-service.yaml` en kopieer de volgende YAML. Het IP-adres zodat deze overeenkomt met uw omgeving bijwerken.

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

Maken van de service en de implementatie met de `kubectl apply` opdracht.

```console
$ kubectl apply -f egress-service.yaml

service "aks-egress" created
```

Het maken van deze service, configureert een nieuwe frontend-IP-adres op de Azure Load Balancer. Als u nog geen andere IP-adressen geconfigureerd, klikt u vervolgens **alle** uitgaande verkeer moet nu gebruiken voor dit adres. Als meerdere adressen zijn geconfigureerd op de Azure Load Balancer, uitgaand verkeer maakt gebruik van het eerste IP-adres op dat load balancer.

## <a name="verify-egress-address"></a>Adres van uitgaand verkeer controleren

Om te controleren dat het openbare IP-adres wordt gebruikt, gebruikt u een service, zoals `checkip.dyndns.org`.

Start en koppelen aan een schil:

```console
$ kubectl run -it --rm aks-ip --image=debian
```

Installeer, indien nodig curl in de container:

```console
$ apt-get update && apt-get install curl -y
```

CURL `checkip.dyndns.org`, die wordt geretourneerd met de uitgaande IP-adres:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

U ziet dat het IP-adres overeenkomt met het statische IP-adres dat is gekoppeld aan de Azure load balancer.

## <a name="ingress-controller"></a>Controller voor binnenkomend verkeer

Om te voorkomen met het beheren van meerdere openbare IP-adressen op de Azure Load Balancer, kunt u overwegen een controller voor binnenkomend verkeer. Ingress-controllers bieden voordelen, zoals taakverdeling, SSL/TLS-beëindiging, ondersteuning voor URI regeneraties en upstream SSL/TLS-versleuteling. Zie voor meer informatie over ingress-domeincontrollers in AKS de [ingangscontroller NGINX configureren in een AKS-cluster] [ ingress-aks-cluster] handleiding.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de software die in dit document wordt gedemonstreerd.

- [Helm CLI][helm-cli-install]
- [NGINX-controller voor binnenkomend verkeer][nginx-ingress]
- [Uitgaande verbindingen van Azure Load Balancer][outbound-connections]

<!-- LINKS - internal -->
[az-resource-show]: /cli/azure/resource#az-resource-show
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[create-aks-cluster]: ./kubernetes-walkthrough.md
[helm-cli-install]: ./kubernetes-helm.md#install-helm-cli
[ingress-aks-cluster]: ./ingress-basic.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create

<!-- LINKS - external -->
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
