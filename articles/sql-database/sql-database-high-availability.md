---
title: Hoge beschikbaarheid - service van Azure SQL Database | Microsoft Docs
description: Meer informatie over de functies en mogelijkheden voor hoge beschikbaarheid van Azure SQL Database-service
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
ms.assetid: 
ms.service: sql-database
ms.custom: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 12/13/2017
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: c0a140c959f14c2e8ceaddad5d323f0900be5d2f
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="high-availability-and-azure-sql-database"></a>Hoge beschikbaarheid en Azure SQL-Database
Microsoft heeft de belofte vanaf het begin van de Azure SQL Database PaaS-aanbieding voor onze klanten die hoge beschikbaarheid (HA) is ingebouwd in de service en de klanten zijn niet vereist als u wilt werken, speciale logica voor het toevoegen of beslissingen rond HA gesteld. Microsoft biedt klanten een SLA en onderhouden van de volledige controle over de HA-systeemconfiguratie en bewerking. De HA-SLA van toepassing op een SQL-database in een regio en biedt geen bescherming in geval van een storing van de totale regio die vanwege factoren die buiten de controle van onze redelijke zijn (bijvoorbeeld natuurramp war, fungeert van terrorisme, riots, overheids-actie of een netwerk of apparaat apparaatstoring buiten ons datacenters, inclusief op de klantensite of tussen de klant en onze Datacenter).

Microsoft gebruikt om de ruimte van het probleem van HA vereenvoudigen, de volgende veronderstellingen:
1.  Fouten van hardware en software zijn onvermijdelijk
2.  Operationele medewerkers maken fouten die tot mislukte leiden
3.  Geplande servicebewerkingen veroorzaken storingen 

Wanneer er dergelijke afzonderlijke gebeurtenissen worden incidentele in de cloud, ze elke week als niet elke dag. 

## <a name="fault-tolerant-sql-databases"></a>Fouttolerantie SQL-databases
Klanten meest geïnteresseerd bent in de tolerantie van hun eigen databases en minder geïnteresseerd bent in de tolerantie van de service SQL Database als geheel. 99,99% beschikbaarheid voor een service is nutteloos als 'mijn database' deel uit van de 0,01% van de databases die zijn niet maakt beschikbaar. Elke database moet zich fouttolerantie en fout risicobeperking moet nooit leiden tot verlies van een transactie die is vastgelegd. 

SQL-Database gebruikt voor gegevens, zowel lokale opslag (LS) op basis van direct gekoppelde schijven VHD en externe opslag (RS) op basis van Azure Premium-opslag-pagina-blobs. 
- Lokale opslag wordt gebruikt in de Premium-databases en groepen die zijn ontworpen voor OLTP-toepassingen met hoge IOPS-vereisten. 
- Externe opslag wordt gebruikt voor Basic en Standard Servicelagen, die is ontworpen voor kleine, koude of grote databases die nodig opslagruimte en rekencapaciteit afzonderlijk van elkaar schalen. Ze een één-pagina-blob voor de database en logboekbestanden en de replicatie en failover mechanismen ingebouwde opslag gebruiken.

In beide gevallen worden de replicatie, foutdetectie en failover-mechanismen van SQL-Database zijn volledig geautomatiseerd en werkt zonder menselijke tussenkomst. Deze architectuur is ontworpen om ervoor te zorgen dat doorgevoerd gegevens nooit verloren en wordt dat gegevens duurzaamheid voorrang hebben op alle anders.

De belangrijkste voordelen:
- Klanten u optimaal wilt profiteren van gerepliceerde databases zonder te configureren of onderhouden ingewikkeld hardware, software, besturingssysteem of virtualisatieomgevingen.
- Volledige ACID-eigenschappen van relationele databases worden beheerd door het systeem.
- Failovers zijn volledig geautomatiseerd zonder verlies van de vastgelegde gegevens.
- Routering van verbindingen met de primaire replica wordt dynamisch beheerd door de service met geen toepassingslogica vereist.
- De hoge mate van automatische redundantie wordt geleverd zonder extra kosten.

> [!NOTE]
> De architectuur beschreven hoge beschikbaarheid is onderworpen aan zonder kennisgeving worden gewijzigd. 

## <a name="data-redundancy"></a>De gegevensredundantie

De oplossing met hoge beschikbaarheid in SQL-Database is gebaseerd op [AlwaysON](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) -technologie van SQL Server en maakt het werkt voor zowel LS RS databases met minimale verschillen. In LS configuratie AlwaysON wordt gebruikt voor persistentie in RS wordt deze gebruikt voor beschikbaarheid (laag RTO). 

## <a name="local-storage"></a>Lokale opslag

In het geval LS elke database wordt online gezet door de management-service (MS) in de besturingselement-ring. Eén primaire replica en ten minste twee secundaire replica's (quorum-set) bevinden zich in een tenant ring die drie onafhankelijke fysieke subsystemen binnen hetzelfde datacenter omvat. Lees- en schrijfbewerkingen worden verzonden door de gateway (GW) naar de primaire replica en de geschreven asynchroon worden gerepliceerd naar de secundaire replica's. SQL-Database gebruikt een schema op basis van een quorum doorvoeren waarbij gegevens worden geschreven naar de primaire en ten minste één secundaire replica voordat de transactie-doorvoeracties.

De [Service Fabric](/azure/service-fabric/service-fabric-overview.md) system failover automatisch opnieuw opgebouwd replica's als knooppunten uitvallen en behoudt het lidmaatschap van een quorum-set als knooppunten afwijken en deelnemen aan het systeem. Gepland onderhoud wordt zorgvuldig gecoördineerd om te voorkomen dat het quorum-set gaan omlaag dan een minimale replica telling (meestal 2). Dit model geschikt is voor Premium-databases, maar het vereist dat de redundantie van zowel berekenings- en -onderdelen en resulteert in een hogere kosten.

## <a name="remote-storage"></a>Externe opslag

Voor externe opslagconfiguraties (Basic en Standard lagen), precies één exemplaar van de database behouden in externe blob-opslag, gebruik van de opslagmogelijkheden van systemen voor duurzaamheid en redundantie bits rotting detectie. 

De architectuur van hoge beschikbaarheid wordt door het volgende diagram geïllustreerd:
 
![architectuur voor hoge beschikbaarheid](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection--recovery"></a>Foutdetectie en herstel 
Een grootschalige gedistribueerd systeem moet een uiterst betrouwbare fout detectiesysteem die betrouwbaar werken en fouten kunt opsporen snel en zo dicht mogelijk bij de klant. Dit systeem is voor SQL-Database op basis van Azure Service Fabric. 

Met de primaire replica is direct duidelijk of en wanneer de primaire replica is mislukt en werk kan niet worden voortgezet omdat alle lees- en schrijfbewerkingen worden uitgevoerd op de primaire replica eerst. Dit proces van het promoveren van een secundaire replica de status van de primaire heeft beoogde hersteltijd (RTO) = 30 seconden en beoogd herstelpunt (RPO) = 0. Om te beperken van de impact van de 30 seconden RTO, is de beste manier om te proberen opnieuw verbinding maken met meerdere keren met een kleinere wachttijd voor verbindingspogingen mislukt.

Wanneer een secundaire replica is mislukt, wordt de database naar een minimale quorum-set, met geen spares is. Het service fabric start het proces herconfiguratie vergelijkbaar met het proces dat volgt op de primaire replica is mislukt, dus na een korte wachten om te bepalen of de fout permanent, een andere secundaire replica is gemaakt. In geval van een tijdelijke status van de out-of-service, zoals een besturingssysteemfout of een upgrade is niet onmiddellijk een nieuwe replica gebouwd zodat het knooppunt opnieuw op te starten in plaats daarvan. 

SQL-Database voor de configuraties externe opslag wordt AlwaysON-functionaliteit met failover-databases tijdens upgrades gebruikt. Daarvoor een nieuwe SQL-exemplaar is geactiveerd uitschakelen van tevoren als onderdeel van de geplande upgrade gebeurtenis en koppelt en herstelt u het databasebestand uit externe opslag. In geval van een proces crashes of andere niet-geplande gebeurtenissen Windows Fabric beheert de beschikbaarheid van het exemplaar en gekoppeld als een laatste stap van het herstel van het bestand van de externe database.

## <a name="conclusion"></a>Conclusie
Azure SQL database is nauw geïntegreerd met het Azure-platform en sterk afhankelijk is van Service Fabric voor foutdetectie en herstel en op Azure Storage-Blobs voor gegevensbescherming. Op hetzelfde moment gebruikt Azure SQL-database de AlwaysOn-technologie van SQL Server-product voor het selectievakje voor replicatie en failover. De combinatie van deze technologieën kan de toepassingen volledig de voordelen van een gemengde opslagmodel en ondersteuning van de zwaarste serviceovereenkomsten. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Fabric-Service](/azure/service-fabric/service-fabric-overview.md)
- Meer informatie over [met Azure Traffic Manager](/traffic-manager/traffic-manager-overview.md) 
