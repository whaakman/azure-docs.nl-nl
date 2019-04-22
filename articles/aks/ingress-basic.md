---
title: Een eenvoudige ingangscontroller maken in Azure Kubernetes Service (AKS)
description: Informatie over het installeren en configureren van een controller voor binnenkomend verkeer van eenvoudige NGINX in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/27/2019
ms.author: iainfou
ms.openlocfilehash: fd9695698f90a1efebb71a2b24a196dd8c911081
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680159"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Maken van een controller voor binnenkomend verkeer in Azure Kubernetes Service (AKS)

Een controller voor inkomend verkeer is een stukje software dat omgekeerde proxy’s, configureerbare verkeersroutering en TLS-beëindiging voor Kubernetes-services biedt. Kubernetes-resources voor inkomend verkeer worden gebruikt om de regels en routes voor uitgaand verkeer worden geconfigureerd voor individuele Kubernetes-services. Met behulp van een controller en regels voor inkomend verkeer kan er één enkel IP-adres worden gebruikt voor het routeren van verkeer naar meerdere services in een Kubernetes-cluster.

In dit artikel leest u over het implementeren van de [controller voor binnenkomend verkeer van NGINX] [ nginx-ingress] in een cluster Azure Kubernetes Service (AKS). Twee toepassingen worden vervolgens uitgevoerd in de AKS-cluster, die toegankelijk via de één IP-adres is.

U kunt ook het volgende doen:

- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van een privé-interne netwerken en IP-adres maken][aks-ingress-internal]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van uw eigen TLS-certificaten maken][aks-ingress-own-tls]
- Een controller voor binnenkomend verkeer die gebruikmaakt van we gaan coderen voor het automatisch genereren van TLS-certificaten maken [met een dynamisch openbaar IP-adres] [ aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel maakt gebruik van Helm om de NGINX-controller voor binnenkomend verkeer, certificaat-manager en een voorbeeld-web-app te installeren. U moet hebben Helm geïnitialiseerd in uw AKS-cluster en het gebruik van een service-account voor Tiller. Zie voor meer informatie over het configureren en het gebruik van Helm [installeren van toepassingen met Helm in Azure Kubernetes Service (AKS)][use-helm].

In dit artikel is ook vereist dat u de Azure CLI versie 2.0.61 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Maken van een controller voor binnenkomend verkeer

Gebruik voor het maken van de controller voor binnenkomend verkeer `Helm` voor het installeren van *nginx-inkomend*. Voor extra redundantie, twee replica's van de controllers van NGINX inkomend verkeer zijn geïmplementeerd met de `--set controller.replicaCount` parameter. Om volledig te profiteren van het uitvoeren van replica's van de controller voor binnenkomend verkeer, zorg ervoor dat er meer dan één knooppunt in uw AKS-cluster.

> [!TIP]
> Het volgende voorbeeld wordt een Kubernetes-naamruimten voor de resources die inkomend verkeer met de naam *ingress-basic*. Geef een naamruimte voor uw omgeving zo nodig. Als uw AKS-cluster niet RBAC ingeschakeld is, voegt u toe `--set rbac.create=false` aan de Helm-opdrachten.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress --namespace ingress-basic --set controller.replicaCount=2
```

Wanneer de load balancer-service van Kubernetes voor de controller voor binnenkomend verkeer van NGINX wordt gemaakt, wordt een dynamisch openbaar IP-adres toegewezen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                                 TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
aspiring-labradoodle-nginx-ingress-controller        LoadBalancer   10.0.61.144    40.117.74.8   80:30386/TCP,443:32276/TCP   6m2s
aspiring-labradoodle-nginx-ingress-default-backend   ClusterIP      10.0.192.145   <none>        80/TCP                       6m2s
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

In het volgende voorbeeld wordt het verkeer naar het adres `http://40.117.74.8/` wordt doorgestuurd naar de service met de naam `aks-helloworld`. Verkeer naar het adres `http://40.117.74.8/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service.

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

Als u wilt testen van de routes voor de controller voor binnenkomend verkeer, blader naar de twee toepassingen. Open een webbrowser naar de IP-adres van uw ingangscontroller NGINX zoals *http://40.117.74.8*. De eerste demo-toepassing wordt weergegeven in de webbrowser, zoals wordt weergegeven in het volgende voorbeeld:

![Eerste-app die achter de controller voor binnenkomend verkeer](media/ingress-basic/app-one.png)

Voeg nu de */hello-world-two* pad naar het IP-adres, zoals *http://40.117.74.8/hello-world-two*. De tweede demo-toepassing met de aangepaste titel wordt weergegeven:

![Tweede app die achter de controller voor binnenkomend verkeer](media/ingress-basic/app-two.png)

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

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
aspiring-labradoodle    1           Wed Mar 27 19:55:37 2019    DEPLOYED    nginx-ingress-1.3.1     0.22.0      ingress-basic
esteemed-koala          1           Wed Mar 27 19:59:18 2019    DEPLOYED    aks-helloworld-0.1.0                ingress-basic
wonderful-puma          1           Wed Mar 27 19:59:07 2019    DEPLOYED    aks-helloworld-0.1.0                ingress-basic
```

Verwijderen van de versies met de `helm delete` opdracht. Het volgende voorbeeld wordt de implementatie van NGINX inkomend verkeer en de twee voorbeeld AKS hello world-apps.

```
$ helm delete aspiring-labradoodle esteemed-koala wonderful-puma

release "aspiring-labradoodle" deleted
release "esteemed-koala" deleted
release "wonderful-puma" deleted
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

- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van een privé-interne netwerken en IP-adres maken][aks-ingress-internal]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van uw eigen TLS-certificaten maken][aks-ingress-own-tls]
- Een controller voor binnenkomend verkeer die gebruikmaakt van we gaan coderen voor het automatisch genereren van TLS-certificaten maken [met een dynamisch openbaar IP-adres] [ aks-ingress-tls] of [met een statisch openbaar IP-adres][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
