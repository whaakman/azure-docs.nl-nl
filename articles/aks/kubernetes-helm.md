---
title: Implementeer containers met Helm in Kubernetes op Azure
description: Het Helm verpakking-hulpprogramma gebruiken voor het implementeren van containers in een cluster Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/01/2018
ms.author: iainfou
ms.openlocfilehash: f9f4c1cccac7e40c7d2fd8f76221bc1870ade45f
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52993239"
---
# <a name="install-applications-with-helm-in-azure-kubernetes-service-aks"></a>Installeren van toepassingen met Helm in Azure Kubernetes Service (AKS)

[Helm] [ helm] is een open-source verpakking-hulpprogramma dat helpt u bij het installeren en beheren van de levenscyclus van toepassingen met Kubernetes. Vergelijkbaar met Linux-pakketmanagers zoals *APT* en *Yum*, Helm wordt gebruikt voor het beheren van Kubernetes grafieken die pakketten van vooraf geconfigureerde Kubernetes-resources zijn.

In dit artikel leest u hoe het configureren en gebruiken van Helm in een Kubernetes-cluster in AKS.

## <a name="before-you-begin"></a>Voordat u begint

De stappen die in dit document wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en hebben een `kubectl` verbinding met het cluster. Als u deze items wilt zien, de [Quick Start voor AKS][aks-quickstart].

U moet ook de Helm-CLI is geïnstalleerd, de client die wordt uitgevoerd op uw systeem voor de ontwikkeling en kunt u starten, stoppen en beheren van toepassingen met Helm. Als u de Azure Cloud Shell gebruikt, wordt de Helm-CLI al geïnstalleerd. Voor installatie-instructies op uw lokale platform Zie, [Helm installeren][helm-install].

## <a name="create-a-service-account"></a>Een service-account maken

Voordat u Helm in een cluster AKS RBAC is ingeschakeld implementeren kunt, moet u een service-account en een binding van de rol voor de Tiller-service. Voor meer informatie over het beveiligen van Helm / Tiller in een RBAC cluster ingeschakeld, Zie [Tiller, naamruimten en RBAC][tiller-rbac]. Als uw AKS-cluster niet RBAC is ingeschakeld, moet u deze stap overslaan.

Maak een bestand met de naam `helm-rbac.yaml` en kopieer de volgende YAML:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
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

Maken van de service-account en de rol binding met de `kubectl apply` opdracht:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="secure-tiller-and-helm"></a>Beveiligde Tiller en Helm

De Helm-client en de Tiller service verifiëren en communiceren met elkaar met behulp van TLS/SSL. Deze verificatiemethode zorgt ervoor dat het beveiligen van het Kubernetes-cluster en welke services kan worden geïmplementeerd. U kunt uw eigen zelfondertekende certificaten genereren voor een betere beveiliging. Elke gebruiker Helm ontvangt hun eigen clientcertificaat en Tiller zou worden geïnitialiseerd in het Kubernetes-cluster met certificaten die zijn toegepast. Zie voor meer informatie, [met behulp van TLS/SSL tussen Helm en Tiller][helm-ssl].

U kunt het niveau van toegang die tiller met het cluster heeft beheren met een cluster Kubernetes RBAC-functionaliteit. U kunt de Kubernetes-naamruimte die Tiller wordt geïmplementeerd in definiëren, en beperken welke naamruimten Tiller bronnen in vervolgens kunt implementeren. Deze aanpak kunt u Tiller-exemplaren in verschillende naamruimten en de limiet voor implementatie grenzen maken en het bereik van de gebruikers van de Helm-client bepaalde naamruimten. Zie voor meer informatie, [Helm op rollen gebaseerd toegangsbeheer][helm-rbac].

## <a name="configure-helm"></a>Helm configureren

