---
title: Maken van een HTTP-controller voor binnenkomend verkeer met een statisch IP-adres in Azure Kubernetes Service (AKS)
description: Informatie over het installeren en configureren van een NGINX-controller voor binnenkomend verkeer met een statisch openbaar IP-adres in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/06/2019
ms.author: iainfou
ms.openlocfilehash: 9f8e05eb3d5741b640fe07c06e39edaf332d3060
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57766084"
---
# <a name="create-an-ingress-controller-with-a-static-public-ip-address-in-azure-kubernetes-service-aks"></a>Maken van een controller voor binnenkomend verkeer met een statisch openbaar IP-adres in Azure Kubernetes Service (AKS)

Een controller voor inkomend verkeer is een stukje software dat omgekeerde proxy’s, configureerbare verkeersroutering en TLS-beëindiging voor Kubernetes-services biedt. Kubernetes-resources voor inkomend verkeer worden gebruikt om de regels en routes voor uitgaand verkeer worden geconfigureerd voor individuele Kubernetes-services. Met behulp van een controller en regels voor inkomend verkeer kan er één enkel IP-adres worden gebruikt voor het routeren van verkeer naar meerdere services in een Kubernetes-cluster.

In dit artikel leest u over het implementeren van de [controller voor binnenkomend verkeer van NGINX] [ nginx-ingress] in een cluster Azure Kubernetes Service (AKS). De controller voor binnenkomend verkeer is geconfigureerd met een statisch openbaar IP-adres. De [Certificaatbeheer] [ cert-manager] project wordt gebruikt om automatisch te genereren en configureer [laten versleutelen] [ lets-encrypt] certificaten. Ten slotte worden twee toepassingen uitgevoerd in de AKS-cluster, die toegankelijk via één IP-adres is.

U kunt ook het volgende doen:

- [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van uw eigen TLS-certificaten maken][aks-ingress-own-tls]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van we gaan coderen voor het automatisch genereren van TLS-certificaten met een dynamisch openbaar IP-adres maken][aks-ingress-tls]

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een bestaand AKS-cluster hebt. Als u een cluster AKS nodig hebt, raadpleegt u de Quick Start voor AKS [met de Azure CLI] [ aks-quickstart-cli] of [met behulp van de Azure-portal][aks-quickstart-portal].

In dit artikel maakt gebruik van Helm om de NGINX-controller voor binnenkomend verkeer, certificaat-manager en een voorbeeld-web-app te installeren. U moet hebben Helm geïnitialiseerd in uw AKS-cluster en het gebruik van een service-account voor Tiller. Zorg ervoor dat u de nieuwste versie van Helm. Zie voor upgrade-instructies, de [Helm installeren docs][helm-install]. Zie voor meer informatie over het configureren en het gebruik van Helm [installeren van toepassingen met Helm in Azure Kubernetes Service (AKS)][use-helm].

In dit artikel is ook vereist dat u de Azure CLI versie 2.0.59 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Maken van een controller voor binnenkomend verkeer

Een NGINX-controller voor binnenkomend verkeer wordt standaard gemaakt met de toewijzing van een nieuwe openbare IP-adres. Dit openbare IP-adres is alleen statische voor de levensduur van de controller voor binnenkomend verkeer en gaat verloren wanneer de controller is verwijderd en opnieuw gemaakt. Een algemene configuratievereiste is voor de NGINX-controller voor binnenkomend verkeer een bestaande statische openbare IP-adres. De statische openbare IP-adres blijft als de controller voor binnenkomend verkeer wordt verwijderd. Deze aanpak kunt u bestaande DNS-records en netwerkconfiguraties gebruiken op een consistente manier gedurende de levenscyclus van uw toepassingen.

Als u maken van een statisch openbaar IP-adres wilt, de eerste keer de naam van de resourcegroep van het AKS-cluster met de [az aks show] [ az-aks-show] opdracht:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
```

Maak vervolgens een openbaar IP-adres met de *statische* voor het gebruik van een toewijzing-methode de [az network public-ip maken] [ az-network-public-ip-create] opdracht. Het volgende voorbeeld wordt een openbaar IP-adres met de naam *myAKSPublicIP* resourcegroep in de vorige stap hebt verkregen in de AKS-cluster:

```azurecli-interactive
az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static
```

Het IP-adres wordt weergegeven, zoals wordt weergegeven in de volgende verkorte uitvoer:

```json
{
  "publicIp": {
    [...]
    "ipAddress": "40.121.63.72",
    [...]
  }
}
```

Nu implementeren de *nginx-inkomend* grafiek met Helm. Voeg de `--set controller.service.loadBalancerIP` parameter, en geef uw eigen openbare IP-adres in de vorige stap hebt gemaakt. Voor extra redundantie, twee replica's van de controllers van NGINX inkomend verkeer zijn geïmplementeerd met de `--set controller.replicaCount` parameter. Om volledig te profiteren van het uitvoeren van replica's van de controller voor binnenkomend verkeer, zorg ervoor dat er meer dan één knooppunt in uw AKS-cluster.

> [!TIP]
> De volgende voorbeelden installeert u de controller voor binnenkomend verkeer en de certificaten in de `kube-system` naamruimte. Indien gewenst, kunt u een andere naamruimte voor uw eigen omgeving. Ook als uw AKS-cluster niet ingeschakeld RBAC is, toe te voegen `--set rbac.create=false` voor de opdrachten.

```console
helm install stable/nginx-ingress \
    --namespace kube-system \
    --set controller.service.loadBalancerIP="40.121.63.72"  \
    --set controller.replicaCount=2
```

Wanneer de load balancer-service van Kubernetes voor de controller voor binnenkomend verkeer van NGINX wordt gemaakt, wordt het statische IP-adres toegewezen, zoals wordt weergegeven in de volgende voorbeelduitvoer:

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                        TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
dinky-panda-nginx-ingress-controller        LoadBalancer   10.0.232.56   40.121.63.72   80:31978/TCP,443:32037/TCP   3m
dinky-panda-nginx-ingress-default-backend   ClusterIP      10.0.95.248   <none>         80/TCP                       3m
```

Geen binnenkomende regels zijn nog, gemaakt, zodat de NGINX-ingangscontroller standaard 404-pagina wordt weergegeven als u naar het openbare IP-adres bladeren. Ingress regels zijn geconfigureerd in de volgende stappen uit.

## <a name="configure-a-dns-name"></a>Configureren van een DNS-naam

Voor de HTTPS-certificaten correct te laten werken, configureert u een FQDN-naam voor het IP-adres voor inkomend verkeer controller. Het volgende script bijwerken met het IP-adres van de controller voor binnenkomend verkeer en een unieke naam die u wilt gebruiken voor de FQDN-naam:

```azurecli-interactive
#!/bin/bash

# Public IP address of your ingress controller
IP="40.121.63.72"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with DNS name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

De controller voor binnenkomend verkeer is nu toegankelijk zijn via de FQDN-naam.

## <a name="install-cert-manager"></a>Certificaat-manager installeren

De NGINX-controller voor binnenkomend verkeer biedt ondersteuning voor TLS-beëindiging. Er zijn verschillende manieren om te halen en certificaten configureren voor HTTPS. In dit artikel wordt gedemonstreerd hoe [Certificaatbeheer][cert-manager], waarmee u automatische [kunt versleutelen] [ lets-encrypt] certificaat genereren en Management-functionaliteit.

> [!NOTE]
> In dit artikel wordt de `staging` omgeving voor laten we versleutelen. Gebruik in productie-implementaties, `letsencrypt-prod` en `https://acme-v02.api.letsencrypt.org/directory` in de resourcedefinities en bij het installeren van de Helm-diagram.

Als u wilt de certificaat-manager-domeincontroller installeren in een cluster RBAC is ingeschakeld, gebruikt u de volgende `helm install` opdracht:

```console
kubectl label namespace kube-system certmanager.k8s.io/disable-validation=true

kubectl apply \
    -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.6/deploy/manifests/00-crds.yaml
    
helm install stable/cert-manager \
    --namespace kube-system \
    --set ingressShim.defaultIssuerName=letsencrypt-staging \
    --set ingressShim.defaultIssuerKind=ClusterIssuer \
    --version v0.6.6
```

> [!TIP]
> Als u een foutbericht, zoals ontvangt `Error: failed to download "stable/cert-manager"`, zorg ervoor dat u hebt met succes is uitgevoerd `helm repo update` voor een lijst van de meest recente beschikbare Helm-grafieken.

Als uw cluster niet RBAC ingeschakeld is, gebruikt u in plaats daarvan de volgende opdracht uit:

```console
kubectl label namespace kube-system certmanager.k8s.io/disable-validation=true

kubectl apply \
    -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.6/deploy/manifests/00-crds.yaml
    
helm install stable/cert-manager \
    --namespace kube-system \
    --set ingressShim.defaultIssuerName=letsencrypt-staging \
    --set ingressShim.defaultIssuerKind=ClusterIssuer \
    --set rbac.create=false \
    --set serviceAccount.create=false \
    --version v0.6.6
```

