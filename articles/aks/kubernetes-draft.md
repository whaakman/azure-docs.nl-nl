---
title: Concept gebruiken met AKS en Azure Container register
description: Concept gebruiken met AKS en Azure Container register
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a77e214c1138ce936b2ec6c521950704e5beb3ff
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="use-draft-with-azure-container-service-aks"></a>Concept gebruiken met Azure Containerservice (AKS)

Concept is een open source-hulpprogramma dat pakket helpt code en voer deze in een cluster Kubernetes. Concept is gericht op de ontwikkelingscyclus herhaling; Als de code wordt ontwikkeld, maar alvorens toe te wijzen aan versiebeheer. Met het concept, u kunt snel opnieuw implementeren een toepassing op Kubernetes als codewijzigingen optreden. Zie voor meer informatie over ontwerp, de [concept documentatie op Github][draft-documentation].

Dit Documentdetails concept gebruiken met een cluster Kubernetes op AKS.

## <a name="prerequisites"></a>Vereisten

Bij de stappen die in dit document worden uiteengezet, wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en een kubectl-verbinding met het cluster hebt gemaakt. Als u deze items moet, Zie de [AKS Quick Start][aks-quickstart].

U moet ook een persoonlijke Docker-register in Azure Container register (ACR). Zie voor instructies over het implementeren van een ACR-exemplaar de [Quick Start Azure Container register][acr-quickstart].

## <a name="install-helm"></a>Helm installeren

De CLI Helm is een client die wordt uitgevoerd op uw ontwikkelsysteem en kunt u starten, stoppen en beheren van toepassingen met Helm grafieken.

U kunt de CLI Helm installeren op een Mac met `brew`. Zie voor aanvullende installatie-opties, [Helm installeren][install-helm].

```console
brew install kubernetes-helm
```

Uitvoer:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="install-draft"></a>Concept installeren

De CLI concept is een client die wordt uitgevoerd op uw ontwikkelsysteem, en kunt dat u snel code implementeren in een cluster met Kubernetes.

Het concept CLI installeren op een Mac-gebruik `brew`. Zie voor aanvullende installatie-opties, wordt de [concept installeren handleiding][install-draft].

```console
brew install draft
```

Uitvoer:

```
==> Installing draft from azure/draft
==> Downloading https://azuredraft.blob.core.windows.net/draft/draft-v0.7.0-darwin-amd64.tar.gz
Already downloaded: /Users/neilpeterson/Library/Caches/Homebrew/draft-0.7.0.tar.gz
==> /usr/local/Cellar/draft/0.7.0/bin/draft init --client-only
🍺  /usr/local/Cellar/draft/0.7.0: 6 files, 61.2MB, built in 1 second
```

## <a name="configure-draft"></a>Concept configureren

Wanneer u concept configureert, moet een container-register worden opgegeven. In dit voorbeeld wordt Azure Container register gebruikt.

Voer de volgende opdracht om op te halen van de naam en de aanmeldingsnaam van server van uw ACR-exemplaar. De opdracht bijwerken met de naam van de resourcegroep met uw ACR-exemplaar.

```console
az acr list --resource-group <resource group> --query "[].{Name:name,LoginServer:loginServer}" --output table
```

Het wachtwoord ACR-exemplaar is ook nodig.

Voer de volgende opdracht om het wachtwoord ACR retourneren. De opdracht bijwerken met de naam van het ACR-exemplaar.

```console
az acr credential show --name <acr name> --query "passwords[0].value" --output table
```

Initialiseren van concept met de `draft init` opdracht.

```console
draft init
```

Tijdens dit proces wordt u gevraagd referenties voor de container register. Wanneer u een Azure Container register, de Register-URL is de naam van de ACR aanmelding-server, de gebruikersnaam is de naam van het ACR-exemplaar en het wachtwoord is het wachtwoord ACR.

```console
1. Enter your Docker registry URL (e.g. docker.io/myuser, quay.io/myuser, myregistry.azurecr.io): <ACR Login Server>
2. Enter your username: <ACR Name>
3. Enter your password: <ACR Password>
```

Hierna kunt concept is geconfigureerd in het cluster Kubernetes en klaar voor gebruik.

```
Draft has been installed into your Kubernetes Cluster.
Happy Sailing!
```

## <a name="run-an-application"></a>Een toepassing uitvoeren

De opslagplaats concept bevat verschillende voorbeeldtoepassingen die kunnen worden gebruikt voor demo concept. Maak een gekloonde kopie van de opslagplaats.

```console
git clone https://github.com/Azure/draft
```

Wijzig in de map met voorbeelden van Java.

```console
cd draft/examples/java/
```

Gebruik de `draft create` opdracht om het proces te starten. Deze opdracht wordt de artefacten die worden gebruikt voor de toepassing uitvoert in een cluster Kubernetes. Deze items zijn een Dockerfile, een grafiek Helm en een `draft.toml` bestand, dat het configuratiebestand concept.

