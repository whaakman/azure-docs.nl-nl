---
title: Implementeer containers met Helm in Kubernetes op Azure
description: Het Helm verpakking-hulpprogramma gebruiken om de containers op een cluster Kubernetes in AKS implementeren
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 531e6d9368b2bf91c48fd41b1e9330879b0df49a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102284"
---
# <a name="use-helm-with-azure-kubernetes-service-aks"></a>Helm gebruiken met Azure Kubernetes-Service (AKS)

[Helm] [ helm] is een open source verpakking hulpprogramma waarmee u kunt installeren en beheren van de levenscyclus van Kubernetes toepassingen. Net als bij Linux pakket managers zoals *APT* en *Yum*, Helm wordt gebruikt voor het beheren van Kubernetes grafieken die pakketten van vooraf geconfigureerde Kubernetes resources zijn.

In dit document wordt stapsgewijs via configureren en gebruiken van Helm in een cluster Kubernetes op AKS.

## <a name="before-you-begin"></a>Voordat u begint

Bij de stappen die in dit document worden uiteengezet, wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en een kubectl-verbinding met het cluster hebt gemaakt. Als u deze items moet weergeven, de [AKS Quick Start][aks-quickstart].

## <a name="install-helm-cli"></a>Helm CLI installeren

De CLI Helm is een client die wordt uitgevoerd op uw ontwikkelsysteem en kunt u starten, stoppen en beheren van toepassingen met Helm.

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

## <a name="create-service-account"></a>Serviceaccount maken

Voordat Helm configureren in een RBAC cluster is ingeschakeld, moet u een service-account en de binding voor de service helmstok rol. Voor meer informatie over het beveiligen van Helm / helmstok in een RBAC cluster ingeschakeld, Zie [helmstok, naamruimten en RBAC][tiller-rbac]. Opmerking: als uw cluster niet RBAC is ingeschakeld, wordt deze stap overslaan.

Maak een bestand met de naam `helm-rbac.yaml` en kopieer de volgende YAML.

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Maken van de service-account en de rol binding met de `kubectl create` opdracht.

```
kubectl create -f helm-rbac.yaml
```

Wanneer met behulp van een RBAC cluster is ingeschakeld, hebt u de opties op het niveau van toegang die helmstok tot het cluster heeft. Zie [Helm: toegangsbeheer op basis van rollen] [ helm-rbac] voor meer informatie over configuratieopties.

## <a name="configure-helm"></a>Helm configureren

Nu installeren met behulp van helmstok de [helm init] [ helm-init] opdracht. Als uw cluster geen RBAC ingeschakeld is, verwijdert u de `--service-account` argument en waarde.

```
helm init --service-account tiller
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

Als u wilt implementeren met behulp van een grafiek Helm Wordpress, gebruiken de [helm installeren] [ helm-install] opdracht.

```azurecli-interactive
helm install stable/wordpress
```

De uitvoer ziet er ongeveer als volgt, maar bevat aanvullende informatie zoals instructies over het gebruik van de Kubernetes-implementatie.

```
NAME:   bilging-ibex
LAST DEPLOYED: Tue Jun  5 14:31:49 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                     READY  STATUS   RESTARTS  AGE
bilging-ibex-mariadb-7557b5474-dmdxn     0/1    Pending  0         1s
bilging-ibex-wordpress-7494c545fb-tskhz  0/1    Pending  0         1s

==> v1/Secret
NAME                    TYPE    DATA  AGE
bilging-ibex-mariadb    Opaque  2     1s
bilging-ibex-wordpress  Opaque  2     1s

==> v1/ConfigMap
NAME                        DATA  AGE
bilging-ibex-mariadb        1     1s
bilging-ibex-mariadb-tests  1     1s

==> v1/PersistentVolumeClaim
NAME                    STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
bilging-ibex-mariadb    Pending  default  1s
bilging-ibex-wordpress  Pending  default  1s

==> v1/Service
NAME                    TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)                     AGE
bilging-ibex-mariadb    ClusterIP     10.0.76.164   <none>       3306/TCP                    1s
bilging-ibex-wordpress  LoadBalancer  10.0.215.250  <pending>    80:30934/TCP,443:31134/TCP  1s

==> v1beta1/Deployment
NAME                    DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
bilging-ibex-mariadb    1        1        1           0          1s
bilging-ibex-wordpress  1        1        1           0          1s
...
```

## <a name="list-helm-releases"></a>Lijst Helm releases

Als een lijst met versies die zijn geïnstalleerd op het cluster wilt weergeven, gebruikt de [helm lijst] [ helm-list] opdracht.

```azurecli-interactive
helm list
```

Uitvoer:

```
NAME            REVISION    UPDATED                     STATUS      CHART           NAMESPACE
bilging-ibex    1           Tue Jun  5 14:31:49 2018    DEPLOYED    wordpress-1.0.9 default
```

## <a name="next-steps"></a>Volgende stappen

Zie de Helm-documentatie voor meer informatie over het beheren van Kubernetes grafieken.

> [!div class="nextstepaction"]
> [Helm documentatie][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/helm/#helm-install
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
