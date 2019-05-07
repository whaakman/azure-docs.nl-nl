---
title: Controleer uw Kubernetes-implementaties op Azure voor de implementatie van best practices
description: Meer informatie over het controleren op implementatie van best practices in uw implementaties in Azure Kubernetes Service met behulp van kube-advisor
services: container-service
author: seanmck
ms.service: container-service
ms.topic: troubleshooting
ms.date: 11/05/2018
ms.author: seanmck
ms.openlocfilehash: 03c5eb2e32a0a8ec51844511276d9efba5651068
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65073763"
---
# <a name="checking-for-kubernetes-best-practices-in-your-cluster"></a>Controleren op aanbevolen procedures voor Kubernetes in uw cluster

Er zijn enkele aanbevolen procedures die u voor uw Kubernetes-implementaties volgen moet om te controleren of de beste prestaties en flexibiliteit voor uw toepassingen. U kunt het hulpprogramma kube-advisor gebruiken om te zoeken voor implementaties die niet zijn deze suggesties te volgen.

## <a name="about-kube-advisor"></a>Over kube-advisor

De [kube-advieshulpprogramma] [ kube-advisor-github] is van een enkele container ontworpen om te worden uitgevoerd op uw cluster. Deze query's van de Kubernetes API-server voor informatie over uw implementaties en retourneert een set met voorgestelde verbeteringen.

Het hulpprogramma kube-advisor kunt rapporteren resourceaanvraag en limieten ontbreekt in de PodSpecs voor Windows-toepassingen, evenals de Linux-toepassingen, maar het hulpprogramma kube-advisor zelf moet worden gepland op een Linux-schil. U kunt plannen dat een schil om uit te voeren op een knooppunt van toepassingen met een specifiek besturingssysteem met een [knooppunt selector] [ k8s-node-selector] in de configuratie van de schil.

> [!NOTE]
> Het hulpprogramma kube-advisor wordt ondersteund door Microsoft op basis van best-effort. Problemen en suggesties moeten worden opgeslagen op GitHub.

## <a name="running-kube-advisor"></a>Actieve kube-advisor

Het hulpprogramma uitvoert op een cluster dat is geconfigureerd voor [op rollen gebaseerd toegangsbeheer (RBAC)](azure-ad-integration.md), met de volgende opdrachten. De eerste opdracht maakt een Kubernetes-service-account. De tweede opdracht wordt het hulpprogramma wordt uitgevoerd in een schil met behulp van het serviceaccount en configureert u de schil voor verwijdering nadat deze is afgesloten. 

```bash
kubectl apply -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml

kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never --overrides="{ \"apiVersion\": \"v1\", \"spec\": { \"serviceAccountName\": \"kube-advisor\" } }"
```

Als u RBAC niet gebruikt, kunt u de opdracht als volgt uitvoeren:

```bash
kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never
```

Binnen een paar seconden ziet u een tabel met een beschrijving van potentiële verbeteringen aan uw implementaties.

![Kube-advisor-uitvoer](media/kube-advisor-tool/kube-advisor-output.png)

## <a name="checks-performed"></a>Controles uitgevoerd

Het hulpprogramma controleert de verschillende Kubernetes aanbevolen procedures, elk met hun eigen voorgestelde oplossing.

### <a name="resource-requests-and-limits"></a>Resourceaanvragen en -limieten

Kubernetes biedt ondersteuning voor definiëren [resource aanvraagt en beperkt op pod-specificaties][kube-cpumem]. De aanvraag definieert de minimale CPU en geheugen die nodig is om uit te voeren van de container. De limiet wordt gedefinieerd voor het maximale CPU en geheugen die moet worden toegestaan.

Er zijn geen aanvragen of limieten worden standaard ingesteld op pod-specificaties. Dit kan leiden tot knooppunten wordt overscheduled en wordt pas over containers. Het hulpprogramma kube-advisor schillen zonder aanvragen gemarkeerd en limieten instellen.

## <a name="cleaning-up"></a>Opschonen

Als uw cluster RBAC is ingeschakeld heeft, kunt u de migratiegegevens van de `ClusterRoleBinding` nadat u het hulpprogramma met de volgende opdracht hebt uitgevoerd:

```bash
kubectl delete -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml
```

Als u het hulpprogramma voor een cluster waarvoor geen RBAC is ingeschakeld uitvoert, is er geen opschoning is vereist.

## <a name="next-steps"></a>Volgende stappen

- [Problemen oplossen met Azure Kubernetes Service](troubleshooting.md)

<!-- RESOURCES -->

[kube-cpumem]: https://github.com/Azure/azure-quickstart-templates
[kube-advisor-github]: https://github.com/azure/kube-advisor
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors