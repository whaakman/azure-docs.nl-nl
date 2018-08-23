---
title: Draft gebruiken met AKS en Azure Container Registry
description: Draft gebruiken met AKS en Azure Container Registry
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/15/2018
ms.author: iainfou
ms.openlocfilehash: a64ada61b2edd0a5c5d2314125b7e2a23444a398
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42058680"
---
# <a name="use-draft-with-azure-kubernetes-service-aks"></a>Draft gebruiken met Azure Kubernetes Service (AKS)

Concept is een open-source-hulpprogramma dat helpt pakket en implementeren van toepassingscontainers in een Kubernetes-cluster, zodat u zich kunt concentreren op de dev-cyclus--de 'inner loop' geconcentreerd ontwikkelen van. Concept werkt als de code wordt ontwikkeld, maar voordat u vastlegt op versiebeheer. Met Draft, kunt u snel een Kubernetes-toepassing te implementeren als er codewijzigingen optreden. Zie voor meer informatie over ontwerp, de [ontwerp-documentatie op Github][draft-documentation].

Dit artikel ziet u hoe u kunt Draft gebruiken met een Kubernetes-cluster in AKS.

## <a name="prerequisites"></a>Vereisten

De stappen die worden beschreven in dit artikel wordt ervan uitgegaan dat u een AKS-cluster hebt gemaakt en hebben een `kubectl` verbinding met het cluster. Als u deze items nodig hebt, raadpleegt u de [Quick Start voor AKS][aks-quickstart].

U moet een privé dockerregister in Azure Container Registry (ACR). Zie voor stappen voor het maken van een ACR-exemplaar de [quickstart voor Azure Container Registry][acr-quickstart].

Helm moet ook worden geïnstalleerd in uw AKS-cluster. Zie voor meer informatie over het installeren en configureren van Helm [gebruik Helm met Azure Kubernetes Service (AKS)][aks-helm].

