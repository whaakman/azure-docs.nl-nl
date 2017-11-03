---
title: Overzicht van Service Fabric in Azure | Microsoft Docs
description: Een overzicht van Service Fabric, waar de toepassingen zijn samengesteld uit veel microservices om schaal en herstelmogelijkheden te bieden. Service Fabric is een platform voor gedistribueerde systemen voor het samenstellen van schaalbare, betrouwbare en toepassingen voor de cloud eenvoudig worden beheerd.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: masnider
ms.assetid: bbcc652a-a790-4bc4-926b-e8cd966587c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: overview
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: msfussell
ms.custom: mvc
ms.openlocfilehash: 8ff0d38a679b673b148dd808050eda82060cfe80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-azure-service-fabric"></a>Overzicht van Azure Service Fabric
Azure Service Fabric is een platform voor gedistribueerde systemen waarmee u gemakkelijk schaalbare en betrouwbare microservices en containers verpakt, implementeert en beheert. Service Fabric adressen ook aanzienlijke uitdagingen bij het ontwikkelen en beheren van systeemeigen cloudtoepassingen. Ontwikkelaars en beheerders kunnen complexe infrastructuurproblemen voorkomen en zich concentreren op het implementeren van bedrijfsspecifieke, veeleisende werkbelastingen die schaalbaar, betrouwbaar en beheerbaar zijn. Service Fabric vertegenwoordigt de volgende generatie platform voor het maken en beheren van deze bedrijfsniveau, laag 1, cloud-toepassingen uitvoeren in containers.

In deze korte video introduceert Service Fabric en microservices:<center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-overview/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="applications-composed-of-microservices"></a>Toepassingen bestaan uit microservices 
Service Fabric kunt u maken en beheren van schaalbare en betrouwbare toepassingen bestaan uit microservices die worden uitgevoerd op high-density op een gedeelde groep machines die worden aangeduid als een cluster. Het biedt een geavanceerde, lichtgewicht runtime voor het bouwen van gedistribueerde, schaalbare, staatloze en stateful microservices uitgevoerd in de containers. Biedt ook mogelijkheden voor uitgebreide toepassing om in te richten, implementeren, bewaken, upgrade/patch en geïmplementeerde toepassingen, met inbegrip van beperkte services te verwijderen.

Service Fabric drijft veel Microsoft-services vandaag, waaronder Azure SQL Database, Azure Cosmos DB, Cortana Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT Hub, Dynamics 365, Skype voor bedrijven en veel core Azure-services.

Service Fabric is afgestemd op cloud systeemeigen services die kunnen begin klein, naar behoefte worden uitgebreid om grootschalige met honderden of duizenden computers te maken.

De hedendaagse Internet scale services zijn opgebouwd uit microservices. Voorbeelden van microservices zijn protocol gateways, gebruikersprofielen, winkelen winkelwagentjes, inventaris verwerking, wachtrijen, en in de cache opslaat. Service Fabric is een microservices-platform die elke microservice (of de container) biedt een unieke naam op die stateless of stateful worden kan.

Service Fabric biedt uitgebreide mogelijkheden voor runtime en de levenscyclus voor toepassingen die bestaan uit een van deze microservices. Deze als host fungeert voor microservices in containers die zijn geïmplementeerd en geactiveerd via de Service Fabric-cluster. Een verplaatsing van virtuele machines naar containers maakt mogelijk een orde van grootte toename in dichtheid. Op deze manier wordt een andere orde van grootte in dichtheid mogelijk wanneer u containers naar microservices in deze containers gaat. Één cluster voor Azure SQL Database omvat bijvoorbeeld honderden computers met tienduizenden containers die als host fungeren voor een totaal van honderden of duizenden databases. Elke database is een stateful Service Fabric-microservice. 

Lees voor meer informatie over de aanpak microservices [waarom een microservices benadering voor het ontwikkelen van toepassingen?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Implementatie van de container en orchestration
Service Fabric is van Microsoft [container orchestrator](service-fabric-cluster-resource-manager-introduction.md) microservices implementeert in een cluster van machines. Microservices kunnen worden ontwikkeld in tal van manieren met behulp van de [Service Fabric modellen programming](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md), naar implementeren [code van uw keuze](service-fabric-deploy-existing-app.md). Belangrijker nog, kunt u beide services in processen en -services in containers in dezelfde toepassing elkaar. Als u alleen wilt [implementeren en beheren van containers](service-fabric-containers-overview.md), Service Fabric is een ideale keuze als een container orchestrator.

## <a name="any-os-any-cloud"></a>Een besturingssysteem in een cloud
Service Fabric kan overal worden uitgevoerd. In veel omgevingen, waaronder Azure of on-premises, op Windows Server of op Linux, kunt u clusters voor Service Fabric. U kunt zelfs clusters maken op andere openbare clouds. De ontwikkelomgeving in de SDK is bovendien **identieke** naar de productieomgeving met geen emulators die betrokken zijn. Met andere woorden, wat wordt uitgevoerd op uw lokale ontwikkeling-cluster worden geïmplementeerd op de clusters in andere omgevingen.

![Service Fabric-platform][Image1]

De Service Fabric .NET SDK is voor de ontwikkeling van Windows geïntegreerd met Visual Studio en Powershell. Zie [voorbereiden van uw ontwikkelomgeving in Windows](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started.md). De Service Fabric Java SDK is geïntegreerd met Eclipse voor Linux-ontwikkeling en Yeoman wordt gebruikt voor het genereren van sjablonen voor Java, .NET Core en containertoepassingen. Zie [uw ontwikkelingsomgeving op Linux voorbereiden](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started.md)

Lees voor meer informatie over het maken van clusters [maken van een cluster op Windows Server- of Linux](service-fabric-deploy-anywhere.md) of voor Azure maken van een cluster [via de Azure-portal](service-fabric-cluster-creation-via-portal.md).

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Staatloze en stateful microservices voor Service Fabric
Service Fabric kunt u toepassingen ontwikkelen die bestaan uit microservices of containers. Staatloze microservices (zoals protocol gateways en webproxy) gedurende niet een veranderlijke status buiten een aanvraag en de reactie van de service. Azure Cloud Services-werkrollen zijn een voorbeeld van een staatloze service. Stateful microservices (zoals gebruikersaccounts, databases, apparaten, on en wachtrijen) onderhouden de status van een veranderlijke, gezaghebbende afgezien van de aanvraag en het antwoord. De hedendaagse Internet-toepassingen bestaan uit een combinatie van staatloze en stateful microservices. 

Een sleutel differentiatie met Service Fabric wordt de nadruk op het ontwikkelen van stateful services, hetzij met de [ingebouwde programmeermodellen ](service-fabric-choose-framework.md) of met beperkte stateful services. De [toepassingsscenario's](service-fabric-application-scenarios.md) beschrijven de scenario's waar stateful services worden gebruikt.


## <a name="application-lifecycle-management"></a>De levenscyclus van Toepassingsbeheer
Service Fabric biedt ondersteuning voor de volledige toepassing lifecycle en CI/CD van cloudtoepassingen, met inbegrip van containers. Deze levenscyclus beheren bevat ontwikkeling tot implementatie, beheer van dagelijkse en op het uiteindelijke buiten gebruik stellen van onderhoud.

Beheermogelijkheden voor service Fabric-toepassing lifecycle toepassingsbeheerders en IT-operators gebruik van eenvoudige, lage-touch-werkstromen voor het inrichten, implementeren, patch inschakelen en bewaken van toepassingen. Deze ingebouwde werkstromen minder aanzienlijk de werkbelasting van IT-operators dat continu beschikbare toepassingen.

De meeste toepassingen bestaan uit een combinatie van staatloze en stateful microservices containers en andere uitvoerbare bestanden die samen zijn geïmplementeerd. Service Fabric kunt doordat de sterke-typen van de toepassingen, de implementatie van meerdere exemplaren van een toepassing. Elk exemplaar wordt beheerd en onafhankelijk bijgewerkt. Houd voor ogen dat Service Fabric implementeren containers of alle uitvoerbare bestanden en zodat ze betrouwbaar. Service Fabric kan bijvoorbeeld .NET, ASP.NET Core, node.js, Windows containers, Linux-containers, Java virtuele machines, scripts, Angular of elke willekeurige waarmee u uw toepassing implementeren.

Service Fabric is geïntegreerd met CI/CD-hulpprogramma's zoals [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Jenkins](https://jenkins.io/index.html), en [Octopus implementeren](https://octopus.com/) en kan worden gebruikt met een ander populaire CI/CD-hulpprogramma.

Lees voor meer informatie over de levenscyclus van Toepassingsbeheer [toepassing lifecycle](service-fabric-application-lifecycle.md). Zie voor meer informatie over het implementeren van code [implementeren van een gast uitvoerbaar bestand](service-fabric-deploy-existing-app.md).

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden
U kunt met behulp van Service Fabric:

* Implementeren naar Azure of lokale datacenters waarop Windows of Linux wordt uitgevoerd met nul codewijzigingen. Eenmalig schrijven en vervolgens implementeert een willekeurige plaats voor elke Service Fabric-cluster.
* Schaalbare toepassingen ontwikkelen die bestaan uit een van de microservices met behulp van de Service Fabric programming modellen, containers of code.
* Maximaal betrouwbare microservices voor staatloze en stateful ontwikkelen. Het ontwerp van uw toepassing met behulp van stateful microservices vereenvoudigen. 
* Gebruik het nieuwe model voor Reliable Actors om cloudobjecten te maken met de code van zelfstandige en status.
* Implementeren en te organiseren containers die containers voor Windows en Linux-containers zijn. Service Fabric is een container op de hoogte, stateful is, gegevens orchestrator.
* Toepassingen implementeren in seconden, op high-density met honderden of duizenden toepassingen of containers per computer.
* Implementeren van verschillende versies van dezelfde toepassing naast elkaar en elke toepassing onafhankelijk upgrade.
* De levenscyclus van uw toepassingen zonder uitvaltijd, met inbegrip van breken en vaste upgrades beheren.
* Uitbreiden of schalen van het aantal knooppunten in een cluster. Als u knooppunten schalen, is uw toepassingen automatisch schalen.
* Bewaken en onderzoeken van de status van uw toepassingen en beleidsregels voor het uitvoeren van automatische reparaties instellen.
* Bekijk de resource balancer indelen de herdistributie van toepassingen in het cluster. Service Fabric vanuit fouten herstelt en optimaliseert de verdeling van de belasting op basis van beschikbare resources.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie:
  * [Waarom een microservices benadering voor het ontwikkelen van toepassingen?](service-fabric-overview-microservices.md)
  * [Overzicht van de terminologie](service-fabric-technical-overview.md)
* Instellen van uw [Windows-ontwikkelomgeving](service-fabric-get-started.md)  
* Instellen van uw [ontwikkelomgeving Linux](service-fabric-get-started-linux.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
