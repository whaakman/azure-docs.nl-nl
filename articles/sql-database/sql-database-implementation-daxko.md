---
title: Azure casestudy - Daxko/CSI voor Azure SQL Database | Microsoft Docs
description: Meer informatie over hoe Daxko/CSI SQL-Database gebruikt voor het versnellen van de ontwikkelingscyclus en voor het verbeteren van de klantenservices en prestaties
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: carlrab
ms.openlocfilehash: 10720f42f7a9b10b42ccaaaad81acca369592f6a
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731207"
---
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI gebruikt Azure om de ontwikkeling versnellen en voor het verbeteren van de klantenservices en prestaties
![Daxko/CSI-Logo](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI-Software geconfronteerd met een uitdaging: het klantenbestand van geschiktheid en recreatie centers is groeit snel, dankzij het succes van de oplossing uitgebreide enterprise-software, maar houden met de behoeften van de IT-infrastructuur voor die klant groeiende basis is van het bedrijf testen IT-personeel. Het bedrijf is steeds beperkt door stijgende overhead van bewerkingen, met name voor het beheren van de groeiende databases. De overhead van deze bewerkingen is nog erger, knippen in resources voor het ontwikkelen van nieuwe initiatieven, zoals nieuwe functies van de mobility-software van het bedrijf.

Op basis van David Molina, directeur van productontwikkeling bij Daxko/CSI, die Azure CSI-Software met het platform-as-a-service (PaaS)-model dat nodig is voor het vereenvoudigen van de database, meer schaalbaarheid en resources zich kunt richten op de software vrij te maken in plaats van ops. "Azure SQL Database is een goede optie voor ons. Omdat u niet hoeft te hoeven maken over het onderhouden van een SQL-Server, een failover-cluster en alle de andere infrastructuur moet is ideaal voor ons."

Nadat u naar Azure migreert, moet CSI-Software een operationele medewerkers van slechts twee voor het beheren van meer dan 600 databases van klanten. Het bedrijf maakt gebruik van Azure SQL Database elastische pools verplaatsen van databases van klanten op basis van grootte en nodig hebt.

Molina blijft, 'onze klanten voelde de wijziging onmiddellijk. Voordat u Elastische pools hadden ze af en toe time-outs en andere problemen tijdens perioden van burst. Met Azure elastische pools, kunnen ze burst naar behoefte en gebruik van de software zonder problemen."

Naast het verbeteren van de prestaties voor klanten, Azure elastische pools vrijgemaakt CSI-Software-resources om zich te richten op het ontwikkelen van nieuwe services en functies, in plaats van omgaan met bewerkingen en beheer. De IT-resources heeft geholpen CSI-Software verbeteren van de software enterprise-aanbieding, SpectrumNG, om te oefenen sportschool leden, personeel-efficiëntie te verbeteren en personeel en leden mobiele toegang bieden voor interactieve taken en realtime meldingen.

Azure heeft ook geholpen CSI-Software te versnellen en verbeteren van de ontwikkeling en kwaliteitscontrole (QA) cyclus door in te schakelen van de opties voor automation. Met Azure implementatie van het bedrijf, kunnen u een build-managers pakket onderdelen met één klik op een knop. Zoals Molina wordt beschreven, 'als onderdeel van de release-cyclus, QA kan nu implementeren in een testomgeving in Azure, dit komt sterk onze stack productie overeen. We kunnen builds onmiddellijk implementeren naar onze dev-omgeving om te screenen wijzigingen. "Dat is een groot voordeel voor ons, omdat we niet pariteit voor het testen voordat u die hebt."

## <a name="offloading-to-the-cloud"></a>Offloading naar de cloud
Voordat u doorgaat naar de cloud, CSI-Software had is gebouwd op basis van de eigen multitenant-infrastructuur in een lokaal datacenter in Houston. Het bedrijf uitgevouwen, geconfronteerd met het toenemende groeiende problemen van inkoop, inrichten en onderhouden van alle van de hardware en software die nodig is voor klanten. IT is een andere knelpunt dat heeft geleid tot een vertraging in de inrichting van nieuwe resources en nieuwe services uitrollen aan klanten personeel voor het afhandelen van bewerkingen geworden.

CSI-Software hebt bekeken in de cloudopties voor het elimineren die overhead, zodat deze op de code, in plaats van bewerkingen richten kan. Het bedrijf gedetecteerd dat veel van de belangrijkste cloudproviders alleen infrastructure-as-a-service (IaaS)-oplossingen waarvoor nog steeds een grote IT-afdeling bieden voor het beheren van de IaaS-stack. De uiteindelijk bepaald CSI-Software of de Azure PaaS-oplossing het meest geschikt is voor de behoeften is. Molina wordt uitgelegd, 'Azure opgehaald uit de weg, de hardware en software, zodat we ons kunnen richten op ons aanbod software tijdens onze IT-overhead verminderen.'

## <a name="making-the-transition-to-azure"></a>Maken van de overgang naar Azure
Na het selecteren van Azure voor de PaaS-oplossing, begon CSI-Software met de back endinfrastructuur en -databases migreren naar de cloud. Voorafgaand aan Azure, SpectrumNG klanten die nodig zijn voor het installeren van een clienttoepassing die gecommuniceerd met een Windows Communication Foundation (WCF)-service op de back-end. Op basis van Molina, "Hoewel sommige klanten alles in hun eigen datacenters gehost, we gemaakt uit het product worden meerdere tenants. We gehost alles in een datacenter in Houston, met behulp van SQL Server als het gegevensarchief.

"Ons product ook aanbieden opgenomen een lid gerichte webportal (geschreven in ASP.net), die is ontworpen om te worden wit etiket afstemmen op de aanwezigheid van de klant en een SOAP-API om de online-pagina's en een integratie van derden te ondersteunen."

De migratie naar de cloud is niet lang duren voor de architectuur. Op basis van Molina, "Het merendeel van de inspanningen behandeld bij het wijzigen van de manier waarop dat we meer informatie over de config-bestand, een wijziging gecentraliseerde connection-string en automatiseren van de verpakking, uploaden en implementeren van onze releases."

Voor het ontwikkelen van de build-automatisering, gebruikt CSI-Software engineers Azure PowerShell en REST-API's pakketten maken en te uploaden naar een faseringsomgeving voor elke nacht release.
De algehele overgang naar een Azure-cloud-gebaseerde implementatie is snel en goed voor het CSI-Software IT-team. Molina wordt uitgelegd, "In alle, hebben we een bèta-omgeving in de cloud binnen drie tot vier weken op het project te nemen. Dat was een verrassende win voor ons."

Na het configureren en testen van de omgeving, CSI-Software is begonnen migreren klanten. Voor klanten die al gebruikmaken CSI-Software die als host fungeert, is de overgang nagenoeg naadloze. Voor klanten die migreren van een on-premises implementatie, de migratie naar de cloud heeft wat extra tijd, maar is nog steeds voornamelijk pijnloze voor zowel klanten als CSI-Software.

Voor nieuwe klanten, CSI-Software van IT-afdeling gebruiken het volgende proces toevoegen naar Azure:

1. Azure PowerShell-scripts worden gebruikt voor het instellen van een nieuwe database voor de klant. alle klanten beginnen in een premium-laag om ervoor te zorgen voldoende initiële doorvoer voor de overgang.
2. Indien mogelijk, CSI-Software maakt gebruik van de Azure SQL-migratiewizard bestaande om gegevens te verplaatsen naar een Azure SQL Database-exemplaar.
3. Ten slotte, Microsoft SQL Server Integration Services (SSIS) worden gebruikt om eventuele verschillen in de gegevens in overeenstemming brengen of om uit te voeren opschoontaken gegevens zoals vereist.

Vandaag de dag worden ongeveer 99 procent van CSI-Software-klanten gehost in Azure, in vier regionale datacenters (Noord-centraal, Zuid-centraal, Oost en West). Door datacenters in geografische regio van elke klant, is de latentie tot een minimum beperkt.

## <a name="azure-elastic-pools-free-up-it-resources"></a>Azure elastische pools vrij te maken de IT-bronnen
Verschillende functies van Azure hebben bijgedragen CSI-Software shift wordt de infrastructuur en bewerkingen die zijn gericht op de functie en ontwikkeling die zijn gericht. Misschien is het grootste voordeel van elastische pools.

CSI-Software biedt momenteel ongeveer 550 databases voor klanten. Voordat de elastische pools was het moeilijk te beheren die veel databases binnen een laag-structuur. OPS-managers moest het toewijzen van service-lagen en compute-grootten op basis van de burst-behoeften van klanten, die belangrijke IT-resource overhead vereist. Met elastische pools kunnen managers tenants een premium of standard-pool, waar nodig, toewijzen en verplaats klanten op basis van grootte en nodig hebt. Klanten voelde de gevolgen van de elastische pools bijna onmiddellijk. voor elastische pools klanten time-outs en andere problemen waren tijdens perioden van burst gebruik, maar met elastische pools klanten activiteitsbursts indien nodig kunnen ondervinden en ze kunnen echter ook doorgaan met SpectrumNG zonder problemen.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure active geo-replicatie versnelt melden
Klanten met verschillende CSI-Software ook profiteren van Azure active geo-replicatie. Met actieve geo-replicatie, kunnen tot maximaal vier leesbare secundaire databases worden geconfigureerd in het datacenter in dezelfde of verschillende regio's. CSI-Software maakt gebruik van actieve geo-replicatie op twee manieren: eerst de secundaire databases zijn beschikbaar in het geval van storing in een datacenter of de onmogelijkheid verbinding maken met de primaire database. en ten tweede secundaire databases kunnen worden gelezen en kunnen worden gebruikt voor de offload van alleen-lezen-werkbelastingen, zoals rapportage van taken. Sommige klanten CSI-Software gebruiken dit voordeel te versnellen reporting werkstromen.

## <a name="csi-software-application-logic-and-architecture"></a>De toepassingslogica CSI-Software en -architectuur
SpectrumNG maakt gebruik van web-rollen. Omdat de toepassing met meerdere tenants is, wordt een WCF-service wordt gebruikt voor het afhandelen van de eerste verbindingsaanvraag van klanten. Als Molina staat, identificeert' de aanvraag elke klant, die vervolgens kan we bouwen een verbindingsreeks om met hun databases te doen wat we moeten doen."

Voor de weblaag van de service maakt CSI-Software gebruik van Azure automatisch schalen, op basis van de dag en tijd. Beschikbare resources automatisch worden verhoogd om te voldoen aan hoger gebruik tijdens kantooruren, op basis van de tijdzone van elke regionaal datacenter. Resources zijn ook ingesteld op het omlaag schalen in het weekend, wanneer de behoeften van klanten lager zijn.

![Daxko/CSI-architectuur](./media/sql-database-implementation-daxko/figure1.png)

Afbeelding 1. Een cloud services-werkrol tekent gestructureerde gegevens van Azure SQL Database en semi-gestructureerde gegevens van de table-opslag. SpectrumNG gebruikers werken interactief met dat gegevens via een cloud services-Webrol.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Met behulp van web-apps en een web-plan-laag voor mobiele apps
Met behulp van Azure SQL Database-resources voor CSI-Software vrijgemaakt om in te schakelen nieuwe initiatieven, met inbegrip van een volledig mobiel platform op basis van een aangepaste API die wordt gehost in Azure web-apps. Met het platform kunnen sportschool leden en het personeel op mobiele apparaten gebruiken om te controleren van schema's, boek klassen en ontvangen van berichten.

Het platform wordt gebruikgemaakt van service oriented architecture (SOA) voordat u een één onderdeel, zoals een verkooppunt systeem (POS) of een verkoopsysteem, op elk gewenst moment verplaatsen naar een andere webplan en stel vervolgens een service voor de ondersteuning van dat onderdeel, terwijl alle andere op de oorspronkelijke webplan. Deze mogelijkheid biedt enorme flexibiliteit CSI-Software en het helpt Houd de kosten laag.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Met Azure kunt CSI-Software ontwikkelaars zich richten op apps en services
Azure SQL Database is niet alleen een boon voor SpectrumNG klanten die profiteren van de service snelle, betrouwbare, het is ook een groot voordeel voor CSI-Software van IT-personeel en ontwikkelaars. Door het offloaden van ops naar Azure in de cloud, CSI-Software beperkt de overhead voor resources en -infrastructuur, aanzienlijk versneld en kan daarom de ontwikkelingscycli en niet meer op behoeften op micromanage-databases op de prestaties optimaliseren voor de tenants.

## <a name="more-information"></a>Meer informatie
* Zie voor meer informatie over Azure elastische pools, [elastische pools](sql-database-elastic-pool.md).
* Zie voor meer informatie over hulpprogramma's voor databases en elastisch schalen, [hulpmiddelen voor elastic database en elastisch schalen](sql-database-elastic-scale-get-started.md).
* Zie voor meer informatie over het migreren van een SQL Server-database, [een SQL Server-database migreren naar Azure](sql-database-cloud-migrate.md).
* Zie voor meer informatie over actieve geo-replicatie, [actieve geo-replicatie](sql-database-geo-replication-overview.md).
* Zie voor meer informatie over Azure Service Bus, [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* Zie voor meer informatie over automatisch schalen, [schalen van cloudservices](../cloud-services/cloud-services-how-to-scale-portal.md).