Tot slot moet u [Docker](https://www.docker.com).

## <a name="install-draft"></a>Draft installeren

De CLI concept is een client die wordt uitgevoerd op uw systeem voor de ontwikkeling en kunt u voor het implementeren van code in een Kubernetes-cluster. Wilt u de CLI Draft installeren op een Mac `brew`. Zie voor aanvullende installatie-opties, de [Draft installatiehandleiding][draft-documentation].

> [!NOTE]
> Als u voorafgaand aan versie 0,12 concept hebt geïnstalleerd, moet u eerst concept verwijderen van uw cluster met `helm delete --purge draft` en verwijder vervolgens de lokale configuratie door te voeren `rm -rf ~/.draft`. Als u van Mac OS gebruikmaakt, voert u `brew upgrade draft`.

```console
brew tap azure/draft
brew install draft
```

Nu initialiseren concept met de `draft init` opdracht:

```console
draft init
```

## <a name="configure-draft"></a>Configureer Draft

Draft lokaal de containerinstallatiekopieën bouwt en wordt vervolgens een ze geïmplementeerd vanuit het lokale register (zoals met Minikube), of gebruikmaakt van een installatiekopieregister die u opgeeft. In dit artikel wordt gebruikgemaakt van Azure Container Registry (ACR), zodat u moet een vertrouwensrelatie tot stand brengen tussen uw AKS-cluster en het ACR-register, en configureer vervolgens Draft als u wilt uw containerinstallatiekopieën naar ACR te pushen.

### <a name="create-trust-between-aks-cluster-and-acr"></a>Een vertrouwensrelatie tussen het AKS-cluster en ACR maken

Als u wilt de vertrouwensrelatie tussen een AKS-cluster en een ACR-register, verleent u machtigingen voor de Azure Active Directory service-principal gebruikt door het AKS-cluster voor toegang tot het ACR-register. In de volgende opdrachten uit, Geef uw eigen `<resourceGroupName>`, Vervang `<aksName>` met de naam van uw AKS-cluster en vervang `<acrName>` met de naam van uw ACR-register:

```azurecli
# Get the service principal ID of your AKS cluster
AKS_SP_ID=$(az aks show --resource-group <resourceGroupName> --name <aksName> --query "servicePrincipalProfile.clientId" -o tsv)

# Get the resource ID of your ACR instance
ACR_RESOURCE_ID=$(az acr show --resource-group <resourceGroupName> --name <acrName> --query "id" -o tsv)

# Create a role assignment for your AKS cluster to access the ACR instance
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

Zie voor meer informatie over deze stappen voor toegang tot ACR [verifiëren met ACR](../container-registry/container-registry-auth-aks.md).

### <a name="configure-draft-to-push-to-and-deploy-from-acr"></a>Configureer Draft om te pushen naar en vanuit ACR implementeren

Er is een vertrouwensrelatie tussen het AKS en ACR, schakel het gebruik van de ACR van uw AKS-cluster.

1. Het configureren van Draft *register* waarde. Vervang in de volgende opdrachten, `<acrName>` met de naam van uw ACR-register:

    ```console
    draft config set registry <acrName>.azurecr.io
    ```

1. Meld u aan bij het ACR-register met [az acr login][az-acr-login]:

    ```azurecli
    az acr login --name <acrName>
    ```

Nadat er een vertrouwensrelatie tussen het AKS en ACR is gemaakt, zijn geen wachtwoorden of geheime informatie vereist voor het pushen naar of ophalen uit het ACR-register. Verificatie gebeurt op het niveau van Azure Resource Manager met behulp van Azure Active Directory.

## <a name="run-an-application"></a>Een toepassing uitvoeren

Om het concept in actie zien, gaat u implementeren een voorbeeldtoepassing van de [Draft-opslagplaats][draft-repo]. Kloon eerst de opslagplaats:

```console
git clone https://github.com/Azure/draft
```

Ga naar de map met Java-voorbeelden:

```console
cd draft/examples/example-java/
```

Gebruik de `draft create` opdracht om het proces te starten. Deze opdracht maakt u de artefacten die worden gebruikt voor de toepassing uitvoert in een Kubernetes-cluster. Deze items zijn onder andere een docker-bestand, een Helm-grafiek en een *draft.toml* -bestand, dat het configuratiebestand van het concept is.

```
$ draft create

--> Draft detected Java (92.205567%)
--> Ready to sail
```

Als u wilt de voorbeeldtoepassing uitvoert in uw AKS-cluster, gebruikt u de `draft up` opdracht. Met deze opdracht bouwt de docker-bestand voor het maken van een containerinstallatiekopie, wordt de image gepusht naar ACR en tot slot installeert de Helm-diagram voor het starten van de toepassing in AKS.

De eerste keer dat deze opdracht wordt uitgevoerd, kunnen pushen en pullen van de container-installatiekopie enige tijd duren. Zodra de basis-lagen in de cache opgeslagen zijn, wordt de gebruikte tijd voor het implementeren van de toepassing aanzienlijk verminderd.

```
$ draft up

Draft Up Started: 'example-java': 01CMZAR1F4T1TJZ8SWJQ70HCNH
example-java: Building Docker Image: SUCCESS ⚓  (73.0720s)
example-java: Pushing Docker Image: SUCCESS ⚓  (19.5727s)
example-java: Releasing Application: SUCCESS ⚓  (4.6979s)
Inspect the logs with `draft logs 01CMZAR1F4T1TJZ8SWJQ70HCNH`
```

Als er problemen zijn met de Docker-installatiekopie pushen is, zorgt u ervoor dat u zich met succes hebt aangemeld bij uw ACR-register met [az acr login][az-acr-login], probeer de `draft up` opdracht opnieuw uit.

## <a name="test-the-application-locally"></a>De toepassing lokaal testen

U kunt de toepassing testen met de `draft connect` opdracht. Deze opdracht een beveiligde verbinding met de Kubernetes-schil proxy's. Als u klaar bent, kan de toepassing worden geopend op de opgegeven URL.

> [!NOTE]
> Het duurt enkele minuten duren voordat de installatiekopie van de container worden gedownload en de toepassing te starten. Als u een foutbericht ontvangt bij het openen van de toepassing, probeert u de verbinding.

```
$ draft connect

Connect to java:4567 on localhost:49804
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567
```

Voor toegang tot uw toepassing, open een webbrowser het adres en poort is opgegeven in de `draft connect` uitvoer, zoals *http://localhost:49804*. 

![Voorbeeld van Java-app die wordt uitgevoerd met Draft](media/kubernetes-draft/sample-app.png)

Gebruik `Control+C` de proxyverbinding te stoppen.

> [!NOTE]
> U kunt ook de `draft up --auto-connect` opdracht uit om te bouwen en implementeren van uw toepassing vervolgens direct verbinding maken met de eerste container die wordt uitgevoerd.

## <a name="access-the-application-on-the-internet"></a>Toegang tot de toepassing op het internet

De vorige stap een proxyverbinding met de toepassingsschil in uw AKS-cluster gemaakt. Als u ontwikkelt en uw toepassing test, kunt u de toepassing beschikbaar maken op het internet. Als u een toepassing op internet beschikbaar, wilt u een Kubernetes-service maken met een type [LoadBalancer][kubernetes-service-loadbalancer], of maak een [controller voor binnenkomend verkeer] [ kubernetes-ingress]. We maken een *LoadBalancer* service.

Eerst, werken de *values.yaml* concept pack om op te geven dat een service met een type *LoadBalancer* moet worden gemaakt:

```console
vi charts/java/values.yaml
```

Zoek de *service.type* eigenschap en werk de waarde van *ClusterIP* naar *LoadBalancer*, zoals weergegeven in het volgende verkorte voorbeeld:

```yaml
[...]
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
[...]
```

Sla op en sluit het bestand en gebruik `draft up` de toepassing opnieuw uit te voeren:

```console
draft up
```

Het duurt een paar minuten voor de service om terug te keren een openbaar IP-adres. U kunt de voortgang controleren met de `kubectl get service` opdracht met de *watch* parameter:

```console
kubectl get service --watch
```

In eerste instantie de *externe IP-adres* voor de service wordt weergegeven als *in behandeling*:

```
NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
example-java-java   LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
```

Zodra het externe IP-adres is gewijzigd van *in behandeling* aan een IP-adres gebruiken `Control+C` stoppen de `kubectl` proces bekijken:

```
NAME                TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
example-java-java   LoadBalancer   10.0.141.72   52.175.224.118  80:32150/TCP   7m
```

Als u wilt zien van de toepassing, blader naar het externe IP-adres van de load balancer met `curl`:

```
$ curl 52.175.224.118

Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>De toepassing herhalen

Nu dat concept is geconfigureerd en de toepassing wordt uitgevoerd in Kubernetes, stelt u zijn voor code iteratie. Code telkens wanneer u de bijgewerkte wilt testen, voert u de `draft up` opdracht voor het bijwerken van de toepassing die wordt uitgevoerd.

Bijwerken van de Java-voorbeeldtoepassing om te wijzigen van de weergegeven tekst in dit voorbeeld. Open de *Hello.java* bestand:

```console
vi src/main/java/helloworld/Hello.java
```

Bijwerken van de weer te geven, uitvoertekst *Hello World, ik ben Java in AKS!*:

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
}
```

Voer de `draft up` opdracht om de toepassing opnieuw te implementeren:

```console
$ draft up

Draft Up Started: 'example-java': 01CMZC9RF0TZT7XPWGFCJE15X4
example-java: Building Docker Image: SUCCESS ⚓  (25.0202s)
example-java: Pushing Docker Image: SUCCESS ⚓  (7.1457s)
example-java: Releasing Application: SUCCESS ⚓  (3.5773s)
Inspect the logs with `draft logs 01CMZC9RF0TZT7XPWGFCJE15X4`
```

Als u wilt zien van de bijgewerkte toepassing, moet u het IP-adres van de load balancer opnieuw curl:

```
$ curl 52.175.224.118

Hello World, I'm Java in AKS!
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het gebruik van concept, raadpleegt u de ontwerp-documentatie op GitHub.

> [!div class="nextstepaction"]
> [Concept-documentatie][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
[draft-repo]: https://github.com/Azure/draft

<!-- LINKS - internal -->
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-helm]: ./kubernetes-helm.md
[kubernetes-ingress]: ./ingress.md
[aks-quickstart]: ./kubernetes-walkthrough.md
[az-acr-login]: /cli/azure/acr#az-acr-login
