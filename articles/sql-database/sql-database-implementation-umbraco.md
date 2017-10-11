---
title: Azure SQL Database Azure casestudy - Umbraco | Microsoft Docs
description: Informatie over hoe Umbraco SQL-Database gebruikt voor het snel inrichten en services van de schaal van de duizenden tenants in de cloud
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5243d31e-3241-4cb0-9470-ad488ff28572
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 6e9c56874bf4bda7f4248a44e274532ed2555153
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco maakt gebruik van Azure SQL Database met snel inrichten en de schaal services voor duizenden tenants in de cloud
![Umbraco-Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco is een populaire open-source inhoudsbeheer systeem (CMS) die kan worden uitgevoerd van kleine campagne of brochure sites voor complexe toepassingen voor Fortune 500-bedrijven en globale media websites. 

> "We hebben wel een grote community van ontwikkelaars die gebruikmaken van het systeem met meer dan 100.000 ontwikkelaars op onze forums en meer dan 350.000 sites die zijn live, Umbraco uitgevoerd."
> 
> — Morten Christensen, technische Lead Umbraco
> 
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-Umbraco/player]
> 
> 

Om te vereenvoudigen implementaties van klanten, Umbraco Umbraco-as-a-Service (UaaS) toegevoegd: een software-as-a-service (SaaS)-oplossing die niet meer nodig on-premises implementaties biedt ingebouwde schalen en verwijdert u de management overhead doordat ontwikkelaars om zich te richten op het gebied van product innovatie in plaats van een oplossing. Umbraco kan bieden alle voordelen die door het flexibele platform as a service (PaaS)-model aangeboden door Microsoft Azure.

UaaS kunnen klanten SaaS Umbraco CMS-mogelijkheden die eerder buiten hun bereik zijn gebruiken. Deze klanten zijn ingericht met een werkende CMS-omgeving die een productiedatabase. Klanten kunnen maximaal twee extra databases voor ontwikkeling en testomgevingen, afhankelijk van de vereisten toevoegen. Wanneer een nieuwe omgeving wordt aangevraagd, een geautomatiseerd proces toegewezen die klant een database automatisch. De nieuwe database kan in seconden, omdat de database al vooraf zijn ingericht met Umbraco uit een Azure elastische groep met beschikbare databases (Zie afbeelding 1).

![Inrichting lifecycle Umbraco](./media/sql-database-implementation-umbraco/figure1.png)

Afbeelding 1. Inrichting van de levenscyclus voor Umbraco als een Service (UaaS)

## <a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure elastische pools en automatisering vereenvoudigen implementaties
Met Azure SQL Database en andere Azure-services, Umbraco klanten kunnen zelf hun omgeving inrichten en Umbraco eenvoudig kunt controleren en beheren van databases als onderdeel van een intuïtieve werkstroom:

1. Inrichten
   
   Umbraco onderhoudt een capaciteit van 200 beschikbare vooraf is ingericht databases van elastische pools. Wanneer een nieuwe klant zich registreert voor UaaS, biedt Umbraco de klant met een nieuwe CMS-omgeving in bijna realtime eraan toe te wijzen een database van de beschikbaarheid van toepassingen.
   
   Wanneer een beschikbaarheid van toepassingen de drempel bereikt, wordt een nieuwe elastische pool wordt gemaakt en nieuwe databases zijn vooraf is ingericht moet worden toegewezen aan klanten, indien nodig.
   
   Implementatie is volledig geautomatiseerd met C# management-bibliotheken en Azure Service Bus-wachtrijen.
2. Gebruikmaken van
   
   Klanten één tot drie-omgevingen (productie, fasering en/of ontwikkeling), elk met een eigen database gebruiken. Klantendatabases zijn in elastische pools, waardoor Umbraco om efficiënt schalen zonder dat te veel inrichten.
   
   ![Overzicht van het project Umbraco](./media/sql-database-implementation-umbraco/figure2.png)
   
   ![Details van de projecten Umbraco](./media/sql-database-implementation-umbraco/figure3.png)
   
   Afbeelding 2. Umbraco-as-a-Service (UaaS) klant website met overzicht van project en details
   
   Azure SQL Database gebruikt's (Database Transaction Units) om de relatieve kracht vereist is voor databasetransacties echte vertegenwoordigen. Voor klanten UaaS, databases doorgaans op ongeveer 10 dtu's werken, maar elk heeft de elasticiteit voor schalen op aanvraag. Dit betekent dat UaaS kunt ervoor zorgen dat gebruikers altijd benodigde resources zelfs tijdens piektijden hebben. Bijvoorbeeld: tijdens een recente zondag Sport-gebeurtenis wordt één UaaS ervaren klantendatabase waar de pieken maximaal 100 dtu's voor de duur van het spel. Azure elastische pools gesteld Umbraco ter ondersteuning van die grote vraag zonder verminderde prestaties.