Voor het implementeren van een eenvoudige Tiller in een AKS-cluster, gebruikt u de [helm init] [ helm-init] opdracht. Als uw cluster niet RBAC ingeschakeld is, verwijdert u de `--service-account` argument en de waarde. Als u TLS/SSL geconfigureerd voor Tiller en Helm, slaat u deze stap basic initialisatie en in plaats daarvan bieden de vereiste `--tiller-tls-` zoals wordt weergegeven in het volgende voorbeeld.

```console
helm init --service-account tiller
```

Als u TLS/SSL tussen Helm en Tiller geconfigureerd bieden de `--tiller-tls-*` parameters en de namen van uw eigen certificaten, zoals wordt weergegeven in het volgende voorbeeld:

```console
helm init \
    --tiller-tls \
    --tiller-tls-cert tiller.cert.pem \
    --tiller-tls-key tiller.key.pem \
    --tiller-tls-verify \
    --tls-ca-cert ca.cert.pem \
    --service-account tiller
```

## <a name="find-helm-charts"></a>Helm-grafieken zoeken

Helm-grafieken worden gebruikt om toepassingen in een Kubernetes-cluster te implementeren. Als u wilt zoeken naar vooraf gemaakte Helm-grafieken, gebruikt u de [helm search] [ helm-search] opdracht:

```console
helm search
```

De uitvoer van de volgende verkorte voorbeeld ziet u enkele van de Helm-grafieken beschikbaar voor gebruik:

```
$ helm search

NAME                           CHART VERSION    APP VERSION  DESCRIPTION
stable/aks-engine-autoscaler   2.2.0            2.1.1        Scales worker nodes within agent pools
stable/aerospike               0.1.7            v3.14.1.2    A Helm chart for Aerospike in Kubernetes
stable/anchore-engine          0.1.7            0.1.10       Anchore container analysis and policy evaluatio...
stable/apm-server              0.1.0            6.2.4        The server receives data from the Elastic APM a...
stable/ark                     1.0.1            0.8.2        A Helm chart for ark
stable/artifactory             7.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/artifactory-ha          0.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/auditbeat               0.1.0            6.2.4        A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler  0.3.3                         Scales worker nodes within autoscaling groups.
stable/bitcoind                0.1.3            0.15.1       Bitcoin is an innovative payment network and a ...
stable/buildkite               0.2.3            3            Agent for Buildkite
stable/burrow                  0.4.4            0.17.1       Burrow is a permissionable smart contract machine
stable/centrifugo              2.0.1            1.7.3        Centrifugo is a real-time messaging server.
stable/cerebro                 0.1.0            0.7.3        A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager            v0.3.3           v0.3.1       A Helm chart for cert-manager
stable/chaoskube               0.7.0            0.8.0        Chaoskube periodically kills random pods in you...
stable/chartmuseum             1.5.0            0.7.0        Helm Chart Repository with support for Amazon S...
stable/chronograf              0.4.5            1.3          Open-source web application written in Go and R...
stable/cluster-autoscaler      0.6.4            1.2.2        Scales worker nodes within autoscaling groups.
stable/cockroachdb             1.1.1            2.0.0        CockroachDB is a scalable, survivable, strongly...
stable/concourse               1.10.1           3.14.1       Concourse is a simple and scalable CI system.
stable/consul                  3.2.0            1.0.0        Highly available and distributed service discov...
stable/coredns                 0.9.0            1.0.6        CoreDNS is a DNS server that chains plugins and...
stable/coscale                 0.2.1            3.9.1        CoScale Agent
stable/dask                    1.0.4            0.17.4       Distributed computation in Python with task sch...
stable/dask-distributed        2.0.2                         DEPRECATED: Distributed computation in Python
stable/datadog                 0.18.0           6.3.0        DataDog Agent
...
```

Voor het bijwerken van de lijst met grafieken, gebruikt u de [helm-opslagplaats update] [ helm-repo-update] opdracht. Het volgende voorbeeld ziet u een update geslaagd opslagplaats:

```console
$ helm repo update

Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Helm-grafieken uitvoeren

Als u wilt installeren grafieken met Helm, gebruikt u de [helm install] [ helm-install] opdracht en geeft u de naam van de grafiek om te installeren. Als u wilt dit in actie zien, moet u gaan we een eenvoudige Wordpress-implementatie met behulp van een Helm-diagram installeren. Als u TLS/SSL is geconfigureerd, voegt u toe de `--tls` parameter om de Helm-clientcertificaat te gebruiken.

```console
helm install stable/wordpress
```

De volgende verkorte voorbeeld-uitvoer toont de status van de implementatie van de Kubernetes-resources die zijn gemaakt door het Helm-diagram:

```
$ helm install stable/wordpress

NAME:   wishful-mastiff
LAST DEPLOYED: Thu Jul 12 15:53:56 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1beta1/Deployment
NAME                       DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
wishful-mastiff-wordpress  1        1        1           0          1s

==> v1beta1/StatefulSet
NAME                     DESIRED  CURRENT  AGE
wishful-mastiff-mariadb  1        1        1s

==> v1/Pod(related)
NAME                                        READY  STATUS   RESTARTS  AGE
wishful-mastiff-wordpress-6f96f8fdf9-q84sz  0/1    Pending  0         1s
wishful-mastiff-mariadb-0                   0/1    Pending  0         1s

==> v1/Secret
NAME                       TYPE    DATA  AGE
wishful-mastiff-mariadb    Opaque  2     2s
wishful-mastiff-wordpress  Opaque  2     2s

==> v1/ConfigMap
NAME                           DATA  AGE
wishful-mastiff-mariadb        1     2s
wishful-mastiff-mariadb-tests  1     2s

==> v1/PersistentVolumeClaim
NAME                       STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
wishful-mastiff-wordpress  Pending  default  2s

==> v1/Service
NAME                       TYPE          CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
wishful-mastiff-mariadb    ClusterIP     10.1.116.54  <none>       3306/TCP                    2s
wishful-mastiff-wordpress  LoadBalancer  10.1.217.64  <pending>    80:31751/TCP,443:31264/TCP  2s
...
```

Het duurt een minuut of twee voor de *externe IP-adres* -adres van de Wordpress-service worden ingevuld en kunt u toegang hebt tot het met een webbrowser.

## <a name="list-helm-releases"></a>Lijst met Helm releases

Een lijst van versies die zijn geïnstalleerd op uw cluster wilt bekijken, gebruikt u de [' helm list '] [ helm-list] opdracht. Het volgende voorbeeld ziet de Wordpress-versie die is geïmplementeerd in de vorige stap. Als u TLS/SSL is geconfigureerd, voegt u toe de `--tls` parameter om de Helm-clientcertificaat te gebruiken.

```console
$ helm list

NAME             REVISION    UPDATED                     STATUS      CHART              NAMESPACE
wishful-mastiff  1           Thu Jul 12 15:53:56 2018    DEPLOYED    wordpress-2.1.3  default
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u een Helm-diagram implementeert, wordt een aantal Kubernetes-resources worden gemaakt. Deze resources bevat schillen, implementaties en services. Voor het opschonen van deze resources, gebruikt u de `helm delete` opdracht en geeft u de naam van de release, zoals gevonden in de vorige `helm list` opdracht. Het volgende voorbeeld wordt de versie met de naam *wishful mastiff*:

```console
$ helm delete wishful-mastiff

release "wishful-mastiff" deleted
```

## <a name="next-steps"></a>Volgende stappen

Zie de Helm-documentatie voor meer informatie over het beheren van implementaties van het Kubernetes-toepassingen met Helm.

> [!div class="nextstepaction"]
> [Helm-documentatie][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac
[helm-ssl]: https://docs.helm.sh/using_helm/#using-ssl-between-helm-and-tiller

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
