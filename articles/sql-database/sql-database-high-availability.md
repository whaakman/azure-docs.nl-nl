---
title: Hoge beschikbaarheid - Azure SQL Database-service | Microsoft Docs
description: Meer informatie over de functies en mogelijkheden voor hoge beschikbaarheid van Azure SQL Database-service
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: sashan
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 06/10/2019
ms.openlocfilehash: a88842802759a5c3ae7af7334bbe125344c978ea
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066905"
---
# <a name="high-availability-and-azure-sql-database"></a>Hoge beschikbaarheid en Azure SQL-Database

Het doel van de architectuur voor hoge beschikbaarheid in Azure SQL Database is om te garanderen dat uw database actief is en actieve 99,99% van de tijd, zonder u zorgen te maken over de gevolgen van het onderhoud en storingen. Azure regelt automatisch essentieel onderhoud taken, zoals het toepassen van patches, back-ups, upgrades voor Windows en SQL, evenals niet-geplande gebeurtenissen, zoals de onderliggende hardware, software of netwerkstoringen.  Wanneer het onderliggende SQL-exemplaar wordt gevuld of failover optreedt, is niet de downtime merkbare als u [maken gebruik van logica voor opnieuw proberen](sql-database-develop-overview.md#resiliency) in uw app. Azure SQL-Database kunt snel herstellen, zelfs in de meest kritieke gevallen ervoor te zorgen dat uw gegevens altijd beschikbaar is.

De oplossing voor hoge beschikbaarheid is ontworpen om ervoor te zorgen dat vastgelegde gegevens nooit verloren vanwege fouten, onderhoudsbewerkingen hebben geen invloed op uw werkbelasting, en wordt de database is niet een single point of failure in uw softwarearchitectuur. Er zijn geen onderhoudsvensters of downtimes moeten waarvoor u de werkbelasting stoppen terwijl de database wordt bijgewerkt of onderhouden. 

Er zijn twee hoge beschikbaarheid architectuur modellen die worden gebruikt in Azure SQL Database:

- Beschikbaarheid van de Standard-model dat is gebaseerd op een scheiding van berekening en opslag.  Dit is afhankelijk van hoge beschikbaarheid en betrouwbaarheid van de externe opslaglaag. Deze architectuur is gericht op budget gebaseerde zakelijke toepassingen die een verslechtering van prestaties optreedt tijdens onderhoudswerkzaamheden kunnen tolereren.
- Premium beschikbaarheid-model dat is gebaseerd op een cluster van database-engine verwerkt. Afhankelijk van het feit dat er altijd een quorum met knooppunten voor beschikbare database-engine. Deze architectuur is gericht op essentiële toepassingen met hoge i/o-prestaties, hoge Transactiesnelheid en garanties minimale prestatie-impact op uw workload tijdens onderhoudswerkzaamheden.

Azure SQL-Database wordt uitgevoerd op de laatste stabiele versie van SQL Server Database Engine en Windows-besturingssysteem en de meeste gebruikers ziet niet dat upgrades continu worden uitgevoerd.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Basic, Standard- en algemene beschikbaarheid van de service tier

Deze Servicelagen gebruikmaken van de beschikbaarheid van de standard-architectuur. De volgende afbeelding ziet u vier verschillende knooppunten met de gescheiden reken- en storage-lagen.

![Scheiding van berekening en opslag](media/sql-database-high-availability/general-purpose-service-tier.png)

De beschikbaarheid van de standard-model bevat twee lagen:

- Een stateless rekenlaag die wordt uitgevoerd de `sqlserver.exe` verwerken en bevat alleen tijdelijke en in de cache opgeslagen gegevens op de gekoppelde SSD, zoals TempDB, model-database, plancache, buffergroep en kolom groep van toepassingen. Dit knooppunt stateless wordt beheerd door Azure Service Fabric, waarmee wordt geïnitialiseerd `sqlserver.exe`, bepaalt de status van het knooppunt en failover naar een ander knooppunt wordt uitgevoerd indien nodig.
- Een stateful gegevenslaag met de databasebestanden (.mdf/.ldf) die zijn opgeslagen in Azure Blob-opslag. Azure blob-opslag heeft ingebouwde beschikbaarheid en redundantie functie. Dit zorgt ervoor dat elke record in het logboekbestand of de pagina in het gegevensbestand behouden blijft, zelfs als SQL Server-proces vastloopt.

Wanneer de database-engine of het besturingssysteem is bijgewerkt, of een storing wordt gedetecteerd, wordt Azure Service Fabric stateless SQL Server-proces verplaatst naar een andere staatloze compute-knooppunt met voldoende beschikbare capaciteit. Gegevens in Azure Blob-opslag wordt niet beïnvloed door de verplaatsing en de gegevens/logboekbestanden zijn gekoppeld aan de nieuwe geïnitialiseerde SQL Server-proces. Dit proces garandeert 99,99% beschikbaarheid, maar zwaar worden belast sommige verslechtering van prestaties optreedt tijdens de overgang kan optreden omdat het nieuwe SQL Server-exemplaar wordt gestart met cold cache.

## <a name="premium-and-business-critical-service-tier-availability"></a>Premium en bedrijfskritiek laag servicebeschikbaarheid

Premium en bedrijfskritiek service-lagen gebruik te maken van het model van de beschikbaarheid van Premium, dat kan worden geïntegreerd compute-resources (SQL Server Database Engine-proces) en opslag (lokaal gekoppelde SSD) op een enkel knooppunt. Hoge beschikbaarheid wordt bereikt door berekeningen en opslag te repliceren op extra knooppunten in het maken van een cluster drie tot vier - knooppunt. 

![Cluster met knooppunten voor database-engine](media/sql-database-high-availability/business-critical-service-tier.png)

Bestanden van de onderliggende database (.mdf/.ldf) worden geplaatst op de gekoppelde SSD-opslag voor zeer lage latentie i/o op uw workload. Hoge beschikbaarheid wordt geïmplementeerd met behulp van een technologie die vergelijkbaar is met SQL Server [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Het cluster bevat één primaire replica (SQL Server-proces) die toegankelijk is voor klantwerkbelastingen voor lezen / schrijven-, en maximaal drie secundaire replica's (berekeningen en opslag) met kopieën van gegevens. Het primaire knooppunt voortdurend wijzigingen verstuurd naar de secundaire knooppunten in de volgorde en zorgt ervoor dat de gegevens naar ten minste één secundaire replica is gesynchroniseerd voordat het doorvoeren van elke transactie. Dit proces zorgt ervoor dat als het primaire knooppunt loopt om welke reden vast, er altijd een volledig gesynchroniseerde knooppunt is failover naar. De failover wordt geïnitieerd door de Azure Service Fabric. Zodra de secundaire replica de nieuwe primaire knooppunt wordt, wordt nog een secundaire replica gemaakt om te controleren of dat het cluster heeft onvoldoende knooppunten (quorum is ingesteld). Nadat failover voltooid is, wordt SQL-verbindingen worden automatisch omgeleid naar de nieuwe primaire knooppunt.

Het model van de beschikbaarheid van premium biedt een extra voordeel, de mogelijkheid alleen-lezen SQL-serververbindingen omleiden naar een van de secundaire replica's. Deze functie is aangeroepen [Read Scale-Out](sql-database-read-scale-out.md). Het biedt 100% extra rekencapaciteit is zonder extra kosten voor ontlasten van alleen-lezen bewerkingen, zoals analytische workloads, van de primaire replica.

## <a name="zone-redundant-configuration"></a>Zone-redundante configuratie

Het cluster met knooppunten voor het model van de beschikbaarheid van premium wordt standaard gemaakt in hetzelfde datacenter. Dankzij de introductie van [Azure Availability Zones](../availability-zones/az-overview.md), SQL-Database verschillende replica's in het cluster voor verschillende beschikbaarheidszones in dezelfde regio kunt plaatsen. Om te voorkomen een single point of failure, is de controle-ring ook gedupliceerd in meerdere zones als drie gateway ringen (GW). De routering naar een specifieke gateway ring wordt bepaald door [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Omdat de zone-redundante-configuratie in de Servicelagen Premium en bedrijfskritiek geen extra databaseredundantie maakt, kunt u dit inschakelen zonder extra kosten. Als u een zone-redundante configuratie selecteert, kunt u uw databases Premium en bedrijfskritiek flexibele aan een veel grotere set van fouten, met inbegrip van datacenter catastrofale uitval, zonder deze te wijzigen naar de toepassingslogica. U kunt ook een bestaande Premium en bedrijfskritiek databases of pools converteren naar de configuratie van de zone-redundante.

Omdat de zone-redundante databases replica's in verschillende datacenters met een onderlinge afstand hebben, kan de verhoogde netwerklatentie verhoogt de tijd die doorvoeren en dus van invloed zijn op de prestaties van sommige OLTP-workloads. U kunt altijd terugkeren naar de configuratie met één zone door de instelling van de redundantie zone uit te schakelen. Dit proces is een online bewerking die vergelijkbaar is met de reguliere service tier-upgrade. De database of pool aan het einde van het proces is gemigreerd vanuit een redundante ring zone naar een enkele zone-ring of vice versa.

> [!IMPORTANT]
> Zone-redundante databases en elastische pools zijn momenteel alleen ondersteund in de Servicelagen Premium en bedrijfskritiek in bepaalde regio's. Wanneer u de laag bedrijfskritiek, is zone-redundante configuratie alleen beschikbaar wanneer de hardware van de compute Gen5 is geselecteerd. Zie voor informatie over de regio's die ondersteuning bieden voor zone-redundante databases up-to-date [serviceondersteuning per regio](../availability-zones/az-overview.md#services-support-by-region).  

De zone-redundante-versie van de architectuur voor hoge beschikbaarheid wordt aangegeven door het volgende diagram:

![hoge beschikbaarheid architectuur zone-redundant](./media/sql-database-high-availability/zone-redundant-business-critical-service-tier.png)

## <a name="accelerated-database-recovery-adr"></a>Versneld databaseherstel (ADR)

[Versnelde Database Recovery (ADR)](sql-database-accelerated-database-recovery.md) wordt een nieuwe SQL database-engine-functie maken die aanzienlijk verbetert de beschikbaarheid van de database, met name bij langlopende transacties uitgevoerd. ADR is momenteel beschikbaar voor individuele databases, elastische pools en Azure SQL Data Warehouse.

## <a name="conclusion"></a>Conclusie

Azure SQL Database biedt een oplossing ingebouwde hoge beschikbaarheid, die is nauw geïntegreerd met het Azure-platform. Dit is afhankelijk van Service Fabric voor foutdetectie en herstel, Azure Blob-opslag voor de beveiliging van gegevens en Beschikbaarheidszones voor hogere fouttolerantie. Bovendien, Azure SQL-database maakt gebruik van de technologie van AlwaysOn-beschikbaarheidsgroep van SQL Server voor replicatie en failover. De combinatie van deze technologieën kan toepassingen volledig te kunnen benutten de voordelen van een gemengde opslag modelleren en ondersteuning voor de meest veeleisende Sla's.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure-Beschikbaarheidszones](../availability-zones/az-overview.md)
- Meer informatie over [Service Fabric](../service-fabric/service-fabric-overview.md)
- Meer informatie over [met Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
- Zie voor meer opties voor hoge beschikbaarheid en herstel na noodgevallen [zakelijke continuïteit](sql-database-business-continuity.md)
