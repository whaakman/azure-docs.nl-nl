---
title: Inkomende gegevens met Azure Container Service (AKS)-cluster configureren
description: Installeer en configureer een NGINX ingress-domeincontroller in een Azure Container Service (AKS)-cluster.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/03/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 4294169e89533150cade700fb89e14c4121c4404
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="https-ingress-on-azure-container-service-aks"></a>HTTPS-inkomend op Azure Containerservice (AKS)

Een domeincontroller inkomend is een onderdeel van de software die biedt reverse proxy worden geconfigureerd voor verkeersroutering en TLS beëindiging voor Kubernetes services. Kubernetes inkomend bronnen worden gebruikt voor het configureren van de regels van toegangsroutes en routes voor afzonderlijke Kubernetes services. Een domeincontroller inkomend en inkomend regels gebruikt, kan één externe adres voor het routeren van verkeer naar meerdere services in een cluster Kubernetes worden gebruikt.

Dit document wordt begeleid bij de Voorbeeldimplementatie van een van de [NGINX inkomend controller] [ nginx-ingress] in een Azure Container Service (AKS)-cluster. Bovendien de [KUBE Bouwstenen] [ kube-lego] project wordt gebruikt om automatisch te genereren en configureer [we versleutelen] [ lets-encrypt] certificaten. Ten slotte worden verschillende toepassingen uitgevoerd in het cluster AKS, die toegankelijk via één adres is.

## <a name="install-an-ingress-controller"></a>Een domeincontroller inkomend installeren

Helm gebruiken om de NGINX inkomend controller te installeren. De controller wordt NGINX inkomend [documentatie] [ nginx-ingress] voor gedetailleerde implementatie-informatie. 

Bijwerken van de grafiek-opslagplaats.

```console
helm repo update
```

De NGINX inkomend controller installeren. Dit voorbeeld installeert de controller in de `kube-system` naamruimte, dit kan worden gewijzigd met een naamruimte van uw keuze.

```
helm install stable/nginx-ingress --namespace kube-system
```

Tijdens de installatie wordt een Azure openbare IP-adres voor de controller inkomend gemaakt. Als u het openbare IP-adres, gebruikt u de opdracht kubectl get-service. Het kan even duren voor het IP-adres moet worden toegewezen aan de service.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   13.82.238.45   80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>         80/TCP                       20m
```

Omdat de inkomende gegevens zijn geen regels hebt gemaakt, als u naar het openbare IP-adres bladeren, wordt u doorgestuurd naar de NGINX inkomend domeincontrollers standaard 404-pagina.

![Standaard NGINX-back-end](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Configureer DNS-naam

Aangezien HTTPS-certificaten worden gebruikt, moet u een FQDN-naam voor het inkomend domeincontrollers IP-adres configureren. In dit voorbeeld wordt een Azure-FQDN gemaakt met de Azure CLI. Het script bijwerken met het IP-adres van de inkomende-controller en de naam die u wilt gebruiken in de FQDN-naam.

```
#!/bin/bash

# Public IP address
IP="52.224.125.195"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get resource group and public ip name
RESOURCEGROUP=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[resourceGroup]" --output tsv)
PIPNAME=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[name]" --output tsv)

# Update public ip address with dns name
az network public-ip update --resource-group $RESOURCEGROUP --name  $PIPNAME --dns-name $DNSNAME
```

Indien nodig, voer de volgende opdracht voor het ophalen van de FQDN-naam. De IP-adres-waarde met die van uw domeincontroller inkomend bijwerken.

```azurecli
az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '52.224.125.195')].[dnsSettings.fqdn]" --output tsv
```

De domeincontroller inkomend is nu toegankelijk zijn via de FQDN-naam.

## <a name="install-kube-lego"></a>KUBE Bouwstenen installeren

De NGINX ingress-controller ondersteunt TLS-beëindiging. Er zijn verschillende manieren op te halen en certificaten configureren voor HTTPS, dit document wordt gedemonstreerd met behulp van [KUBE Bouwstenen][kube-lego], waarmee u automatische [kuntversleutelen] [ lets-encrypt] genereren en de beheerfunctionaliteit van het certificaat.

Gebruik de volgende Helm installeren voor het installeren van de controller KUBE Bouwstenen opdracht. Het e-mailadres bijwerken met een van uw organisatie.

```
helm install stable/kube-lego \
  --set config.LEGO_EMAIL=user@contoso.com \
  --set config.LEGO_URL=https://acme-v01.api.letsencrypt.org/directory
```

Zie voor meer informatie over KUBE Bouwstenen configuratie de [KUBE Bouwstenen documentatie][kube-lego].

## <a name="run-application"></a>Toepassing uitvoeren

Op dit moment zijn een domeincontroller inkomend en een oplossing voor het beheer van certificaten geconfigureerd. Nu u enkele toepassingen uitvoeren in uw cluster AKS.

In dit voorbeeld wordt Helm gebruikt voor het uitvoeren van meerdere exemplaren van de toepassing van een eenvoudig hello world.

Toevoegen voordat u de toepassing, de Azure-voorbeelden Helm opslagplaats op uw ontwikkelsysteem.

```
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

 Voer de AKS Hallo wereld-grafiek met de volgende opdracht:

```
helm install azure-samples/aks-helloworld
```

Nu een tweede exemplaar van de Hallo wereld-toepassing installeren.

Geef voor het tweede exemplaar, een nieuwe titel zo in dat de twee toepassingen visueel distinct. U moet ook een unieke naam opgeven. Deze configuraties kunnen worden weergegeven in de volgende opdracht.

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Inkomend route maken

Beide toepassingen zijn nu echter uitgevoerd op uw cluster Kubernetes is geconfigureerd met een service van het type `ClusterIP`. De toepassingen zijn als zodanig niet toegankelijk zijn vanaf internet. Maak een resource van de inkomende gegevens Kubernetes om te zorgen dat de beschikbaar. De resource inkomend configureert u de regels die verkeer naar een van de twee toepassingen sturen.

Maak een bestandsnaam `hello-world-ingress.yaml` en kopieer de volgende YAML.

Let op dat het verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/` wordt doorgestuurd naar de service met de naam `aks-helloworld`. Verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service.

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/tls-acme: "true"
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

- [NGINX ingress-controller][nginx-ingress]
- [KUBE-LEGO][kube-lego]

<!-- LINKS - external -->
[kube-lego]: https://github.com/jetstack/kube-lego
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
