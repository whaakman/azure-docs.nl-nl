---
title: Gebruik uw eigen TLS-certificaten voor inkomend verkeer met Azure Kubernetes Service (AKS)-cluster
description: Informatie over het installeren en configureren van een NGINX-controller voor binnenkomend verkeer die gebruikmaakt van uw eigen certificaten in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/23/2018
ms.author: iainfou
ms.openlocfilehash: c109febc90c9dd8d9b17489c9e612f677695bd25
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55727089"
---
# <a name="create-an-https-ingress-controller-and-use-your-own-tls-certificates-on-azure-kubernetes-service-aks"></a>Maken van een HTTPS-controller voor binnenkomend verkeer en gebruik uw eigen TLS-certificaten in Azure Kubernetes Service (AKS)

Een controller voor binnenkomend verkeer is een stukje software die voorziet in omgekeerde proxy, configureerbare verkeersroutering en TLS-beëindiging voor Kubernetes-services. Kubernetes-resources voor inkomend verkeer worden gebruikt voor het configureren van de ingress regels en routes voor afzonderlijke Kubernetes-services. Met behulp van een controller voor binnenkomend verkeer en ingress regels, kan één IP-adres worden gebruikt voor het routeren van verkeer naar meerdere services in een Kubernetes-cluster.

In dit artikel leest u over het implementeren van de [controller voor binnenkomend verkeer van NGINX] [ nginx-ingress] in een cluster Azure Kubernetes Service (AKS). U uw eigen certificaten genereren en een Kubernetes-geheim maken voor gebruik met de route inkomend verkeer. Ten slotte worden twee toepassingen uitgevoerd in de AKS-cluster, die toegankelijk via één IP-adres is.

U kunt ook het volgende doen:

