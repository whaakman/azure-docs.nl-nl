---
title: CoreDNS aanpassen voor Azure Kubernetes Service (AKS)
description: Informatie over het aanpassen van CoreDNS subdomeinen toevoegen of uitbreiden met aangepaste DNS-eindpunten met behulp van Azure Kubernetes Service (AKS)
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 03/15/2019
ms.author: jenoller
ms.openlocfilehash: 247665f58dd064565f0e9aebc9859e97ce0ab0c0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836976"
---
# <a name="customize-coredns-with-azure-kubernetes-service"></a>CoreDNS met Azure Kubernetes Service aanpassen

Azure Kubernetes Service (AKS) maakt gebruik van de [CoreDNS][coredns] project voor het cluster DNS-beheer en de resolutie met alle *1.12.x* en hogere clusters. Voorheen was is het kube-DNS-project gebruikt. Dit project kube-dns is nu verouderd. Zie voor meer informatie over CoreDNS aanpassing en Kubernetes, de [officiële documentatie van de upstream][corednsk8s].

Als u AKS is een beheerde service, u kunt de configuratie van de belangrijkste voor CoreDNS niet wijzigen (een *CoreFile*). In plaats daarvan het gebruik van een Kubernetes *ConfigMap* om de standaardinstellingen te overschrijven. Als de standaard AKS CoreDNS ConfigMaps wilt weergeven, gebruikt de `kubectl get configmaps coredns -o yaml` opdracht.

In dit artikel wordt beschreven hoe u met ConfigMaps voor basic aanpassingsopties van CoreDNS in AKS.

> [!NOTE]
> `kube-dns` verschillende aangeboden [aanpassingsopties][kubednsblog] via een Kubernetes-config-kaart. CoreDNS is **niet** achterwaarts compatibel met kube-dns. Alle aanpassingen die u eerder hebt gebruikt, moeten worden bijgewerkt voor gebruik met CoreDNS.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

## <a name="what-is-supportedunsupported"></a>Wat is er ondersteund/niet-ondersteunde

Alle ingebouwde CoreDNS invoegtoepassingen worden ondersteund. Er is geen Plug-ins voor de invoegtoepassing op/derde partij worden ondersteund. 

## <a name="rewrite-dns"></a>Herschrijf de DNS

Een scenario hebt u is om uit te voeren op dynamische DNS-naam regeneraties. Vervang in het volgende voorbeeld wordt `<domain to be written>` met uw eigen volledig gekwalificeerde domeinnaam. Maak een bestand met de naam `corednsms.yaml` en plak het volgende voorbeeldconfiguratie:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    <domain to be rewritten>.com:53 {
        errors
        cache 30
        rewrite name substring <domain to be rewritten>.com default.svc.cluster.local
        proxy .  /etc/resolv.conf # you can redirect this to a specific DNS server such as 10.0.0.10
    }
```

Maakt de ConfigMap via de [kubectl toepassen configmap][kubectl-apply] opdracht en geeft u de naam van uw YAML-manifest:

```console
kubectl apply -f corednsms.yaml
```

Om te controleren of de aanpassingen zijn toegepast, gebruiken de [kubectl ophalen configmaps][kubectl-get] en geef uw *coredns aangepaste* ConfigMap:

```
kubectl get configmaps --namespace=kube-system coredns-custom -o yaml
```

Nu CoreDNS laden van de ConfigMap forceren. De [kubectl verwijderen pod][kubectl delete] opdracht is niet schadelijke en niet leidt tot uitvaltijd. De `kube-dns` schillen zijn verwijderd, en ze weer in het Kubernetes-Scheduler worden gemaakt. Deze nieuwe pods bevatten de wijziging in de TTL-waarde.

```console
kubectl delete pod --namespace kube-system -l k8s-app=kube-dns
```

> [!Note]
> De bovenstaande opdracht is juist. Terwijl we wijzigt `coredns`, de implementatie is onder de **kube-DNS-** naam.

## <a name="custom-proxy-server"></a>Aangepaste proxy-server

Als u nodig hebt om op te geven van een proxyserver voor uw verkeer, kunt u een ConfigMap voor het aanpassen van DNS maken. Werk in het volgende voorbeeld wordt de `proxy` naam en adres met de waarden voor uw eigen omgeving. Maak een bestand met de naam `corednsms.yaml` en plak het volgende voorbeeldconfiguratie:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    <domain to be rewritten>.com:53 {
        proxy foo.com 1.1.1.1
    }
```

Zoals in de eerdere voorbeelden, maakt de ConfigMap via de [kubectl toepassen configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] voor de Kubernetes Scheduler deze opnieuw maken:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="use-custom-domains"></a>Aangepaste domeinen gebruiken

U wilt configureren van aangepaste domeinen dat alleen intern kunnen worden omgezet. Bijvoorbeeld, u wilt mogelijk kunt oplossen door het aangepaste domein *puglife.local*, wat niet een geldig domein op het hoogste niveau. Het AKS-cluster kan zonder een aangepast domein ConfigMap, het adres niet omzetten.

In het volgende voorbeeld wordt het aangepaste domein- en IP-adres naar direct verkeer naar door de waarden voor uw eigen omgeving bijgewerkt. Maak een bestand met de naam `corednsms.yaml` en plak het volgende voorbeeldconfiguratie:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  puglife.server: |
    puglife.local:53 {
        errors
        cache 30
        proxy . 192.11.0.1  # this is my test/dev DNS server
    }
```

Zoals in de eerdere voorbeelden, maakt de ConfigMap via de [kubectl toepassen configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] voor de Kubernetes Scheduler deze opnieuw maken:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="stub-domains"></a>Stub-domeinen

CoreDNS kan ook worden gebruikt om stub-domeinen te configureren. In het volgende voorbeeld, moet u de aangepaste domeinen en IP-adressen bijwerken door de waarden voor uw eigen omgeving. Maak een bestand met de naam `corednsms.yaml` en plak het volgende voorbeeldconfiguratie:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    abc.com:53 {
        errors
        cache 30
        proxy . 1.2.3.4
    }
    my.cluster.local:53 {
        errors
        cache 30
        proxy . 2.3.4.5
    }

```

Zoals in de eerdere voorbeelden, maakt de ConfigMap via de [kubectl toepassen configmap][kubectl-apply] command and specify the name of your YAML manifest. Then, force CoreDNS to reload the ConfigMap using the [kubectl delete pod][kubectl delete] voor de Kubernetes Scheduler deze opnieuw maken:

```console
kubectl apply -f corednsms.yaml
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="hosts-plugin"></a>Hosts-invoegtoepassing

Als alle ingebouwde invoegtoepassingen worden ondersteund. Dit betekent dat de CoreDNS [Hosts][coredns hosts] -invoegtoepassing is beschikbaar om aan te passen ook:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom # this is the name of the configmap you can overwrite with your changes
  namespace: kube-system
data:
    test.override: |
          hosts example.hosts example.org { # example.hosts must be a file
              10.0.0.1 example.org
              fallthrough
          }
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u enkele voorbeeldscenario voor aanpassing CoreDNS geleerd. Zie voor meer informatie op het project CoreDNS [de CoreDNS upstream-projectpagina][coredns].

Zie voor meer informatie over de basisconcepten van het netwerk, [concepten voor toepassingen in AKS netwerk][concepts-network].

<!-- LINKS - external -->
[kubednsblog]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[coredns]: https://coredns.io/
[corednsk8s]: https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns
[dnscache]: https://coredns.io/plugins/cache/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[coredns hosts]: https://coredns.io/plugins/hosts/

<!-- LINKS - internal -->
[concepts-network]: concepts-network.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
