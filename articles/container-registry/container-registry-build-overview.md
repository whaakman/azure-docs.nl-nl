---
title: OS- en framework patchen met Azure Container Registry Build (ACR Build) automatiseren
description: Een Kennismaking met ACR Build, een reeks functies in Azure Container Registry beveiligde biedt, geautomatiseerde build van container-installatiekopie en patching uit handen in de cloud.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 07/28/2018
ms.author: marsma
ms.openlocfilehash: 532817c6289c1718fd82a502e04dc10715ee7203
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343097"
---
# <a name="automate-os-and-framework-patching-with-acr-build"></a>OS- en framework patchen met ACR Build automatiseren

Containers bieden nieuwe niveaus voor virtualisatie, isoleren van toepassings- en developer-afhankelijkheden van infrastructuur en operationele vereisten. Wat resteert, is echter de noodzaak om de manier waarop deze toepassingsvirtualisatie is gevuld.

**ACR Build** is een suite met functies in Azure Container Registry. Het cloud-gebaseerde container-installatiekopie maken voor Linux, Windows en ARM biedt, en kunt automatiseren [OS en framework patching uit handen](#automate-os-and-framework-patching) voor uw Docker-containers.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="what-is-acr-build"></a>Wat is ACR Build?

Build van Azure Container Registry is een Azure-eigen container image build-service. Hiermee inner loop-ontwikkeling ACR Build in de cloud met on-demand-containerinstallatiekopie bouwt en geautomatiseerde builds op code doorvoeren en base broninstallatiekopie bijwerken.

Trigger-containerinstallatiekopie maakt automatisch als de code wordt doorgevoerd aan een Git-opslagplaats, of wanneer de basisinstallatiekopie van de container wordt bijgewerkt. Met de basisinstallatiekopie update wordt geactiveerd, kunt u uw besturingssysteem automatiseren en application framework-werkstroom, beveiligde omgevingen beheren terwijl ze voldoen aan de beveiligings-principals van onveranderbare containers patches.

## <a name="quick-build-inner-loop-extended-to-the-cloud"></a>Snelle Build: binnenste-lus uitgebreid naar de cloud

Het begin van het beheer van de levenscyclus start voordat ontwikkelaars hun eerste coderegels doorvoeren. De ACR Build [snel bouwen](container-registry-tutorial-quick-build.md) functie kunnen een ervaring met geïntegreerde lokale inner loop-ontwikkeling, offloading builds naar Azure. Met snelle wordt gemaakt, kunt u uw geautomatiseerde builddefinities voordat het doorvoeren van uw code controleren.

Met behulp van de vertrouwde `docker build` indeling, de [az acr build] [ az-acr-build] opdracht in de Azure CLI wordt een lokale context, verzonden naar de ACR Build-service en standaard de gemaakte installatiekopie naar het register bij gepusht is voltooid. ACR Build volgt de registers met geo-replicatie inschakelen van verspreide development-teams gebruikmaken van het dichtstbijzijnde gerepliceerde register.

ACR Build is bedoeld als een container van primitieve levenscyclus. Bijvoorbeeld, integreren met ACR Build uw CI/CD-oplossing. Door het uitvoeren van [az login] [ az-login] met een [service-principal][az-login-service-principal], uw CI/CD-oplossing kan verlenen [az acrbouwen] [ az-acr-build] opdrachten uit om te worden gehouden, trappen af met installatiekopieën builds.

Informatie over het gebruik van snelle Builds in de eerste ACR Build zelfstudie [containerinstallatiekopieën in de cloud bouwen met Azure Container Registry Build](container-registry-tutorial-quick-build.md).

## <a name="automatic-build-on-source-code-commit"></a>Automatische build op bron code doorvoeren

Gebruik ACR Build om automatisch een containerinstallatiekopie bouwen wanneer code wordt doorgevoerd aan een Git-opslagplaats. Buildtaken, kunnen worden geconfigureerd met de Azure CLI-opdracht [az acr build-taak][az-acr-build-task], kunt u een Git-opslagplaats en eventueel een vertakking en docker-bestand op te geven. Als uw team code naar de opslagplaats doorvoeringen, wordt een build van de containerinstallatiekopie die is gedefinieerd in de opslagplaats geactiveerd door een webhook ACR Build-gemaakt.

Meer informatie over het activeren van builds op bron code doorvoeren in de tweede ACR Build zelfstudie [automatiseren containerinstallatiekopie maakt met Azure Container Registry Build](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>OS- en framework patching uit handen automatiseren

De kracht van de ACR Build voor het verbeteren van uw container build-pijplijn echt zijn afkomstig uit de mogelijkheid voor het detecteren van een update naar een basisinstallatiekopie. Wanneer de bijgewerkte installatiekopie naar het register is gepusht, bouwen ACR Build automatisch op basis van deze toepassing afbeeldingen.

Containerinstallatiekopieën kunnen grotendeels worden onderverdeeld in *basis* afbeeldingen en *toepassing* afbeeldingen. De basisinstallatiekopieën bevatten doorgaans het besturingssysteem en toepassingsframeworks waarop uw toepassing is gemaakt, samen met andere aanpassingen. Deze basisinstallatiekopieën zijn doorgaans gebaseerd op openbare upstream afbeeldingen, bijvoorbeeld: [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet], of [Node.js][base-node]. Verschillende van uw toepassingsinstallatiekopieën mogelijk delen een gemeenschappelijke basisinstallatiekopie.

Wanneer de installatiekopie van een besturingssysteem of app-framework wordt bijgewerkt door de upstream maintainer, moet bijvoorbeeld met een kritieke OS-beveiligingspatch, u ook bijwerken de basisinstallatiekopieën om op te nemen van de kritieke oplossing. De installatiekopie van elke toepassing moet vervolgens ook opnieuw worden opgebouwd zodanig dat deze upstream oplossingen nu opgenomen in de basisinstallatiekopie.

Omdat de basisinstallatiekopie afhankelijkheden ACR Build dynamisch worden gedetecteerd bij het samenstellen van een containerinstallatiekopie, kan detecteren wanneer de basisinstallatiekopie van de toepassingsinstallatiekopie van een wordt bijgewerkt. Met een vooraf geconfigureerde [taak bouwen](container-registry-tutorial-base-image-update.md#create-build-task), ACR Build vervolgens **automatisch wordt de installatiekopie van elke toepassing** voor u. Met deze automatische detectie en opnieuw opbouwen, ACR Build bespaart u tijd en energie normaal gesproken moeten handmatig bijhouden en bijwerken van elke toepassing een installatiekopie van die verwijzen naar uw bijgewerkte basisinstallatiekopie.

Meer informatie over het besturingssysteem en framework patching uit handen in de zelfstudie derde ACR Build [automatiseren installatiekopie is gebaseerd op updates van basisinstallatiekopieën met Azure Container Registry Build](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Basisinstallatiekopie updates trigger bouwt voor de eerste Preview-versie, alleen wanneer de basis- en de toepassingsdatabase afbeeldingen bevinden zich in de dezelfde Azure container registry of openbaar toegankelijke Docker Hub-opslagplaatsen.

## <a name="next-steps"></a>Volgende stappen

Wanneer u klaar om OS en framework patchen bent met het bouwen van uw containerinstallatiekopieën in de cloud te automatiseren, bekijk de driedelige ACR Build zelfstudie serie.

> [!div class="nextstepaction"]
> [Zelfstudie: Containerinstallatiekopieën bouwen in de cloud met Azure Container Registry Build](container-registry-tutorial-quick-build.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