```console
draft create
```

Uitvoer:

```
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Om de toepassing uitvoert op een cluster Kubernetes, gebruiken de `draft up` opdracht. Met deze opdracht de code en configuratie toepassingsbestanden naar het cluster Kubernetes geüpload. Vervolgens wordt uitgevoerd de Dockerfile voor het maken van een installatiekopie van een container, stuurt de afbeelding in het register van de container, en ten slotte wordt uitgevoerd van de grafiek Helm om de toepassing te starten.

```console
draft up
```

Uitvoer:

```
Draft Up Started: 'open-jaguar'
open-jaguar: Building Docker Image: SUCCESS ⚓  (28.0342s)
open-jaguar: Pushing Docker Image: SUCCESS ⚓  (7.0647s)
open-jaguar: Releasing Application: SUCCESS ⚓  (4.5056s)
open-jaguar: Build ID: 01BW3VVNZYQ5NQ8V1QSDGNVD0S
```

## <a name="test-the-application"></a>De toepassing testen

U kunt de toepassing testen met de `draft connect` opdracht. Deze opdracht proxy's een verbinding met de Kubernetes schil zodat een veilige verbinding met lokale. Na voltooiing wordt de toepassing toegankelijk is op de opgegeven URL.

In sommige gevallen kan het enkele minuten duren voordat de container-afbeelding die moet worden gedownload en de toepassing te starten duren. Als u een foutbericht ontvangt bij het openen van de toepassing, probeert u de verbinding.

```console
draft connect
```

Uitvoer:

```
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Wanneer u klaar bent met het gebruik van de toepassing testen `Control+C` stoppen van de proxyverbinding.

## <a name="expose-application"></a>Toepassing weergeven

Wanneer u een toepassing test in Kubernetes, is het raadzaam om de toepassing beschikbaar maken op het internet. U kunt dit doen met een Kubernetes-service van het type [LoadBalancer] [ kubernetes-service-loadbalancer] of een [inkomend controller][kubernetes-ingress]. Dit documentgegevens met een Kubernetes-service.


Eerst het concept pack moet worden bijgewerkt om aan te geven dat een service met een type `LoadBalancer` moet worden gemaakt. Om dit te doen bijwerken van het type van de service in de `values.yaml` bestand.

```console
vi chart/java/values.yaml
```

Zoek de `service.type` eigenschap en werk de waarde van `ClusterIP` naar `LoadBalancer`.

```yaml
replicaCount: 2
image:
  repository: openjdk
  tag: 8-jdk-alpine
  pullPolicy: IfNotPresent
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
  ```

Voer `draft up` opnieuw uitvoeren van de toepassing.

```console
draft up
```

Het kan enkele minuten duren voordat de Service te retourneren van een openbaar IP-adres. Gebruik van de voortgang controleren de `kubectl get service` opdracht met een controle.

```console
kubectl get service -w
```

In eerste instantie de *extern IP-* voor de service wordt weergegeven als `pending`.

```
deadly-squid-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

Nadat u het externe IP-adres is gewijzigd van `pending` naar een `IP address`, gebruik `Control+C` het proces van de controle kubectl te stoppen.

```
deadly-squid-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
```

Overzicht van de toepassing, blader naar het externe IP-adres.

```console
curl 52.175.224.118
```

Uitvoer:

```
Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>De toepassing herhalen

Nu dat concept is geconfigureerd en de toepassing wordt uitgevoerd in Kubernetes, stelt u zijn voor code herhaling. Elke keer dat u wilt testen bijgewerkte code, voert u de `draft up` opdracht voor het bijwerken van de toepassing uitgevoerd.

Werk de Java-toepassing hello world in dit voorbeeld.

```console
vi src/main/java/helloworld/Hello.java
```

De tekst Hello World bijwerken.

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java - Draft Rocks!");
    }
}
```

Voer de `draft up` opdracht voor het implementeren van de toepassing.

```console
draft up
```

Uitvoer

```
Draft Up Started: 'deadly-squid'
deadly-squid: Building Docker Image: SUCCESS ⚓  (18.0813s)
deadly-squid: Pushing Docker Image: SUCCESS ⚓  (7.9394s)
deadly-squid: Releasing Application: SUCCESS ⚓  (6.5005s)
deadly-squid: Build ID: 01BWK8C8X922F5C0HCQ8FT12RR
```

Ten slotte de toepassing om te zien van de updates weergeven.

```console
curl 52.175.224.118
```

Uitvoer:

```
Hello World, I'm Java - Draft Rocks!
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van concept, de ontwerp-documentatie op GitHub.

> [!div class="nextstepaction"]
> [Concept documentatie][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[install-draft]: https://github.com/Azure/draft/blob/master/docs/install.md
[install-helm]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[kubernetes-ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer

<!-- LINKS - internal -->
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-quickstart]: ./kubernetes-walkthrough.md