---
title: Concept gebruiken met AKS en Azure Container register
description: Concept gebruiken met AKS en Azure Container register
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2ab79e3a6308d01d836a82f356f43eccb6af9791
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="use-draft-with-azure-container-service-aks"></a>Concept gebruiken met Azure Containerservice (AKS)

Concept is een open-source hulpprogramma waarmee bevatten en implementeren van deze containers in een cluster Kubernetes, zodat u zich kunt concentreren op basis van de dev-cyclus--de 'interne loop' van geconcentreerd ontwikkeling. Concept werkt als de code wordt ontwikkeld, maar alvorens toe te wijzen aan versiebeheer. Met het concept, u kunt snel opnieuw implementeren een toepassing op Kubernetes als codewijzigingen optreden. Zie voor meer informatie over ontwerp, de [concept documentatie op Github][draft-documentation].

Dit Documentdetails concept gebruiken met een cluster Kubernetes op AKS.

## <a name="prerequisites"></a>Vereisten

Bij de stappen die in dit document worden uiteengezet, wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en een kubectl-verbinding met het cluster hebt gemaakt. Als u deze items moet, Zie de [AKS Quick Start][aks-quickstart].

U moet ook een persoonlijke Docker-register in Azure Container register (ACR). Zie voor instructies over het implementeren van een ACR-exemplaar de [Quick Start Azure Container register][acr-quickstart].

Helm moet ook worden geïnstalleerd in uw cluster AKS. Zie voor meer informatie over het installeren van helm [Helm voor gebruik met Azure Container Service (AKS)][aks-helm].

Tot slot moet u [Docker](https://www.docker.com).

## <a name="install-draft"></a>Concept installeren

De CLI concept is een client die wordt uitgevoerd op uw ontwikkelsysteem, en kunt dat u snel code implementeren in een cluster met Kubernetes. 

> [!NOTE] 
> Als concept voorafgaand aan versie 0,12 is geïnstalleerd, moet u eerst concept verwijderen uit uw cluster met `helm delete --purge draft` en verwijder vervolgens de lokale configuratie door te voeren `rm -rf ~/.draft`. U kunt uitvoeren als u van Mac OS gebruikmaakt, `brew upgrade draft`.

Het concept CLI installeren op een Mac-gebruik `brew`. Zie voor aanvullende installatie-opties, wordt de [concept installeren handleiding][install-draft].

```console
brew tap azure/draft
brew install draft
```

Nu initialiseren concept met de `draft init` opdracht.

```console
draft init
```

## <a name="configure-draft"></a>Concept configureren

Concept maakt de installatiekopieën van de container lokaal en vervolgens ofwel implementeert uit het lokale register (bij Minikube), of moet u het register van de afbeelding moet worden gebruikt. In dit voorbeeld wordt de Azure Container register (ACR), dus moet u een vertrouwensrelatie tot stand tussen uw cluster AKS en het register ACR brengen en concept voor het pushen van de container naar ACR configureren.

### <a name="create-trust-between-aks-cluster-and-acr"></a>Een vertrouwensrelatie tussen AKS cluster en ACR maken

Als u wilt een vertrouwensrelatie tussen een AKS-cluster en een ACR-register, moet u de Azure Active Directory Service Principal gebruikt met AKS door de rol van Inzender toe te voegen met het bereik van de opslagplaats ACR wijzigen. Om dit te doen, voer de volgende opdrachten, vervangen _&lt;aks-rg-name&gt;_ en _&lt;aks clusternaam&gt;_ met de resourcegroep en de naam van uw AKS cluster, en _&lt;acr-rg-naam&gt;_ en _&lt;acr-repo-name&gt;_ met de resourcenaam voor groep en de opslagplaats van uw ACR de opslagplaats die u wilt een vertrouwensrelatie maken.

```console
export AKS_SP_ID=$(az aks show -g <aks-rg-name> -n <aks-cluster-name> --query "servicePrincipalProfile.clientId" -o tsv)
export ACR_RESOURCE_ID=$(az acr show -g <acr-rg-name> -n <acr-repo-name> --query "id" -o tsv)
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

(Deze stappen en andere verificatiemethoden voor toegang tot ACR lopen [verificatie met ACR](../container-registry/container-registry-auth-aks.md).)

### <a name="configure-draft-to-push-to-and-deploy-from-acr"></a>Concept voor het pushen naar en implementeren van ACR configureren

Nu dat er een vertrouwensrelatie tussen AKS en ACR is, wordt in de volgende stappen uit het gebruik van ACR van het cluster AKS inschakelen.
1. Stel de configuratie van het concept `registry` waarde door het uitvoeren van `draft config set registry <registry name>.azurecr.io`, waarbij _&lt;naam van routeringsregister&lt;_ is de naam van uw ACR-register.
2. Meld u aan bij het register ACR door het uitvoeren van `az acr login -n <registry name>`. 

Omdat u bent nu aangemeld op lokaal ACR en u een vertrouwensrelatie met AKS en ACR hebt gemaakt, zijn geen wachtwoorden of geheimen naar push of pull van ACR in AKS vereist. De verificatie vindt op het niveau van Azure Resource Manager met Azure Active Directory. 

## <a name="run-an-application"></a>Een toepassing uitvoeren

De opslagplaats concept bevat verschillende voorbeeldtoepassingen die kunnen worden gebruikt voor demo concept. Maak een gekloonde kopie van de opslagplaats.

```console
git clone https://github.com/Azure/draft
```

Wijzig in de map met voorbeelden van Java.

```console
cd draft/examples/example-java/
```

Gebruik de `draft create` opdracht om het proces te starten. Deze opdracht wordt de artefacten die worden gebruikt voor de toepassing uitvoert in een cluster Kubernetes. Deze items zijn een Dockerfile, een grafiek Helm en een `draft.toml` bestand, dat het configuratiebestand concept.

```console
draft create
```

Uitvoer:

```console
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Om de toepassing uitvoert op een cluster Kubernetes, gebruiken de `draft up` opdracht. Met deze opdracht maakt de Dockerfile voor het maken van een installatiekopie van een container, de installatiekopie van het verstuurd naar ACR en tot slot installeert de grafiek Helm voor het starten van de toepassing in AKS.

De eerste keer die wordt uitgevoerd, kunnen worden gepusht en binnenhalen van de installatiekopie van de container enige tijd duren; Zodra de base lagen in cache zijn opgeslagen, is de tijd is aanzienlijk worden verminderd.

```console
draft up
```

Uitvoer:

```console
Draft Up Started: 'example-java'
example-java: Building Docker Image: SUCCESS ⚓  (1.0003s)
example-java: Pushing Docker Image: SUCCESS ⚓  (3.0007s)
example-java: Releasing Application: SUCCESS ⚓  (0.9322s)
example-java: Build ID: 01C9NPDYQQH2CZENDMZW7ESJAM
Inspect the logs with `draft logs 01C9NPDYQQH2CZENDMZW7ESJAM`
```

## <a name="test-the-application"></a>De toepassing testen

U kunt de toepassing testen met de `draft connect` opdracht. Deze opdracht proxy's een verbinding met de Kubernetes schil zodat een veilige verbinding met lokale. Na voltooiing wordt de toepassing toegankelijk is op de opgegeven URL.

In sommige gevallen kan het enkele minuten duren voordat de container-afbeelding die moet worden gedownload en de toepassing te starten duren. Als u een foutbericht ontvangt bij het openen van de toepassing, probeert u de verbinding.

```console
draft connect
```

Uitvoer:

```console
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

U kunt nu uw toepassing testen door te bladeren naar http://localhost:46143 (voor het voorgaande voorbeeld; de poort mogelijk andere). Wanneer u klaar bent met het gebruik van de toepassing testen `Control+C` stoppen van de proxyverbinding.

> [!NOTE]
> U kunt ook de `draft up --auto-connect` opdracht voor het bouwen en implementeren van uw toepassing en direct verbinding maken met de eerste actieve container te maken van de herhaling sneller bladeren.

## <a name="expose-application"></a>Toepassing weergeven

Wanneer u een toepassing test in Kubernetes, is het raadzaam om de toepassing beschikbaar maken op het internet. U kunt dit doen met een Kubernetes-service van het type [LoadBalancer] [ kubernetes-service-loadbalancer] of een [inkomend controller][kubernetes-ingress]. Dit documentgegevens met een Kubernetes-service.


Eerst het concept pack moet worden bijgewerkt om aan te geven dat een service met een type `LoadBalancer` moet worden gemaakt. Om dit te doen bijwerken van het type van de service in de `values.yaml` bestand.

```console
vi charts/java/values.yaml
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
example-java-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

Zodra het EXTERNE IP-adres is gewijzigd van `pending` in een `IP address`, gebruikt u `Control+C` om het controleproces van kubectl te stoppen.

```
example-java-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
```

Als u de toepassing wilt zien, bladert u naar het externe IP-adres.

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
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
}
```

Voer de `draft up --auto-connect` opdracht voor het implementeren van de toepassing zodra een schil is gereed om te reageren.

```console
draft up --auto-connect
```

Uitvoer

```
Draft Up Started: 'example-java'
example-java: Building Docker Image: SUCCESS ⚓  (1.0003s)
example-java: Pushing Docker Image: SUCCESS ⚓  (4.0010s)
example-java: Releasing Application: SUCCESS ⚓  (1.1336s)
example-java: Build ID: 01C9NPMJP6YM985GHKDR2J64KC
Inspect the logs with `draft logs 01C9NPMJP6YM985GHKDR2J64KC`
Connect to java:4567 on localhost:39249
Your connection is still active.
Connect to java:4567 on localhost:39249
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567

```

Ten slotte de toepassing om te zien van de updates weergeven.

```console
curl 52.175.224.118
```

Uitvoer:

```
Hello World, I'm Java in AKS!
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van concept, de ontwerp-documentatie op GitHub.

> [!div class="nextstepaction"]
> [Concept documentatie][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[install-draft]: https://github.com/Azure/draft/blob/master/docs/install.md
[kubernetes-ingress]: ./ingress.md
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer

<!-- LINKS - internal -->
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-helm]: ./kubernetes-helm.md
[aks-quickstart]: ./kubernetes-walkthrough.md