3. Bewaken
   
   Umbraco monitors database activiteit met dashboards binnen de Azure-portal, samen met aangepaste e-mailwaarschuwingen.
4. Herstel na noodgevallen
   
   Azure biedt twee opties voor noodherstel (DR): actieve geo-replicatie en geo-herstel. De optie voor DR die een bedrijf moet selecteren is afhankelijk van de [zakelijke continuïteit doelstellingen](sql-database-business-continuity.md).
   
   actieve geo-replicatie biedt de snelste mate van antwoord in het geval van downtime. Gebruik van actieve geo-replicatie, kunt u maximaal vier secundaire databases op servers in verschillende regio's en u kunt vervolgens failover naar een van de secundaire replica's starten als er een storing optreedt.
   
   Umbraco geen geo-replicatie vereist, maar deze te profiteren van Azure geo-restore om te zorgen voor minimale downtime in het geval van een storing. geo-restore afhankelijk van de databaseback-ups in geografisch redundante Azure-opslag. Waarmee gebruikers van een back-up herstellen als er een storing in de primaire regio.
5. Deactivering inrichten
   
   Wanneer een project-omgeving wordt verwijderd, worden alle bijbehorende databases (ontwikkeling, staging of live) tijdens het opschonen van Azure Service Bus-wachtrij verwijderd. Deze geautomatiseerde proces worden teruggezet in de niet-gebruikte databases Umbraco van elastische database-beschikbaarheid van toepassingen, zodat ze beschikbaar zijn voor het inrichten van toekomstige behoud maximaal worden gebruikt.

## <a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Elastische pools toestaan UaaS schalen met gemak
Door gebruik te maken van Azure elastische pools, kunt Umbraco optimaliseren voor klanten zonder te boven of onder inrichten. Umbraco heeft momenteel bijna 3.000 databases tussen 19 elastische pools, met de mogelijkheid om eenvoudig schalen naar behoefte aan een van hun bestaande 325,000 klanten of nieuwe klanten die gereed zijn voor implementatie van een CMS in de cloud.

In feite volgens Morten Christensen, technische leiden op Umbraco, 'UaaS is nu met de groei van ongeveer 30 nieuwe klanten per dag. Onze klanten plezier met het gemak van het inrichten van nieuwe projecten in seconden, updates onmiddellijk naar hun live sites publiceren van een ontwikkelomgeving met één klik-implementatie kunnen zijn, en wijzig net zo snel als ze fouten vinden.'

Als een klant een tweede en/of derde omgeving niet meer nodig, het hoeft te verwijderen die omgevingen. Die bronnen die kunnen worden gebruikt voor andere klanten als onderdeel van de groep van de elastische database beschikbaarheid Umbraco vrijgemaakt.

![Architectuur voor Umbraco-implementatie](./media/sql-database-implementation-umbraco/figure4.png)

Afbeelding 3. Architectuur van UaaS-implementatie in Microsoft Azure

## <a name="the-path-from-datacenter-to-cloud"></a>Het pad van het datacenter naar cloud
Wanneer de ontwikkelaars Umbraco in eerste instantie de beslissing genomen om te verplaatsen naar een SaaS-model, kent ze dat een rendabele en schaalbare manier moeten zou voor het bouwen van de service.

> 'elastische pools zijn een bij uitstek geschikt voor onze SaaS-aanbieding omdat we capaciteit omhoog en omlaag naar behoefte kunt kiezen. Inrichting is eenvoudig en met onze setup we gebruik kunt houden met een maximum."
> 
> — Morten Christensen, technische Lead Umbraco
> 
> 

'Wilden we onze tijd besteden aan het oplossen van problemen van onze klanten, niet infrastructuurbeheer. We wilden gemakkelijk voor onze klanten om op te halen van de meeste waarde' zegt Niels Hartvig, oprichting van Umbraco. "We in eerste instantie die als host fungeert voor de servers onszelf overwogen, maar capaciteitsplanning had een nachtmerrie." Tegelijk komt Umbraco niet gebruikmaken van elke databasebeheerders die een toegevoegde waarde van sleutel voor het gebruik van UaaS onderstrepingstekens.

Een belangrijk doel voor de ontwikkelaars Umbraco is op een manier voor klanten om in te richten omgevingen snel en zonder capaciteitsbeperkingen UaaS. Maar bieden een toegewijde gehoste service in Umbraco-datacenters moest worden veel van de overtollige capaciteit voor het afhandelen van bursts bij de verwerking. Die zou hebben bedoeld aanzienlijk beheerinfrastructuur die zou hebben is regelmatig onderbenutte toe te voegen.

