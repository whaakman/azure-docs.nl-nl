---
title: Een controller voor binnenkomend verkeer voor een intern netwerk maken in Azure Kubernetes Service (AKS)
description: Informatie over het installeren en configureren van een NGINX-controller voor binnenkomend verkeer voor een interne, particuliere netwerk in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: bd3009a5abf6bd34be0e5dd45abfac628ae8a4c8
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093578"
---
# <a name="create-an-ingress-controller-to-an-internal-virtual-network-in-azure-kubernetes-service-aks"></a>Een controller voor binnenkomend verkeer naar een intern virtueel netwerk maken in Azure Kubernetes Service (AKS)

Een controller voor binnenkomend verkeer is een stukje software die voorziet in omgekeerde proxy, configureerbare verkeersroutering en TLS-beëindiging voor Kubernetes-services. Kubernetes-resources voor inkomend verkeer worden gebruikt voor het configureren van de ingress regels en routes voor afzonderlijke Kubernetes-services. Met behulp van een controller voor binnenkomend verkeer en ingress regels, kan één IP-adres worden gebruikt voor het routeren van verkeer naar meerdere services in een Kubernetes-cluster.

In dit artikel leest u over het implementeren van de [controller voor binnenkomend verkeer van NGINX] [ nginx-ingress] in een cluster Azure Kubernetes Service (AKS). De controller voor binnenkomend verkeer is geconfigureerd op een interne, privé-netwerk en IP-adres. Er zijn geen externe toegang is toegestaan. Twee toepassingen worden vervolgens uitgevoerd in de AKS-cluster, die toegankelijk via de één IP-adres is.

U kunt ook het volgende doen:

- [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van uw eigen TLS-certificaten maken][aks-ingress-own-tls]
- Een controller voor binnenkomend verkeer die gebruikmaakt van we gaan coderen voor het automatisch genereren van TLS-certificaten maken [met een dynamisch openbaar IP-adres] [ aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel maakt gebruik van Helm om de NGINX-controller voor binnenkomend verkeer, certificaat-manager en een voorbeeld-web-app te installeren. U moet hebben Helm geïnitialiseerd in uw AKS-cluster en het gebruik van een service-account voor Tiller. Zie voor meer informatie over het configureren en het gebruik van Helm [installeren van toepassingen met Helm in Azure Kubernetes Service (AKS)][use-helm].

In dit artikel is ook vereist dat u de Azure CLI versie 2.0.41 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Maken van een controller voor binnenkomend verkeer

Een NGINX-controller voor binnenkomend verkeer wordt standaard gemaakt met de toewijzing van een dynamische openbare IP-adres. Een algemene configuratievereiste is het gebruik van een privé-interne netwerken en IP-adres. Deze aanpak kunt u toegang tot uw services voor interne gebruikers, zonder externe toegang te beperken.

Maak een bestand met de naam *interne ingress.yaml* met behulp van het volgende voorbeeld-manifestbestand. In dit voorbeeld wijst *10.240.0.42* naar de *loadBalancerIP* resource. Uw eigen interne IP-adres opgeven voor gebruik met de controller voor binnenkomend verkeer. Zorg ervoor dat dit IP-adres niet al wordt gebruikt binnen uw virtuele netwerk.

```yaml
controller:
  service:
    loadBalancerIP: 10.240.0.42
    annotations:
      service.beta.kubernetes.io/azure-load-balancer-internal: "true"
```

Nu implementeren de *nginx-inkomend* grafiek met Helm. Toevoegen voor het gebruik van het manifestbestand in de vorige stap hebt gemaakt, de `-f internal-ingress.yaml` parameter. Voor extra redundantie, twee replica's van de controllers van NGINX inkomend verkeer zijn geïmplementeerd met de `--set controller.replicaCount` parameter. Om volledig te profiteren van het uitvoeren van replica's van de controller voor binnenkomend verkeer, zorg ervoor dat er meer dan één knooppunt in uw AKS-cluster.

> [!TIP]
> Het volgende voorbeeld installeert de controller voor binnenkomend verkeer in de `kube-system` naamruimte. Indien gewenst, kunt u een andere naamruimte voor uw eigen omgeving. Als uw AKS-cluster niet RBAC ingeschakeld is, voegt u toe `--set rbac.create=false` voor de opdrachten.

```console
helm install stable/nginx-ingress \
    --namespace kube-system \
    -f internal-ingress.yaml \
    --set controller.replicaCount=2
```

Wanneer de load balancer-service van Kubernetes voor de controller voor binnenkomend verkeer van NGINX wordt gemaakt, wordt uw interne IP-adres toegewezen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                              TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
alternating-coral-nginx-ingress-controller        LoadBalancer   10.0.97.109   10.240.0.42   80:31507/TCP,443:30707/TCP   1m
alternating-coral-nginx-ingress-default-backend   ClusterIP      10.0.134.66   <none>        80/TCP                       1m
```

Geen binnenkomende regels zijn nog, gemaakt, zodat de NGINX-ingangscontroller standaard 404-pagina wordt weergegeven als u naar het interne IP-adres bladeren. Ingress regels zijn geconfigureerd in de volgende stappen uit.

## <a name="run-demo-applications"></a>Demotoepassingen uitvoeren

Als u wilt de controller voor binnenkomend verkeer in actie zien, moet u gaan we twee demotoepassingen uitvoeren in uw AKS-cluster. In dit voorbeeld Helm gebruikt om twee instanties van een eenvoudige 'Hallo wereld'-toepassing implementeren.

Voordat u het voorbeeld Helm-grafieken installeren kunt, de Azure-voorbeelden opslagplaats toevoegen aan uw omgeving Helm als volgt:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

De eerste demo-toepassing maken van een Helm-diagram met de volgende opdracht:

```console
helm install azure-samples/aks-helloworld
```

Nu een tweede exemplaar van de demo-toepassing installeren. Voor de tweede instantie geeft u een nieuwe titel zodat de twee toepassingen, visueel verschilt. U ook opgeven een unieke naam:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Een binnenkomende route maken

Beide toepassingen worden nu op uw Kubernetes-cluster uitgevoerd. Om verkeer te routeren naar elke toepassing, een Kubernetes-ingress-resource te maken. De resource met inkomend verkeer configureert u de regels die verkeer naar een van de twee toepassingen routeren.

In het volgende voorbeeld wordt het verkeer naar het adres `http://10.240.0.42/` wordt doorgestuurd naar de service met de naam `aks-helloworld`. Verkeer naar het adres `http://10.240.0.42/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service.

Maak een bestand met de naam `hello-world-ingress.yaml` en kopieer het volgende voorbeeld YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Maakt de ingress resource via de `kubectl apply -f hello-world-ingress.yaml` opdracht.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Testen van de controller voor binnenkomend verkeer

Als u wilt testen van de routes voor de controller voor binnenkomend verkeer, blader naar de twee toepassingen met een webclient. Indien nodig, kunt u deze functionaliteit alleen in een schil op het AKS-cluster snel testen. Een test-schil maken en koppelen van een terminalsessie toe:

```console
kubectl run -it --rm aks-ingress-test --image=debian
```

Installeer `curl` in de schil met `apt-get`:

```console
apt-get update && apt-get install -y curl
```

Nu toegang tot het adres van uw Kubernetes inkomend controller met `curl`, zoals *http://10.240.0.42*. Geef dat uw eigen interne IP-adres opgegeven bij de implementatie van de controller voor binnenkomend verkeer in de eerste stap van dit artikel.

```console
curl -L http://10.240.0.42
```

Er is geen aanvullende pad is opgegeven met het adres, dus de controller voor binnenkomend verkeer standaard ingesteld op de */* route. De eerste demo-toepassing wordt geretourneerd, zoals wordt weergegeven in de volgende verkorte voorbeeld-uitvoer:

```
$ curl -L 10.240.0.42

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

Voeg nu */hello-world-two* pad naar het adres, zoals *http://10.240.0.42/hello-world-two*. De tweede demo-toepassing met de aangepaste titel wordt geretourneerd, zoals wordt weergegeven in de volgende verkorte voorbeeld-uitvoer:

```
$ curl -L -k http://10.240.0.42/hello-world-two

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>AKS Ingress Demo</title>
[...]
```

## <a name="clean-up-resources"></a>Resources opschonen

In dit artikel gebruikt Helm om de onderdelen van inkomend verkeer en voorbeeld-apps te installeren. Wanneer u een Helm-diagram implementeert, wordt een aantal Kubernetes-resources worden gemaakt. Deze resources bevat schillen, implementaties en services. Om op te schonen deze resources, moet u eerst de Helm-versies met lijst de `helm list` opdracht. Zoeken naar grafieken met de naam *nginx-inkomend* en *aks-helloworld*, zoals weergegeven in de volgende voorbeelduitvoer:

```
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
kissing-ferret      1           Tue Oct 16 17:13:39 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
intended-lemur      1           Tue Oct 16 17:20:59 2018    DEPLOYED    aks-helloworld-0.1.0                default
pioneering-wombat   1           Tue Oct 16 17:21:05 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Verwijderen van de versies met de `helm delete` opdracht. Het volgende voorbeeld wordt de implementatie van NGINX inkomend verkeer en de twee voorbeeld AKS hello world-apps.

```
$ helm delete kissing-ferret intended-lemur pioneering-wombat

release "kissing-ferret" deleted
release "intended-lemur" deleted
release "pioneering-wombat" deleted
```

Verwijder vervolgens de Helm-opslagplaats voor de AKS hello world-app:

```console
helm repo remove azure-samples
```

Ten slotte verwijdert u de route voor inkomend verkeer die doorgestuurd verkeer naar de voorbeeld-apps:

```console
kubectl delete -f hello-world-ingress.yaml
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel bevat enkele externe onderdelen over AKS. Zie voor meer informatie over deze onderdelen, de volgende project-pagina's:

- [Helm CLI][helm-cli]
- [NGINX-controller voor binnenkomend verkeer][nginx-ingress]

U kunt ook het volgende doen:

- [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Maken van een controller voor binnenkomend verkeer met een dynamische openbare IP-adres en we gaan coderen voor het automatisch genereren van TLS-certificaten configureren][aks-ingress-tls]
- [Maken van een controller voor binnenkomend verkeer met een statisch openbaar IP-adres en we gaan coderen voor het automatisch genereren van TLS-certificaten configureren][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
