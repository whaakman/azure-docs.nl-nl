---
title: Privé-Docker-containerregisters in Azure - overzicht
description: Kennismaking met de Azure Container Registry-service, waarmee u cloudgebaseerde, beheerde en persoonlijke Docker-registers kunt maken.
services: container-registry
author: stevelas
ms.service: container-registry
ms.topic: overview
ms.date: 09/25/2018
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: 0ffad78afd6acdfbcea8df970358c639706f14b8
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814245"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Inleiding tot privé-Docker-containerregisters in Azure

Azure Container Registry is een beheerde service voor [Docker-registers](https://docs.docker.com/registry/) gebaseerd op de open-source Docker Registry 2.0. Maak en onderhoud Azure-containerregisters om uw persoonlijke installatiekopieën voor [Docker-containers](https://www.docker.com/what-docker) op te slaan en te beheren.

Gebruik containerregisters in Azure met uw bestaande pijplijnen voor containerontwikkeling en -implementatie. Gebruik Azure Container Registry Build (ACR Build) om containerinstallatiekopieën in Azure te bouwen. Bouw op aanvraag of automatiseer builds volledig door het vastleggen van broncode en buildtriggers voor updates van basisinstallatiekopieën.

Zie het [Docker-overzicht](https://docs.docker.com/engine/docker-overview/) Voor achtergrondinformatie over Docker en containers.

## <a name="use-cases"></a>Gebruiksvoorbeelden

Haal installatiekopieën op vanuit een Azure-containerregister en push ze naar verschillende implementatiedoelen:

* **Schaalbare indelingssystemen** die toepassingen in een container beheren in hostclusters, waaronder [Kubernetes](http://kubernetes.io/docs/), [DC/OS](https://docs.mesosphere.com/) en [Docker Swarm](https://docs.docker.com/swarm/).
* **Azure-services** die het bouwen en uitvoeren van toepassingen op schaal ondersteunen, waaronder [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) en andere.

Ontwikkelaars kunnen ook naar een containerregister pushen als onderdeel van een ontwikkelingswerkstroom met containers. Bijvoorbeeld naar een containerregister vanuit doorlopende integratie- implementatieprogramma's als [Azure DevOps Services](https://docs.microsoft.com/azure/devops/) of [Jenkins](https://jenkins.io/).

Configureer ACR-taken om toepassingsinstallatiekopieën automatisch opnieuw te bouwen wanneer de basisinstallatiekopieën zijn bijgewerkt. Gebruik ACR Tasks om installatiekopiebuilds te automatiseren wanneer uw team code registreert in een Git-opslagplaats.

## <a name="key-concepts"></a>Belangrijkste concepten

* **Register**: maak een of meerdere containerregisters in uw Azure-abonnement. Registers zijn beschikbaar in drie SKU's: [Basic, Standard en Premium](container-registry-skus.md), die alle ondersteuning bieden voor integratie van webhooks, registerverificatie met Azure Active Directory en functionaliteit voor verwijderen. Maak een register op dezelfde Azure-locatie als uw implementaties om te profiteren van lokale opslag dichtbij in het netwerk van uw containerinstallatiekopieën. Gebruik de [geo-replicatie](container-registry-geo-replication.md)functie van Premium-registers voor geavanceerde replicatie- en distributiescenario's voor containerinstallatiekopieën. Een volledig gekwalificeerde registernaam heeft de notatie `myregistry.azurecr.io`.

  U kunt [toegang beheren](container-registry-authentication.md) tot een containerregister met behulp van een Azure-identiteit, een door Azure Active Directory ondersteunde [service-principal](../active-directory/develop/app-objects-and-service-principals.md) of een opgegeven beheeraccount. Meld u aan bij het register met de Azure-opdrachtregelinterface of de standaard `docker login`-opdracht.

* **Opslagplaats**: een register bevat een of meer opslagplaatsen, waarin groepen containerinstallatiekopieën worden opgeslagen. Azure Container Registry ondersteunt naamruimten voor opslagplaatsen op meerdere niveaus. Met naamruimten op meerdere niveaus kunt u installatiekopieën groeperen die gerelateerd zijn aan een specifieke app, of apps groeperen die gerelateerd zijn aan specifieke ontwikkelingsteams of operationele teams. Bijvoorbeeld:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` staat voor een bedrijfsbrede installatiekopie
  * `myregistry.azurecr.io/warrantydept/dotnet-build` staat voor een installatiekopie die wordt gebruikt om .NET-apps te maken die op de garantieafdeling worden gedeeld
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` staat voor een webinstallatiekopie, opgenomen in de app voor klantinzendingen die eigendom is van de garantieafdeling

* **Installatiekopie**: installatiekopieën worden opgeslagen in een opslagplaats. Elke installatiekopie is een alleen-lezenmomentopname van een met Docker compatibele container. Azure-containerregisters kunnen zowel Windows- als Linux-installatiekopieën bevatten. U beheert de namen van de installatiekopieën voor al uw containerimplementaties. Gebruik standaard-[Docker-opdrachten](https://docs.docker.com/engine/reference/commandline/) om installatiekopieën naar een opslagplaats te pushen of een installatiekopie uit een opslagplaats op te halen. Naast containerinstallatiekopieën worden in Azure Container Registry [gerelateerde inhoudsindelingen](container-registry-image-formats.md), zoals [Helm-grafieken](container-registry-helm-repos.md), die worden gebruikt om toepassingen in Kubernetes te implementeren.

* **Container**: een container definieert een softwaretoepassing en de afhankelijkheden opgenomen in een compleet bestandssysteem inclusief code, runtime, systeemwerkset en bibliotheken. U kunt Docker-containers uitvoeren op basis van Windows- of Linux-installatiekopieën die u ophaalt uit een containerregister. Containers die op een enkele machine worden uitgevoerd, delen de kernel van het besturingssysteem. Docker-containers zijn volledig overdraagbaar naar alle grote distributies van Linux, macOS en Windows.

## <a name="azure-container-registry-tasks"></a>Azure Container Registry Tasks

[Azure Container Registry Tasks](container-registry-tasks-overview.md) (ACR Tasks) is een reeks functies in Azure Container Registry om gestroomlijnde en efficiënte Docker-containerinstallatiekopiebuilds te maken in Azure. Gebruik ACR Tasks om uw interne ontwikkelingsactiviteiten uit te breiden naar de cloud door `docker build`-bewerkingen te offloaden naar Azure. Configureer buildtaken om uw container-OS- en frameworkpatchingpijplijn te automatiseren en automatisch installatiekopieën te maken wanneer uw team code met bronbeheer doorvoert.

[Taken met meerdere stappen](container-registry-tasks-overview.md#multi-step-tasks-preview), een preview-functie van ACR Tasks, biedt een stapsgewijze taakdefinitie en -uitvoering voor het ontwikkelen, testen en patchen van containerinstallatiekopieën in de cloud. Taakstappen definiëren afzonderlijke ontwikkel- en pushbewerkingen voor containerinstallatiekopieën. Ze kunnen ook de uitvoering definiëren van een of meer containers, waarbij elke stap de container als uitvoeringsomgeving gebruikt.

## <a name="next-steps"></a>Volgende stappen

* [Een containerregister maken met Azure Portal](container-registry-get-started-portal.md)
* [Een containerregister maken met de Azure-CLI](container-registry-get-started-azure-cli.md)
* [OS- en frameworkpatching automatiseren met ACR Tasks](container-registry-tasks-overview.md)
