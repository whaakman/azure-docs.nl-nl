---
title: Hoge beschikbaarheid - service van Azure SQL Database | Microsoft Docs
description: Meer informatie over de functies en mogelijkheden voor hoge beschikbaarheid van Azure SQL Database-service
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 03/19/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: d26fe28d301cf563dc6bdb3d9e17903dea3e73fc
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="high-availability-and-azure-sql-database"></a>Hoge beschikbaarheid en Azure SQL-Database
Microsoft heeft de belofte aangebracht in de klanten die hoge beschikbaarheid (HA) is ingebouwd in de service en de klanten hoeven niet te werken, speciale logica voor het toevoegen of beslissingen rond HA vanaf het begin van de Azure SQL Database PaaS-aanbieding. Microsoft onderhoudt volledige controle over de HA-systeemconfiguratie en bewerking, biedt klanten een SLA. De HA-SLA van toepassing op een SQL-database in een regio en biedt geen bescherming in geval van een mislukking van de totale regio die wordt veroorzaakt door factoren buiten het beheer van Microsoft redelijkerwijs (bijvoorbeeld een natuurramp, war, besluiten van terrorisme, riots, overheids-actie of een netwerk- of apparaatstoring buiten de datacenters van Microsoft, waaronder op klant sites of tussen sites van de klant en Datacenter van Microsoft).

Microsoft gebruikt om de ruimte van het probleem van HA vereenvoudigen, de volgende veronderstellingen:
1.  Fouten van hardware en software zijn onvermijdelijk
2.  Operationele medewerkers maken fouten die tot mislukte leiden
3.  Geplande servicebewerkingen veroorzaken storingen 

Dergelijke afzonderlijke gebeurtenissen zijn incidentele in de cloud, ze optreden, elke week als dat niet elke dag. 

## <a name="fault-tolerant-sql-databases"></a>Fouttolerantie SQL-databases
Klanten meest geïnteresseerd bent in de tolerantie van hun eigen databases en minder geïnteresseerd bent in de tolerantie van de service SQL Database als geheel. 99,99% beschikbaarheid voor een service is nutteloos als 'mijn database' deel uit van de 0,01% van de databases die zijn niet maakt beschikbaar. Elke database moet zich fouttolerantie en fout risicobeperking moet nooit leiden tot verlies van een transactie die is vastgelegd. 

SQL-Database gebruikt voor gegevens, zowel lokale opslag (LS) op basis van direct gekoppelde schijven VHD en externe opslag (RS) op basis van Azure Premium-opslag-pagina-blobs. 
- Lokale opslag wordt gebruikt in de Premium-databases en groepen die zijn ontworpen voor bedrijfskritieke OLTP toepassingen met een hoge IOPS-vereisten. 
- Externe opslag wordt gebruikt voor Basic en Standard Servicelagen, die zijn bestemd voor budget georiënteerde zakelijke workloads waarvoor opslag is vereist en rekencapaciteit afzonderlijk van elkaar schalen. Ze een één-pagina-blob voor de database en logboekbestanden en de replicatie en failover mechanismen ingebouwde opslag gebruiken.

In beide gevallen worden de replicatie, foutdetectie en failover-mechanismen van SQL-Database zijn volledig geautomatiseerd en werkt zonder menselijke tussenkomst. Deze architectuur is ontworpen om ervoor te zorgen dat doorgevoerd gegevens nooit verloren en wordt dat gegevens duurzaamheid voorrang hebben op alle anders.

De belangrijkste voordelen:
- Klanten u optimaal wilt profiteren van gerepliceerde databases zonder te configureren of onderhouden ingewikkeld hardware, software, besturingssystemen of virtualisatieomgevingen.
- Volledige ACID-eigenschappen van relationele databases worden beheerd door het systeem.
- Failovers zijn volledig geautomatiseerd zonder verlies van de vastgelegde gegevens.
- Routering van verbindingen met de primaire replica wordt dynamisch beheerd door de service met geen toepassingslogica vereist.
- De hoge mate van automatische redundantie wordt geleverd zonder extra kosten.

> [!NOTE]
> De architectuur beschreven hoge beschikbaarheid is onderworpen aan zonder kennisgeving worden gewijzigd. 

## <a name="data-redundancy"></a>De gegevensredundantie

De oplossing met hoge beschikbaarheid in SQL-Database is gebaseerd op [altijd op beschikbaarheidsgroepen](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) -technologie van SQL Server en maakt het werkt voor zowel LS RS databases met minimale verschillen. De technologie altijd op beschikbaarheid van groep wordt gebruikt voor persistentie in LS configuratie, terwijl in RS wordt gebruikt voor beschikbaarheid (laag RTO). 

## <a name="local-storage-configuration"></a>Configuratie van de lokale opslag

In deze configuratie, elke database wordt online gezet door de management-service (MS) in de besturingselement-ring. Eén primaire replica en ten minste twee secundaire replica's (quorum-set) bevinden zich in een tenant ring die drie onafhankelijke fysieke subsystemen binnen hetzelfde datacenter omvat. Lees- en schrijfbewerkingen worden verzonden door de gateway (GW) naar de primaire replica en de geschreven asynchroon worden gerepliceerd naar de secundaire replica's. SQL-Database gebruikt een schema op basis van een quorum doorvoeren waarbij gegevens worden geschreven naar de primaire en ten minste één secundaire replica voordat de transactie-doorvoeracties.

De [Service Fabric](../service-fabric/service-fabric-overview.md) system failover automatisch opnieuw opgebouwd replica's als knooppunten uitvallen en behoudt het lidmaatschap van een quorum-set als knooppunten afwijken en deelnemen aan het systeem. Gepland onderhoud wordt zorgvuldig gecoördineerd om te voorkomen dat het quorum-set gaan omlaag dan een minimale replica telling (meestal 2). Dit model geschikt is voor Premium-databases, maar het vereist dat de redundantie van zowel berekenings- en -onderdelen en resulteert in een hogere kosten.

## <a name="remote-storage-configuration"></a>Configuratie van de externe opslag

Voor externe opslagconfiguraties (Basic en Standard lagen), wordt precies één kopie beheerd in externe blob-opslag, met behulp van de opslagmogelijkheden van systemen voor duurzaamheid en redundantie bits rotting detectie. 

De architectuur van hoge beschikbaarheid wordt door het volgende diagram geïllustreerd:
 
