---
title: Een controller voor binnenkomend verkeer voor een intern netwerk maken in Azure Kubernetes Service (AKS)
description: Informatie over het installeren en configureren van een NGINX-controller voor binnenkomend verkeer voor een interne, particuliere netwerk in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: iainfou
ms.openlocfilehash: f3986b68242d580d9a6bd0e0cc38ce2c9d3aeeb5
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430963"
---
# <a name="create-an-ingress-controller-to-an-internal-virtual-network-in-azure-kubernetes-service-aks"></a>Een controller voor binnenkomend verkeer naar een intern virtueel netwerk maken in Azure Kubernetes Service (AKS)

Een controller voor inkomend verkeer is een stukje software dat omgekeerde proxy’s, configureerbare verkeersroutering en TLS-beëindiging voor Kubernetes-services biedt. Kubernetes-resources voor inkomend verkeer worden gebruikt om de regels en routes voor uitgaand verkeer worden geconfigureerd voor individuele Kubernetes-services. Met behulp van een controller en regels voor inkomend verkeer kan er één enkel IP-adres worden gebruikt voor het routeren van verkeer naar meerdere services in een Kubernetes-cluster.

In dit artikel leest u over het implementeren van de [controller voor binnenkomend verkeer van NGINX] [ nginx-ingress] in een cluster Azure Kubernetes Service (AKS). De controller voor binnenkomend verkeer is geconfigureerd op een interne, privé-netwerk en IP-adres. Er zijn geen externe toegang is toegestaan. Twee toepassingen worden vervolgens uitgevoerd in de AKS-cluster, die toegankelijk via de één IP-adres is.

U kunt ook het volgende doen:

- [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van uw eigen TLS-certificaten maken][aks-ingress-own-tls]
- Een controller voor binnenkomend verkeer die gebruikmaakt van we gaan coderen voor het automatisch genereren van TLS-certificaten maken [met een dynamisch openbaar IP-adres] [ aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel maakt gebruik van Helm om de NGINX-controller voor binnenkomend verkeer, certificaat-manager en een voorbeeld-web-app te installeren. U moet hebben Helm geïnitialiseerd in uw AKS-cluster en het gebruik van een service-account voor Tiller. Zie voor meer informatie over het configureren en het gebruik van Helm [installeren van toepassingen met Helm in Azure Kubernetes Service (AKS)][use-helm].

In dit artikel is ook vereist dat u de Azure CLI versie 2.0.64 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

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

De controller voor binnenkomend verkeer moet ook worden gepland op een Linux-knooppunt. Windows Server-knooppunten (momenteel in preview in AKS) mag niet de controller voor binnenkomend verkeer uitvoeren. Een knooppunt selector is opgegeven met behulp van de `--set nodeSelector` parameter om te zien van de Kubernetes-scheduler om uit te voeren van de NGINX-controller voor binnenkomend verkeer op een knooppunt op basis van Linux.

> [!TIP]
> Het volgende voorbeeld wordt een Kubernetes-naamruimten voor de resources die inkomend verkeer met de naam *ingress-basic*. Geef een naamruimte voor uw omgeving zo nodig. Als uw AKS-cluster niet RBAC ingeschakeld is, voegt u toe `--set rbac.create=false` aan de Helm-opdrachten.

> [!TIP]
> Als u wilt inschakelen [client bron-IP-bewaring] [ client-source-ip] toevoegen voor aanvragen voor containers in het cluster, `--set controller.service.externalTrafficPolicy=Local` aan de Helm-installatieopdracht. De bron van de client IP wordt opgeslagen in de aanvraagheader onder *X doorgestuurd voor*. Als u een controller voor binnenkomend verkeer voor client bron IP-bescherming is ingeschakeld, wordt SSL Pass Through-query niet werkt.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress \
    --namespace ingress-basic \
    -f internal-ingress.yaml \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Wanneer de load balancer-service van Kubernetes voor de controller voor binnenkomend verkeer van NGINX wordt gemaakt, wordt uw interne IP-adres toegewezen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

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
helm install azure-samples/aks-helloworld --namespace ingress-basic
```

Nu een tweede exemplaar van de demo-toepassing installeren. Voor de tweede instantie geeft u een nieuwe titel zodat de twee toepassingen, visueel verschilt. U ook opgeven een unieke naam:

```console
helm install azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
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
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

Maakt de ingress resource via de `kubectl apply -f hello-world-ingress.yaml` opdracht.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Testen van de controller voor binnenkomend verkeer

Als u wilt testen van de routes voor de controller voor binnenkomend verkeer, blader naar de twee toepassingen met een webclient. Indien nodig, kunt u deze functionaliteit alleen in een schil op het AKS-cluster snel testen. Een test-schil maken en koppelen van een terminalsessie toe:

```console
kubectl run -it --rm aks-ingress-test --image=debian --namespace ingress-basic
```

Installeer `curl` in de schil met `apt-get`:

```console
apt-get update && apt-get install -y curl
```

Nu toegang tot het adres van uw Kubernetes inkomend controller met `curl`, zoals *http://10.240.0.42* . Geef dat uw eigen interne IP-adres opgegeven bij de implementatie van de controller voor binnenkomend verkeer in de eerste stap van dit artikel.

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

Voeg nu */hello-world-two* pad naar het adres, zoals *http://10.240.0.42/hello-world-two* . De tweede demo-toepassing met de aangepaste titel wordt geretourneerd, zoals wordt weergegeven in de volgende verkorte voorbeeld-uitvoer:

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

In dit artikel gebruikt Helm om de onderdelen van inkomend verkeer en voorbeeld-apps te installeren. Wanneer u een Helm-diagram implementeert, wordt een aantal Kubernetes-resources worden gemaakt. Deze resources bevat schillen, implementaties en services. Als u wilt deze resources opschonen, kunt u ofwel de gehele voorbeeld-naamruimte of de afzonderlijke resources verwijderen.

### <a name="delete-the-sample-namespace-and-all-resources"></a>De voorbeeld-naamruimte en alle resources verwijderen

Als u wilt verwijderen van de naamruimte van het volledige voorbeeld, gebruiken de `kubectl delete` opdracht en geeft u de naamruimtenaam van uw. Alle resources in de naamruimte worden verwijderd.

```console
kubectl delete namespace ingress-basic
```

Verwijder vervolgens de Helm-opslagplaats voor de AKS hello world-app:

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Afzonderlijke resources verwijderen

U kunt ook is een gedetailleerdere aanpak om de afzonderlijke resources gemaakt te verwijderen. Lijst met de Helm-versies met de `helm list` opdracht. Zoeken naar grafieken met de naam *nginx-inkomend* en *aks-helloworld*, zoals weergegeven in de volgende voorbeelduitvoer:

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

Verwijder de route voor inkomend verkeer die doorgestuurd verkeer naar de voorbeeld-apps:

```console
kubectl delete -f hello-world-ingress.yaml
```

Ten slotte kunt u verwijderen het zelf naamruimte. Gebruik de `kubectl delete` opdracht en geeft u de naamruimtenaam van uw:

```console
kubectl delete namespace ingress-basic
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
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers