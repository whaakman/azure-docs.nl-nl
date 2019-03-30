---
title: Architectuur van Azure Service Fabric | Microsoft Docs
description: Service Fabric is een platform voor gedistribueerde systemen gebruikt voor het bouwen van schaalbare, betrouwbare en gemakkelijk te beheren toepassingen voor de cloud. In dit artikel ziet u de architectuur van Service Fabric.
services: service-fabric
documentationcenter: .net
author: rishirsinha
manager: chackdan
editor: rishirsinha
ms.assetid: 6b554243-70cb-4c22-9b28-1a8b4703f45e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/12/2017
ms.author: rsinha
ms.openlocfilehash: a1e68e2e39ea6f1c8cf8669e2e02d8dacaf0f284
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664587"
---
# <a name="service-fabric-architecture"></a>Service Fabric-architectuur
Service Fabric is gebouwd met gelaagde subsystemen. Deze subsystemen kunnen u toepassingen schrijft die zijn:

* Hoge beschikbaarheid
* Schaalbaar
* Beheerbare
* Testable

Het volgende diagram toont de belangrijkste subsystemen van Service Fabric.

![Diagram van Service Fabric-architectuur](media/service-fabric-architecture/service-fabric-architecture.png)

In een gedistribueerd systeem is de mogelijkheid om veilig te communiceren tussen knooppunten in een cluster van cruciaal belang. Op basis van de stack is het transport-subsysteem, waarmee u veilige communicatie tussen knooppunten. Boven het transport subsysteem wordt veroorzaakt door de federation-subsysteem, waarmee clusters van de verschillende knooppunten in een enkele entiteit (met de naam van clusters) zodat Service Fabric kan detecteren van fouten, het uitvoeren van leider en consistente routering bieden. Het subsysteem betrouwbaarheid, gelaagd boven op de federation-subsysteem is verantwoordelijk voor de betrouwbaarheid van de Service Fabric-services via mechanismen zoals replicatie, resourcebeheer en failover. Het subsysteem voor Federatie is ook opslagstructuur achter het subsysteem die als host fungeert en activering, die u de levenscyclus van een toepassing op een enkel knooppunt beheert. Het subsysteem management beheert de levenscyclus van toepassingen en services. Het subsysteem testbaarheid helpt ontwikkelaars van toepassingen testen van hun services via gesimuleerde fouten voor en na de implementatie van toepassingen en services in productieomgevingen. Service Fabric biedt de mogelijkheid om op te lossen servicelocaties via de communicatiesubsysteem. De programmeermodellen voor toepassingen beschikbaar gemaakt voor ontwikkelaars zijn gelaagd boven op deze subsystemen samen met het toepassingsmodel om in te schakelen van hulpprogramma's.

## <a name="transport-subsystem"></a>Subsysteem voor transport
Het subsysteem voor transport implementeert een point-to-point datagram-communicatiekanaal. Dit kanaal wordt gebruikt voor communicatie in service fabric-clusters en communicatie tussen de service fabric-cluster en clients. Het ondersteunt één richting en aanvraag / antwoord communicatiepatronen, die de basis voor het implementeren van broadcast en multicast in de Federation-laag. Het subsysteem voor transport communicatie beveiligt met behulp van X509 certificaten of Windows-beveiliging. Dit subsysteem wordt intern gebruikt door de Service Fabric en is niet rechtstreeks toegankelijk zijn voor ontwikkelaars voor het programmeren van toepassing.

## <a name="federation-subsystem"></a>Federation-subsysteem
Als u wilt reden dan ook over een set knooppunten in een gedistribueerd systeem, moet u een consistente weergave van het systeem. De federation-subsysteem maakt gebruik van de communicatie primitieven geleverd door het subsysteem voor transport en dan aan elkaar gehecht verschillende knooppunten in een centrale cluster dat u deze kunt reden over. Het biedt de gedistribueerde systemen primitieven die nodig zijn voor de andere subsystemen - foutdetectie, selectie van leider en routering, consistent zijn. Het subsysteem voor Federatie is gebaseerd op gedistribueerde hash-tabellen met een spatie van 128-bits-token. Het subsysteem maakt een ringtopologie via de knooppunten, met elk knooppunt in de ring die een subset van de token ruimte voor de eigenaar wordt toegewezen. Voor foutdetectie, maakt een leasemechanisme op basis van hart kloppen in de race en arbitrage gebruik van de laag. De federation-subsysteem garandeert ook via het complexe lid worden en vertrek-protocollen die slechts één eigenaar van een token op elk gewenst moment bestaat. Dit biedt selectie van leider en consistente routering garanties.

## <a name="reliability-subsystem"></a>Betrouwbaarheid subsysteem
Het subsysteem voor de betrouwbaarheid biedt het mechanisme om de status van een Service Fabric-service maximaal beschikbaar is via het gebruik van maken de *Replicator*, *Failover Manager*, en *Resource Balancer*.

