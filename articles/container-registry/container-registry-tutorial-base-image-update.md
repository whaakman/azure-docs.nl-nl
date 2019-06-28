---
title: 'Zelfstudie: Builds van containerinstallatiekopieën automatiseren wanneer een basisinstallatiekopie wordt bijgewerkt - Azure Container Registry-taken'
description: In deze zelfstudie leert u hoe u een Azure Container Registry-taak configureert om builds van containerinstallatiekopieën automatisch te activeren in de cloud wanneer een basisinstallatiekopie wordt bijgewerkt.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: tutorial
ms.date: 06/12/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 7d7cba63060756bff786b9475275e5262627cae9
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295721"
---
# <a name="tutorial-automate-container-image-builds-when-a-base-image-is-updated-in-an-azure-container-registry"></a>Zelfstudie: Builds van containerinstallatiekopieën automatiseren wanneer een basisinstallatiekopie wordt bijgewerkt in een Azure-containerregister 

ACR Tasks ondersteunt de uitvoering van geautomatiseerde builds wanneer de basisinstallatiekopie van een container wordt bijgewerkt, bijvoorbeeld wanneer u het besturingssysteem of toepassingsframework patcht in een van uw basisinstallatiekopieën. In deze zelfstudie leert u hoe u in ACR Tasks een taak maakt waarmee een build in de cloud wordt geactiveerd wanneer de basisinstallatiekopie van een container naar uw register is gepusht.

Deze laatste zelfstudie in de serie wordt het volgende behandeld:

> [!div class="checklist"]
> * De basisinstallatiekopie bouwen
> * Een build-taak voor de toepassingsinstallatiekopie maken
> * De basisinstallatiekopie bijwerken om een taak voor een toepassingsinstallatiekopie bij te werken
> * De geactiveerde taak weergeven
> * De bijgewerkte toepassingsinstallatiekopie controleren

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u Azure CLI lokaal wilt gebruiken, moet u ervoor zorgen dat versie **2.0.46** of hoger van Azure CLI is geïnstalleerd. Voer `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden van de CLI, Zie [Azure CLI installeren][azure-cli].

## <a name="prerequisites"></a>Vereisten

### <a name="complete-the-previous-tutorials"></a>De vorige zelfstudies voltooien

In deze zelfstudie wordt ervan uitgegaan dat u de stappen van de eerste twee zelfstudies in de serie al hebt uitgevoerd:

* Azure Container Registry maken
* Voorbeeldopslagplaats splitsen
* Voorbeeldopslagplaats klonen
* Persoonlijk toegangstoken van GitHub maken

Als u de eerste twee zelfstudies nog niet hebt afgerond, moet u dit doen voordat u verdergaat:

[Zelfstudie: containerinstallatiekopieën bouwen in de cloud met Azure Container Registry Tasks](container-registry-tutorial-quick-task.md)

[Builds van containerinstallatiekopieën automatiseren met Azure Container Registry Tasks](container-registry-tutorial-build-task.md)

### <a name="configure-the-environment"></a>De omgeving configureren

Vul deze variabelen voor de shell-omgeving in met waarden die geschikt zijn voor uw omgeving. Hoewel deze stap strikt genomen niet vereist is, vereenvoudigt u hiermee de uitvoering van meerregelige Azure CLI-opdrachten in deze zelfstudie. Als u deze omgevingsvariabelen niet invult, moet u elke bijbehorende waarde handmatig vervangen wanneer deze wordt weergegeven in de voorbeeldopdrachten.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Basisinstallatiekopieën

Dockerfiles waarmee de meeste containerinstallatiekopieën worden gedefinieerd, geven een bovenliggende installatiekopie aan waarop de containerinstallatiekopie is gebaseerd. Deze bovenliggende installatiekopie wordt vaak aangeduid als de *basisinstallatiekopie*. Doorgaans bevatten het besturingssysteem, bijvoorbeeld basisinstallatiekopieën [Alpine Linux][base-alpine] or [Windows Nano Server][base-windows]op die de rest van de lagen van de container worden toegepast. Ze ook advies inwinnen toepassingsframeworks zoals [Node.js][base-knooppunt] of [.NET Core][base-dotnet].

### <a name="base-image-updates"></a>Updates van basisinstallatiekopieën

Een basisinstallatiekopie wordt vaak door de installatiekopie-maintainer bijgewerkt om nieuwe functies of verbeteringen toe te voegen aan het besturingssysteem of framework in de installatiekopie. Beveiligingspatches zijn een andere veelvoorkomende oorzaak van een update van de basisinstallatiekopie.

Wanneer een basisinstallatiekopie wordt bijgewerkt, is het noodzakelijk dat u op basis hiervan de containerinstallatiekopieën in uw register opnieuw bouwt om de nieuwe functies en oplossingen op te nemen. Met ACR Tasks hebt u de mogelijkheid om automatisch installatiekopieën te maken wanneer een containerinstallatiekopie wordt bijgewerkt.

### <a name="tasks-triggered-by-a-base-image-update"></a>Taken die zijn geactiveerd door een basisinstallatiekopie-update

* Op dit moment voor het compileren van de installatiekopieën van een docker-bestand detecteert een ACR-taak afhankelijk is van de basisinstallatiekopieën in dezelfde Azure container registry, een openbare opslagplaats voor Docker Hub of een openbare opslagplaats in Microsoft Container Registry. Als de basisinstallatiekopie die is opgegeven in de `FROM` instructie bevinden zich in een van deze locaties, wordt de ACR-taak toegevoegd een haakje om te controleren of de installatiekopie is opnieuw opgebouwd elk gewenst moment de basis is bijgewerkt.

* Wanneer u een ACR-taak met maakt de [az acr-taak maken][az-acr-task-create] opdracht, de taak wordt standaard *ingeschakeld* voor de trigger door een update van de basisinstallatiekopie. Dat wil zeggen, de `base-image-trigger-enabled` eigenschap is ingesteld op True. Als u dit gedrag in een taak uitschakelt wilt, werkt u de eigenschap op False. Bijvoorbeeld, voer de volgende [az acr taakupdate][az-acr-task-update] opdracht:

  ```azurecli
  az acr task update --myregistry --name mytask --base-image-trigger-enabled False
  ```

* Om in te schakelen van een taak ACR om te bepalen en bijhouden van de containerinstallatiekopie van een afhankelijkheden, waaronder de basisinstallatiekopie--moet u eerst de taak activeren **ten minste één keer**. Bijvoorbeeld, activeert de taak handmatig met behulp van de [az acr-taak uitvoeren][az-acr-task-run] opdracht.

* Voor het activeren van een taak op updates van basisinstallatiekopieën, de basisinstallatiekopie moet hebben een *stabiel* labels, zoals `node:9-alpine`. Deze labels is normaal dat voor een basisinstallatiekopie die wordt bijgewerkt met het besturingssysteem en framework patches voor een laatste stabiele versie. Als de basisinstallatiekopie met een nieuwe versie-tag is bijgewerkt, wordt er een taak niet geactiveerd. Zie voor meer informatie over afbeeldingen taggen, het [best practices richtlijnen](https://stevelasker.blog/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/). 

### <a name="base-image-update-scenario"></a>Bijwerkscenario van basisinstallatiekopieën

In deze zelfstudie wordt u door een bijwerkscenario van een basisinstallatiekopie geleid. De [codevoorbeeld][code-sample] bevat twee docker-bestanden: de installatiekopie van een toepassing, en een afbeelding Hiermee geeft u aan als de basis. In de volgende secties maakt u een ACR-taak die een versie van de installatiekopie van de toepassing automatisch wordt geactiveerd wanneer een nieuwe versie van de basisinstallatiekopie wordt doorgestuurd naar dezelfde container registry.

[Dockerfile-app][dockerfile-app]: Een kleine Node.js-webtoepassing die een statische webpagina maakt waarop de Node.js-versie ervan wordt weergegeven. De versietekenreeks wordt gesimuleerd: deze geeft de inhoud weer van een omgevingsvariabele, `NODE_VERSION`, die is gedefinieerd in de basisinstallatiekopie.

[Dockerfile-base][dockerfile-base]: De installatiekopie die `Dockerfile-app` opgeeft als basis. Deze is gebaseerd op een [knooppunt][base-node] installatiekopie en bevat de `NODE_VERSION` omgevingsvariabele.

In de volgende gedeeltes maakt u een taak, werkt u de waarde `NODE_VERSION` in het Docker-bestand van de basisinstallatiekopie bij en gebruikt u ACR Tasks om de basisinstallatiekopie te maken. Zodra de ACR-taak de nieuwe basisinstallatiekopie naar uw register pusht, wordt er automatisch een build van de toepassingsinstallatiekopie geactiveerd. Optioneel kunt u de containerinstallatiekopie van de toepassing lokaal uitvoeren om de verschillende versietekenreeksen in de ingebouwde installatiekopieën te bekijken.

In deze zelfstudie, uw ACR-taak bouwt en een containerinstallatiekopie toepassing is opgegeven in een Dockerfile pushes. ACR-taken kunnen ook uitgevoerd [taken meerdere stappen](container-registry-tasks-multi-step.md), met behulp van een YAML-bestand voor het definiëren van de stappen voor het bouwen, push en eventueel meerdere containers te testen.

## <a name="build-the-base-image"></a>De basisinstallatiekopie bouwen

Start met het bouwen van de basisinstallatiekopie met een *snelle taak* van ACR Tasks. Zoals beschreven in de [eerste zelfstudie](container-registry-tutorial-quick-task.md) in de reeks, wordt met dit proces niet alleen de installatiekopie gebouwd, maar wordt deze ook naar het containerregister gepusht als de build is gelukt.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-a-task"></a>Een taak maken

Maak vervolgens een taak met [az acr-taak maken][az-acr-task-create]:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

> [!IMPORTANT]
> Als u taken voor het eerder hebt gemaakt tijdens de Preview-versie met de `az acr build-task` opdracht, deze taken moeten opnieuw worden gemaakt met behulp van de [az acr taak][az-acr-task] opdracht.

Deze taak is vergelijkbaar met de snelle taak die u hebt gemaakt in de [vorige zelfstudie](container-registry-tutorial-build-task.md). De taak geeft ACR Tasks de opdracht om een installatiekopiebuild te activeren wanneer doorvoeracties worden gepusht naar de opslagplaats die met `--context` is opgegeven. Terwijl het Dockerfile dat is gebruikt voor het bouwen van de installatiekopie in de vorige zelfstudie geeft u de installatiekopie van een openbare basis (`FROM node:9-alpine`), het Dockerfile in deze taak [Dockerfile-app][dockerfile-app], Hiermee geeft u een basisinstallatiekopie in het hetzelfde register:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Deze configuratie kunt eenvoudig een patch framework in de basisinstallatiekopie simuleren verderop in deze zelfstudie.

## <a name="build-the-application-container"></a>De toepassingscontainer bouwen

Gebruik [az acr-taak uitvoeren][az-acr-task-run] handmatig activeren van de taak en de installatiekopie van de toepassing te bouwen. Deze stap zorgt ervoor dat de taak van de installatiekopie van de toepassing afhankelijkheid op de basisinstallatiekopie volgt.

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

Zodra de taak is voltooid, moet u de **run-id** (bijvoorbeeld 'da6') noteren als u de volgende optionele stap wilt voltooien.

### <a name="optional-run-application-container-locally"></a>Optioneel: Toepassingscontainer lokaal uitvoeren

Als u lokaal werkt (niet in Cloud Shell) en Docker hebt geïnstalleerd, voert u de container uit om de toepassing te bekijken in een webbrowser voordat u de basisinstallatiekopie opnieuw bouwt. Als u Cloud Shell gebruikt, slaat u deze sectie over (Cloud Shell biedt geen ondersteuning voor `az acr login` of `docker run`).

Eerst worden geverifieerd bij uw containerregister met [az acr login][az-acr-login]:

```azurecli
az acr login --name $ACR_NAME
```

Voer de container nu lokaal uit met `docker run`. Vervang **\<run-id\>** door de gevonden run-id in de uitvoer van de vorige stap (bijvoorbeeld 'da6'). In dit voorbeeld-namen van de container `myapp` en bevat de `--rm` parameter voor de container verwijderen wanneer u deze stoppen.

```bash
docker run -d -p 8080:80 --name myapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Ga naar `http://localhost:8080` in uw browser. U ziet nu het weergegeven nummer van de Node.js-versie op de webpagina, zoals hieronder weergegeven. In een latere stap verhoogt u de versie door een 'a' toe te voegen aan de versietekenreeks.

![Schermafbeelding van de voorbeeldtoepassing die wordt weergegeven in de browser][base-update-01]

Als u wilt stoppen en verwijderen van de container, moet u de volgende opdracht uitvoeren:

```bash
docker stop myapp
```

## <a name="list-the-builds"></a>De builds weergeven

Lijst met de taak wordt vervolgens uitgevoerd dat taken van de ACR is voltooid voor uw register met de [az acr list-wordt uitgevoerd][az-acr-task-list-runs] opdracht:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Als u de vorige zelfstudie hebt voltooid (en het register niet hebt verwijderd), ziet u de uitvoer zoals hieronder weergegeven. Noteer het aantal taak-runs en de meest recente run-id, zodat u de uitvoer kunt vergelijken nadat u in het volgende gedeelte de basisinstallatiekopie hebt bijgewerkt.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK            PLATFORM    STATUS     TRIGGER     STARTED               DURATION
--------  --------------  ----------  ---------  ----------  --------------------  ----------
da6       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual      2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit  2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual      2018-09-17T22:29:59Z  00:00:57
```

## <a name="update-the-base-image"></a>De basisinstallatiekopie bijwerken

Hier kunt u een frameworkpatch in de basisinstallatiekopie simuleren. Bewerk de **Dockerfile-basis**  en voeg een 'a' toe achter het versienummer dat is gedefinieerd in `NODE_VERSION`:

```Dockerfile
ENV NODE_VERSION 9.11.2a
```

Voer een snelle taak uit om de gewijzigde basisinstallatiekopie te bouwen. Noteer de waarde van **run-id** in de uitvoer.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Zodra de build is voltooid en de ACR-taak de nieuwe basisinstallatiekopie naar uw register heeft gepusht, wordt automatisch een build van de toepassingsinstallatiekopie geactiveerd. Het kan enkele ogenblikken duren voordat de toepassingsinstallatiekopie wordt geactiveerd door de taak die u eerder hebt gemaakt, omdat met de taak de pas gemaakte en gepushte basisinstallatiekopie moet worden gedetecteerd.

## <a name="list-updated-build"></a>Bijgewerkte build weergeven

Nu u de basisinstallatiekopie hebt bijgewerkt, geeft u opnieuw een lijst van uw taken weer om deze te vergelijken met de vorige lijst. Als de uitvoer in eerste instantie niet verschilt, voert u regelmatig de opdracht uit tot de nieuwe taak-run in de lijst wordt weergegeven.

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

De uitvoer lijkt op het volgende. De TRIGGER voor de als laatste uitgevoerde build moet Update van installatiekopie zijn. Hiermee wordt aangegeven dat de taak is gestart door uw snelle taak van de basisinstallatiekopie.

```console
$ az acr task list-builds --registry $ACR_NAME --output table

Run ID    TASK            PLATFORM    STATUS     TRIGGER       STARTED               DURATION
--------  --------------  ----------  ---------  ------------  --------------------  ----------
da8       taskhelloworld  Linux       Succeeded  Image Update  2018-09-17T23:11:50Z  00:00:33
da7                       Linux       Succeeded  Manual        2018-09-17T23:11:27Z  00:00:35
da6       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual        2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit    2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual        2018-09-17T22:29:59Z  00:00:57
```

Als u de volgende optionele stap van de nieuw gebouwde container wilt uitvoeren om het bijgewerkte versienummer te zien, noteert u de **RUN ID**-waarde voor het bijwerken van de door de installatiekopie getriggerde build (in de vorige uitvoer is dit 'da8').

### <a name="optional-run-newly-built-image"></a>Optioneel: Nieuw gebouwde installatiekopie uitvoeren

Als u lokaal werkt (niet in Cloud Shell) en u hebt Docker geïnstalleerd, voert u de nieuwe toepassingsinstallatiekopie uit zodra de build is voltooid. Vervang `<run-id>` door de run-id uit de vorige stap. Als u Cloud Shell gebruikt, slaat u deze sectie over (Cloud Shell biedt geen ondersteuning voor `docker run`).

```bash
docker run -d -p 8081:80 --name updatedapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Ga naar http://localhost:8081 in uw browser. U ziet nu het bijgewerkte nummer van de Node.js-versie (met de 'a') op de webpagina:

![Schermafbeelding van de voorbeeldtoepassing die wordt weergegeven in de browser][base-update-02]

U hebt uw **basis**installatiekopie bijgewerkt met een nieuw versienummer, maar de **toepassings**installatiekopie die als laatste is gebouwd, geeft nieuwe versie weer. ACR Tasks heeft uw wijziging van de basisinstallatiekopie herkend en heeft de toepassingsinstallatiekopie automatisch opnieuw opgebouwd.

Als u wilt stoppen en verwijderen van de container, moet u de volgende opdracht uitvoeren:

```bash
docker stop updatedapp
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u alle resources wilt verwijderen die u in deze zelfstudiereeks hebt gemaakt, met inbegrip van het containerregister, containerexemplaar, sleutelkluis en service-principal, gebruikt u de volgende opdrachten:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een taak configureert om automatisch builds van containerinstallatiekopieën te activeren wanneer een basisinstallatiekopie is bijgewerkt. Ga nu verder naar meer informatie over verificatie voor uw containerregister.

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
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-task-list-runs]: /cli/azure/acr
[az-acr-task]: /cli/azure/acr

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png