Zie voor meer informatie over Certificaatbeheer configuratie, de [Certificaatbeheer project][cert-manager].

## <a name="create-a-ca-cluster-issuer"></a>Een uitgevende CA-cluster maken

Voordat u certificaten kunnen worden uitgegeven, certificaat-manager vereist een [verlener] [ cert-manager-issuer] of [ClusterIssuer] [ cert-manager-cluster-issuer] resource. Deze Kubernetes-resources zijn identiek in functionaliteit, maar `Issuer` werkt in één enkele naamruimte en `ClusterIssuer` werkt met alle naamruimten. Zie voor meer informatie de [Certificaatbeheer verlener] [ cert-manager-issuer] documentatie.

Maken van de verlener van een cluster, zoals `cluster-issuer.yaml`, met behulp van het volgende voorbeeld-manifest. Het e-mailadres bijwerken met een geldig adres van uw organisatie:

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    http01: {}
```

Gebruik voor het maken van de uitgever van de `kubectl apply -f cluster-issuer.yaml` opdracht.

```
$ kubectl apply -f cluster-issuer.yaml

clusterissuer.certmanager.k8s.io/letsencrypt-staging created
```

## <a name="create-a-certificate-object"></a>Een certificaatobject maken

Vervolgens moet een certificaat-bron worden gemaakt. De certificaatresource definieert de gewenste X.509-certificaat. Zie voor meer informatie, [Certificaatbeheer certificaten][cert-manager-certificates].

CERT-manager heeft automatisch waarschijnlijk een certificaatobject voor u met behulp van ingress-shim, die automatisch wordt geïmplementeerd met Certificaatbeheer sinds v0.2.2 gemaakt. Zie voor meer informatie de [ingress-shim documentatie][ingress-shim].

Om te controleren of het certificaat correct is gemaakt, gebruikt u de `kubectl describe certificate tls-secret` opdracht.

Als het certificaat is uitgegeven, ziet u uitvoer die vergelijkbaar is met het volgende:
```
Type    Reason          Age   From          Message
----    ------          ----  ----          -------
  Normal  CreateOrder     11m   cert-manager  Created new ACME order, attempting validation...
  Normal  DomainVerified  10m   cert-manager  Domain "demo-aks-ingress.eastus.cloudapp.azure.com" verified with "http-01" validation
  Normal  IssueCert       10m   cert-manager  Issuing certificate...
  Normal  CertObtained    10m   cert-manager  Obtained certificate from ACME server
  Normal  CertIssued      10m   cert-manager  Certificate issued successfully
```

Als u maken van een extra certificaatresource wilt, kunt u dit doen met het volgende voorbeeld-manifest. Maak een bestand met de naam *certificates.yaml* en werk de *dnsNames* en *domeinen* aan de DNS-naam die u in de vorige stap hebt gemaakt. Als u een controller voor intern gebruik bestemde binnenkomend verkeer gebruikt, geeft u de interne DNS-naam voor uw service.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Gebruik voor het maken van de certificaatresource de `kubectl apply -f certificates.yaml` opdracht.

```
$ kubectl apply -f certificates.yaml

certificate.certmanager.k8s.io/tls-secret created
```

## <a name="run-demo-applications"></a>Demotoepassingen uitvoeren

Een controller voor binnenkomend verkeer en een oplossing voor het beheer van certificaten zijn geconfigureerd. Nu gaan we twee run demo voor toepassingen in uw AKS-cluster. In dit voorbeeld Helm gebruikt om twee instanties van een eenvoudige 'Hallo wereld'-toepassing implementeren.

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

In het volgende voorbeeld wordt het verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/` wordt doorgestuurd naar de service met de naam `aks-helloworld`. Verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service. Update de *hosts* en *host* aan de DNS-naam die u in de vorige stap hebt gemaakt.

Maak een bestand met de naam `hello-world-ingress.yaml` en kopieer het volgende voorbeeld YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
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

Open een webbrowser naar de FQDN-naam van uw ingangscontroller Kubernetes zoals *https://demo-aks-ingress.eastus.cloudapp.azure.com*.