* De replicatie zorgt ervoor dat in de primaire service-replica de status verandert automatisch moeten worden gerepliceerd naar de secundaire replica's onderhouden van de consistentie tussen de primaire en secundaire replica's in een service-replicaset. De replicatie is verantwoordelijk voor quorumbeheer tussen de replica's in de replicaset. De interactie met de failover-eenheid aan de lijst met bewerkingen om te repliceren en de herconfiguratie van agent biedt dit met de configuratie van de replicaset. Deze configuratie geeft aan welke replica's die de bewerkingen moeten worden gerepliceerd. Service Fabric biedt een standaard replicator Fabric Replicator, die kan worden gebruikt door de programming model-API om de status van de service maximaal beschikbare en betrouwbare genoemd.
* De Failover Manager zorgt ervoor dat wanneer knooppunten zijn toegevoegd aan of verwijderd uit het cluster, de belasting wordt automatisch opnieuw verdeeld over de beschikbare knooppunten. Als een knooppunt in het cluster is mislukt, wordt het cluster automatisch opnieuw geconfigureerd de service-replica's voor het onderhouden van beschikbaarheid.
* De Resource Manager service-replica's geplaatst in het domein van de fout in het cluster en zorgt ervoor dat alle failover-eenheden operationeel zijn. De Resource Manager ook geeft een balans tussen service-resources voor de onderliggende gedeelde groep met clusterknooppunten te bereiken van optimale uniform verdeling.

## <a name="management-subsystem"></a>Subsysteem voor beheer
Het subsysteem voor beheer biedt end-to-end-service en levensduurbeheer van toepassingen. PowerShell-cmdlets en beheer-API's kunt u inrichten, implementeren, patchen, bijwerken en inrichting ongedaan maken toepassingen zonder verlies van beschikbaarheid. Het subsysteem management wordt dit door de volgende services uitgevoerd.

* **Clusterbeheer**: Dit is de primaire service die met de Failover Manager van betrouwbaarheid communiceert en plaatst u de toepassingen op de knooppunten op basis van de plaatsingsbeperkingen voor de service. De Resource Manager in failover-subsysteem zorgt ervoor dat de beperkingen die nooit verbroken zijn. Clustermanager beheert de levenscyclus van de toepassingen van inrichten inrichting ongedaan maken. Het is geïntegreerd met de health manager om ervoor te zorgen dat de beschikbaarheid is niet verloren vanuit het oogpunt van de semantische health tijdens upgrades.
* **Health Manager**: Deze service kunt statuscontrole van toepassingen, services en cluster-entiteiten. Cluster-entiteiten (zoals knooppunten, servicepartities en replica's) kunnen rapporteren dat de gezondheid van gegevens, die vervolgens kan worden samengevoegd in de gecentraliseerde health store. Deze informatie health biedt een momentopname van een algemene point-in-time-status van de services en de knooppunten die zijn verdeeld over meerdere knooppunten in het cluster, zodat u kunt de benodigde corrigerende actie ondernemen. Status query dat API 's kunt u de health-gebeurtenissen query gerapporteerd aan de health-subsysteem. De health-query API's geven de van onbewerkte gezondheidsgegevens die zijn opgeslagen in de health store of de samengevoegde geïnterpreteerd statusgegevens voor een specifieke cluster-entiteit.
* **Afbeelding Store**: Deze service biedt opslag en distributie van de binaire bestanden voor de toepassing. Deze service biedt een eenvoudige gedistribueerd bestandsarchief waarin de toepassingen zijn geüpload naar en gedownload uit.

## <a name="hosting-subsystem"></a>Hosting-subsysteem
Clustermanager informeert de hosting subsysteem (uitgevoerd op elk knooppunt) welke services nodig om voor een bepaald knooppunt te beheren. Het subsysteem voor hosting beheert vervolgens de levenscyclus van de toepassing op dat knooppunt. De interactie met de betrouwbaarheid en de gezondheid van onderdelen om ervoor te zorgen dat de replica's goed worden geplaatst en in orde zijn.

## <a name="communication-subsystem"></a>Communicatiesubsysteem
Dit subsysteem biedt betrouwbare berichten in de detectie van het cluster en de service via de Naming-service. De Naming-service wordt servicenamen omgezet naar een locatie in het cluster en waarmee gebruikers voor het beheren van servicenamen en eigenschappen. Clients, dat is de Naming-service gebruikt, kunnen veilig communiceren met een willekeurig knooppunt in het cluster kunt oplossen door de naam van een service en de metagegevens op te halen. Met behulp van een eenvoudige Naming-client-API, kunnen gebruikers van Service Fabric ontwikkelen met services en clients waarvoor het omzetten van de huidige netwerklocatie ondanks knooppunt dynamiek of het wijzigen van het formaat van het cluster.

## <a name="testability-subsystem"></a>Subsysteem voor testbaarheid
Testbaarheid is een suite met hulpprogramma's die speciaal is ontworpen voor het testen van services die zijn gemaakt in Service Fabric. De hulpprogramma's kunt een ontwikkelaar eenvoudig zinvolle fouten veroorzaken en Testscenario's om te oefenen en valideer de verschillende statussen en overgangen die een service tijdens de levensduur, allemaal in een gecontroleerde en veilige manier optreden zal uitvoeren. Testbaarheid biedt ook een mechanisme voor het uitvoeren van meer tests die kunnen herhalen tot en met verschillende mogelijke fouten zonder verlies van beschikbaarheid. Dit biedt u een test in productie-omgeving.

