---
title: Terminologie voor Azure Service Fabric Mesh | Microsoft Docs
description: Meer informatie over algemene voorwaarden voor Azure Service Fabric NET.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: db7a89ffa4ff1a2eb271869c93b7beb088fd13b3
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076156"
---
# <a name="service-fabric-mesh-terminology"></a>Service Fabric-NET-terminologie

Azure Service Fabric NET is een volledig beheerde service waarmee ontwikkelaars microservices-toepassingen implementeren zonder beheer van virtuele machines, opslag, of netwerken. Dit artikel worden de terminologie die wordt gebruikt door Azure Service Fabric NET om te begrijpen van de termen die worden gebruikt in de documentatie.

## <a name="service-fabric"></a>Service Fabric

Service Fabric is een open-source-platform voor gedistribueerde systemen waarmee u eenvoudig kunnen worden verpakt, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric is de orchestrator dat wordt gebruikt door Service Fabric NET. Service Fabric biedt opties voor het bouwen en uitvoeren van uw microservices-toepassingen. U kunt elk gewenst framework schrijven van uw services en kiezen waar u wilt uitvoeren van de toepassing uit meerdere mogelijkheden van de omgeving.

## <a name="deployment-and-application-models"></a>Implementatie en toepassing modellen 

Voor het implementeren van uw services, moet u om te beschrijven hoe u deze wilt uitvoeren. Service Fabric ondersteunt drie verschillende implementatiemodellen:

### <a name="resource-model"></a>Resourcemodel
Resources zijn iets die afzonderlijk kunnen worden geïmplementeerd naar Service Fabric, met inbegrip van toepassingen, services, netwerken en volumes. Resources worden gedefinieerd met behulp van een YAML-bestand of de JSON-bestand met behulp van de Resource-Model van Azure-schema.

Het resourcemodel is de eenvoudigste manier om te beschrijven van uw Service Fabric-toepassingen. De nadruk ligt op de eenvoudige implementatie en beheer van services in containers.

Resources kunnen worden geïmplementeerd waar dan ook die service Fabric wordt uitgevoerd.

### <a name="native-model"></a>Systeemeigen model
Het model systeemeigen toepassing biedt uw toepassingen met volledige op laag niveau toegang tot Service Fabric. Toepassingen en services worden gedefinieerd als het geregistreerde type in XML-manifestbestanden.

De systeemeigen model biedt ondersteuning voor het Reliable Services-framework, waarmee u toegang tot de Service Fabric-runtime-API's en cluster-API's in C# en Java. Het oorspronkelijke gegevensmodel biedt ook ondersteuning voor willekeurige containers en uitvoerbare bestanden.

Het oorspronkelijke gegevensmodel wordt niet ondersteund in de Service Fabric-NET-omgeving.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) maakt deel uit van de Docker-project. Service Fabric biedt beperkte ondersteuning voor het implementeren van toepassingen die gebruikmaken van het model Docker Compose.

## <a name="environments"></a>Omgevingen

Service Fabric is een open-source platform-technologie die verschillende verschillende services en producten zijn gebaseerd op. Microsoft biedt de volgende opties:

 - **Service Fabric NET**: een volledig beheerde service voor het uitvoeren van Service Fabric-toepassingen in Microsoft Azure.
 - **Azure Service Fabric**: de door Azure gehoste Service Fabric-cluster-aanbieding. Deze service biedt integratie tussen Service Fabric en de Azure-infrastructuur, samen met de upgrade en configuratie van beheer van Service Fabric-clusters.
 - **Zelfstandige service Fabric**: een set hulpprogramma's de installatie en configuratie voor [overal Service Fabric-clusters implementeren](/azure/service-fabric/service-fabric-deploy-anywhere) (on-premises of op elke andere cloudprovider). Niet beheerd door Azure.
 - **Service Fabric-cluster voor ontwikkeling**: biedt een lokale ontwikkeling onder Windows, Linux of Mac voor ontwikkeling van Service Fabric-toepassingen.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Omgeving framework en implementatiemodel ondersteuningsmatrix
Verschillende omgevingen hebben een ander niveau van ondersteuning van frameworks en -implementatiemodellen. De volgende tabel beschrijft de ondersteunde framework en combinaties van implementatie-model.

|Frameworks\Deployment model |Resourcemodel |Manifest van model | Opstellen|
|---|---|---|---|
|Reliable Actors en betrouwbare Services |Niet ondersteund |Ondersteund |Niet ondersteund |
|Elk andere willekeurig framework en taal |Ondersteund in containers |Ondersteund als processen en in containers |Ondersteund in containers |

De volgende tabel beschrijft de ondersteunde omgeving en de implementatie model combinaties.

|Environment\Deployment model |Resourcemodel |Manifest van model |Opstellen |
|---|---|---|---|
|Azure Service Fabric Mesh |Ondersteund |Niet ondersteund|Niet ondersteund |
|Alle andere omgevingen |Ondersteunde (sommige resources zijn vereisten voor het werken in een omgeving) |Ondersteund |Beperkte ondersteuning |

## <a name="next-steps"></a>Volgende stappen

Lees het overzicht voor meer informatie over Service Fabric NET:
- [Service Fabric-NET-overzicht](service-fabric-mesh-overview.md)