Zoals deze voorbeelden `letsencrypt-staging`, het verleende SSL-certificaat niet wordt vertrouwd door de browser. Accepteer de waarschuwing om door te gaan naar uw toepassing. Informatie over het certificaat wordt dit weergegeven *vals LE tussenliggende X1* certificaat is uitgegeven door laten versleutelen. Dit certificaat valse geeft `cert-manager` verwerkt de aanvraag goed en ontvangen van een certificaat van de provider:

![We gaan coderen staging-certificaat](media/ingress/staging-certificate.png)

Wanneer u wijzigen we versleutelen gebruiken `prod` in plaats van `staging`, een vertrouwd certificaat dat is uitgegeven door laten versleutelen wordt gebruikt, zoals wordt weergegeven in het volgende voorbeeld:

![Laten we gecodeerd certificaat](media/ingress/certificate.png)

De demo-toepassing wordt weergegeven in de webbrowser:

![Voorbeeld van de toepassing een](media/ingress/app-one.png)

Voeg nu de */hello-world-two* pad naar de FQDN-naam, zoals *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two*. De tweede demo-toepassing met de aangepaste titel wordt weergegeven:

![Voorbeeld van de toepassing twee](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>Resources opschonen

In dit artikel gebruikt Helm voor het installeren van de onderdelen van inkomend verkeer, certificaten en voorbeeld-apps. Wanneer u een Helm-diagram implementeert, wordt een aantal Kubernetes-resources worden gemaakt. Deze resources bevat schillen, implementaties en services. Als u wilt opschonen, moet u eerst de certificaat-resources verwijderen:

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Nu lijst de Helm-versies met de `helm list` opdracht. Zoeken naar grafieken met de naam *nginx-inkomend*, *Certificaatbeheer*, en *aks-helloworld*, zoals weergegeven in de volgende voorbeelduitvoer:

```
$ helm list

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
waxen-hamster           1           Wed Mar  6 23:16:00 2019    DEPLOYED    nginx-ingress-1.3.1   0.22.0        kube-system
alliterating-peacock    1           Wed Mar  6 23:17:37 2019    DEPLOYED    cert-manager-v0.6.6     v0.6.2      kube-system
mollified-armadillo     1           Wed Mar  6 23:26:04 2019    DEPLOYED    aks-helloworld-0.1.0                default
wondering-clam          1           Wed Mar  6 23:26:07 2019    DEPLOYED    aks-helloworld-0.1.0                default
```

Verwijderen van de versies met de `helm delete` opdracht. Het volgende voorbeeld wordt de implementatie van NGINX inkomend verkeer, certificaatbeheerder en de twee voorbeeld AKS hello world-apps.

```
$ helm delete waxen-hamster alliterating-peacock mollified-armadillo wondering-clam

release "billowing-kitten" deleted
release "loitering-waterbuffalo" deleted
release "flabby-deer" deleted
release "linting-echidna" deleted
```

Verwijder vervolgens de Helm-opslagplaats voor de AKS hello world-app:

```console
helm repo remove azure-samples
```

Verwijder de route voor inkomend verkeer die doorgestuurd verkeer naar de voorbeeld-apps:

```console
kubectl delete -f hello-world-ingress.yaml
```

Ten slotte, verwijder de statische openbare IP-adres hebt gemaakt voor de controller voor binnenkomend verkeer. Geef uw *MC_* cluster de naam van resourcegroep hebt verkregen in de eerste stap van dit artikel, zoals *MC_myResourceGroup_myAKSCluster_eastus*:

```azurecli-interactive
az network public-ip delete --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel bevat enkele externe onderdelen over AKS. Zie voor meer informatie over deze onderdelen, de volgende project-pagina's:

- [Helm CLI][helm-cli]
- [NGINX-controller voor binnenkomend verkeer][nginx-ingress]
- [cert-manager][cert-manager]

U kunt ook het volgende doen:

- [Een eenvoudige ingangscontroller met verbinding met het externe netwerk maken][aks-ingress-basic]
- [De HTTP-aanvraag routering invoegtoepassing inschakelen][aks-http-app-routing]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van een privé-interne netwerken en IP-adres maken][aks-ingress-internal]
- [Een controller voor binnenkomend verkeer die gebruikmaakt van uw eigen TLS-certificaten maken][aks-ingress-own-tls]
- [Maken van een controller voor binnenkomend verkeer met een dynamische openbare IP-adres en we gaan coderen voor het automatisch genereren van TLS-certificaten configureren][aks-ingress-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[ingress-shim]: http://docs.cert-manager.io/en/latest/tasks/issuing-certificates/ingress-shim.html

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
