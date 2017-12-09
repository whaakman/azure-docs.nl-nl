---
title: Zelfstudie voor Azure Container register - voorbereiden van een Azure container geogerepliceerde register
description: Maken van een Azure container registry, geo-replicatie configureren, de installatiekopie van een Docker voorbereiden en implementeren op het register. Deel 1 van een serie drie delen.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/26/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 7ae0fbf5f7566bd3f1f6591501b8b004a1e5cb0f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="prepare-a-geo-replicated-azure-container-registry"></a>Een Azure container geogerepliceerde register voorbereiden

Een Azure container-register is een persoonlijke Docker-register geïmplementeerd in Azure die u kunt netwerk sluiten om uw implementaties te houden. In deze set van drie zelfstudie artikelen u informatie over het gebruik van geo-replicatie voor het implementeren van een ASP.NET Core-webtoepassing in een container Linux wordt uitgevoerd op twee [Web-Apps voor Containers](../app-service/containers/index.yml) exemplaren. U ziet hoe Azure automatisch implementeert de installatiekopie op elk exemplaar van de Web-App vanuit de dichtstbijzijnde geogerepliceerde opslagplaats.

In deze zelfstudie maakt deel uit van een in een reeks drie delen:

> [!div class="checklist"]
> * Maken van een Azure container geogerepliceerde register
> * Klonen van de broncode van de toepassing van GitHub
> * Installatiekopie maken van een Docker container van toepassingsbron
> * De container-installatiekopie aan het register push

In volgende zelfstudies leert implementeert u de container uit uw persoonlijke register voor een web-app uitgevoerd in de twee Azure-regio's. U vervolgens de code in de toepassing bijwerken en beide exemplaren van de Web-App bijwerken met één `docker push` toe aan het register.

## <a name="before-you-begin"></a>Voordat u begint

Deze zelfstudie vereist dat u de Azure CLI versie 2.0.20 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

