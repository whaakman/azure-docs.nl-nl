---
title: Ontwikkelen op Azure Kubernetes Service (AKS) met Draft
description: Draft gebruiken met AKS en Azure Container Registry
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 06/20/2019
ms.author: zarhoads
ms.openlocfilehash: bd099b9d76e17eda36be1650ef5081e5aaa7e53a
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303532"
---
# <a name="quickstart-develop-on-azure-kubernetes-service-aks-with-draft"></a>Quickstart: Ontwikkelen op Azure Kubernetes Service (AKS) met Draft

Concept is een open-source-hulpprogramma dat helpt pakket en toepassingscontainers uitvoeren in een Kubernetes-cluster. Met Draft, kunt u snel een Kubernetes-toepassing te implementeren zoals wijzigingen in de code plaatsvinden zonder uw wijzigingen doorgevoerd aan versiebeheer. Zie voor meer informatie over ontwerp, de [ontwerp-documentatie op GitHub][draft-documentation].

Dit artikel leest u hoe het concept-pakket en het uitvoeren van een toepassing op AKS.


## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free) maken.
* [Azure CLI geïnstalleerd](/cli/azure/install-azure-cli?view=azure-cli-latest).
* Docker is geïnstalleerd en geconfigureerd. Docker biedt pakketten die Docker configureren op een [Mac][docker-for-mac], [Windows][docker-for-windows], of [Linux][docker voor linux] system.
* [Helm geïnstalleerd](https://github.com/helm/helm/blob/master/docs/install.md).
* [Draft geïnstalleerd][draft-documentation].

## <a name="create-an-azure-kubernetes-service-cluster"></a>Een Azure Kubernetes Service-cluster maken

Een AKS-cluster maken. De onderstaande opdrachten maakt u een resourcegroep met de naam MyResourceGroup en een AKS-cluster MyAKS genoemd.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --generate-ssh-keys
```

## <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken
Voor het gebruik van concept voor het uitvoeren van uw toepassing in uw AKS-cluster, moet u een Azure Container Registry voor het opslaan van uw containerinstallatiekopieën. Het onderstaande voorbeeld gebruikt [az acr maken][az-acr-create] te maken van een ACR met de naam *MyDraftACR* in de *MyResourceGroup* resourcegroep met de *Basic* SKU. U moet de registernaam van uw eigen unieke opgeven. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. De SKU *Basic* is een toegangspunt voor ontwikkelingsdoeleinden dat is geoptimaliseerd voor kosten, met een balans tussen opslag en doorvoer.

```azurecli
az acr create --resource-group MyResourceGroup --name MyDraftACR --sku Basic
```

De uitvoer lijkt op die in het volgende voorbeeld. Noteer de *loginServer* waarde voor uw ACR, aangezien deze worden gebruikt in een latere stap. In het onderstaande voorbeeld, *mydraftacr.azurecr.io* is de *loginServer* voor *MyDraftACR*.

```console
{
  "adminUserEnabled": false,
  "creationDate": "2019-06-11T13:35:17.998425+00:00",
  "id": "/subscriptions/<ID>/resourceGroups/MyResourceGroup/providers/Microsoft.ContainerRegistry/registries/MyDraftACR",
  "location": "eastus",
  "loginServer": "mydraftacr.azurecr.io",
  "name": "MyDraftACR",
  "networkRuleSet": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "MyResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```


Concept gebruiken van het ACR-exemplaar, moet u eerst zich aanmelden. Gebruik de [az acr login][az-acr-login] opdracht aan te melden. Het onderstaande voorbeeld wordt zich aanmelden bij een ACR met de naam *MyDraftACR*.

```azurecli
az acr login --name MyDraftACR
```

De opdracht retourneert het bericht *Aanmelden geslaagd* wanneer deze is uitgevoerd.

## <a name="create-trust-between-aks-cluster-and-acr"></a>Een vertrouwensrelatie tussen het AKS-cluster en ACR maken

Uw AKS-cluster moet ook toegang tot uw ACR voor het ophalen van de containerinstallatiekopieën en voer ze uit. U kunt toegang naar de ACR van AKS door een vertrouwensrelatie tot stand brengen. Als u wilt de vertrouwensrelatie tussen een AKS-cluster en een ACR-register, verleent u machtigingen voor de Azure Active Directory service-principal gebruikt door het AKS-cluster voor toegang tot het ACR-register. De volgende opdrachten machtigingen verlenen voor de service-principal van de *MyAKS* -cluster in de *MyResourceGroup* naar de *MyDraftACR* ACR in de  *MyResourceGroup*.

```azurecli
# Get the service principal ID of your AKS cluster
AKS_SP_ID=$(az aks show --resource-group MyResourceGroup --name MyAKS --query "servicePrincipalProfile.clientId" -o tsv)

# Get the resource ID of your ACR instance
ACR_RESOURCE_ID=$(az acr show --resource-group MyResourceGroup --name MyDraftACR --query "id" -o tsv)

# Create a role assignment for your AKS cluster to access the ACR instance
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

## <a name="connect-to-your-aks-cluster"></a>Verbinding maken met uw AKS-cluster

Voor verbinding met het Kubernetes-cluster vanaf uw lokale computer, gebruikt u [kubectl][kubectl], de Kubernetes-opdrachtregelclient.

Als u Azure Cloud Shell gebruikt, is `kubectl` al geïnstalleerd. Als u het lokaal wilt installeren, gebruikt u de opdracht [az aks install-cli][]:

```azurecli
az aks install-cli
```

Gebruik de opdracht [az aks get-credentials][] om `kubectl` zodanig te configureren dat er verbinding wordt gemaakt met het Kubernetes-cluster. Het volgende voorbeeld wordt de referenties voor het AKS-cluster met de naam *MyAKS* in de *MyResourceGroup*:

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

## <a name="create-a-service-account-for-helm"></a>Een service-account maken voor Helm

Voordat u Helm in een cluster AKS RBAC is ingeschakeld implementeren kunt, moet u een service-account en een binding van de rol voor de Tiller-service. Voor meer informatie over het beveiligen van Helm / Tiller in een RBAC cluster ingeschakeld, Zie [Tiller, naamruimten en RBAC][tiller-rbac]. Als uw AKS-cluster niet RBAC is ingeschakeld, moet u deze stap overslaan.

Maak een bestand met de naam `helm-rbac.yaml` en kopieer de volgende YAML:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Maken van de service-account en de rol binding met de `kubectl apply` opdracht:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="configure-helm"></a>Helm configureren
Voor het implementeren van een eenvoudige Tiller in een AKS-cluster, gebruikt u de [helm init][helm-init] opdracht. Als uw cluster niet RBAC is ingeschakeld, verwijdert u de `--service-account` argument en de waarde.

```console
helm init --service-account tiller --node-selectors "beta.kubernetes.io/os"="linux"
```

## <a name="configure-draft"></a>Configureer Draft

Als u dit nog niet hebt concept geconfigureerd op uw lokale computer, voert u `draft init`:

```console
$ draft init
Installing default plugins...
Installation of default plugins complete
Installing default pack repositories...
...
Happy Sailing!
```

U moet ook het configureren van Draft gebruiken de *loginServer* van uw ACR. De volgende opdracht maakt gebruik van `draft config set` gebruiken `mydraftacr.azurecr.io` als een register.

```console
draft config set registry mydraftacr.azurecr.io
```

U concept voor het gebruik van uw ACR hebt geconfigureerd en Draft containerinstallatiekopieën naar uw ACR te pushen. Bij het ontwerp uw toepassing in uw AKS-cluster wordt uitgevoerd, zijn geen wachtwoorden of geheimen vereist voor het pushen naar of ophalen uit het ACR-register. Omdat een vertrouwensrelatie tussen het AKS-cluster en de ACR is gemaakt, wordt de verificatie vindt op het niveau van Azure Resource Manager met behulp van Azure Active Directory.

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

Deze snelstartgids maakt gebruik van [een voorbeeld van java-toepassing vanuit het concept GitHub-opslagplaats][example-java]. De toepassing uit GitHub te klonen en navigeer naar de `draft/examples/example-java/` directory.

```console
git clone https://github.com/Azure/draft
cd draft/examples/example-java/
```

## <a name="run-the-sample-application-with-draft"></a>De voorbeeldtoepassing uitvoeren met Draft

Gebruik de `draft create` opdracht voor het voorbereiden van de toepassing.

```console
draft create
```

Deze opdracht maakt u de artefacten die worden gebruikt voor de toepassing uitvoert in een Kubernetes-cluster. Deze items zijn onder andere een docker-bestand, een Helm-grafiek en een *draft.toml* -bestand, dat het configuratiebestand van het concept is.

```console
$ draft create

--> Draft detected Java (92.205567%)
--> Ready to sail
```

Als u wilt de voorbeeldtoepassing uitvoert in uw AKS-cluster, gebruikt u de `draft up` opdracht.

```console
draft up
```

Met deze opdracht bouwt de docker-bestand voor het maken van een containerinstallatiekopie, wordt de image gepusht naar uw ACR en installeert het Helm-diagram voor het starten van de toepassing in AKS. De eerste keer dat u deze opdracht uitvoert kunnen pushen en pullen van de container-installatiekopie enige tijd duren. Zodra de basis-lagen in de cache opgeslagen zijn, wordt de gebruikte tijd voor het implementeren van de toepassing aanzienlijk verminderd.

```
$ draft up

Draft Up Started: 'example-java': 01CMZAR1F4T1TJZ8SWJQ70HCNH
example-java: Building Docker Image: SUCCESS ⚓  (73.0720s)
example-java: Pushing Docker Image: SUCCESS ⚓  (19.5727s)
example-java: Releasing Application: SUCCESS ⚓  (4.6979s)
Inspect the logs with `draft logs 01CMZAR1F4T1TJZ8SWJQ70HCNH`
```

## <a name="connect-to-the-running-sample-application-from-your-local-machine"></a>Verbinding maken met het uitvoeren van uw lokale computer-voorbeeldtoepassing

U kunt de toepassing testen met de `draft connect` opdracht.

```console
draft connect
```

Deze opdracht een beveiligde verbinding met de Kubernetes-schil proxy's. Als u klaar bent, kan de toepassing worden geopend op de opgegeven URL.

```console
$ draft connect

Connect to java:4567 on localhost:49804
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567
```

Navigeer naar de toepassing in een browser met behulp van de `localhost` url om te zien van de voorbeeldtoepassing. In het bovenstaande voorbeeld wordt de url is `http://localhost:49804`. Stop de verbinding met `Ctrl+c`.

## <a name="access-the-application-on-the-internet"></a>Toegang tot de toepassing op het internet

De vorige stap een proxyverbinding met de toepassingsschil in uw AKS-cluster gemaakt. Als u ontwikkelt en uw toepassing test, kunt u de toepassing beschikbaar maken op het internet. Als u een toepassing op internet beschikbaar, wilt u een Kubernetes-service kunt maken met een type [LoadBalancer][kubernetes-service-loadbalancer].

Update `charts/example-java/values.yaml` maken een *LoadBalancer* service. Wijzig de waarde van *service.type* van *ClusterIP* naar *LoadBalancer*.

```yaml
...
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
...
```

Uw wijzigingen hebt opgeslagen, sluit het bestand en uitvoeren `draft up` de toepassing opnieuw uit te voeren.

```console
draft up
```

Het duurt een paar minuten voor de service om terug te keren een openbaar IP-adres. U kunt de voortgang controleren met de `kubectl get service` opdracht met de *watch* parameter:

```console
$ kubectl get service --watch

NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
example-java-java   LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
...
example-java-java   LoadBalancer   10.0.141.72   52.175.224.118  80:32150/TCP   7m
```

Navigeer naar de load balancer van uw toepassing in een browser met behulp van de *externe IP-adres* om te zien van de voorbeeldtoepassing. In het bovenstaande voorbeeld het IP-adres is `52.175.224.118`.

## <a name="iterate-on-the-application"></a>De toepassing herhalen

U kunt uw toepassing sequentieel door lokaal wijzigen en opnieuw uit te voeren `draft up`.

Bijwerken van het bericht geretourneerd op [regel 7 van src/main/java/helloworld/Hello.java][example-java-hello-l7]

```java
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
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

De bijgewerkte toepassing bekijken, gaat u naar het IP-adres van de load balancer opnieuw en controleer of dat uw wijzigingen worden weergegeven.

## <a name="delete-the-cluster"></a>Het cluster verwijderen

Wanneer het cluster niet meer nodig hebt is, gebruikt u de [az group delete][az-group-delete] opdracht voor het verwijderen van de resourcegroep, het AKS-cluster, het containerregister, de containerinstallatiekopieën er opgeslagen en alle gerelateerde resources.

```azurecli-interactive
az group delete --name MyResourceGroup --yes --no-wait
```

> [!NOTE]
> Wanneer u het cluster verwijdert, wordt de Azure Active Directory-service-principal die door het AKS-cluster wordt gebruikt niet verwijderd. Zie voor stappen voor het verwijderen van de service-principal [AKS service-principal overwegingen en verwijdering][sp-delete].

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het gebruik van concept, raadpleegt u de ontwerp-documentatie op GitHub.

> [!div class="nextstepaction"]
> [Concept-documentatie][draft-documentation]


[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-create]: /cli/azure/acr#az-acr-login
[az-group-delete]: /cli/azure/group#az-group-delete
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[kubernetes-ingress]: ./ingress-basic.md

[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[example-java]: https://github.com/Azure/draft/tree/master/examples/example-java
[example-java-hello-l7]: https://github.com/Azure/draft/blob/master/examples/example-java/src/main/java/helloworld/Hello.java#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
[helm-init]: https://docs.helm.sh/helm/#helm-init
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac
