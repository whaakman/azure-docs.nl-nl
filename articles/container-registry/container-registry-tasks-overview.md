---
title: Automatiseer het bouwen en patching uit handen containerinstallatiekopieën met taken van Azure Container Registry (ACR-taken)
description: Een inleiding tot ACR taken, een reeks functies in Azure Container Registry biedt beveiligde, geautomatiseerde container-installatiekopie bouwen, beheren en patching uit handen in de cloud.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 5089650996693b81e548bba8d89c0de29a8afd93
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147990"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>Container-installatiekopie builds en onderhoud met ACR taken automatiseren

Containers bieden nieuwe niveaus voor virtualisatie, isoleren van toepassings- en developer-afhankelijkheden van infrastructuur en operationele vereisten. Wat resteert, is echter de noodzaak om op te lossen, hoe deze toepassingsvirtualisatie wordt beheerd en is een patch uitgevoerd gedurende de levenscyclus van de container.

## <a name="what-is-acr-tasks"></a>Wat is ACR taken?

**ACR taken** is een suite met functies in Azure Container Registry. Het cloud-gebaseerde container-installatiekopie maken voor Linux, Windows en ARM biedt, en kunt automatiseren [OS en framework patching uit handen](#automate-os-and-framework-patching) voor uw Docker-containers. ACR taken wordt niet alleen de 'inner loop' ontwikkelingscyclus naar de cloud met compileren van installatiekopieën op aanvraag container is een uitbreiding, maar er wordt een gebeurtenishub geautomatiseerde builds op bron code doorvoeren of wanneer de basisinstallatiekopie van de container wordt bijgewerkt. Met de basisinstallatiekopie update wordt geactiveerd, kunt u uw besturingssysteem automatiseren en application framework-werkstroom, beveiligde omgevingen beheren terwijl ze voldoen aan de beveiligings-principals van onveranderbare containers patches.

Bouwen en testen van containerinstallatiekopieën met ACR-taken in vier manieren:

* [Snelle taak](#quick-task): Bouw en push-container afbeeldingen op aanvraag, in Azure, zonder een lokale Docker-Engine-installatie. Denkt `docker build`, `docker push` in de cloud. Samengesteld op basis van de lokale broncode of een Git-opslagplaats.
* [Bouwen op bron doorvoercode](#automatic-build-on-source-code-commit): Een build van container-installatiekopie automatisch wordt geactiveerd wanneer de code wordt doorgevoerd aan een Git-opslagplaats.
* [Bouwen op updates van basisinstallatiekopieën](#automate-os-and-framework-patching): Een build van container-installatiekopie geactiveerd wanneer de basisinstallatiekopie van die installatiekopie is bijgewerkt.
* [Taken met meerdere stappen](#multi-step-tasks): WebTest met meerdere stappen taken die afbeeldingen bouwen, containers run as-opdrachten en installatiekopieën pushen naar een register definiëren. Deze functie van de ACR-taken ondersteunt de uitvoering van de taak op aanvraag en parallelle installatiekopie bouwen, testen en push-bewerkingen.

## <a name="quick-task"></a>Snelle taken

De binnenste lus ontwikkelingscyclus, het iteratief proces van het schrijven van code, het ontwikkelen en testen van uw toepassing voordat u vastlegt op broncodebeheer, is in feite het begin van het levenscyclusbeheer van de container.

Voordat u uw eerste line-of-doorvoeren-code, van de taken van de ACR [snelle taak](container-registry-tutorial-quick-task.md) functie krijgt u een geïntegreerde ontwikkelervaring door het offloaden van uw containerinstallatiekopie is gebaseerd op Azure. Met snelle taken, kunt u uw geautomatiseerde definities bouwen en mogelijke problemen voordat het doorvoeren van uw code catch controleren.

Met behulp van de vertrouwde `docker build` indeling, de [az acr build] [ az-acr-build] opdracht in de Azure CLI gebruikt u een *context* (de set van bestanden om te bouwen), verzendt het ACR-taken en standaard de gemaakte installatiekopie naar het register na voltooiing gepusht.

Zie voor een introductie, de Snelstartgids voor het [bouwen en uitvoeren van een containerinstallatiekopie](container-registry-quickstart-task-cli.md) in Azure Container Registry.  

De volgende tabel ziet u enkele voorbeelden van context ondersteunde locaties voor ACR taken:

| Locatie van de context | Description | Voorbeeld |
| ---------------- | ----------- | ------- |
| Lokaal bestandssysteem | Bestanden in een map op het lokale bestandssysteem. | `/home/user/projects/myapp` |
| Master-vertakking van GitHub | De vertakking van de bestanden in de master (of andere standaard) van een GitHub-opslagplaats.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub-vertakking | Specifieke vertakking van een GitHub-opslagplaats.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub-submap | Bestanden in een submap in een GitHub-opslagplaats. Voorbeeld ziet u de combinatie van een vertakking en submap-specificatie. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| Externe tarball | Bestanden in een gecomprimeerd archief op een externe webserver. | `http://remoteserver/myapp.tar.gz` |

Taken van de ACR is bedoeld als een container van primitieve levenscyclus. Bijvoorbeeld taken ACR integreren in uw CI/CD-oplossing. Door het uitvoeren van [az login] [ az-login] met een [service-principal][az-login-service-principal], uw CI/CD-oplossing kan verlenen [az acrbouwen] [ az-acr-build] opdrachten uit om te worden gehouden, trappen af met installatiekopieën builds.

Informatie over het gebruik van snelle taken in de eerste zelfstudie van de ACR-taken [installatiekopieën in de cloud met Azure Container Registry taken compileren](container-registry-tutorial-quick-task.md).

## <a name="automatic-build-on-source-code-commit"></a>Automatische build op bron code doorvoeren

ACR-taken gebruiken om automatisch een containerinstallatiekopie bouwen wanneer de code wordt doorgevoerd aan een Git-opslagplaats. Buildtaken, kunnen worden geconfigureerd met de Azure CLI-opdracht [az acr taak][az-acr-task], kunt u een Git-opslagplaats en eventueel een vertakking en docker-bestand op te geven. Als uw team code naar de opslagplaats doorvoeringen, wordt een build van de containerinstallatiekopie die is gedefinieerd in de opslagplaats geactiveerd door een webhook ACR taken gemaakt.

> [!IMPORTANT]
> Als u eerder taken hebt gemaakt tijdens de preview met de opdracht `az acr build-task`, moeten deze taken opnieuw worden gemaakt met de opdracht [az acr task][az-acr-task].

Meer informatie over het activeren van builds op bron code doorvoeren in de tweede zelfstudie ACR taken [automatiseren containerinstallatiekopie maakt met Azure Container Registry taken](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>OS- en framework patching uit handen automatiseren

De kracht van de ACR-taken voor het verbeteren van de werkstroom van uw container build echt zijn afkomstig uit de mogelijkheid voor het detecteren van een update naar een basisinstallatiekopie. Wanneer de bijgewerkte installatiekopie naar het register is gepusht, of een basisinstallatiekopie wordt bijgewerkt in een openbare opslagplaats, zoals in Docker Hub, kunt ACR taken automatisch op basis van deze toepassing afbeeldingen bouwen.

Containerinstallatiekopieën kunnen grotendeels worden onderverdeeld in *basis* afbeeldingen en *toepassing* afbeeldingen. De basisinstallatiekopieën bevatten doorgaans het besturingssysteem en toepassingsframeworks waarop uw toepassing is gemaakt, samen met andere aanpassingen. Deze basisinstallatiekopieën zijn zich doorgaans op basis van openbare upstream afbeeldingen, bijvoorbeeld: [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet], of [Node.js ][base-node]. Verschillende van uw toepassingsinstallatiekopieën mogelijk delen een gemeenschappelijke basisinstallatiekopie.

Wanneer de installatiekopie van een besturingssysteem of app-framework wordt bijgewerkt door de upstream maintainer, moet bijvoorbeeld met een kritieke OS-beveiligingspatch, u ook bijwerken de basisinstallatiekopieën om op te nemen van de kritieke oplossing. De installatiekopie van elke toepassing moet vervolgens ook opnieuw worden opgebouwd zodanig dat deze upstream oplossingen nu opgenomen in de basisinstallatiekopie.

Omdat het ACR-taken gedetecteerd dynamisch basisinstallatiekopie afhankelijkheden bij het samenstellen van een containerinstallatiekopie, kan detecteren wanneer de basisinstallatiekopie van de toepassingsinstallatiekopie van een wordt bijgewerkt. Met een vooraf geconfigureerde [taak bouwen](container-registry-tutorial-base-image-update.md#create-a-task), ACR taken vervolgens **automatisch wordt de installatiekopie van elke toepassing** voor u. Met deze automatische detectie en opnieuw opbouwen, ACR taken bespaart u tijd en energie normaal gesproken moeten handmatig bijhouden en bijwerken van elke toepassing een installatiekopie van die verwijzen naar uw bijgewerkte basisinstallatiekopie.

Meer informatie over het besturingssysteem en framework patching uit handen in de derde zelfstudie ACR taken [automatiseren installatiekopie is gebaseerd op updates van basisinstallatiekopieën met Azure Container Registry taken](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Op dit moment werkt basisinstallatiekopie trigger builds alleen als zowel de basisklassen en toepassing afbeeldingen bevinden zich in dezelfde Azure container registry, of de base bevindt zich in een openbare Docker Hub of Microsoft Container Registry-opslagplaats.

## <a name="multi-step-tasks"></a>Taken met meerdere stappen

WebTest met meerdere stappen taken bieden op basis van een stap taakdefinitie en kan worden uitgevoerd voor het bouwen, testen en toepassen van patches containerinstallatiekopieën in de cloud. Taakstappen definiëren afzonderlijke ontwikkel- en pushbewerkingen voor containerinstallatiekopieën. Ze kunnen ook de uitvoering definiëren van een of meer containers, waarbij elke stap de container als uitvoeringsomgeving gebruikt.

U kunt bijvoorbeeld een taak meerdere stappen waarmee de volgende maken:

1. De afbeelding van een web-toepassing bouwen
1. De web-App-container uitvoeren
1. Bouw een web-toepassing test-installatiekopie
1. Uitvoeren van de toepassing test webcontainer die tests uit op basis van de actieve voert App-container
1. Als de tests lukken, een Helm-grafiek Archiefpakket bouwen
1. Voer een `helm upgrade` met behulp van de nieuwe Archiefpakket van de Helm-grafiek

Taken met meerdere stappen kunnen u het gebouw, uitvoeren en testen van een afbeelding in meer samenstelbare stappen, met ondersteuning voor de afhankelijkheid tussen stap splitsen. Met meerdere stappen taken in de ACR-taken hebt u meer nauwkeurige controle over de installatiekopie van het ontwikkelen, testen, en OS en framework werkstromen patches.

Meer informatie over taken meerdere stappen in [WebTest met meerdere stappen bouwen, testen en patch-taken uitvoeren in ACR taken](container-registry-tasks-multi-step.md).

## <a name="view-task-logs"></a>Logboeken van de taak weergeven

Elke uitvoering van de taak genereert logboekuitvoer die u controleren kunt om te bepalen of de taakstappen is uitgevoerd. Als u de [az acr build](/cli/azure/acr#az-acr-build), [az acr uitvoeren](/cli/azure/acr#az-acr-run), of [az acr-taak uitvoeren](/cli/azure/acr/task#az-acr-task-run) opdracht voor het activeren van de taak, logboekuitvoer voor de taak wordt uitgevoerd worden gestreamd naar de console en ook worden opgeslagen voor later voor het ophalen. De logboeken voor een taak uitvoeren in Azure portal of gebruik de [az acr taak logboeken](/cli/azure/acr/task#az-acr-task-logs) opdracht.

Vanaf juli 2019, gegevens en logboeken voor de taak wordt uitgevoerd in een register wordt worden standaard 30 dagen bewaard en vervolgens automatisch opgeschoond. Als u archiveren van de gegevens voor een taak wordt uitgevoerd wilt, schakelt u archiveren met behulp van de [az acr update-uitgevoerd](/cli/azure/acr/task#az-acr-task-update-run) opdracht. Het volgende voorbeeld wordt het archiveren van de taak uitvoeren *cf11* in register *myregistry*.

```azurecli
az acr task update-run --registry myregistry --run-id cf11 --no-archive false
```

## <a name="next-steps"></a>Volgende stappen

Wanneer u klaar bent om OS en framework patchen met het bouwen van uw containerinstallatiekopieën in de cloud automatiseren, leest de driedelige [ACR taken reeks](container-registry-tutorial-quick-task.md).

Installeer desgewenst de [Docker-extensie voor Visual Studio Code](https://code.visualstudio.com/docs/azure/docker) en de [Azure-Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) -extensie voor het werken met uw Azure-containerregisters. Pull- en installatiekopieën pushen naar een Azure container registry of uitvoeren van taken van de ACR, alles vanuit Visual Studio Code.

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task]: /cli/azure/acr
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
