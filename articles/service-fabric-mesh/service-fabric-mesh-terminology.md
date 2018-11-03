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
ms.openlocfilehash: 925d97658a299bea983b16ae6b507159ef0e9e62
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979128"
---
# <a name="service-fabric-mesh-terminology"></a>Service Fabric-NET-terminologie

Azure Service Fabric Mesh is een volledig beheerde service waarmee ontwikkelaars microservices-toepassingen kunnen implementeren zonder virtuele machines, opslag of netwerken hoeven te beheren. Dit artikel worden de terminologie die wordt gebruikt door Azure Service Fabric NET om te begrijpen van de termen die worden gebruikt in de documentatie.

## <a name="service-fabric"></a>Service Fabric

[Service Fabric](/azure/service-fabric/) is een open-source-platform voor gedistribueerde systemen waarmee u eenvoudig kunnen worden verpakt, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric is de orchestrator dat wordt gebruikt door Service Fabric NET. Service Fabric biedt opties voor het bouwen en uitvoeren van uw microservices-toepassingen. U kunt elk gewenst framework schrijven van uw services en kiezen waar u wilt uitvoeren van de toepassing uit meerdere mogelijkheden van de omgeving.

## <a name="application-and-service-concepts"></a>Toepassing en serviceconcepten

**Service Fabric-NET toepassing**: Service Fabric NET-toepassingen worden beschreven door de [Resourcemodel](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources) (YAML en JSON-resource-bestanden) en kan worden geïmplementeerd in een omgeving waarin de Service Fabric wordt uitgevoerd.

**Service Fabric-systeemeigen toepassing**: Native Service Fabric-toepassingen worden beschreven door de [systeemeigen toepassingsmodel](/azure/service-fabric/service-fabric-application-model) (op basis van een XML-toepassings- en servicemanifesten).  Service Fabric-systeemeigen toepassingen uitvoeren niet in Service Fabric NET.

**Toepassing**: NET van een Service Fabric-toepassing is de eenheid van de implementatie, versiebeheer en levensduur van een Mesh-toepassing. De levenscyclus van elk toepassingsexemplaar kan onafhankelijk van elkaar worden beheerd.  Toepassingen bestaan uit een of meer code-servicepakketten en -instellingen. Een toepassing is gedefinieerd met behulp van het schema van de Azure Resource-Model (RM).  Services worden omschreven als de eigenschappen van de bron van de toepassing in een RM-sjabloon.  Netwerken en de volumes die worden gebruikt door de toepassing wordt verwezen door de toepassing.  Bij het maken van een toepassing, zijn de toepassing, de service (s), het netwerk en de volumes gemodelleerd met behulp van het Resourcemodel dat Service Fabric.

**Service**: een service in een toepassing een microservice vertegenwoordigt en vervult een functie volledig en zelfstandig. Elke service bestaat uit een of meer codepakketten die alles wat nodig is om uit te voeren van de containerinstallatiekopie die is gekoppeld aan het codepakket wordt beschreven.  Het aantal services in een toepassing kunt omhoog en omlaag worden geschaald.

**Codepakket**: codepakketten beschrijven alles wat nodig is om uit te voeren van de installatiekopie van de container zijn gekoppeld aan de codepakket, met inbegrip van het volgende:

* De containernaam van de, versie en -register
* CPU en geheugen-resources die vereist voor elke container
* Network-eindpunten
* Volumes te koppelen aan de container, die verwijst naar een ander volume-resource.

## <a name="deployment-and-application-models"></a>Implementatie en toepassing modellen 

Voor het implementeren van uw services, moet u om te beschrijven hoe u deze wilt uitvoeren. Service Fabric ondersteunt drie verschillende implementatiemodellen:

### <a name="resource-model"></a>Resourcemodel
Service Fabric-Resources worden alle items die afzonderlijk kunnen worden geïmplementeerd naar Service Fabric; met inbegrip van toepassingen, services, netwerken en volumes. Resources worden gedefinieerd met behulp van een JSON-bestand naar een clustereindpunt kan worden geïmplementeerd.  Voor Service Fabric NET, wordt het Model van Azure-Resource-schema gebruikt. Een YAML-bestandsschema kan ook worden gebruikt om eenvoudiger definitiebestanden. Resources kunnen worden geïmplementeerd waar dan ook die service Fabric wordt uitgevoerd. Het resourcemodel is de eenvoudigste manier om te beschrijven van uw Service Fabric-toepassingen. De nadruk ligt op de eenvoudige implementatie en beheer van services in containers. Voor meer informatie lezen [Inleiding tot de Service Fabric-Resourcemodel](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources).

### <a name="native-model"></a>Systeemeigen model
Het model systeemeigen toepassing biedt uw toepassingen met volledige op laag niveau toegang tot Service Fabric. Toepassingen en services worden gedefinieerd als het geregistreerde type in XML-manifestbestanden.

De systeemeigen model biedt ondersteuning voor het Reliable Services-framework, waarmee u toegang tot de Service Fabric-runtime-API's en cluster-API's in C# en Java. Het oorspronkelijke gegevensmodel biedt ook ondersteuning voor willekeurige containers en uitvoerbare bestanden.

Het oorspronkelijke gegevensmodel wordt niet ondersteund in de Service Fabric-NET-omgeving.  Zie voor meer informatie, [programming model overview](/azure/service-fabric/service-fabric-choose-framework).

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

| Type van de toepassing | Wordt beschreven in | Azure Service Fabric Mesh | Azure Service Fabric-Clusters (elk besturingssysteem)| Lokale cluster | Zelfstandig cluster |
|---|---|---|---|---|---|---|---|---|---|
| Service Fabric Mesh-toepassingen | Resource-Model (YAML & JSON) | Ondersteund |Niet ondersteund | Windows - ondersteund, Linux en Mac-niet ondersteund | Windows-niet ondersteund |
|Native service Fabric-toepassingen | Systeemeigen toepassingsmodel (XML) | Niet ondersteund| Ondersteund|Ondersteund|Windows: ondersteund|

De volgende tabel beschrijft de modellen van de andere toepassing en de tooling die voor hen op basis van Service Fabric bestaat.

| Type van de toepassing | Wordt beschreven in | Visual Studio | Eclipse | SFCTL | AZ-CLI | PowerShell|
|---|---|---|---|---|---|---|---|---|---|
| Service Fabric Mesh-toepassingen | Resource-Model (YAML & JSON) | VISUAL STUDIO 2017 |Niet ondersteund |Niet ondersteund | Ondersteund - alleen Mesh-omgeving | Niet ondersteund|
|Native service Fabric-toepassingen | Systeemeigen toepassingsmodel (XML) | Visual Studio 2017 en VS 2015| Ondersteund|Ondersteund|Ondersteund|Ondersteund|

## <a name="next-steps"></a>Volgende stappen

Lees [Wat is Service Fabric Mesh?](service-fabric-mesh-overview.md) voor meer informatie over Service Fabric Mesh.

Vind antwoorden op [Veelgestelde vragen over](service-fabric-mesh-faq.md).
