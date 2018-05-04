---
title: OS- en framework patchen met Azure Container register bouwen (ACR Build) automatiseren
description: Een inleiding tot ACR bouwen, een reeks functies in Azure Container register veilige, waarmee automatisch maken van de container image en patchen in de cloud.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 04/30/2018
ms.author: marsma
ms.openlocfilehash: 0e10b415f3d1c7ca502cfd63ebd27a6adcb69635
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="automate-os-and-framework-patching-with-acr-build"></a>OS- en framework patchen met ACR bouwen automatiseren

Containers bieden nieuwe niveaus van virtualisatie, toepassing en ontwikkelaars afhankelijkheden van infrastructuur- en operationele vereisten isoleren. Wat blijft, is echter de noodzaak voor het oplossen van hoe deze toepassingsvirtualisatie is hersteld.

**ACR bouwen**, een reeks functies in Azure Container register, biedt niet alleen systeemeigen container image build mogelijkheid, maar ook automatiseert [OS en framework patchen](#automate-os-and-framework-patching) voor uw Docker-containers.

> [!IMPORTANT]
> ACR bouwen momenteel preview en wordt alleen ondersteund door Azure container registers in de **VS-Oost** en **West-Europa** regio's. Voorbeelden voor u beschikbaar zijn aangebracht op voorwaarde dat u akkoord met gaat de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functionaliteit kunnen wijzigen voordat deze functionaliteit algemeen beschikbaar wordt.

## <a name="what-is-acr-build"></a>Wat is ACR bouwen?

Azure Container register bouwen is een Azure-systeemeigen container image build-service. Kan interne lus ontwikkeling ACR bouwen in de cloud met de installatiekopie van de container op aanvraag gebaseerd en geautomatiseerde bouwt voort op code doorvoeren en base broninstallatiekopie bijwerken.

De installatiekopie van de trigger-container bouwt automatisch als code is doorgevoerd naar een Git-opslagplaats, of wanneer de basisinstallatiekopie van een container wordt bijgewerkt. Met basisinstallatiekopie update triggers, kunt u uw besturingssysteem automatiseren en application framework-werkstroom, beveiligde omgevingen terwijl ze voldoen aan de principals van niet-wijzigbaar containers onderhouden patchen.

## <a name="quick-build-inner-loop-extended-to-the-cloud"></a>Snelle Build: interne-lus uitgebreid naar de cloud

Het begin van het levenscyclusbeheer wordt gestart voordat de ontwikkelaars hun eerste coderegels doorvoeren. De ACR bouwen [snelle bouwen](container-registry-tutorial-quick-build.md) functie kunt u een geïntegreerde lokale interne lus ontwikkeling-ervaring, offloading builds naar Azure. Met snelle wordt gemaakt, kunt u uw geautomatiseerde build-definities voordat het doorvoeren van uw code te controleren.

Met behulp van de vertrouwde `docker build` indeling, de [az acr build] [ az-acr-build] opdracht in de Azure CLI duurt een lokale context, verzendt het naar de service ACR bouwen en pushes standaard installatiekopie van het ingebouwde in het register op voltooiing. ACR bouwen volgt de registers geogerepliceerde verspreid ontwikkelteams gebruikmaken van het dichtstbijzijnde gerepliceerde register inschakelen. Tijdens de preview, ACR build is al beschikbaar in de regio's VS-Oost en West-Europa.

ACR bouwen is bedoeld als een container van primitieve levenscyclus. Bijvoorbeeld, integreren ACR bouwen in uw oplossing CI/CD. Door het uitvoeren van [az aanmelding] [ az-login] met een [service-principal][az-login-service-principal], uw oplossing CI/CD kan vervolgens uitgeven [az acrbouwen] [ az-acr-build] installatiekopie builds van opdrachten uit om te starten.

Informatie over het gebruik van snelle Builds in de eerste ACR bouwen zelfstudie [maken van installatiekopieën van de container in de cloud met Azure Container register bouwen](container-registry-tutorial-quick-build.md).

## <a name="automatic-build-on-source-code-commit"></a>Automatische build op bron code doorvoeren

Gebruik ACR bouwen voor het automatisch starten van een installatiekopie van een container maken als code is doorgevoerd naar een Git-opslagplaats. Buildtaken, kunnen worden geconfigureerd met de Azure CLI-opdracht [az acr opbouwtaak][az-acr-build-task], kunt u een Git-opslagplaats en eventueel een vertakking en Dockerfile opgeven. Als uw team doorgevoerd code aan de opslagplaats, activeert een webhook ACR bouwen gemaakt een build van de installatiekopie van de container gedefinieerd in de opslagplaats.

Meer informatie over het activeren van builds op bron code doorvoeren in de tweede ACR bouwen zelfstudie [automatiseren container image te maken met het register van Container in Azure bouwen](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>OS- en framework patchen automatiseren

De kracht van ACR bouwen voor het verbeteren van de container build-pipeline echt zijn afkomstig van de mogelijkheid voor het detecteren van een update aan voor een basisinstallatiekopie. Wanneer de bijgewerkte installatiekopie toe aan het register wordt doorgeschoven, kan ACR bouwen automatisch samenstellen op basis van deze toepassing afbeeldingen.

Container afbeeldingen kunnen ruwweg worden onderverdeeld in *base* installatiekopieën en *toepassing* installatiekopieën. Uw basisinstallatiekopieën omvatten het besturingssysteem en App-frameworks waarop uw toepassing is gebouwd, samen met andere aanpassingen. Deze basisinstallatiekopieën zijn zelf meestal gebaseerd op openbare upstream-installatiekopieën, bijvoorbeeld [Alpine Linux] [ base-alpine] of [Node.js][base-node]. Enkele van uw toepassing afbeeldingen mogelijk een algemene basisinstallatiekopie delen.

Wanneer de installatiekopie van een besturingssysteem of app-framework wordt bijgewerkt door de upstream maintainer, moet bijvoorbeeld met een kritieke OS beveiligingspatch, u ook bijwerken uw basisinstallatiekopieën met de kritieke correctie. De installatiekopie van elke toepassing moet vervolgens ook opnieuw worden opgebouwd zodanig dat deze upstream oplossingen nu opgenomen in uw basisinstallatiekopie.

Omdat ACR bouwen basisinstallatiekopie afhankelijkheden dynamisch ontdekt bij het samenstellen van een installatiekopie van een container, kunnen worden gedetecteerd wanneer de basisinstallatiekopie van de toepassingsinstallatiekopie van een wordt bijgewerkt. Met een vooraf geconfigureerde [bouwen taak](container-registry-tutorial-base-image-update.md#create-build-task), ACR bouwen vervolgens **automatisch wordt de installatiekopie van elke toepassing** voor u. Met deze automatische detectie en opnieuw opbouwen, ACR bouwen bespaart u tijd en moeite normaal gesproken nodig om handmatig te volgen en elke toepassing bijwerken die verwijzen naar uw bijgewerkte basisinstallatiekopie een installatiekopie.

Meer informatie over het besturingssysteem en framework patchen in de zelfstudie derde ACR bouwen [automatiseren installatiekopie is gebaseerd op update basisinstallatiekopie met Azure Container register bouwen](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Hoewel ACR bouwen in preview, bouwt basisinstallatiekopie updates trigger alleen als de basis- en toepassingsproblemen afbeeldingen zich bevinden in het register met dezelfde Azure-container.

## <a name="next-steps"></a>Volgende stappen

Wanneer u klaar bent om OS en framework patchen door het maken van installatiekopieën container in de cloud te automatiseren, controleert u uit de drie delen ACR bouwen zelfstudie reeks.

> [!div class="nextstepaction"]
> [Maken van installatiekopieën van de container in de cloud met Azure Container register bouwen](container-registry-tutorial-quick-build.md)

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
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