- [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van een privé-interne netwerken en IP-adres maken][aks-ingress-internal]
- Een controller voor binnenkomend verkeer die gebruikmaakt van we gaan coderen voor het automatisch genereren van TLS-certificaten maken [met een dynamisch openbaar IP-adres] [ aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel maakt gebruik van Helm om de NGINX-controller voor binnenkomend verkeer en een voorbeeld-web-app te installeren. U moet hebben Helm geïnitialiseerd in uw AKS-cluster en het gebruik van een service-account voor Tiller. Zorg ervoor dat u de nieuwste versie van Helm. Zie voor upgrade-instructies, de [Helm installeren docs][helm-install]. Zie voor meer informatie over het configureren en het gebruik van Helm [installeren van toepassingen met Helm in Azure Kubernetes Service (AKS)][use-helm].

In dit artikel is ook vereist dat u de Azure CLI versie 2.0.41 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Maken van een controller voor binnenkomend verkeer

Gebruik voor het maken van de controller voor binnenkomend verkeer `Helm` voor het installeren van *nginx-inkomend*. Voor extra redundantie, twee replica's van de controllers van NGINX inkomend verkeer zijn geïmplementeerd met de `--set controller.replicaCount` parameter. Om volledig te profiteren van het uitvoeren van replica's van de controller voor binnenkomend verkeer, zorg ervoor dat er meer dan één knooppunt in uw AKS-cluster.

> [!TIP]
> Het volgende voorbeeld installeert de controller voor binnenkomend verkeer in de `kube-system` naamruimte. Indien gewenst, kunt u een andere naamruimte voor uw eigen omgeving. Als uw AKS-cluster niet RBAC ingeschakeld is, voegt u toe `--set rbac.create=false` voor de opdrachten.

```console
helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2
```

Tijdens de installatie van is een openbaar IP-adres van Azure gemaakt voor de controller voor binnenkomend verkeer. Dit openbare IP-adres wordt statisch voor de levensduur van de controller voor binnenkomend verkeer. Als u de controller voor binnenkomend verkeer verwijdert, wordt de toewijzing van het openbare IP-adres gaat verloren. Als u vervolgens een extra ingangscontroller maakt, wordt een nieuw openbaar IP-adres toegewezen. Als u het gebruik van het openbare IP-adres behouden wilt, kunt u in plaats daarvan [maken van een controller voor binnenkomend verkeer met een statisch openbaar IP-adres][aks-ingress-static-tls].

Als u het openbare IP-adres, gebruikt de `kubectl get service` opdracht. Het duurt een paar minuten voor het IP-adres moet worden toegewezen aan de service.

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                          TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
virulent-seal-nginx-ingress-controller        LoadBalancer   10.0.48.240   40.87.46.190   80:31159/TCP,443:30657/TCP   7m
virulent-seal-nginx-ingress-default-backend   ClusterIP      10.0.50.5     <none>         80/TCP                       7m
```

Noteer dit openbare IP-adres, zoals het in de laatste stap wordt gebruikt om de implementatie te testen.

Nog zijn geen binnenkomende regels gemaakt. Als u naar het openbare IP-adres bladert, wordt de NGINX-ingangscontroller standaard 404-pagina weergegeven.

## <a name="generate-tls-certificates"></a>TLS-certificaten genereren

In dit artikel gaan we genereren een zelfondertekend certificaat met `openssl`. Voor gebruik in productieomgevingen, moet u een vertrouwde, ondertekende certificaat via een provider of uw eigen certificeringsinstantie (CA) aanvragen. In de volgende stap maakt u een Kubernetes genereren *geheim* met behulp van de TLS-certificaat en de persoonlijke sleutel die is gegenereerd door OpenSSL.

Het volgende voorbeeld wordt een RSA 2048-bits X 509-certificaat is geldig tot 365 dagen met de naam gegenereerd *aks-ingress-tls.crt*. De naam van het persoonlijke sleutelbestand *aks-ingress-tls.key*. Een geheim Kubernetes TLS vereist beide van deze bestanden.

In dit artikel werkt met de *demo.azure.com* algemene naam van het onderwerp en hoeft niet te worden gewijzigd. Voor gebruik in productieomgevingen, Geef uw eigen organisatie-waarden op voor de `-subj` parameter:

```console
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -out aks-ingress-tls.crt \
    -keyout aks-ingress-tls.key \
    -subj "/CN=demo.azure.com/O=aks-ingress-tls"
```

## <a name="create-kubernetes-secret-for-the-tls-certificate"></a>Kubernetes-geheim voor het TLS-certificaat maken

Kubernetes met de TLS-certificaat en de persoonlijke sleutel voor de controller voor binnenkomend verkeer, zodat u maken en gebruiken van een geheim. Het geheim is één keer gedefinieerd en gebruikt het certificaat en de sleutelbestand in de vorige stap hebt gemaakt. U vervolgens verwijzen naar dit geheim bij het definiëren van routes voor inkomend verkeer.

Het volgende voorbeeld wordt een geheime naam *aks-ingress-tls*:

```console
kubectl create secret tls aks-ingress-tls \
    --key aks-ingress-tls.key \
    --cert aks-ingress-tls.crt
```

## <a name="run-demo-applications"></a>Demotoepassingen uitvoeren

Een controller voor binnenkomend verkeer en een geheim met uw certificaat zijn geconfigureerd. Nu gaan we twee run demo voor toepassingen in uw AKS-cluster. In dit voorbeeld Helm gebruikt om twee instanties van een eenvoudige 'Hallo wereld'-toepassing implementeren.

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

Beide toepassingen wordt nu uitgevoerd in uw Kubernetes-cluster, maar ze zijn geconfigureerd met een service van het type `ClusterIP`. Daarom zijn de toepassingen niet toegankelijk is vanaf internet. Om deze openbaar beschikbaar maken door een Kubernetes-ingress-resource te maken. De resource met inkomend verkeer configureert u de regels die verkeer naar een van de twee toepassingen routeren.

In het volgende voorbeeld wordt het verkeer naar het adres `https://demo.azure.com/` wordt doorgestuurd naar de service met de naam `aks-helloworld`. Verkeer naar het adres `https://demo.azure.com/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service. Voor dit artikel hoeft u niet te wijzigen de hostnamen van deze demo. Geef de namen die zijn opgegeven als onderdeel van het certificaat aanvragen en genereren voor gebruik in productieomgevingen.

> [!TIP]
> Als de hostnaam die is opgegeven tijdens het proces van de aanvraag certificaat, de CN-naam, komt niet overeen met de host die is gedefinieerd in uw route inkomend verkeer, u inkomend verkeer controller geeft een *Kubernetes inkomend Controller valse certificaat*. Zorg ervoor dat uw certificaat en binnenkomende route host namen overeenkomen.

De *tls* gedeelte wordt uitgelegd dat de route inkomend verkeer naar het gebruik van het geheim met de naam *aks-ingress-tls* voor de host *demo.azure.com*. Nogmaals, voor gebruik in productieomgevingen, uw eigen hostadres opgeven.

Maak een bestand met de naam `hello-world-ingress.yaml` en kopieer het volgende voorbeeld YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo.azure.com
    secretName: aks-ingress-tls
  rules:
  - host: demo.azure.com
    http:
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

## <a name="test-the-ingress-configuration"></a>De configuratie van de inkomende gegevens testen

Voor het testen van de certificaten met onze vals *demo.azure.com* hosten, gebruikt u `curl` en geef de *--oplossen* parameter. Deze parameter kunt u toewijzen de *demo.azure.com* naam naar het openbare IP-adres van de controller voor binnenkomend verkeer. Geef het openbare IP-adres van uw eigen controller voor binnenkomend verkeer, zoals wordt weergegeven in het volgende voorbeeld:

```
curl -v -k --resolve demo.azure.com:443:40.87.46.190 https://demo.azure.com
```

Er is geen aanvullende pad is opgegeven met het adres, dus de controller voor binnenkomend verkeer standaard ingesteld op de */* route. De eerste demo-toepassing wordt geretourneerd, zoals wordt weergegeven in de volgende verkorte voorbeeld-uitvoer:

```
$ curl -v -k --resolve demo.azure.com:443:40.87.46.190 https://demo.azure.com

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

De *- v* parameter in onze `curl` opdracht voert uitgebreide informatie, met inbegrip van het TLS-certificaat dat is ontvangen. Halverwege via de uitvoer curl, kunt u controleren of uw eigen TLS-certificaat is gebruikt. De *-k* parameter blijft het laden van de pagina, hoewel we maken gebruik van een zelfondertekend certificaat. Het volgende voorbeeld ziet u dat de *verlener: CN=demo.Azure.com; O = aks-ingress-tls* certificaat is gebruikt:

```
[...]
* Server certificate:
*  subject: CN=demo.azure.com; O=aks-ingress-tls
*  start date: Oct 22 22:13:54 2018 GMT
*  expire date: Oct 22 22:13:54 2019 GMT
*  issuer: CN=demo.azure.com; O=aks-ingress-tls
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
[...]
```

Voeg nu */hello-world-two* pad naar het adres, zoals `https://demo.azure.com/hello-world-two`. De tweede demo-toepassing met de aangepaste titel wordt geretourneerd, zoals wordt weergegeven in de volgende verkorte voorbeeld-uitvoer:

```
$ curl -v -k --resolve demo.azure.com:443:137.117.36.18 https://demo.azure.com/hello-world-two

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>AKS Ingress Demo</title>
[...]
```

## <a name="clean-up-resources"></a>Resources opschonen

In dit artikel gebruikt Helm om de onderdelen van inkomend verkeer en voorbeeld-apps te installeren. Wanneer u een Helm-diagram implementeert, wordt een aantal Kubernetes-resources worden gemaakt. Deze bronnen omvatten schillen, implementaties en services. Als u wilt opschonen, moet u eerst de Helm-versies met lijst de `helm list` opdracht. Zoeken naar grafieken met de naam *nginx-inkomend* en *aks-helloworld*, zoals weergegeven in de volgende voorbeelduitvoer:

```
$ helm list

NAME            REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
virulent-seal   1           Tue Oct 23 16:37:24 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
billowing-guppy 1           Tue Oct 23 16:41:38 2018    DEPLOYED    aks-helloworld-0.1.0                default
listless-quokka 1           Tue Oct 23 16:41:30 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Verwijderen van de versies met de `helm delete` opdracht. Het volgende voorbeeld wordt de implementatie van NGINX inkomend verkeer en de twee voorbeeld AKS hello world-apps.

```
$ helm delete virulent-seal billowing-guppy listless-quokka

release "virulent-seal" deleted
release "billowing-guppy" deleted
release "listless-quokka" deleted
```

Verwijder vervolgens de Helm-opslagplaats voor de AKS hello world-app:

```console
helm repo remove azure-samples
```

Verwijder de route voor inkomend verkeer die doorgestuurd verkeer naar de voorbeeld-apps:

```console
kubectl delete -f hello-world-ingress.yaml
```

Ten slotte verwijdert u het certificaat geheim:

```console
kubectl delete secret aks-ingress-tls
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel bevat enkele externe onderdelen over AKS. Zie voor meer informatie over deze onderdelen, de volgende project-pagina's:

- [Helm CLI][helm-cli]
- [NGINX-controller voor binnenkomend verkeer][nginx-ingress]

U kunt ook het volgende doen:

- [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van een privé-interne netwerken en IP-adres maken][aks-ingress-internal]
- Een controller voor binnenkomend verkeer die gebruikmaakt van we gaan coderen voor het automatisch genereren van TLS-certificaten maken [met een dynamisch openbaar IP-adres] [ aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-tls]: ingress-tls.md
