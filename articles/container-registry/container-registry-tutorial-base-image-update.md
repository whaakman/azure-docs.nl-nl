---
title: 'Zelfstudie: Automatiseren van builds van containerinstallatiekopieën bij update van basis installatiekopie met Azure Container Registry Build'
description: In deze zelfstudie leert u hoe u een build-taak configureert om builds van containerinstallatiekopieën automatisch te activeren in de cloud wanneer een basis installatiekopie wordt bijgewerkt.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 531aeaacf0bd70521d70afb45d141fc3296ebb04
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="tutorial-automate-image-builds-on-base-image-update-with-azure-container-registry-build"></a>Zelfstudie: Automatiseren van builds van installatiekopieën bij update van basisinstallatiekopie met Azure Container Registry Build

ACR Build ondersteunt de uitvoering van geautomatiseerde builds wanneer de basisinstallatiekopie van een container wordt bijgewerkt, bijvoorbeeld wanneer u het besturingssysteem of toepassingsframework patcht in een van uw basisinstallatiekopieën. In deze zelfstudie leert u hoe een build-taak maakt in ACR Build die een build in de cloud activeert wanneer de basisinstallatiekopie van een container naar uw register is gepusht.

Deze zelfstudie, de laatste in de serie, gaat over het volgende:

> [!div class="checklist"]
> * De basisinstallatiekopie bouwen
> * Een build-taak voor de toepassingsinstallatiekopie maken
> * De basisinstallatiekopie voor het activeren van een build voor een toepassingsinstallatiekopie bijwerken
> * De geactiveerde build weergeven
> * De bijgewerkte toepassingsinstallatiekopie controleren

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u de Azure CLI lokaal wilt gebruiken, moet u Azure CLI versie **2.0.32** of hoger hebben geïnstalleerd. Voer `az --version` uit om de versie te bekijken. Als u de CLI wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

## <a name="prerequisites"></a>Vereisten

### <a name="complete-previous-tutorials"></a>Vorige zelfstudies voltooien

In deze zelfstudie wordt ervan uitgegaan dat u de stappen van de eerste twee zelfstudies in de reeks al hebt uitgevoerd:

* Azure Container Registry maken
* Voorbeeldopslagplaats fork
* Voorbeeldopslagplaats kloon
* Persoonlijk toegangstoken van GitHub maken

Als u dit nog niet hebt gedaan, voltooit u de eerste twee zelfstudies voordat u verdergaat:

[Zelfstudie: Containerinstallatiekopieën bouwen in de cloud met Azure Container Registry Build](container-registry-tutorial-quick-build.md)

[Builds van containerinstallatiekopieën automatiseren met Azure Container Registry Build](container-registry-tutorial-build-task.md)

### <a name="configure-environment"></a>Omgeving configureren

Vul eerst deze shell omgevingsvariabelen in met waarden die geschikt zijn voor uw omgeving. Dit is niet strikt vereist, maar vereenvoudigt de uitvoer van meerregelige Azure CLI-opdrachten in deze zelfstudie. Als u deze niet invult, moet u elke waarde handmatig vervangen waar ze worden weergegeven in de voorbeeldopdrachten.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Basisinstallatiekopieën

Dockerfiles die de meeste containerinstallatiekopieën definiëren, specificeren een bovenliggende installatiekopie waarop deze is gebaseerd, vaak aangeduid als de *basisinstallatiekopie*. Basisinstallatiekopieën bevatten doorgaans het besturingssysteem, bijvoorbeeld [Alpine Linux][base-alpine] of [Windows Nano Server][base-windows], waarop de rest van lagen van de container worden toegepast. Ze kunnen ook toepassingsframeworks zoals [Node.js][base-node] of [.NET Core][base-dotnet] bevatten.

### <a name="base-image-updates"></a>Updates van basisinstallatiekopieën

Een basisinstallatiekopie wordt vaak bijgewerkt door de installatiekopie-maintainer om nieuwe functies of verbeteringen aan het besturingssysteem of framework in de installatiekopie op te nemen. Beveiligingspatches zijn een andere veelvoorkomende oorzaak van een update van de basisinstallatiekopie.

Wanneer een basisinstallatiekopie wordt bijgewerkt, is het noodzakelijk containerinstallatiekopieën te herbouwen in uw register op basis hiervan om de nieuwe functies en oplossingen op te nemen. ACR Build biedt de mogelijkheid om automatisch installatiekopieën te maken wanneer een containerinstallatiekopie wordt bijgewerkt.

### <a name="base-image-update-scenario"></a>Bijwerkscenario van basisinstallatiekopieën

In deze zelfstudie wordt u door een bijwerkscenario van een basisinstallatiekopie geleid. Het [codevoorbeeld] [ code-sample] bevat twee Dockerfiles: een toepassingsinstallatiekopie en een installatiekopie als basis. In de volgende secties maakt u een ACR Build-taak die automatisch een build van de toepassingsinstallatiekopie activeert wanneer een nieuwe versie van het de basisinstallatiekopie naar uw containerregister wordt gepusht.

