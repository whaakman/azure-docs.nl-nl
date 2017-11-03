---
title: Architectuur van Azure Service Fabric | Microsoft Docs
description: Service Fabric is een platform voor gedistribueerde systemen gebruikt voor het ontwikkelen van toepassingen voor schaalbare, betrouwbare en gemakkelijk te beheren voor de cloud. In dit artikel wordt de architectuur van Service Fabric.
services: service-fabric
documentationcenter: .net
author: rishirsinha
manager: timlt
editor: rishirsinha
ms.assetid: 6b554243-70cb-4c22-9b28-1a8b4703f45e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/12/2017
ms.author: rsinha
ms.openlocfilehash: 3d1f9d6136622e0e9fc1e725d8230dbedd6af24a
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="service-fabric-architecture"></a>Service Fabric-architectuur
Service Fabric is gebouwd met gelaagde subsystemen. Deze subsystemen kunnen u toepassingen die zijn geschreven:

* Maximaal beschikbare
* Schaalbaar
* Beheerbare
* Testable

Het volgende diagram toont de belangrijkste subsystemen van Service Fabric.

![Diagram van Service Fabric-architectuur](media/service-fabric-architecture/service-fabric-architecture.png)

In een gedistribueerde systeem is de mogelijkheid beveiligde communicatie tussen knooppunten in een cluster van cruciaal belang. Het grondtal van de stack is het subsysteem transport, waarmee u veilige communicatie tussen knooppunten. Subsysteem liggen boven het transport de federation-subsysteem, waarmee clusters van de andere knooppunten in één entiteit (met de naam clusters) zodat de Service Fabric kunt fouten opsporen, opvulteken keuze uitvoeren en bieden consistente routering. Het subsysteem betrouwbaarheid, gelaagd boven op het subsysteem federation is verantwoordelijk voor de betrouwbaarheid van Service Fabric-services via mechanismen zoals replicatie bronbeheer en failover. Het subsysteem federation voltooiingscallback ook het subsysteem hosting en activering, die de levenscyclus van een toepassing op één knooppunt beheert. Het subsysteem management beheert de levenscyclus van toepassingen en services. Het subsysteem testbaarheid kan ontwikkelaars hun services via gesimuleerde fouten testen voor en na de implementatie van toepassingen en services in productieomgevingen. Service Fabric biedt de mogelijkheid om op te lossen servicelocaties via de communicatiesubsysteem. De toepassing programmeermodellen blootgesteld aan ontwikkelaars zijn gelaagd boven op deze subsystemen samen met de toepassingsmodel tooling inschakelen.

## <a name="transport-subsystem"></a>Subsysteem voor transport
Het subsysteem transport implementeert een point-to-point communicatie datagramkanaal. Dit kanaal wordt gebruikt voor communicatie in service fabric-clusters en de communicatie tussen de service fabric-cluster en clients. Deze ondersteuning biedt voor één richting en aanvragen / antwoorden communicatiepatronen, die de basis voor het implementeren van broadcast en multicast in de Federation-laag. Het subsysteem transport communicatie beveiligt met behulp van X509 certificaten of Windows-beveiliging. Dit subsysteem wordt intern gebruikt door de Service Fabric en is niet rechtstreeks toegankelijk zijn voor ontwikkelaars voor het programmeren van toepassing.

## <a name="federation-subsystem"></a>Subsysteem voor Federatie
Om de reden dan ook over een verzameling van knooppunten in een gedistribueerde systeem, moet u een consistente weergave van het systeem. Het subsysteem federation maakt gebruik van de communicatie primitieven geleverd door het subsysteem transport en dan de verschillende knooppunten in een uniform cluster die deze reden kan over aan elkaar gehecht. Het biedt de gedistribueerde systemen primitieven nodig is voor de andere subsystemen - foutdetectie, opvulteken keuze en consistente routering. Het subsysteem voor Federatie is ingebouwd in gedistribueerde hashtabellen met een 128-bits token ruimte. Het subsysteem maakt een ringtopologie op de knooppunten, met op elk knooppunt in de ring is een subset van de token ruimte voor de eigenaar wordt toegewezen. Voor de detectie is mislukt gebruikt de laag een leasemechanisme op basis van hart kloppen in de race en arbitrage. Het subsysteem federation garandeert ook via het complexe lid worden en vertrek protocollen die slechts één eigenaar van een token op elk gewenst moment bestaat. Dit biedt opvulteken keuze en consistente routering garanties.

## <a name="reliability-subsystem"></a>Subsysteem voor betrouwbaarheid
Het subsysteem betrouwbaarheid biedt het mechanisme om de status van een Service Fabric-service maximaal beschikbaar met behulp van de *Replicator*, *Failover Manager*, en *Resource Balancer*.

* De Replicator zorgt ervoor dat wijzigingen in de replica van primaire service automatisch worden gerepliceerd naar de secundaire replica's, onderhouden van de consistentie tussen de primaire en secundaire replica's in een service-replicaset. De replicatie is verantwoordelijk voor quorumbeheer tussen de replica's in de replicaset. Deze samenwerkt met de failover-eenheid voor de lijst met bewerkingen voor replicatie en de agent herconfiguratie biedt dit met de configuratie van de replicaset. Deze configuratie geeft aan welke replica de bewerkingen moeten worden gerepliceerd. Service Fabric bevat een standaard replicator Fabric Replicator, die kan worden gebruikt door de programming model API waarmee de status van de service maximaal beschikbare en betrouwbare aangeroepen.
* De Failover Manager zorgt ervoor dat als knooppunten worden toegevoegd aan of verwijderd uit het cluster, de belasting wordt automatisch opnieuw verdeeld over de beschikbare knooppunten. Als een knooppunt in het cluster uitvalt, wordt het cluster automatisch opnieuw configureren de service-replica's voor het onderhouden van beschikbaarheid.
* De Resource Manager service replica's geplaatst in domein van de fout in het cluster en zorgt ervoor dat alle failover-eenheden operationeel zijn. De Resource Manager ook een compromis tussen de servicebronnen via de onderliggende gedeelde groep van clusterknooppunten voor optimale uniform verdeling.

## <a name="management-subsystem"></a>Subsysteem voor beheer
Het subsysteem management biedt end-to-end-service en de levenscyclus van Toepassingsbeheer. PowerShell-cmdlets en administratieve API's kunt u inrichten, implementeren, patch, upgrade en ongedaan inrichten toepassingen zonder verlies van beschikbaarheid. Het subsysteem management voert dit via de volgende services.

* **Clusterbeheer**: dit is de primaire service die met Failover Manager van betrouwbaarheid communiceert plaatsen van de toepassingen op de knooppunten op basis van de plaatsingsbeperkingen voor de service. De Resource Manager in failover-subsysteem zorgt ervoor dat de beperkingen nooit verbroken. Clusterbeheer beheert de levenscyclus van de toepassingen van de inrichting ongedaan inrichten. Het is geïntegreerd met de health manager om ervoor te zorgen dat de beschikbaarheid van de toepassing niet verloren vanuit het oogpunt van de semantische health van tijdens upgrades is.
* **De Health Manager**: met deze service kan de statuscontrole van toepassingen, services en entiteiten van de cluster. Cluster-entiteiten (zoals knooppunten, servicepartities en replica's) kunnen melden statusgegevens, klikt u vervolgens in het archief gecentraliseerde status wordt geaggregeerd. Deze informatie health biedt een momentopname van een algemene point-in-time-status van de services en de knooppunten die zijn verdeeld over meerdere knooppunten in het cluster, zodat u de benodigde corrigerende actie ondernemen. Health-query-API's kunnen u de health-gebeurtenissen query gerapporteerd aan de health-subsysteem. De health-query API's retourneren de onbewerkte statusgegevens opgeslagen in de health store of de geaggregeerde geïnterpreteerd statusgegevens voor een specifieke cluster-entiteit.
* **Image Store**: deze service biedt opslag en distributie van de binaire bestanden van de toepassing. Deze service biedt een eenvoudige gedistribueerde archief waar de toepassingen zijn geüpload naar en van gedownload.

## <a name="hosting-subsystem"></a>Subsysteem voor hosting
Clusterbeheer informeert het hosting subsysteem (uitgevoerd op elk knooppunt) welke services moet beheren voor een bepaald knooppunt. Het subsysteem voor hosting beheert vervolgens de levenscyclus van de toepassing op dat knooppunt. Deze samenwerkt met de status en -betrouwbaarheid onderdelen om ervoor te zorgen dat de replica correct zijn ingevoerd, en zijn in orde.

## <a name="communication-subsystem"></a>Communicatiesubsysteem
Dit subsysteem biedt betrouwbare berichten binnen de detectie van het cluster en de service via de Naming service. De service Naming servicenamen worden omgezet in een locatie in het cluster en kan gebruikers beheren servicenamen en eigenschappen. Via de Naming service, communiceren clients veilig met een willekeurig knooppunt in het cluster een servicenaam oplossen en ophalen van metagegevens. Met behulp van een eenvoudige Naming client API kunnen gebruikers van Service Fabric ontwikkelen services en clients die geschikt is voor het omzetten van de huidige netwerklocatie ondanks knooppunt dynamiek of het wijzigen van het formaat van het cluster.

## <a name="testability-subsystem"></a>Subsysteem voor testbaarheid
Testbaarheid is een suite met hulpprogramma's die speciaal is ontworpen voor het testen van services is gebaseerd op de Service Fabric. De hulpprogramma's kunnen een ontwikkelaar gemakkelijk zinvolle fouten veroorzaken en Testscenario's als u wilt uitoefenen en valideren van de talloze statussen en overgangen die een service tijdens de levensduur, allemaal in een beheerde en veilige wijze optreden zal uitgevoerd. Testbaarheid biedt ook een mechanisme om uit te voeren langer tests die kunnen sequentieel via verschillende mogelijke fouten zonder verlies van beschikbaarheid. Dit biedt u een test in productie-omgeving.

