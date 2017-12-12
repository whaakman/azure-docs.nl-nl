---
title: Implementeer containers met Helm in Kubernetes op Azure
description: Het Helm verpakking-hulpprogramma gebruiken om de containers op een cluster Kubernetes in AKS implementeren
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 39c6de1ce2443cf027d7cde067281355ea0b7207
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="use-helm-with-azure-container-service-aks"></a>Helm gebruiken met Azure Containerservice (AKS)

[Helm] [ helm] is een open source verpakking hulpprogramma waarmee u kunt installeren en beheren van de levenscyclus van Kubernetes toepassingen. Net als bij Linux pakket managers zoals *APT* en *Yum*, Helm wordt gebruikt voor het beheren van Kubernetes grafieken die pakketten van vooraf geconfigureerde Kubernetes resources zijn.

In dit document wordt stapsgewijs via configureren en gebruiken van Helm in een cluster Kubernetes op AKS.

## <a name="before-you-begin"></a>Voordat u begint

Bij de stappen die in dit document worden uiteengezet, wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en een kubectl-verbinding met het cluster hebt gemaakt. Als u deze items moet weergeven, de [AKS Quick Start][aks-quickstart].

## <a name="install-helm-cli"></a>Helm CLI installeren

De CLI Helm is een client die wordt uitgevoerd op uw ontwikkelsysteem en kunt u starten, stoppen en beheren van toepassingen met Helm grafieken.

Als u Azure CloudShell, worden de CLI Helm is al geïnstalleerd. De CLI Helm installeren op een Mac-gebruik `brew`. Zie voor aanvullende installatie-opties voor [Helm installeren][helm-install-options].

```console
brew install kubernetes-helm
```

Uitvoer:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="configure-helm"></a>Helm configureren

De [helm init] [ helm-init] opdracht Helm onderdelen installeren in een cluster Kubernetes en client-side '-configuraties maken wordt gebruikt. Voer de volgende opdracht Helm op uw cluster AKS installeren en configureren van de client Helm.

```azurecli-interactive
helm init
```

Uitvoer:

```
$HELM_HOME has been configured at /home/user/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.
Happy Helming!
```

## <a name="find-helm-charts"></a>Helm grafieken vinden

Helm grafieken worden gebruikt voor het implementeren van toepassingen in een cluster met Kubernetes. Als u wilt zoeken naar vooraf gemaakte Helm grafieken, gebruiken de [helm zoeken] [ helm-search] opdracht.

```azurecli-interactive
helm search
```

De uitvoer ziet er ongeveer als volgt, maar met veel meer grafieken.

```
NAME                            VERSION DESCRIPTION
stable/acs-engine-autoscaler    2.0.0   Scales worker nodes within agent pools
stable/artifactory              6.1.0   Universal Repository Manager supporting all maj...
stable/aws-cluster-autoscaler   0.3.1   Scales worker nodes within autoscaling groups.
stable/buildkite                0.2.0   Agent for Buildkite
stable/centrifugo               2.0.0   Centrifugo is a real-time messaging server.
stable/chaoskube                0.5.0   Chaoskube periodically kills random pods in you...
stable/chronograf               0.3.0   Open-source web application written in Go and R...
stable/cluster-autoscaler       0.2.0   Scales worker nodes within autoscaling groups.
stable/cockroachdb              0.5.0   CockroachDB is a scalable, survivable, strongly...
stable/concourse                0.7.0   Concourse is a simple and scalable CI system.
stable/consul                   0.4.1   Highly available and distributed service discov...
stable/coredns                  0.5.0   CoreDNS is a DNS server that chains middleware ...
stable/coscale                  0.2.0   CoScale Agent
stable/dask-distributed         2.0.0   Distributed computation in Python
stable/datadog                  0.8.0   DataDog Agent
...
```

Voor het bijwerken van de lijst met grafieken, gebruiken de [helm opslagplaats update] [ helm-repo-update] opdracht.

```azurecli-interactive
helm repo update
```

Uitvoer:

```
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Helm grafieken uitvoeren

Voor het implementeren van een inkomend NGINX controller gebruiken de [helm installeren] [ helm-install] opdracht.

```azurecli-interactive
helm install stable/nginx-ingress
```

De uitvoer ziet er ongeveer als volgt, maar bevat aanvullende informatie zoals instructies over het gebruik van de Kubernetes-implementatie.

```
NAME:   tufted-ocelot
LAST DEPLOYED: Thu Oct  5 00:48:04 2017
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                    DATA  AGE
tufted-ocelot-nginx-ingress-controller  1     5s

==> v1/Service
NAME                                         CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
tufted-ocelot-nginx-ingress-controller       10.0.140.10  <pending>    80:30486/TCP,443:31358/TCP  5s
tufted-ocelot-nginx-ingress-default-backend  10.0.34.132  <none>       80/TCP                      5s

==> v1beta1/Deployment
NAME                                         DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
tufted-ocelot-nginx-ingress-controller       1        1        1           0          5s
tufted-ocelot-nginx-ingress-default-backend  1        1        1           1          5s
...
```

Zie voor meer informatie over het gebruik van een inkomend NGINX controller met Kubernetes [NGINX inkomend Controller][nginx-ingress].

## <a name="list-helm-charts"></a>Lijst Helm grafieken

Als een lijst met grafieken geïnstalleerd op het cluster wilt weergeven, gebruikt de [helm lijst] [ helm-list] opdracht.

```azurecli-interactive
helm list
```

Uitvoer:

```
NAME            REVISION    UPDATED                     STATUS      CHART               NAMESPACE
bilging-ant     1           Thu Oct  5 00:11:11 2017    DEPLOYED    nginx-ingress-0.8.7 default
```

## <a name="next-steps"></a>Volgende stappen

Zie de Helm-documentatie voor meer informatie over het beheren van Kubernetes grafieken.

> [!div class="nextstepaction"]
> [Helm documentatie][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://github.com/kubernetes/helm/blob/master/docs/index.md
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/helm/#helm-install
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md