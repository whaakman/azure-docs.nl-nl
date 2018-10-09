---
title: Hoge beschikbaarheid - Azure SQL Database-service | Microsoft Docs
description: Meer informatie over de functies en mogelijkheden voor hoge beschikbaarheid van Azure SQL Database-service
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 10/05/2018
ms.openlocfilehash: 1c6fb3660f395e709207e788b4ef648a69cae48d
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868572"
---
# <a name="high-availability-and-azure-sql-database"></a>Hoge beschikbaarheid en Azure SQL-Database

Azure SQL Database is de maximaal beschikbare database Platform as a Service dat ervoor zorgt dat uw database omhoog en actieve 99,99% van de tijd, zonder u zorgen te maken over het onderhoud en storingen. Dit is een volledig beheerde SQL Server Database Engine-proces die wordt gehost in de Azure-cloud die ervoor zorgt dat uw SQL Server-database altijd bijgewerkt/patch uitgevoerd is zonder gevolgen voor uw workload. Wanneer een exemplaar is een patch uitgevoerd of een failover uitvoert, de uitvaltijd is doorgaans niet opvallen als u [maken gebruik van logica voor opnieuw proberen](sql-database-develop-overview.md#resiliency) in uw app. Als de tijd voor het voltooien van een failover langer dan 60 seconden is, moet u een ondersteuningsaanvraag openen. Azure SQL-Database kunt snel herstellen, zelfs in de meest kritieke gevallen ervoor te zorgen dat uw gegevens altijd beschikbaar is.

Azure-platform volledig elke Azure SQL-Database wordt beheerd en zonder verlies van gegevens en een hoog percentage van de beschikbaarheid van gegevens wordt gegarandeerd. Azure regelt automatisch toepassen van patches, back-ups, replicatie, foutdetectie, onderliggende mogelijke hardware, software of netwerkstoringen, distribueren oplossingen voor problemen, failovers, upgrades van de database en andere onderhoudstaken. SQL Server-technici hebben geïmplementeerd de bekendste procedures ervoor te zorgen dat alle onderhoudsbewerkingen wordt uitgevoerd in minder dan 0,01% tijd van de levensduur van uw database. Deze architectuur is ontworpen om ervoor te zorgen dat vastgelegde gegevens nooit kwijt is en dat er onderhoudsbewerkingen worden uitgevoerd zonder gevolgen voor werkbelasting. Er zijn geen onderhoudsvensters of downtimes moeten waarvoor u de werkbelasting stoppen terwijl de database wordt bijgewerkt of onderhouden. Ingebouwde hoge beschikbaarheid in Azure SQL Database zorgt ervoor dat de database kan enkel storingspunt in de softwarearchitectuur niet.

Azure SQL Database is gebaseerd op SQL Server Database Engine-architectuur die wordt aangepast aan de cloudomgeving om ervoor te zorgen, zelfs in het geval van infrastructuuruitval voor 99,99% beschikbaarheid. Er zijn twee hoge beschikbaarheid architectuur modellen die worden gebruikt in Azure SQL Database (van beide ervoor te zorgen dat de beschikbaarheid van 99,99%):
- Standard/voor algemeen gebruik service tier model dat is gebaseerd op een scheiding van berekening en opslag. Deze architectuur is gebaseerd op hoge beschikbaarheid en betrouwbaarheid van de storage-laag, maar dat er enkele mogelijke prestatievermindering tijdens onderhoudswerkzaamheden.
- Kritieke-Premium/business laag-model dat is gebaseerd op een cluster van database-engine verwerkt. Deze architectuur is gebaseerd op een feit dat er altijd een quorum met knooppunten voor beschikbare database-engine en heeft minimale prestatie-impact op de werkbelasting, zelfs tijdens onderhoudswerkzaamheden.

Azure upgrades en patches onderliggende besturingssysteem, stuurprogramma's en SQL Server Database Engine transparant met de minimale uitvaltijd voor eindgebruikers. Azure SQL-Database wordt uitgevoerd op de laatste stabiele versie van SQL Server Database Engine en Windows-besturingssysteem en de meeste van de gebruikers niet ziet dat de upgrades continu worden uitgevoerd.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Basic, Standard- en algemene beschikbaarheid van de service tier

Standard beschikbaarheid verwijst naar de SLA van 99,99% dat wordt toegepast in de Servicelagen Basic, Standard en algemeen gebruik. Hoge beschikbaarheid in deze architectuur wordt bereikt door scheiding van reken- en opslaglaag en de replicatie van gegevens in de opslaglaag.

De volgende afbeelding ziet vier knooppunten in de standard-architectuur met de gescheiden reken- en storage-lagen.

![Scheiding van berekening en opslag](media/sql-database-managed-instance/general-purpose-service-tier.png)

In het model standard beschikbaarheid zijn er twee lagen:

- Een stateless compute-laag die het proces sqlserver.exe wordt uitgevoerd en bevat alleen tijdelijke en in de cache opgeslagen gegevens (bijvoorbeeld: plancache, buffergroep, kolom-archief van toepassingen). Deze stateless SQL Server-knooppunt wordt beheerd door Azure Service Fabric dat proces initialiseert, bepaalt de status van het knooppunt en failover naar een andere locatie uitvoert, indien nodig.
- Een stateful gegevenslaag met databasebestanden (.mdf/.ldf) die zijn opgeslagen in Azure Premium Storage. Azure Storage zorgt ervoor dat er worden geen gegevens verloren gaan van een record die in een databasebestand wordt geplaatst. Azure Storage heeft ingebouwde beschikbaarheid/redundantie die ervoor zorgt dat elke record in een logboekbestand of de pagina in bestand behouden blijft, zelfs als SQL Server-proces vastloopt.

Wanneer de upgrade van database-engine of -besturingssysteem wordt uitgevoerd, een deel van de onderliggende infrastructuur is mislukt of als een belangrijk probleem wordt gedetecteerd in Sql Server-proces, Azure Service Fabric stateless SQL Server-proces wordt verplaatst naar een andere staatloze compute-knooppunt. Er is een set van ongebruikte knooppunten die wacht op nieuwe compute-service in het geval van failover uitvoeren om het aantal failover-tijd. Gegevens in Azure Storage-laag wordt niet beïnvloed en gegevens/logboekbestanden zijn gekoppeld aan de nieuwe geïnitialiseerde SQL Server-proces. Dit proces garandeert 99,99% beschikbaarheid, maar dat er bepaalde prestatie-invloeden van zware werkbelasting die wordt uitgevoerd vanwege enige tijd en de nieuwe SQL Server-knooppunt van het feit begint met cold cache.

## <a name="premium-and-business-critical-service-tier-availability"></a>Premium en bedrijfskritiek laag servicebeschikbaarheid

Premium-beschikbaarheid is ingeschakeld in de Servicelagen Premium en bedrijfskritiek van Azure SQL Database en het is bedoeld voor intensieve werkbelastingen die invloed op de prestaties vanwege de lopende onderhoudsbewerkingen kunnen tolereren.

Azure SQL-database in de premium-model, kan worden geïntegreerd reken- en op één knooppunt. Hoge beschikbaarheid in deze architectuur wordt bereikt door middel van replicatie van berekening (SQL Server Database Engine-proces) en opslag (lokaal gekoppelde SSD) in 4 knooppunten geïmplementeerd [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) cluster.

![Cluster met knooppunten voor database-engine](media/sql-database-managed-instance/business-critical-service-tier.png)

De SQL Server Database Engine-proces- en onderliggende mdf/ldf-bestanden worden geplaatst op hetzelfde knooppunt met lage latentie aan uw workload biedt lokaal gekoppelde SSD-opslag. Hoge beschikbaarheid is geïmplementeerd met behulp van standaard [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Elke database is een cluster van de databaseknooppunten met één primaire database, die toegankelijk is voor de workload van een klant en een drie secundaire processen met kopieën van gegevens. Het primaire knooppunt pushes voortdurend de wijzigingen op secundaire knooppunten om ervoor te zorgen dat de gegevens beschikbaar op secundaire replica's, zijn als het primaire knooppunt om een bepaalde reden vastloopt. Failover wordt verwerkt door de SQL Server Database Engine – één secundaire replica wordt het primaire knooppunt en een nieuwe secundaire replica is gemaakt om ervoor te zorgen voldoende knooppunten in het cluster. De werkbelasting wordt automatisch omgeleid naar de nieuwe primaire knooppunt.

Bovendien bedrijfskritiek cluster beschikt over een ingebouwde [Read Scale-Out](sql-database-read-scale-out.md) mogelijkheid waarmee u kunt gratis van kosten in rekening gebracht ingebouwde alleen-lezen-knooppunt kan worden gebruikt voor het kenmerk alleen-lezen query's uitvoeren (bijvoorbeeld rapporten) die niet van invloed op prestaties van uw primaire workload.

## <a name="zone-redundant-configuration-preview"></a>Zone-redundante configuratie (preview)

De quorum-set replica's voor de lokale opslag-configuraties worden standaard gemaakt in hetzelfde datacenter. Dankzij de introductie van [Azure Availability Zones](../availability-zones/az-overview.md), hebt u de mogelijkheid om de verschillende replica's in de quorum-sets voor verschillende beschikbaarheidszones in dezelfde regio. Om te voorkomen een single point of failure, is de controle-ring ook gedupliceerd in meerdere zones als drie gateway ringen (GW). De routering naar een specifieke gateway ring wordt bepaald door [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Omdat de configuratie van de zone-redundante geen extra databaseredundantie maakt, het gebruik van de Beschikbaarheidszones (preview) in de Servicelagen Premium en bedrijfskritiek is beschikbaar op zonder extra kosten. Als u een zone-redundante database selecteert, kunt u uw databases Premium en bedrijfskritiek flexibele aan een veel grotere set van fouten, met inbegrip van datacenter catastrofale uitval, zonder deze te wijzigen van de toepassingslogica. U kunt ook een bestaande Premium en bedrijfskritiek databases of pools converteren naar de configuratie van de zone-redundante.

Omdat de redundante zone quorum-set replica's in verschillende datacenters met een onderlinge afstand heeft, kan de verhoogde netwerklatentie verhoogt de tijd die doorvoeren en dus van invloed zijn op de prestaties van sommige OLTP-workloads. U kunt altijd terugkeren naar de configuratie met één zone door de instelling van de redundantie zone uit te schakelen. Dit proces is een grootte van gegevens en is vergelijkbaar met de reguliere service tier-update. De database of pool aan het einde van het proces is gemigreerd vanuit een redundante ring zone naar een enkele zone-ring of vice versa.

> [!IMPORTANT]
> Zone-redundante databases en elastische pools zijn momenteel alleen ondersteund in de Premium-servicelaag. Tijdens de preview-versie, back-ups en audit records worden opgeslagen in de RA-GRS-opslag en daarom mogelijk niet automatisch beschikbaar in het geval van een storing in de hele zone. 

De zone-redundante-versie van de architectuur voor hoge beschikbaarheid wordt aangegeven door het volgende diagram:
 
![hoge beschikbaarheid architectuur zone-redundant](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="conclusion"></a>Conclusie
Azure SQL Database is nauw geïntegreerd met het Azure-platform en is sterk afhankelijk van de Service Fabric voor foutdetectie en herstel, op Azure Storage-Blobs voor gegevensbescherming en Beschikbaarheidszones voor hogere fouttolerantie. Op hetzelfde moment, maakt Azure SQL-database volledig gebruik van de technologie AlwaysOn-beschikbaarheidsgroep van SQL Server-vak product voor replicatie en failover. De combinatie van deze technologieën kan de toepassingen volledig profiteren van de voordelen van een gemengde opslagmodel en ondersteuning voor de meest veeleisende Sla's. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure-Beschikbaarheidszones](../availability-zones/az-overview.md)
- Meer informatie over [Service Fabric](../service-fabric/service-fabric-overview.md)
- Meer informatie over [met Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 