[Dockerfile-app][dockerfile-app]: een kleine Node.js-webtoepassing die een statische webpagina maakt waarop de Node.js-versie ervan wordt weergegeven. De versiestring wordt gesimuleerd, in die zin dat deze de inhoud weergeeft van een omgevingsvariabele, `NODE_VERSION`, die is gedefinieerd in de basisinstallatiekopie.

[Dockerfile-basis][dockerfile-base]: de installatiekopie die `Dockerfile-app` opgeeft als basis. Dit is zelf gebaseerd op een installatiekopie van een [knooppunt][base-node] en bevat de `NODE_VERSION` omgevingsvariabele.

In de volgende secties maakt u een build-taak, werkt u de `NODE_VERSION`-waarde bij in de Dockerfile van de basisinstallatiekopie en gebruikt u ACR Build voor het bouwen van de basisinstallatiekopie. Wanneer ACR Build de nieuwe basisinstallatiekopie naar uw register pusht, wordt automatisch een build van de toepassingsinstallatiekopie geactiveerd. Optioneel voert u de containerinstallatiekopie van de toepassing lokaal uit om de verschillende versiestrings in de ingebouwde installatiekopieën te zien.

## <a name="build-base-image"></a>Basisinstallatiekopie bouwen

Start met het bouwen van de basisinstallatiekopie met een ACR Build *Quick Build*. Zoals beschreven in de [eerste zelfstudie](container-registry-tutorial-quick-build.md) in de reeks wordt hierdoor niet alleen de installatiekopie gebouwd, maar wordt deze ook naar het containerregister gepusht als de build is gelukt.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-build-task"></a>Een build-taak maken

Maak vervolgens een build-taak met [az acr build-task create][az-acr-build-task-create]:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --build-arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

Deze build-taak is vergelijkbaar met de taak die is gemaakt in de [vorige zelfstudie](container-registry-tutorial-build-task.md). Het geeft ACR Build de opdracht om een installatiekopiebuild te activeren wanneer doorvoeracties naar de door `--context` gespecificeerde opslagplaats worden gepusht.

Het verschil zit in gedrag, in die zin dat het ook een build van de installatiekopie activeert wanneer zijn *basisinstallatiekopie*  wordt bijgewerkt. De Dockerfile die is opgegeven door het `--file` argument, [Dockerfile-app][dockerfile-app], ondersteunt het opgeven van een installatiekopie uit hetzelfde register als zijn basis:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Wanneer u een build-taak uitvoert, detecteert ACR Build de afhankelijkheden van een installatiekopie. Als de in de `FROM`-instructie opgegeven basisinstallatiekopie deel uitmaakt van hetzelfde register, wordt een haakje toegevoegd om te controleren of deze installatiekopie elke keer dat de basis wordt bijgewerkt ook wordt herbouwd.

> [!NOTE]
> Tijdens de preview ondersteunt ACR Build het activeren van een afgeleide installatiekopiebuild alleen wanneer zowel de basisinstallatiekopie als de installatiekopie waarnaar wordt verwezen zich in hetzelfde Azure-containerregister bevinden.

## <a name="build-application-container"></a>Toepassingscontainer bouwen

Als u ACR Build wilt inschakelen voor het bepalen en volgen van de afhankelijkheden van een containerinstallatiekopie - met inbegrip van de basisinstallatiekopie - **moet** u eerst de build-taak **ten minste eenmaal activeren**.

Gebruik [az acr build-task run] [ az-acr-build-task-run] om handmatig de build-taak te activeren en de toepassingsinstallatiekopie te bouwen:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Zodra de build is voltooid, moet u de **Build ID** (bijvoorbeeld 'aa6') noteren als u de volgende optionele stap wilt voltooien.

### <a name="optional-run-application-container-locally"></a>Optioneel: toepassingscontainer lokaal uitvoeren

Als u lokaal werkt (niet in de Cloud Shell) en u hebt Docker geïnstalleerd, voer dan de container uit om de weergegeven toepassing te zien in een webbrowser, voordat u de basisinstallatiekopie herbouwt. Als u de Cloud Shell gebruikt, slaat u deze sectie over (Cloud Shell biedt geen ondersteuning voor `az acr login` of `docker run`).

Meld u eerst aan bij uw containerregister met [az acr login][az-acr-login]:

```azurecli
az acr login --name $ACR_NAME
```

Voer de container nu lokaal uit met `docker run`. Vervang **\<build-id\>** door de gevonden Build-ID in de uitvoer van de vorige stap (bijvoorbeeld 'aa6').

```azurecli
docker run -d -p 8080:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Navigeer naar http://localhost:8080 in uw browser. U ziet nu het weergegeven nummer van de Node.js-versie op de webpagina, zoals hieronder weergegeven. In een latere stap verhoogt u de versie door een ‘a’ toe te voegen aan de versiestring.

![Schermafbeelding van de voorbeeldtoepassing die wordt weergegeven in de browser][base-update-01]

## <a name="list-builds"></a>Builds weergeven

Geef vervolgens een lijst weer van de builds die ACR Build voor uw register heeft voltooid:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Als u de vorige zelfstudie hebt voltooid (en het register niet hebt verwijderd), ziet u de uitvoer zoals hieronder weergegeven. Noteer het aantal builds en de meest recente BUILD-ID, zodat u de uitvoer kunt vergelijken na het bijwerken van de basisinstallatiekopie in de volgende sectie.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
----------  ---------------  ----------  ---------  ----------  --------------------  ----------
aa6         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T20:00:12Z  00:00:50
aa5                          Linux       Succeeded  Manual      2018-05-10T19:57:35Z  00:00:55
aa4         buildhelloworld  Linux       Succeeded  Git Commit  2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual      2018-05-10T19:10:14Z  00:00:55
```

## <a name="update-base-image"></a>Basisinstallatiekopie bijwerken

Hier kunt u een framework-patch in de basisinstallatiekopie simuleren. Bewerk de **Dockerfile-basis**  en voeg een ‘a’ toe achter het versienummer dat is gedefinieerd in `NODE_VERSION`:

```Dockerfile
ENV NODE_VERSION 9.11.1a
```

Voer een Quick Build uit in ACR Build om de gewijzigde basisinstallatiekopie op te bouwen. Noteer de **Build-ID** in de uitvoer.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Zodra de build is voltooid en ACR Build de nieuwe basisinstallatiekopie naar uw register heeft gepusht, wordt automatisch een build van de toepassingsinstallatiekopie geactiveerd. Het kan enkele ogenblikken duren voordat de ACR Build-taak die u eerder hebt gemaakt de toepassingsinstallatiekopie start, aangezien het de pas voltooide en gepushte basisinstallatiekopie moet detecteren.

## <a name="list-builds"></a>Builds weergeven

Nu u de basisinstallatiekopie hebt bijgewerkt, maakt u opnieuw een lijst met uw builds om deze te vergelijken met de vorige lijst. Als de uitvoer in eerste instantie niet verschilt, voer dan regelmatig de opdracht uit om de nieuwe build in de lijst zien te verschijnen.

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

De uitvoer lijkt op het volgende. De TRIGGER voor de als laatste uitgevoerde build moet Update van installatiekopie zijn, die aangeeft dat de build-taak werd gestart door uw Quick Start van de basisinstallatiekopie.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER       STARTED               DURATION
----------  ---------------  ----------  ---------  ------------  --------------------  ----------
aa8         buildhelloworld  Linux       Succeeded  Image Update  2018-05-10T20:09:52Z  00:00:45
aa7                          Linux       Succeeded  Manual        2018-05-10T20:09:17Z  00:00:40
aa6         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T20:00:12Z  00:00:50
aa5                          Linux       Succeeded  Manual        2018-05-10T19:57:35Z  00:00:55
aa4         buildhelloworld  Linux       Succeeded  Git Commit    2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual        2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual        2018-05-10T19:10:14Z  00:00:55
```

Als u de volgende optionele stap van de nieuw gebouwde container wilt uitvoeren om het bijgewerkte versienummer te zien, noteert u de **BUILD ID**-waarde voor het bijwerken van de installatiekopie getriggerde build (in de vorige uitvoer is dit 'aa8').

### <a name="optional-run-newly-built-image"></a>Optioneel: nieuw gebouwde installatiekopie uitvoeren

Als u lokaal werkt (niet in de Cloud Shell) en u hebt Docker geïnstalleerd, voert u de nieuwe toepassingsinstallatiekopie uit zodra de build is voltooid. Vervang `<build-id>` door de BUILD-ID die u in de vorige stap hebt verkregen. Als u de Cloud Shell gebruikt, slaat u deze sectie over (Cloud Shell biedt geen ondersteuning voor `docker run`).

```bash
docker run -d -p 8081:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Navigeer naar http://localhost:8081 in uw browser. U ziet nu het bijgewerkte nummer van de Node.js-versie (met de ‘a’) op de webpagina:

![Schermafbeelding van de voorbeeldtoepassing die wordt weergegeven in de browser][base-update-02]

Wat belangrijk is te weten, is dat u uw **basis**installatiekopie hebt bijgewerkt met een nieuw versienummer, maar dat de laatst gebouwde **toepassings**installatiekopie de nieuwe versie weergeeft. ACR Build heeft uw wijziging in de basisinstallatiekopie opgepikt en heeft uw toepassingsinstallatiekopie automatisch opnieuw opgebouwd.

## <a name="clean-up-resources"></a>Resources opschonen

Als u alle resources wilt verwijderen die u in deze zelfstudiereeks hebt gemaakt, met inbegrip van het containerregister, containerexemplaar, sleutelkluis en service-principal, geeft u de volgende opdrachten:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een build-taak configureert om builds van containerinstallatiekopieën automatisch te activeren wanneer een basisinstallatiekopie is bijgewerkt. Ga nu verder naar meer informatie over verificatie voor uw containerregister.

> [!div class="nextstepaction"]
> [Verificatie in Azure Container Registry](container-registry-authentication.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png