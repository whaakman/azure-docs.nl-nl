---
title: Inkomend verkeer met Azure Kubernetes Service (AKS)-cluster configureren
description: Installeren en configureren van een NGINX-controller voor binnenkomend verkeer in een cluster Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/25/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bd223e9eebac495d7336c618b831528505c30959
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857392"
---
# <a name="https-ingress-on-azure-kubernetes-service-aks"></a>HTTPS inkomend verkeer op Azure Kubernetes Service (AKS)

Een controller voor binnenkomend verkeer is een stukje software die voorziet in omgekeerde proxy, configureerbare verkeersroutering en TLS-beëindiging voor Kubernetes-services. Kubernetes-resources voor inkomend verkeer worden gebruikt voor het configureren van de ingress regels en routes voor afzonderlijke Kubernetes-services. Met behulp van een controller voor binnenkomend verkeer en ingress regels, kan één extern adres worden gebruikt voor het routeren van verkeer naar meerdere services in een Kubernetes-cluster.

Dit document helpt bij de Voorbeeldimplementatie van een van de [controller voor binnenkomend verkeer van NGINX] [ nginx-ingress] in een cluster Azure Kubernetes Service (AKS). Bovendien de [Certificaatbeheer] [ cert-manager] project wordt gebruikt om automatisch te genereren en configureer [laten versleutelen] [ lets-encrypt] certificaten. Ten slotte worden verschillende toepassingen uitgevoerd in de AKS-cluster, die toegankelijk via één adres is.

## <a name="install-an-ingress-controller"></a>Installeren van een controller voor binnenkomend verkeer

Helm gebruiken voor het installeren van de NGINX-controller voor binnenkomend verkeer. Zie de NGINX-controller voor binnenkomend verkeer [documentatie] [ nginx-ingress] voor gedetailleerde implementatie-informatie.

In dit voorbeeld installeert de controller in de `kube-system` naamruimte, dit kan worden gewijzigd met een naamruimte van uw keuze. Als uw AKS-cluster niet RBAC ingeschakeld is, voegt u toe `--set rbac.create=false` aan de opdracht. Zie voor meer informatie de [nginx-inkomend grafiek][nginx-ingress].

```bash
helm install stable/nginx-ingress --namespace kube-system
```

Tijdens de installatie van is een openbaar IP-adres van Azure gemaakt voor de controller voor binnenkomend verkeer. Als u het openbare IP-adres, gebruikt u de opdracht kubectl get service. Het duurt enige tijd voor het IP-adres moet worden toegewezen aan de service.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Omdat er geen binnenkomende regels hebt gemaakt, als u naar het openbare IP-adres bladert, wordt u doorgestuurd naar de ingress controllers 404 standaardpagina van NGINX.

