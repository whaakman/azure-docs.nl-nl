---
title: Privé-Docker-containerregisters in Azure - overzicht
description: Kennismaking met de Azure Container Registry-service, waarmee u cloudgebaseerde, beheerde en persoonlijke Docker-registers kunt maken.
services: container-registry
author: stevelas
ms.service: container-registry
ms.topic: overview
ms.date: 06/28/2019
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: b8b4b5fc3ec15d921ff5580aff4d0202be1d38b9
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797904"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Inleiding tot privé-Docker-containerregisters in Azure

Azure Container Registry is een beheerde, persoonlijke Docker-registerservice op basis van de open-source Docker Registry 2.0. Maken en beheren van Azure-containerregisters voor het opslaan en beheren van uw persoonlijke Docker-containerinstallatiekopieën.

Gebruik van Azure-containerregisters met uw bestaande container-ontwikkeling en implementatie pijplijnen of installatiekopieën in Azure compileren met Azure Container Registry taken. Bouwen op aanvraag, of volledig automatiseren van builds met triggers zoals broncode wordt doorgevoerd en updates van installatiekopieën baseren.

Zie voor meer informatie over Docker en het register concepten, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/) en [over registers, -opslagplaatsen en installatiekopieën](container-registry-concepts.md).

## <a name="use-cases"></a>Gebruiksvoorbeelden

Haal installatiekopieën op vanuit een Azure-containerregister en push ze naar verschillende implementatiedoelen:

* **Schaalbare indelingssystemen** die toepassingen in een container beheren in hostclusters, waaronder [Kubernetes](https://kubernetes.io/docs/), [DC/OS](https://docs.mesosphere.com/) en [Docker Swarm](https://docs.docker.com/swarm/).
* **Azure-services** die het bouwen en uitvoeren van toepassingen op schaal ondersteunen, waaronder [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) en andere.

Ontwikkelaars kunnen ook naar een containerregister pushen als onderdeel van een ontwikkelingswerkstroom met containers. Bijvoorbeeld: een containerregister vanuit een continue integratie en levering hulpprogramma zoals [Azure pijplijnen](/azure/devops/pipelines/get-started/what-is-azure-pipelines.md) of [Jenkins](https://jenkins.io/).

Configureren van de ACR-taken voor toepassingsinstallatiekopieën automatisch opnieuw opbouwen wanneer hun basisinstallatiekopieën zijn bijgewerkt, of compileren van installatiekopieën automatiseren wanneer uw team code worden doorgevoerd in een Git-opslagplaats. WebTest met meerdere stappen taken voor het automatiseren van het bouwen, testen en patch toepassen op meerdere containerinstallatiekopieën parallel in de cloud maken.

Azure biedt verschillende hulpprogramma's zoals Azure-opdrachtregelinterface, Azure-portal en API-ondersteuning voor het beheren van uw Azure-containerregisters. Installeer desgewenst de [Docker-extensie voor Visual Studio Code](https://code.visualstudio.com/docs/azure/docker) en de [Azure-Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) -extensie voor het werken met uw Azure-containerregisters. Pull- en installatiekopieën pushen naar een Azure container registry of uitvoeren van taken van de ACR, alles vanuit Visual Studio Code.

## <a name="key-features"></a>Belangrijke functies

* **Register-SKU's** -maken van een of meer containerregisters in uw Azure-abonnement. Registers zijn beschikbaar in drie SKU's: [Basic, Standard en Premium](container-registry-skus.md), die ondersteuning biedt voor integratie van webhooks, registerverificatie met Azure Active Directory en verwijderfunctionaliteit voor. Maak een register op dezelfde Azure-locatie als uw implementaties om te profiteren van lokale opslag dichtbij in het netwerk van uw containerinstallatiekopieën. Gebruik de [geo-replicatie](container-registry-geo-replication.md)functie van Premium-registers voor geavanceerde replicatie- en distributiescenario's voor containerinstallatiekopieën. 

  U kunt [toegang beheren](container-registry-authentication.md) tot een containerregister met behulp van een Azure-identiteit, een door Azure Active Directory ondersteunde [service-principal](../active-directory/develop/app-objects-and-service-principals.md) of een opgegeven beheeraccount. Meld u aan bij het register met de Azure CLI of de standaard `docker login` opdracht.

* **Ondersteunde installatiekopieën en artefacten** -gegroepeerd in een opslagplaats, elke installatiekopie is een momentopname van een alleen-lezen van een compatibel is met Docker-container. Azure-containerregisters kunnen zowel Windows- als Linux-installatiekopieën bevatten. U beheert de namen van de installatiekopieën voor al uw containerimplementaties. Gebruik standaard-[Docker-opdrachten](https://docs.docker.com/engine/reference/commandline/) om installatiekopieën naar een opslagplaats te pushen of een installatiekopie uit een opslagplaats op te halen. Naast containerinstallatiekopieën met Docker, Azure Container Registry slaat [gerelateerde inhoud indelingen](container-registry-image-formats.md) zoals [Helm-grafieken](container-registry-helm-repos.md) en installatiekopieën die zijn gemaakt op de [Open Container initiatief OCI ()-installatiekopie Specificatie van indeling](https://github.com/opencontainers/image-spec/blob/master/spec.md).

* **Azure Container Registry taken** -gebruik [Azure Container Registry taken](container-registry-tasks-overview.md) (ACR taken) voor het stroomlijnen van het bouwen, testen, pushen en installatiekopieën in Azure implementeren. Bijvoorbeeld: ACR-taken gebruiken om uit te breiden van uw ontwikkeling binnenste-lus naar de cloud door het offloaden van `docker build` bewerkingen naar Azure. Configureer buildtaken om uw container-OS- en frameworkpatchingpijplijn te automatiseren en automatisch installatiekopieën te maken wanneer uw team code met bronbeheer doorvoert.

  [Taken met meerdere stappen](container-registry-tasks-overview.md#multi-step-tasks) bieden op basis van een stap taakdefinitie en kan worden uitgevoerd voor het ontwikkelen, testen en patchen van containerinstallatiekopieën in de cloud. Taakstappen definiëren afzonderlijke ontwikkel- en pushbewerkingen voor containerinstallatiekopieën. Ze kunnen ook de uitvoering definiëren van een of meer containers, waarbij elke stap de container als uitvoeringsomgeving gebruikt.

## <a name="next-steps"></a>Volgende stappen

* [Een containerregister maken met Azure Portal](container-registry-get-started-portal.md)
* [Een containerregister maken met de Azure-CLI](container-registry-get-started-azure-cli.md)
* [Container-builds en onderhoud met ACR taken automatiseren](container-registry-tasks-overview.md)
