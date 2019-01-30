---
title: Kubernetes-Service worden uitgevoerd
titleSuffix: Text Analytics -  Azure Cognitive Services
description: De taal detecteren-container, met een actieve voorbeeld met de Azure Kubernetes Service implementeren en testen in een webbrowser.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 01/22/2019
ms.author: diberry
ms.openlocfilehash: 8af5d60ff3e9deabb82f91c04f668703ed489440
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222028"
---
# <a name="deploy-the-language-detection-container-to-azure-kubernetes-service"></a>De taal detecteren-container implementeren in Azure Kubernetes Service

Informatie over het implementeren van de taal detecteren-container. Deze procedure ziet u hoe de lokale Docker-containers te maken, de containers naar uw eigen privé containerregister pushen, uitvoeren van de container in een Kubernetes-cluster en test in een webbrowser. 

## <a name="prerequisites"></a>Vereisten

Deze procedure moet diverse hulpprogramma's die moeten worden geïnstalleerd en lokaal uitvoeren. Gebruik niet Azure Cloud shell. 

* Gebruik een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* [GIT](https://git-scm.com/downloads) voor uw besturingssysteem, zodat u kunt klonen de [voorbeeld](https://github.com/Azure-Samples/cognitive-services-containers-samples) gebruikt in deze procedure. 
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 
* [Docker-engine](https://www.docker.com/products/docker-engine) en te valideren dat de Docker-CLI in een consolevenster weergegeven werkt.
* [kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.13.1/bin/windows/amd64/kubectl.exe). 
* Een Azure-resource met de juiste prijscategorie. Niet alle Prijscategorieën werken met deze container:
    * **Tekstanalyse** resource met F0 of standaardprijzen alleen lagen.
    * **Cognitive Services** resource met de S0 prijscategorie.

## <a name="running-the-sample"></a>Het voorbeeld uitvoeren

Deze procedure wordt geladen en wordt het voorbeeld Cognitive Services-Container voor de taaldetectie wordt uitgevoerd. Het voorbeeld bestaat uit twee containers, één voor de clienttoepassing en één voor de Cognitive Services-container. U moet beide deze installatiekopieën pushen naar uw eigen Azure Container Registry. Wanneer ze zich op uw eigen register bevinden, maakt u een Azure Kubernetes Service om deze installatiekopieën en de containers worden uitgevoerd. Wanneer de containers worden uitgevoerd, gebruikt u de **kubectl** CLI om te bekijken van de prestaties van containers. Toegang tot de clienttoepassing met een HTTP-aanvraag en de resultaten bekijken. 

![Conceptuele beeld van de voorbeeld-containers uitvoeren](../media/how-tos/container-instance-sample/containers.png)

## <a name="the-sample-containers"></a>De voorbeeld-containers

Het voorbeeld bestaat uit twee installatiekopieën van containers, één voor de frontend-website. De tweede afbeelding is de taal detecteren-container retourneren van de gedetecteerde taal (cultuur) van de tekst. Beide containers zijn toegankelijk via een extern IP-adres wanneer u klaar bent. 

### <a name="the-language-frontend-container"></a>De taal-frontend-container

Deze website is gelijk aan uw eigen client-side '-toepassing waarmee aanvragen van het eindpunt van de detectie van taal. Als de procedure is voltooid, krijgt u de gedetecteerde taal van een tekenreeks door het openen van de website-container in een browser met `http://<external-IP>/<text-to-analyze>`. Een voorbeeld van deze URL is `http://132.12.23.255/helloworld!`. Het resultaat in de browser is `English`.

### <a name="the-language-container"></a>De taal-container

De taal detecteren-container, in deze specifieke procedure, is toegankelijk voor elke externe aanvraag. De container is niet op geen enkele manier gewijzigd, zodat de standaard Cognitive Services-container-specifieke taaldetectie API beschikbaar is. 

Voor deze container is die API een POST-aanvraag voor de taaldetectie van. Als met alle Cognitive Services-containers, kunt u meer informatie over de container van de gehoste Swagger-informatie, `http://<external-IP>:5000/swagger/index.html`. 

Poort 5000 is de standaardpoort gebruikt in combinatie met de Cognitive Services-containers. 

## <a name="create-azure-container-registry-service"></a>Azure Container Registry-service maken

Als u wilt de container geïmplementeerd naar de Azure Kubernetes Service, de containerinstallatiekopieën toegankelijk moeten zijn. Maak uw eigen Azure Container Registry-service voor het hosten van de installatiekopieën. 

1. Meld u aan bij de Azure CLI 

    ```azurecli
    az login
    ```

1. Maak een resourcegroep met de naam `cogserv-container-rg` voor het opslaan van elke resource in deze procedure hebt gemaakt.

    ```azurecli
    az group create --name cogserv-container-rg --location westus
    ```

1. Vervolgens maken van uw eigen Azure Container Registry met de indeling van de naam van uw `registry`, zoals `pattyregistry`. Geen streepjes gebruiken of underline tekens in de naam.

    ```azurecli
    az acr create --resource-group cogserv-container-rg --name pattyregistry --sku Basic
    ```

    Sla de resultaten aan de **loginServer** eigenschap. Dit is onderdeel van de gehoste container-adres, we later tijdens de `language.yml` bestand.

    ```console
    >az acr create --resource-group cogserv-container-rg --name pattyregistry --sku Basic
    {
        "adminUserEnabled": false,
        "creationDate": "2019-01-02T23:49:53.783549+00:00",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/cogserv-container-rg/providers/Microsoft.ContainerRegistry/registries/pattyregistry",
        "location": "westus",
        "loginServer": "pattyregistry.azurecr.io",
        "name": "pattyregistry",
        "provisioningState": "Succeeded",
        "resourceGroup": "cogserv-container-rg",
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

1. Meld u aan bij uw containerregister. U moet zich aanmelden voordat u installatiekopieën naar uw register pushen kunt.

    ```azurecli
    az acr login --name pattyregistry
    ```

## <a name="get-website-docker-image"></a>Website Docker-installatiekopie ophalen 

1. De voorbeeldcode gebruikt in deze procedure is in de opslagplaats met voorbeelden containers voor Cognitive Services. Kloon de opslagplaats om een lokale kopie van het voorbeeld.

    ```console
    git clone https://github.com/Azure-Samples/cognitive-services-containers-samples
    ```

    Zodra de opslagplaats op uw lokale computer is, vindt u de website in de [\dotnet\Language\FrontendService](https://github.com/Azure-Samples/cognitive-services-containers-samples/tree/master/dotnet/Language/FrontendService) directory. Deze website fungeert als de clienttoepassing aanroepen van de taaldetectie API die wordt gehost in de taal detecteren-container.  

1. Bouw de Docker-installatiekopie voor deze website. Zorg ervoor dat de console is in de [\FrontendService](https://github.com/Azure-Samples/cognitive-services-containers-samples/tree/master/dotnet/Language/FrontendService) map waarin het Dockerfile dat zich bevindt, wanneer u de volgende opdracht uitvoeren:

    ```console
    docker build -t language-frontend -t pattiyregistry.azurecr.io/language-frontend:v1 . 
    ```

    Voor het volgen van de versie voor uw containerregister, Voeg het label met een versie-indeling, zoals `v1`. 

1. Push de installatiekopie naar uw containerregister. Dit kan enkele minuten duren. 

    ```console
    docker push pattyregistry.azurecr.io/language-frontend:v1
    ```

    Als er een `unauthorized: authentication required` fout en meld u aan met de `az acr login --name <your-container-registry-name>` opdracht. 

    Wanneer het proces is voltooid, zijn de resultaten moeten vergelijkbaar met:

    ```console
    > docker push pattyregistry.azurecr.io/language-frontend:v1
    The push refers to repository [pattyregistry.azurecr.io/language-frontend]
    82ff52ee6c73: Pushed
    07599c047227: Pushed
    816caf41a9a1: Pushed
    2924be3aed17: Pushed
    45b83a23806f: Pushed
    ef68f6734aa4: Pushed
    v1: digest: sha256:31930445deee181605c0cde53dab5a104528dc1ff57e5b3b34324f0d8a0eb286 size: 1580
    ```

## <a name="get-language-detection-docker-image"></a>Taal detecteren Docker-installatiekopie ophalen 

1. De nieuwste versie van de Docker-installatiekopie op te halen op de lokale computer. Dit kan enkele minuten duren. Als er een nieuwere versie van deze container, wijzig de waarde van `1.1.006770001-amd64-preview` naar de nieuwste versie. 

    ```console
    docker pull mcr.microsoft.com/azure-cognitive-services/language:1.1.006770001-amd64-preview
    ```

1. Tag de installatiekopie met uw containerregister. De meest recente versie te zoeken en vervangen door de versie `1.1.006770001-amd64-preview` als u een recentere versie hebt. 

    ```console
    docker tag mcr.microsoft.com/azure-cognitive-services/language pattiyregistry.azurecr.io/language:1.1.006770001-amd64-preview
    ```

1. Push de installatiekopie naar uw containerregister. Dit kan enkele minuten duren. 

    ```console
    docker push pattyregistry.azurecr.io/language:1.1.006770001-amd64-preview
    ```

## <a name="get-container-registry-credentials"></a>Container Registry-referenties ophalen

De volgende stappen worden gevolgd om de vereiste informatie om uw container registry met de Azure Kubernetes Service verbinding te maken die later in deze procedure maakt u.

1. Service-principal maken.

    ```azurecli
    az ad sp create-for-rbac --skip-assignment
    ```

    De resultaten opslaan `appId` waarde voor de toegewezen gebruiker-parameter in stap 3, `<appId>`. Sla de `password` voor de volgende sectie clientgeheim parameter `<client-secret>`.

    ```console
    > az ad sp create-for-rbac --skip-assignment
    {
      "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "displayName": "azure-cli-2018-12-31-18-39-32",
      "name": "http://azure-cli-2018-12-31-18-39-32",
      "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
    ```

1. Ophalen van uw container-id-register.

    ```azurecli
    az acr show --resource-group cogserv-container-rg --name pattyregistry --query "id" --o table
    ```

    Sla de uitvoer voor de waarde van de scope-parameter `<acrId>`, in de volgende stap. Het lijkt:

    ```console
    > az acr show --resource-group cogserv-container-rg --name pattyregistry --query "id" --o table
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/cogserv-container-rg/providers/Microsoft.ContainerRegistry/registries/pattyregistry
    ```

    Sla de volledige waarde voor stap 3 in deze sectie. 

1. De juiste om toegang te verlenen voor het AKS-cluster installatiekopieën die zijn opgeslagen in uw container registry kunt gebruiken, moet u een roltoewijzing maken. Vervang <appId> en <acrId> door de waarden die worden verzameld in de vorige twee stappen.

    ```azurecli
    az role assignment create --assignee <appId> --scope <acrId> --role Reader
    ```

## <a name="create-azure-kubernetes-service"></a>Azure Kubernetes Service maken

1. Maak de Kubernetes-cluster. Alle parameterwaarden worden in de vorige secties, met uitzondering van de parameter name. Kies een naam die aangeeft wie zoals gemaakt en het doel, `patty-kube`. 

    ```azurecli
    az aks create --resource-group cogserv-container-rg --name patty-kube --node-count 2  --service-principal <appId>  --client-secret <client-secret>  --generate-ssh-keys
    ```

    Deze stap kan enkele minuten duren. Het resultaat is: 

    ```console
    > az aks create --resource-group cogserv-container-rg --name patty-kube --node-count 2  --service-principal <appId>  --client-secret <client-secret>  --generate-ssh-keys
    {
      "aadProfile": null,
      "addonProfiles": null,
      "agentPoolProfiles": [
        {
          "count": 2,
          "dnsPrefix": null,
          "fqdn": null,
          "maxPods": 110,
          "name": "nodepool1",
          "osDiskSizeGb": 30,
          "osType": "Linux",
          "ports": null,
          "storageProfile": "ManagedDisks",
          "vmSize": "Standard_DS1_v2",
          "vnetSubnetId": null
        }
      ],
      "dnsPrefix": "patty-kube--65a101",
      "enableRbac": true,
      "fqdn": "patty-kube--65a101-341f1f54.hcp.westus.azmk8s.io",
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/cogserv-container-rg/providers/Microsoft.ContainerService/managedClusters/patty-kube",
      "kubernetesVersion": "1.9.11",
      "linuxProfile": {
        "adminUsername": "azureuser",
        "ssh": {
          "publicKeys": [
            {
              "keyData": "ssh-rsa AAAAB3NzaC...ohR2d81mFC
            }
          ]
        }
      },
      "location": "westus",
      "name": "patty-kube",
      "networkProfile": {
        "dnsServiceIp": "10.0.0.10",
        "dockerBridgeCidr": "172.17.0.1/16",
        "networkPlugin": "kubenet",
        "networkPolicy": null,
        "podCidr": "10.244.0.0/16",
        "serviceCidr": "10.0.0.0/16"
      },
      "nodeResourceGroup": "MC_patty_westus",
      "provisioningState": "Succeeded",
      "resourceGroup": "cogserv-container-rg",
      "servicePrincipalProfile": {
        "clientId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "keyVaultSecretRef": null,
        "secret": null
      },
      "tags": null,
      "type": "Microsoft.ContainerService/ManagedClusters"
    }
    ```

    De service is gemaakt, maar dit hoeft niet de website-container of de taal detecteren container nog.  

1. Haal de referenties van het Kubernetes-cluster. 

    ```azurecli
    az aks get-credentials --resource-group cogserv-container-rg --name patty-kube
    ```

## <a name="load-the-orchestration-definition-into-your-kubernetes-service"></a>De orchestration-definitie laden in uw Kubernetes-service

In deze sectie wordt de **kubectl** CLI om te communiceren met de Azure Kubernetes Service. 

1. Controleer voordat u het laden van de orchestration-definitie, **kubectl** toegang heeft tot de knooppunten.

    ```console
    kubectl get nodes
    ```

    Het antwoord lijkt:

    ```console
    > kubectl get nodes
    NAME                       STATUS    ROLES     AGE       VERSION
    aks-nodepool1-13756812-0   Ready     agent     6m        v1.9.11
    aks-nodepool1-13756812-1   Ready     agent     6m        v1.9.11
    ```

1. Kopieer het volgende bestand en geef deze de naam `language.yml`. Het bestand heeft een `service` sectie en een `deployment` sectie voor de containertypen twee de `language-frontend` website-container en de `language` detectie-container. 

    [!code-yml[Kubernetes orchestration file for the Cognitive Services containers sample](~/samples-cogserv-containers/Kubernetes/language/language.yml "Kubernetes orchestration file for the Cognitive Services containers sample")]

1. Wijzig de taal-frontend implementatie regels van `language.yml` op basis van de volgende tabel om uw eigen namen van containerinstallatiekopieën register, het clientgeheim en de text analytics-instellingen toevoegen.

    Implementatie-instellingen voor taal-frontend|Doel|
    |--|--|
    |Regel 32<br> `image` De eigenschap|Installatiekopie-locatie voor de front-end installatiekopie in uw Container Registry<br>`<container-registry-name>.azurecr.io/language-frontend:v1`|
    |Line 44<br> `name` De eigenschap|Container Registry geheim voor de installatiekopie, aangeduid als `<client-secret>` in een vorige sectie.|

1. Wijzig de taal implementatie regels van `language.yml` op basis van de volgende tabel om uw eigen namen van containerinstallatiekopieën register, het clientgeheim en de text analytics-instellingen toevoegen.

    |Implementatie-instellingen voor taal|Doel|
    |--|--|
    |Line 78<br> `image` De eigenschap|Installatiekopie-locatie voor de installatiekopie van de taal in uw Container Registry<br>`<container-registry-name>.azurecr.io/language:1.1.006770001-amd64-preview`|
    |Regel 95<br> `name` De eigenschap|Container Registry geheim voor de installatiekopie, aangeduid als `<client-secret>` in een vorige sectie.|
    |Regel 91<br> `apiKey` De eigenschap|Uw tekstsleutel voor de analytics-resource|
    |Regel 92<br> `billing` De eigenschap|De facturering-eindpunt voor de text analytics-resource.<br>`https://westus.api.cognitive.microsoft.com/text/analytics/v2.0`|

    Omdat de **apiKey** en **facturering eindpunt** zijn ingesteld als onderdeel van de definitie van de Kubernetes-indeling, de website-container hoeft niet te weten over deze of ze doorgeven als onderdeel van de aanvraag. De container website verwijst naar de container van de detectie van taal door de naam van de orchestrator `language`. 

1. De orchestration-definitiebestand voor dit voorbeeld laden vanuit de map waar u hebt gemaakt en opgeslagen de `language.yml`. 

    ```console
    kubectl apply -f language.yml
    ```

    Het antwoord is:

    ```console
    > kubectl apply -f language.yml
    service "language-frontend" created
    deployment.apps "language-frontend" created
    service "language" created
    deployment.apps "language" created
    ```

## <a name="get-external-ips-of-containers"></a>Externe IP-adressen van containers ophalen

Voor de twee containers, controleert u of de `language-frontend` en `language` services worden uitgevoerd en ophalen van het externe IP-adres. 

```console
kubectl get all
```

```console
> kubectl get all
NAME                                     READY     STATUS    RESTARTS   AGE
pod/language-586849d8dc-7zvz5            1/1       Running   0          13h
pod/language-frontend-68b9969969-bz9bg   1/1       Running   1          13h

NAME                        TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)          AGE
service/kubernetes          ClusterIP      10.0.0.1      <none>          443/TCP          14h
service/language            LoadBalancer   10.0.39.169   104.42.172.68   5000:30161/TCP   13h
service/language-frontend   LoadBalancer   10.0.42.136   104.42.37.219   80:30943/TCP     13h

NAME                                      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/language            1         1         1            1           13h
deployment.extensions/language-frontend   1         1         1            1           13h

NAME                                                 DESIRED   CURRENT   READY     AGE
replicaset.extensions/language-586849d8dc            1         1         1         13h
replicaset.extensions/language-frontend-68b9969969   1         1         1         13h

NAME                                DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/language            1         1         1            1           13h
deployment.apps/language-frontend   1         1         1            1           13h

NAME                                           DESIRED   CURRENT   READY     AGE
replicaset.apps/language-586849d8dc            1         1         1         13h
replicaset.apps/language-frontend-68b9969969   1         1         1         13h
```

Als de `EXTERNAL-IP` voor de service wordt weergegeven zoals in behandeling, opnieuw de opdracht uitvoeren totdat het IP-adres wordt weergegeven voordat u doorgaat met de volgende stap. 

## <a name="test-the-language-detection-container"></a>De taal detecteren-container testen

Open een browser en navigeer naar het externe IP-adres van de `language` container uit het vorige gedeelte: `http://<external-ip>:5000/swagger/index.html`. U kunt de `Try it` functie van de API voor het testen van het eindpunt van de detectie van taal. 

![Swagger-documentatie van de container weergeven](../media/how-tos/container-instance-sample/language-detection-container-swagger-documentation.png)

## <a name="test-the-client-application-container"></a>De container van de client-toepassing testen

Wijzig de URL in de browser naar het externe IP-adres van de `language-frontend` container met behulp van de volgende indeling: `http://<external-ip>/helloworld`. De Engelse tekst van de cultuur `helloworld` wordt voorspeld als `English`.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met het cluster, verwijdert u de Azure-resourcegroep. 

```azure-cli
az group delete --name cogserv-container-rg
```

## <a name="related-information"></a>Gerelateerde informatie

* [kubectl voor Docker-gebruikers](https://kubernetes.io/docs/reference/kubectl/docker-cli-to-kubectl/)

## <a name="next-steps"></a>Volgende stappen 

* Meer [Cognitive Services-Containers](../../cognitive-services-container-support.md)
* Gebruik de Text Analytics verbonden Service] (.. / vs-tekst-verbonden-service.md)


<!--
kubectl get secrets

>az aks browse --resource-group diberry-cogserv-container-rg --name diberry-kubernetes-languagedetection

kubectl proxy

http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/pod/default/language-frontend-6d65bdb77c-8f4qv?namespace=default

kubectl describe pod language-frontend-6d65bdb77c
-->