![Standaard NGINX-back-end](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Configureren van DNS-naam

Omdat het HTTPS-certificaten worden gebruikt, moet u een FQDN-naam voor het IP-adres voor inkomend verkeer domeincontrollers configureren. In dit voorbeeld wordt een Azure-FQDN gemaakt met de Azure CLI. Het script bijwerken met het IP-adres van de controller voor binnenkomend verkeer en de naam die u wilt gebruiken in de FQDN-naam.

```bash
#!/bin/bash

# Public IP address
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with dns name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

Nu moet de controller voor binnenkomend verkeer toegankelijk zijn via de FQDN-naam.

## <a name="install-cert-manager"></a>Certificaat-manager installeren

De NGINX-controller voor binnenkomend verkeer biedt ondersteuning voor TLS-beëindiging. Er zijn verschillende manieren om te halen en certificaten configureren voor HTTPS, in dit document wordt gedemonstreerd met behulp van [Certificaatbeheer][cert-manager], waarmee u automatische [kuntversleutelen] [ lets-encrypt] generatie en de beheerfunctionaliteit van het certificaat.

Gebruik de volgende Helm installeren voor het installeren van de controller Certificaatbeheer opdracht.

```bash
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Als uw cluster niet RBAC ingeschakeld is, moet u deze opdracht gebruiken.

```bash
helm install stable/cert-manager \
  --set ingressShim.defaultIssuerName=letsencrypt-prod \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

Zie voor meer informatie over Certificaatbeheer configuratie, de [Certificaatbeheer project][cert-manager].

## <a name="create-ca-cluster-issuer"></a>Uitgever van CA-cluster maken

Voordat u certificaten kunnen worden uitgegeven, certificaat-manager vereist een [verlener] [ cert-manager-issuer] of [ClusterIssuer] [ cert-manager-cluster-issuer] resource. De resources zijn identiek in functionaliteit echter `Issuer` werkt in één enkele naamruimte waar `ClusterIssuer` werkt met alle naamruimten. Zie voor meer informatie de [Certificaatbeheer verlener] [ cert-manager-issuer] documentatie.

De verlener van een cluster met behulp van de volgende manifest maken. Het e-mailadres bijwerken met een geldig adres van uw organisatie.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-prod
    http01: {}
```

## <a name="create-certificate-object"></a>Certificaatobject maken

Vervolgens moet een certificaat-bron worden gemaakt. De certificaatresource definieert de gewenste X.509-certificaat. Voor meer informatie ziet, [Certificaatbeheer certificaten][cert-manager-certificates].

De certificaatresource met de volgende manifest maken.

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
    name: letsencrypt-prod
    kind: ClusterIssuer
```

## <a name="run-application"></a>Toepassing uitvoeren

Op dit moment zijn een controller voor binnenkomend verkeer en een oplossing voor het beheer van certificaten geconfigureerd. Voer nu een paar toepassingen in uw AKS-cluster.

In dit voorbeeld wordt Helm gebruikt om meerdere exemplaren van een eenvoudige hello world-toepassing uitvoeren.

Toevoegen voordat u de toepassing uitvoert, de Azure-voorbeelden Helm-opslagplaats op uw ontwikkelsysteem.

```bash
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Voer de AKS hello world-grafiek met de volgende opdracht:

```bash
helm install azure-samples/aks-helloworld
```

Nu een tweede exemplaar van de hello world-toepassing installeren.

Voor het tweede exemplaar, moet u een nieuwe titel opgeven zodat de twee toepassingen, visueel verschilt. U moet ook een unieke naam opgeven. Deze configuraties kunnen worden weergegeven in de volgende opdracht.

```bash
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Binnenkomende route maken

Beide toepassingen zijn nu echter die worden uitgevoerd op uw Kubernetes-cluster zijn geconfigureerd met een service van het type `ClusterIP`. De toepassingen zijn daarom niet toegankelijk is vanaf internet. Deze beschikbaar te maken, moet u een Kubernetes ingress-resource maken. De resource met inkomend verkeer configureert u de regels die verkeer naar een van de twee toepassingen routeren.

Maken van een bestandsnaam `hello-world-ingress.yaml` en kopieer de volgende YAML.

Houd er rekening mee houden dat het verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/` wordt doorgestuurd naar de service met de naam `aks-helloworld`. Verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-prod
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

De resource met inkomend verkeer met maakt de `kubectl apply` opdracht.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>De configuratie van de inkomende gegevens testen

Blader naar de FQDN-naam van de controller voor binnenkomend verkeer van Kubernetes, ziet u de hello world-toepassing.

![Voorbeeld van de toepassing een](media/ingress/app-one.png)

Blader nu naar de FQDN-naam van de controller voor binnenkomend verkeer met de `/hello-world-two` pad, ziet u de hello world-toepassing met de aangepaste titel.

![Voorbeeld van de toepassing twee](media/ingress/app-two.png)

U ziet ook dat de verbinding is versleuteld en dat een certificaat dat is uitgegeven door laten versleutelen wordt gebruikt.

![Hiermee kunt versleutelen van certificaat](media/ingress/certificate.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de software die in dit document wordt gedemonstreerd.

- [Helm CLI][helm-cli]
- [NGINX-controller voor binnenkomend verkeer][nginx-ingress]
- [certificaat-manager][cert-manager]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
