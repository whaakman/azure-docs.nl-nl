---
title: Azure SQL Database Azure casestudy - Daxko/CSI | Microsoft Docs
description: Meer informatie over hoe Daxko/CSI SQL-Database gebruikt de ontwikkelingscyclus en en de klantenservices en de prestaties verbeterd
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 00c8a713-f20c-4d6b-b8b7-0c1b9ba5f05b
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Inactive
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 7a05836be4a0879fa7103d070c683f45c06cd741
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI gebruikt Azure de ontwikkelingscyclus en en de klantenservices en de prestaties verbeterd
![Daxko/CSI-Logo](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI Software geconfronteerd met een challenge: klantendatabase geschiktheid en opnieuw gemaakt is groeien snel, dankzij het succes van de oplossing uitgebreide-softwareproducten voor ondernemingen, maar houden met de behoeften van de IT-infrastructuur voor die klant groeiende basis van het bedrijf is testen IT-personeel. Het bedrijf is steeds beperkt door stijgende operations-overhead, met name voor het beheren van de groeiende databases. Slechter, die overhead operations is knippen in ontwikkeling resources voor nieuwe initiatieven, zoals nieuwe functies voor mobiliteit voor software van het bedrijf.

Door Azure geleverde volgens David Molina, directeur van ontwikkeling van het Product bij Daxko/CSI CSI Software met het platform as a service (PaaS)-model dat nodig is voor de database-beheer te vereenvoudigen, de schaalbaarheid en resources zich richten op de software vrij te maken in plaats van ops. "Azure SQL Database is een goede optie voor ons. Omdat u niet hoeft te hoeven maken over het onderhouden van een SQL-Server, een failover-cluster en alle de andere infrastructuur moet is ideaal voor ons."

Sinds migreren naar Azure, moet CSI Software een operationele personeel van slechts twee voor het beheren van meer dan 600 klantendatabases. Het bedrijf gebruikt Azure SQL Database elastische pools verplaatsen van de klantendatabases op basis van de grootte en nodig.

Molina blijft, 'onze klanten merkbaar de wijziging onmiddellijk. Voordat elastische pools hadden van tijd tot tijd ze-outs en andere problemen tijdens burst perioden. Met Azure elastische pools, kunnen ze burst naar behoefte en gebruik van de software zonder problemen."

Naast het verbeteren van de prestaties voor klanten, Azure elastische pools vrijgemaakt CSI Software resources te concentreren op het ontwikkelen van nieuwe services en functies in plaats van te moeten omgaan met bewerkingen en beheer. De IT-bronnen heeft geholpen CSI Software verbeteren van de enterprise-software biedt, SpectrumNG om te benaderen maakt leden, personeel-efficiëntie te verbeteren en personeel en leden mobiele toegang bieden voor interactieve taken en realtime meldingen.

Azure heeft ook CSI Software versnellen en de ontwikkeling en kwaliteit-assurance (QA) cyclus verbeteren doordat automatiseringsopties geholpen. Met Azure-implementatie van het bedrijf kunnen managers build-onderdelen met één klik op een knop verpakken. Zoals in Molina wordt beschreven, 'als onderdeel van de releasecyclus QA is nu in staat om te implementeren voor een testomgeving in Azure, dit komt sterk onze productie-stack overeen. We kunnen builds onmiddellijk implementeren op onze dev-omgeving naar doorloop wijzigingen. Die is een grote win voor ons, omdat we pariteit voor testen voorafgaand aan die niet beschikbaar."

## <a name="offloading-to-the-cloud"></a>Offloading naar de cloud
Voordat u doorgaat naar de cloud, CSI Software had is gebouwd op basis de eigen multitenant-infrastructuur in een lokaal datacenter in Houston. Het bedrijf is uitgevouwen, het toenemende groeipijn van aanschaffen, het inrichten en beheren van alle hardware en software die nodig is voor de ondersteuning van zijn klanten te maken. IT is een andere knelpunt dat heeft geleid tot een vertraging van nieuwe resources inrichten en implementeren van nieuwe services aan klanten personeel voor het afhandelen van bewerkingen geworden.

CSI Software opgezocht in de cloud-opties voor die overhead verwijderen zodat deze zich op de code, in plaats van bewerkingen concentreren kan. Het bedrijf wordt gedetecteerd dat veel van de bovenste cloudproviders alleen oplossingen aan te infrastructure as a service (IaaS) waarvoor nog steeds een grote IT-medewerkers bieden voor het beheren van de IaaS-stack. In de end bepaald CSI Software dat de Azure-PaaS-oplossing het meest geschikt is voor de behoeften is. Molina wordt uitgelegd, "Azure haalt de hardware en software uit de weg, zodat we ons kunnen richten op de offerings software bij onze IT-overhead verminderen."

## <a name="making-the-transition-to-azure"></a>Waardoor de overgang naar Azure
Na het selecteren van Azure voor de PaaS-oplossing, begon CSI Software met het migreren van de back-end-infrastructuur en de databases naar de cloud. Voorafgaand aan Azure, SpectrumNG klanten nodig voor het installeren van een clienttoepassing die gecommuniceerd met een Windows Communication Foundation (WCF)-service op de back-end. Volgens Molina moesten' Hoewel sommige klanten alles in hun eigen datacenters gehost, uit het product worden multitenant. We gehost alles in een datacenter in Houston, met behulp van SQL Server als gegevensopslag.

"Ons product bieden ook opgenomen lid gerichte webportal (geschreven in ASP.net), die is ontworpen voor het niet overeenkomen met de website van de klant en een SOAP-API voor de ondersteuning van de online-pagina's en eventuele integratie van derden wit gelabeld."

De migratie naar de cloud is niet lang duren voor de architectuur. Volgens Molina, 'Het merendeel van de inspanning afgehandeld aanpassing van de manier die we config bestandsgegevens, een wijziging gecentraliseerde verbindingsreeks en automatisering van de verpakking, uploaden en implementeren van onze releases lezen'.

Voor het ontwikkelen van de build-automatisering gebruikt CSI softwareontwikkelaars Azure PowerShell en REST-API's pakketten maken en te uploaden naar een testomgeving voor elke nacht release.
De algehele overgang naar een Azure-cloud-gebaseerde implementatie is snel en zonder problemen gegaan voor het CSI Software IT-team. Molina wordt uitgelegd, 'In alle, hebben we een beta-omgeving in de cloud binnen drie tot vier weken op het project te nemen. Dat was een verrassend win voor ons."

Na het configureren en testen van de omgeving, CSI Software begon met het migreren van klanten. Voor klanten die al gebruikmaken van CSI Software die als host fungeert, is de overgang bijna naadloze. Voor klanten die migratie van een on-premises implementatie is de migratie naar de cloud wat extra tijd nodig was, maar is nog steeds voornamelijk pijn vrij voor klanten en CSI Software.

Voor nieuwe klanten, CSI Software van IT-medewerkers gebruiken het volgende proces voor het ingebouwde naar Azure:

1. Azure PowerShell-scripts worden gebruikt met ronddraaien een nieuwe database voor de klant. alle klanten start uit op een premium-laag om ervoor te zorgen voldoende initiële doorvoer voor de overgang.
2. Indien mogelijk, CSI Software maakt gebruik van de Azure SQL Migration Wizard bestaande om gegevens te verplaatsen naar een Azure SQL Database-exemplaar.
3. Ten slotte worden Microsoft SQL Server Integration Services (SSIS) gebruikt om eventuele verschillen in de gegevens in overeenstemming brengen of om uit te voeren opschoontaken gegevens zoals vereist.

Vandaag de dag worden ongeveer 99 procent van klanten CSI Software gehost in Azure, in vier regionale datacenters (Noord-centraal, Zuid-centraal, Oost en West). Doordat de datacentra in elke klant geografische regio, wordt de latentie tot een minimum beperkt.

## <a name="azure-elastic-pools-free-up-it-resources"></a>Azure elastische pools om een IT-bronnen vrij te maken
Verschillende functies van Azure hebben bijgedragen CSI Software shift wordt de infrastructuur en bewerkingen voor een functie en ontwikkeling die gericht is gericht. Mogelijk is het grootste voordeel van elastische pools.

Op dit moment biedt CSI Software ongeveer 550 databases voor klanten. Vóór elastische pools was het moeilijk te beheren dat veel databases binnen een structuur laag. OPS managers moest prestatielagen op basis van de burst behoeften van klanten, wat aanzienlijk IT-resource overhead vereist toewijzen. Met elastische pools kunnen managers tenants premium- of standard-pool naar gelang van toepassing, toewijst en verplaats klanten op basis van de grootte en nodig. Klanten merkbaar de gevolgen van de elastische pools bijna onmiddellijk; voordat elastische pools klanten-outs en andere problemen waren tijdens perioden burst gebruik, maar met elastische pools klanten activiteit bursts naar behoefte kunnen krijgen en kunnen ze doorgaan met het SpectrumNG gebruiken zonder problemen.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure actieve geo-replicatie versnelt rapportage
Klanten met verschillende CSI Software zijn ook profiteren van Azure actieve geo-replicatie. Met actieve geo-replicatie, kunnen maximaal vier leesbare secundaire databases worden geconfigureerd in de dezelfde of verschillende datacenterregio's. CSI Software maakt gebruik van actieve geo-replicatie op twee manieren: eerst de secundaire databases zijn beschikbaar in het geval van storing in een datacenter of het onvermogen om verbinding maken met de primaire database. en vervolgens de secundaire databases kunnen worden gelezen en kunnen worden gebruikt voor het offloaden van alleen-lezen werkbelastingen, zoals rapportage van taken. Sommige klanten CSI Software gebruikmaken van deze benefit reporting werkstromen te versnellen.

## <a name="csi-software-application-logic-and-architecture"></a>De toepassingslogica CSI Software en -architectuur
SpectrumNG maakt gebruik van web-rollen. Omdat de toepassing multitenant is, wordt een WCF-service wordt gebruikt voor het verwerken van de eerste verbinding aanvragen van klanten. Als Molina, raken identificeert' het verzoek elke klant, die vervolgens kan we bouwen van een verbindingsreeks uit voor de databases te doen wat we moeten doen."

Voor de weblaag van de service maakt CSI Software gebruik van Azure automatisch schalen op basis van de dag en tijd. Beschikbare resources worden automatisch verhoogd zodat hoger gebruik tijdens de kantooruren, volgens de tijdzone van elke regionale datacenter. Resources zijn ook ingesteld op omlaag schalen in het weekend, wanneer de klant moet lager zijn.

![Daxko/CSI-architectuur](./media/sql-database-implementation-daxko/figure1.png)

Afbeelding 1. Een cloud services-werkrol tekent gestructureerde gegevens van Azure SQL Database en semi-gestructureerde gegevens uit de tabelopslag. SpectrumNG gebruikers communiceren met dat gegevens via een cloud services-Webrol.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Met behulp van web-apps en een laag web-plan voor mobiele apps
Nieuwe initiatieven, met inbegrip van een volledige mobiel platform inschakelen met behulp van Azure SQL Database-resources voor CSI Software vrijgemaakt op basis van een aangepaste API gebruiken die worden gehost in Azure-web-apps. Het platform kunt maakt leden en medewerkers om mobiele apparaten gebruiken voor het controleren van schema's, klassen boek en ontvangen van berichten.

Het platform service oriented architecture (SOA) gebruikt voor het nemen van één onderdeel, zoals een verkooppunten (POS) of een verkoop-systeem: onderweg verplaatsen naar een andere web-planning en vervolgens draaien van een service voor ondersteuning van dit onderdeel, terwijl alle andere objecten op de oorspronkelijke web planning. Deze mogelijkheid biedt grote flexibiliteit CSI Software en het helpt kostenbeperkend.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure kunt CSI Software ontwikkelaars zich richten op de apps en services
Azure SQL-Database niet alleen een weblog SpectrumNG-klanten die gebruikmaken van de snelle, betrouwbare service, maar ook een groot voordeel voor CSI Software van IT-personeel en ontwikkelaars. Door het offloaden van ops naar Azure in de cloud CSI Software beperkt de overhead voor resources en infrastructuur, aanzienlijk versnelde eigen ontwikkelingscycli en niet langer moet op micromanage-databases op de prestaties optimaliseren voor de tenants.

## <a name="more-information"></a>Meer informatie
* Zie voor meer informatie over Azure elastische pools, [elastische pools](sql-database-elastic-pool.md).
* Zie voor meer informatie over database-hulpprogramma's en elastisch schalen, [hulpmiddelen voor elastische databases en elastisch schalen](sql-database-elastic-scale-get-started.md).
* Zie voor meer informatie over het migreren van een SQL Server-database, Zie [een SQL Server-database migreren naar Azure](sql-database-cloud-migrate.md).
* Zie voor meer informatie over actieve geo-replicatie, [actieve geo-replicatie](sql-database-geo-replication-overview.md).
* Zie voor meer informatie over webrollen en werkrollen, [werkrollen](../fundamentals-introduction-to-azure.md#compute).    
* Zie voor meer informatie over Azure Service Bus, [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* Zie voor meer informatie over automatisch schalen, [cloudservices schalen](../cloud-services/cloud-services-how-to-scale-portal.md).