![architectuur voor hoge beschikbaarheid](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection-and-recovery"></a>Foutdetectie en herstel 
Een grootschalige gedistribueerd systeem moet een uiterst betrouwbare fout detectiesysteem die betrouwbaar werken en fouten kunt opsporen snel en zo dicht mogelijk bij de klant. Dit systeem is voor SQL-Database op basis van Azure Service Fabric. 

Met de primaire replica is direct duidelijk of en wanneer de primaire replica is mislukt en werk kan niet worden voortgezet omdat alle lees- en schrijfbewerkingen worden uitgevoerd op de primaire replica eerst. Dit proces van het promoveren van een secundaire replica de status van de primaire heeft beoogde hersteltijd (RTO) = 30 seconden en beoogd herstelpunt (RPO) = 0. Om te beperken van de impact van de 30 seconden RTO, is de beste manier om te proberen opnieuw verbinding maken met meerdere keren met een kleinere wachttijd voor verbindingspogingen mislukt.

Wanneer een secundaire replica is mislukt, wordt de database naar een minimale quorum-set, met geen spares is. Het service fabric start het proces herconfiguratie vergelijkbaar met het proces dat volgt op de primaire replica is mislukt, dus na een korte wachten om te bepalen of de fout permanent, een andere secundaire replica is gemaakt. In geval van een tijdelijke status van de out-of-service, zoals een besturingssysteemfout of een upgrade is niet onmiddellijk een nieuwe replica gebouwd zodat het knooppunt opnieuw op te starten in plaats daarvan. 

Voor de configuraties externe opslag gebruikt SQL-Database Always ON functionaliteit voor databases failover tijdens upgrades. Daarvoor een nieuwe SQL-exemplaar is geactiveerd uitschakelen van tevoren als onderdeel van de geplande upgrade gebeurtenis en koppelt en herstelt u het databasebestand uit externe opslag. In geval van een proces crashes of andere niet-geplande gebeurtenissen Windows Fabric beheert de beschikbaarheid van het exemplaar en gekoppeld als een laatste stap van het herstel van het bestand van de externe database.

## <a name="zone-redundant-configuration-preview"></a>Zone-redundante configuratie (preview)

Standaard worden de quorum-set replica's voor de lokale opslagconfiguraties gemaakt in hetzelfde datacenter. Dankzij de introductie van [Azure beschikbaarheid Zones](../availability-zones/az-overview.md), hebt u de mogelijkheid om de andere replica's in het quorum-stelt aan andere beschikbaarheid zones in dezelfde regio. Om te voorkomen een potentieel risico, is de besturingselement-ring ook gedupliceerd in meerdere zones als drie gateway ringen (GW). De routering naar een specifieke gateway ring wordt beheerd door [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Omdat de configuratie van de zone-redundante geen extra databaseredundantie maakt, het gebruik van beschikbaarheid Zones in de Premium servicecategorie is beschikbaar op zonder extra kosten. Als u een zone redundant database selecteert, kunt u uw Premium-databases robuuste naar een veel grotere set van fouten, met inbegrip van datacenter catastrofale uitval, zonder eventuele wijzigingen van de toepassingslogica. U kunt ook alle bestaande Premium-databases of groep aan de zone-redundante configuratie converteren.

Omdat de redundante zone quorum-set replica's in verschillende datacenters met sommige afstand tussen deze twee heeft, mag de toegenomen netwerklatentie vergroot de toegewezen tijd en dus invloed op de prestaties van sommige OLTP-werkbelastingen. Altijd kunt u terugkeren naar de configuratie met één zone door het uitschakelen van de instelling van de redundantie van de zone. Dit proces heeft een grootte van gegevens en is vergelijkbaar met de reguliere service level objective (SLO)-update. De database of de groep van toepassingen aan het einde van het proces is gemigreerd uit een zone redundant-ring naar een enkele zone ring of vice versa.

> [!IMPORTANT]
> Zone-redundante databases en elastische pools worden alleen ondersteund in de Premium servicecategorie. Tijdens de openbare preview, back-ups en audit records worden opgeslagen in de opslag van de RA-GRS en daarom mogelijk niet automatisch beschikbaar in het geval van een onderbreking van de hele zone. 

De zone redundant versie van de architectuur van hoge beschikbaarheid wordt door het volgende diagram geïllustreerd:
 
![hoge beschikbaarheid architectuur zoneredundant](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="conclusion"></a>Conclusie
Azure SQL Database is nauw geïntegreerd met het Azure-platform en maximaal afhankelijk is van het Service Fabric voor foutdetectie en herstel van Azure Storage-Blobs voor gegevensbescherming en beschikbaarheid Zones voor hogere fouttolerantie. Op hetzelfde moment Azure SQL database volledig maakt gebruik van de AlwaysOn-technologie van SQL Server-product vak voor replicatie en failover. De combinatie van deze technologieën kan de toepassingen volledig de voordelen van een gemengde opslagmodel en ondersteuning van de zwaarste serviceovereenkomsten. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure beschikbaarheid Zones](../availability-zones/az-overview.md)
- Meer informatie over [Fabric-Service](../service-fabric/service-fabric-overview.md)
- Meer informatie over [met Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 
