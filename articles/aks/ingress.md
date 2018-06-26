---
title: Inkomende gegevens met Azure Kubernetes Service (AKS)-cluster configureren
description: Installeer en configureer een inkomend NGINX domeincontroller in een Azure Kubernetes Service (AKS)-cluster.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/25/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: fcf0b6f3b7f6d75006d8c10aab041c25fc0d8c39
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751282"
---
# <a name="https-ingress-on-azure-kubernetes-service-aks"></a>HTTPS-inkomend voor Azure Kubernetes-Service (AKS)

Een domeincontroller inkomend is een onderdeel van de software die biedt reverse proxy worden geconfigureerd voor verkeersroutering en TLS beëindiging voor Kubernetes services. Kubernetes inkomend bronnen worden gebruikt voor het configureren van de regels van toegangsroutes en routes voor afzonderlijke Kubernetes services. Een domeincontroller inkomend en inkomend regels gebruikt, kan één externe adres voor het routeren van verkeer naar meerdere services in een cluster Kubernetes worden gebruikt.

Dit document wordt begeleid bij de Voorbeeldimplementatie van een van de [NGINX inkomend controller] [ nginx-ingress] in een Azure Kubernetes Service (AKS)-cluster. Bovendien de [Certificaatbeheer] [ cert-manager] project wordt gebruikt om automatisch te genereren en configureer [we versleutelen] [ lets-encrypt] certificaten. Ten slotte worden verschillende toepassingen uitgevoerd in het cluster AKS, die toegankelijk via één adres is.

## <a name="install-an-ingress-controller"></a>Een domeincontroller inkomend installeren

Helm gebruiken om de NGINX inkomend controller te installeren. De controller wordt NGINX inkomend [documentatie] [ nginx-ingress] voor gedetailleerde implementatie-informatie.

Dit voorbeeld installeert de controller in de `kube-system` naamruimte, dit kan worden gewijzigd met een naamruimte van uw keuze. Als uw cluster AKS geen RBAC ingeschakeld is, voegt u `--set rbac.create=false` aan de opdracht. Zie voor meer informatie de [nginx-inkomend grafiek][nginx-ingress].

```bash
helm install stable/nginx-ingress --namespace kube-system
```

Tijdens de installatie wordt een Azure openbare IP-adres voor de controller inkomend gemaakt. Als u het openbare IP-adres, gebruikt u de opdracht kubectl get-service. Het kan even duren voor het IP-adres moet worden toegewezen aan de service.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Omdat de inkomende gegevens zijn geen regels hebt gemaakt, als u naar het openbare IP-adres bladeren, wordt u doorgestuurd naar de NGINX inkomend domeincontrollers standaard 404-pagina.

![Standaard NGINX-back-end](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Configureer DNS-naam

Aangezien HTTPS-certificaten worden gebruikt, moet u een FQDN-naam voor het inkomend domeincontrollers IP-adres configureren. In dit voorbeeld wordt een Azure-FQDN gemaakt met de Azure CLI. Het script bijwerken met het IP-adres van de inkomende-controller en de naam die u wilt gebruiken in de FQDN-naam.

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

Nu moet de controller inkomend toegankelijk zijn via de FQDN-naam.

## <a name="install-cert-manager"></a>Cert manager installeren

De NGINX ingress-controller ondersteunt TLS-beëindiging. Er zijn verschillende manieren op te halen en certificaten configureren voor HTTPS, dit document wordt gedemonstreerd met behulp van [Certificaatbeheer][cert-manager], waarmee u automatische [kuntversleutelen] [ lets-encrypt] genereren en de beheerfunctionaliteit van het certificaat.

Gebruik de volgende Helm installeren voor het installeren van de controller Certificaatbeheer opdracht.

```bash
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Als uw cluster geen RBAC ingeschakeld is, gebruikt u deze opdracht.

```bash
helm install stable/cert-manager \
  --set ingressShim.defaultIssuerName=letsencrypt-prod \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

Zie voor meer informatie over Certificaatbeheer configuratie de [Certificaatbeheer project][cert-manager].

## <a name="create-ca-cluster-issuer"></a>CA-verlener voor cluster maken

Voordat certificaten kunnen worden uitgegeven, cert-manager vereist een [verlener] [ cert-manager-issuer] of [ClusterIssuer] [ cert-manager-cluster-issuer] resource. De resources zijn identiek in functionaliteit echter `Issuer` werkt in een enkele naamruimte waar `ClusterIssuer` werkt in alle naamruimten. Zie voor meer informatie de [Certificaatbeheer verlener] [ cert-manager-issuer] documentatie.

Maken van de uitgever van een cluster met behulp van de volgende manifest. Het e-mailadres bijwerken met een geldig adres van uw organisatie.

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

Vervolgens moet een certificaat-bron worden gemaakt. De resource certificaat definieert het gewenste X.509-certificaat. Voor meer informatie ziet, [Certificaatbeheer certificaten][cert-manager-certificates].

De resource certificaat maken met het volgende manifest.

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

Op dit moment zijn een domeincontroller inkomend en een oplossing voor het beheer van certificaten geconfigureerd. Nu u enkele toepassingen uitvoeren in uw cluster AKS.

In dit voorbeeld wordt Helm gebruikt voor het uitvoeren van meerdere exemplaren van de toepassing van een eenvoudig hello world.

Toevoegen voordat u de toepassing, de Azure-voorbeelden Helm opslagplaats op uw ontwikkelsysteem.

```bash
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Voer de AKS Hallo wereld-grafiek met de volgende opdracht:

```bash
helm install azure-samples/aks-helloworld
```

Nu een tweede exemplaar van de Hallo wereld-toepassing installeren.

Geef voor het tweede exemplaar, een nieuwe titel zo in dat de twee toepassingen visueel distinct. U moet ook een unieke naam opgeven. Deze configuraties kunnen worden weergegeven in de volgende opdracht.

```bash
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Inkomend route maken

Beide toepassingen zijn nu echter uitgevoerd op uw cluster Kubernetes is geconfigureerd met een service van het type `ClusterIP`. De toepassingen zijn als zodanig niet toegankelijk zijn vanaf internet. Maak een resource van de inkomende gegevens Kubernetes om te zorgen dat de beschikbaar. De resource inkomend configureert u de regels die verkeer naar een van de twee toepassingen sturen.

Maak een bestandsnaam `hello-world-ingress.yaml` en kopieer de volgende YAML.

Let op dat het verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/` wordt doorgestuurd naar de service met de naam `aks-helloworld`. Verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service.

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

Maak de ingress-resource met de `kubectl apply` opdracht.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Test de configuratie van de inkomende gegevens

Blader naar de FQDN-naam van uw Kubernetes ingress-controller, ziet u de Hallo wereld-toepassing.

![Voorbeeld van de toepassing een](media/ingress/app-one.png)

Nu Blader naar de FQDN-naam van de controller inkomend met de `/hello-world-two` pad, ziet u de toepassing hello world met de aangepaste titel.

![Voorbeeld van de toepassing twee](media/ingress/app-two.png)

U ziet ook dat de verbinding is versleuteld en dat een certificaat zijn uitgegeven door laten versleutelen wordt gebruikt.

![Hiermee kunt versleutelen van certificaat](media/ingress/certificate.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de software die in dit document wordt gedemonstreerd.

- [Helm CLI][helm-cli]
- [NGINX ingress-controller][nginx-ingress]
- [Certificaatbeheer][cert-manager]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