In deze zelfstudie wordt ervan uitgegaan dat u een basiskennis hebt van Docker-kernconcepten zoals containers, containerinstallatiekopieën en Docker-basisopdrachten. Zie, indien nodig, [Aan de slag met Docker]( https://docs.docker.com/get-started/) voor een uitleg van de basisprincipes van containers.

Voor deze zelfstudie hebt u een Docker-ontwikkelomgeving nodig. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

Azure Cloud-Shell is dan de Docker-onderdelen die nodig zijn voor het voltooien van elke stap in deze zelfstudie niet opgenomen. Daarom raden we aan een lokale installatie van de Azure CLI en Docker-ontwikkelomgeving.

> [!IMPORTANT]
> Het onderdeel geo-replicatie van Azure Container register is geopend in **preview**. Voorbeelden voor u beschikbaar zijn aangebracht op voorwaarde dat u akkoord met gaat de [aanvullende gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Sommige aspecten van deze functie kunnen worden gewijzigd vóór de algemene beschikbaarheid (GA).
>

## <a name="create-a-container-registry"></a>Een containerregister maken

Meld u aan bij [Azure Portal](http://portal.azure.com).

Selecteer **nieuwe** > **Containers** > **Azure Container register**.

![Een register container maken in de Azure portal][tut-portal-01]

Uw nieuwe registersleutel met de volgende instellingen configureren:

* **Naam van routeringsregister**: maken van een registernaam die is globaal uniek zijn binnen Azure en 5 50 alfanumerieke tekens bevat
* **Resourcegroep**: **nieuwe maken** > `myResourceGroup`
* **Locatie**:`West US`
* **Gebruiker met beheerdersrechten**: `Enable` (vereist voor Web-App voor de Containers pull-installatiekopieën)
* **SKU**: `Premium` (vereist voor de geo-replicatie)

Selecteer **maken** voor het implementeren van het ACR-exemplaar.

![Een register container maken in de Azure portal][tut-portal-02]

In de rest van deze zelfstudie gebruiken we `<acrName>` als tijdelijke aanduiding voor de container **naam van Routeringsregister** die u hebt gekozen.

> [!TIP]
> Omdat Azure container registers doorgaans langlevende resources die worden gebruikt op meerdere hosts van de container, wordt u aangeraden dat u het register in een eigen resourcegroep maken. Als u registers geogerepliceerde en webhooks configureert, worden deze extra bronnen in dezelfde resourcegroep geplaatst.
>

## <a name="configure-geo-replication"></a>Geo-replicatie configureren

Nu dat u een Premium-register hebt, kunt u geo-replicatie kunt configureren. Uw web-app die u in de volgende zelfstudie in twee gebieden uit te voeren configureert, kan vervolgens de installatiekopieën van de container uit het register van de dichtstbijzijnde pull.

Navigeer naar uw nieuwe container register in de Azure portal en selecteert u een **replicaties** onder **SERVICES**:

![Replicaties in het register in Azure portal container gebruikersinterface][tut-portal-03]

Een kaart wordt weergegeven met de groene Zeshoeken die Azure-regio's beschikbaar zijn voor de geo-replicatie:

 ![Kaart van regio in de Azure-portal][tut-map-01]

Het register repliceren naar de regio VS-Oost als u de groene zeshoek selecteert, selecteert u vervolgens **maken** onder **replicatie maken**:

 ![UI-replicatie maken in de Azure portal][tut-portal-04]

Wanneer de replicatie voltooid is, duidt op de portal *gereed* voor beide regio's. Gebruik de **vernieuwen** klikken om de status van de replicatie vernieuwd; dit kan een paar minuten duren doet voor de replica's worden gemaakt en gesynchroniseerd.

![Replicatiestatus gebruikersinterface in de Azure portal][tut-portal-05]

## <a name="container-registry-login"></a>Container register aanmelding

Nu dat u geo-replicatie hebt geconfigureerd, de installatiekopie van een container maken en dit doorgeven aan het register. U moet eerst aanmelden bij uw ACR-exemplaar voordat u installatiekopieën aan. Met [Basic, Standard en Premium-SKU's](container-registry-skus.md), kunt u met behulp van uw Azure identiteit verifiëren.

Gebruik de [az acr aanmelding](https://docs.microsoft.com/cli/azure/acr#az_acr_login) opdracht om te verifiëren en de referenties voor het register in de cache. Vervang `<acrName>` met de naam van het register die u in de vorige stappen hebt gemaakt.

```azurecli
az acr login --name <acrName>
```

De opdracht retourneert `Login Succeeded` wanneer u klaar.

## <a name="get-application-code"></a>Toepassingscode ophalen

Het voorbeeld in deze zelfstudie bevat een kleine webtoepassing gebouwd met [ASP.NET Core](http://dot.net). De app fungeert een HTML-pagina waarop wordt weergegeven de regio van waaruit de installatiekopie van het register van Azure-Container is geïmplementeerd.

![Zelfstudie-app weergegeven in browser][tut-app-01]

Git gebruiken om te downloaden van het voorbeeld naar een lokale map en `cd` naar de map:

```bash
git clone https://github.com/Azure-Samples/acr-helloworld.git
cd acr-helloworld
```

## <a name="update-dockerfile"></a>Dockerfile bijwerken

De Dockerfile opgenomen in het voorbeeld ziet u hoe de container is gebouwd. Het starten van een officiële [aspnetcore](https://store.docker.com/community/images/microsoft/aspnetcore) afbeelding, kopieert de toepassingsbestanden naar de container afhankelijkheden installeert, wordt de uitvoer op basis van het officiële gecompileerd [aspnetcore-build](https://store.docker.com/community/images/microsoft/aspnetcore-build) afbeelding, en tot slot, een installatiekopie van een geoptimaliseerde aspnetcore is gebaseerd.

De Dockerfile bevindt zich op `./AcrHelloworld/Dockerfile` in de gekloonde bron.

```dockerfile
FROM microsoft/aspnetcore:2.0 AS base
# Update <acrName> with the name of your registry
# Example: uniqueregistryname.azurecr.io
ENV DOCKER_REGISTRY <acrName>.azurecr.io
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.sln ./
COPY AcrHelloworld/AcrHelloworld.csproj AcrHelloworld/
RUN dotnet restore
COPY . .
WORKDIR /src/AcrHelloworld
RUN dotnet build -c Release -o /app

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS production
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "AcrHelloworld.dll"]
```

De toepassing in de *acr helloworld* installatiekopie probeert om te bepalen van de regio van waaruit de container door in DNS te voor informatie over het register aanmeldingsserver werd geïmplementeerd. Moet u het register aanmeldings-URL voor server in de `DOCKER_REGISTRY` omgevingsvariabele in de Dockerfile.

Eerst ophalen van het register aanmeldings-URL voor server met de `az acr show` opdracht. Vervang `<acrName>` met de naam van het register die u in de vorige stappen hebt gemaakt.

```azurecli
az acr show --name <acrName> --query "{acrLoginServer:loginServer}" --output table
```

Uitvoer:

```bash
AcrLoginServer
-----------------------------
uniqueregistryname.azurecr.io
```

Werk vervolgens de `DOCKER_REGISTRY` regel met het register aanmeldings-URL van server. In dit voorbeeld gaan we de regel naar aanleiding van onze register Voorbeeldnaam bijwerken *uniqueregistryname*:

```dockerfile
ENV DOCKER_REGISTRY uniqueregistryname.azurecr.io
```

## <a name="build-container-image"></a>Een installatiekopie van de container maken

Nu dat u de Dockerfile met de URL van het register bijgewerkt hebt, kunt u `docker build` om de installatiekopie van de container te maken. Voer de volgende opdracht voor het bouwen van de installatiekopie en deze tag met de URL van uw persoonlijke register opnieuw vervangen `<acrName>` met de naam van het register:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

Uitvoer van meerdere regels worden weergegeven als de Docker-installatiekopie wordt gemaakt (hier weergegeven ingekorte):

```bash
Sending build context to Docker daemon  523.8kB
Step 1/18 : FROM microsoft/aspnetcore:2.0 AS base
2.0: Pulling from microsoft/aspnetcore
3e17c6eae66c: Pulling fs layer
...
Step 18/18 : ENTRYPOINT dotnet AcrHelloworld.dll
 ---> Running in 6906d98c47a1
 ---> c9ca1763cfb1
Removing intermediate container 6906d98c47a1
Successfully built c9ca1763cfb1
Successfully tagged uniqueregistryname.azurecr.io/acr-helloworld:v1
```

Gebruik de `docker images` opdracht om te zien van de installatiekopie van het ingebouwde:

```bash
docker images
```

Uitvoer:

```bash
REPOSITORY                                      TAG    IMAGE ID        CREATED               SIZE
uniqueregistryname.azurecr.io/acr-helloworld    v1     01ac48d5c8cf    About a minute ago    284MB
...
```

## <a name="push-image-to-azure-container-registry"></a>Push-installatiekopie in Azure Container register

Gebruik tot slot de `docker push` opdracht voor de push-de *acr helloworld* installatiekopie toe aan het register. Vervang `<acrName>` met de naam van het register.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Omdat u het register voor de geo-replicatie hebt geconfigureerd, de afbeelding automatisch wordt gerepliceerd naar zowel de *VS-West* en *VS-Oost* regio's met deze één `docker push` opdracht.

Uitvoer:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
cd54739c444b: Pushed
d6803756744a: Pushed
b7b1f3a15779: Pushed
a89567dff12d: Pushed
59c7b561ff56: Pushed
9a2f9413d9e4: Pushed
a75caa09eb1f: Pushed
v1: digest: sha256:0799014f91384bda5b87591170b1242bcd719f07a03d1f9a1ddbae72b3543970 size: 1792
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt gemaakt van een container privé, geogerepliceerde register, een installatiekopie van de container en gebouwd en vervolgens naar het register gepusht die installatiekopie. Met de volgende stappen in deze zelfstudie maakt u:

> [!div class="checklist"]
> * Een Azure container geogerepliceerde register gemaakt
> * De broncode gekloonde toepassing vanuit GitHub
> * Een installatiekopie van de Docker-container van toepassingsbron ingebouwd
> * De container-installatiekopie aan het register gepusht

Ga naar de volgende zelfstudie voor meer informatie over het implementeren van de container op meerdere Web-Apps voor Containers-exemplaren waarvoor geo-replicatie voor de installatiekopieën van het lokaal.

> [!div class="nextstepaction"]
> [Web-app uit het register van Azure-Container implementeren](container-registry-tutorial-deploy-app.md)

<!-- IMAGES -->
[tut-portal-01]: ./media/container-registry-tutorial-prepare-registry/tut-portal-01.png
[tut-portal-02]: ./media/container-registry-tutorial-prepare-registry/tut-portal-02.png
[tut-portal-03]: ./media/container-registry-tutorial-prepare-registry/tut-portal-03.png
[tut-portal-04]: ./media/container-registry-tutorial-prepare-registry/tut-portal-04.png
[tut-portal-05]: ./media/container-registry-tutorial-prepare-registry/tut-portal-05.png
[tut-app-01]: ./media/container-registry-tutorial-prepare-registry/tut-app-01.png
[tut-map-01]: ./media/container-registry-tutorial-prepare-registry/tut-map-01.png