Het ontwikkelteam Umbraco wilde bovendien een oplossing waarmee ze zo veel mogelijk van hun bestaande code mogelijk hergebruiken. Als ontwikkelaar Umbraco statussen Mikkel Madsen, 'wij zijn blij met de Microsoft-ontwikkelprogramma's die al bekend bent met, zoals Microsoft SQL Server, Microsoft Azure SQL Database, ASP.net en Internet informatieservices (IIS zijn). Cloud voordat investeren in een IaaS- of een PaaS-oplossing, we wilden om ervoor te zorgen dat deze zou ondersteuning voor onze Microsoft-programma's en platformen, zodat we grote wijzigingen aanbrengen in onze code base geen hebt. "

Om te voldoen aan de criteria, Umbraco opgezocht voor een cloud partner met de volgende situaties:

* Voldoende capaciteit en betrouwbaarheid
* Ondersteuning voor Microsoft ontwikkelingsprogramma's, zodat Umbraco engineers zou niet worden afgedwongen volledig doelen hun ontwikkelomgeving
* Aanwezigheid in alle van de geografische markten waarin UaaS (bedrijven nodig hebben concurreert om ervoor te zorgen dat toegang te krijgen hun gegevens snel tot en dat er gegevens worden opgeslagen op een locatie die voldoet aan de regionale wettelijke vereisten)

## <a name="why-umbraco-chose-azure-for-uaas"></a>Waarom Azure in Umbraco hebt gekozen voor UaaS
Op basis van Morten Christensen 'met inbegrip van alle opties voor onze we geselecteerd Azure omdat deze is voldaan aan alle onze criteria, beheerbaarheid en schaalbaarheid voor bekend zijn en kosteneffectiviteit. We de omgevingen op Azure Virtual machines instellen en elke omgeving heeft een eigen Azure SQL Database-exemplaar met de exemplaren in elastische pools. Door het scheiden van databases tussen ontwikkeling, fasering en productieomgevingen, bieden we onze klanten isolatie van de krachtige prestaties met de schaal overeenkomen: een enorme win. "

Morten blijft, 'voordat we servers voor Webdatabases handmatig inrichten moesten. Er zijn momenteel geen nu denken. Alles wordt automatisch uitgevoerd, vanaf de inrichting worden opgeschoond. "

Er is ook Morten tevreden met de schaal mogelijkheden van Azure. 'elastische pools zijn een bij uitstek geschikt voor onze SaaS-aanbieding omdat we capaciteit omhoog en omlaag naar behoefte kunt kiezen. Inrichting is eenvoudig en met onze setup we gebruik kunt houden met een maximum." Morten statussen, 'de eenvoud van elastische pools, samen met de zekerheid van de service tier-gebaseerde dtu's kunt ons voor het inrichten van nieuwe resourcegroepen op aanvraag. Een van onze klanten met grotere een onlangs piek aan 100 dtu's in de productieomgeving. Met Azure onze elastische pools opgegeven voor de databases van de klant met de resources die ze nodig in realtime zijn zonder te voorspellen DTU vereisten. Kortom, onze klanten de tijd Schakel rond die ze verwachten ophalen en we kunnen voldoen aan de serviceovereenkomsten prestaties."

Mikkel Madsen dit opgeteld: 'We helemaal bent gewonnen de krachtige Azure algoritme die een gemeenschappelijk SaaS-scenario (nieuwe klanten voor onboarding in realtime op grote schaal) verbinding maakt met het patroon van onze toepassing (vooraf inrichten databases, beide ontwikkeling en live) boven op de onderliggende -technologie (met behulp van Azure Service Bus-wachtrijen in combinatie met Azure SQL Database).'

## <a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Met Azure overschrijdt UaaS verwachtingen van klanten
Omdat Azure als de partner van de cloud kiezen, is Umbraco kunnen UaaS klanten geoptimaliseerde inhoud management prestaties bieden zonder de vereiste van een host zichzelf oplossing investering voor IT-resource. Zoals Morten zegt: 'We het gemak van ontwikkelaars en schaalbaarheid waarmee Azure ons favoriete en onze klanten zijn geweldig de functies en betrouwbaarheid. Over het algemeen is een uitstekende win voor ons!'

## <a name="more-information"></a>Meer informatie
* Zie voor meer informatie over Azure elastische pools, [elastische pools](sql-database-elastic-pool.md).
* Zie voor meer informatie over Azure Service Bus, [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* Zie voor meer informatie over webrollen en werkrollen, [werkrollen](../fundamentals-introduction-to-azure.md#compute).    
* Zie voor meer informatie over virtuele netwerken, [virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/).    
* Zie voor meer informatie over back-up en herstel, [bedrijfscontinuïteit](sql-database-business-continuity.md).    
* Zie voor meer informatie over het bewaken van ppols, [bewaking van groepen](sql-database-elastic-pool-manage-portal.md).    
* Zie voor meer informatie over Umbraco als een Service, [Umbraco](https://umbraco.com/cloud).

