---
title: Hoge beschikbaarheid - Azure SQL Database-service | Microsoft Docs
description: Meer informatie over de functies en mogelijkheden voor hoge beschikbaarheid van Azure SQL Database-service
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: jovanpop
ms.reviewer: carlrab, sashan
ms.openlocfilehash: 2283b7559bb0dc7e8333949a8e6382d562162123
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092484"
---
# <a name="high-availability-and-azure-sql-database"></a>Hoge beschikbaarheid en Azure SQL-Database

Azure SQL Database is de maximaal beschikbare database Platform as a Service dat ervoor zorgt dat uw database omhoog en actieve 99,99% van de tijd, zonder u zorgen te maken over het onderhoud en storingen. Dit is een volledig beheerde SQL Server Database Engine-proces die wordt gehost in de Azure-cloud die ervoor zorgt dat uw SQL Server-database altijd bijgewerkt/patch uitgevoerd is zonder gevolgen voor uw workload. Azure SQL-Database kunt snel herstellen, zelfs in de meest kritieke gevallen ervoor te zorgen dat uw gegevens altijd beschikbaar is.

Azure-platform volledig elke Azure SQL-Database wordt beheerd en zonder verlies van gegevens en een hoog percentage van de beschikbaarheid van gegevens wordt gegarandeerd. Azure regelt automatisch toepassen van patches, back-ups, replicatie, foutdetectie, onderliggende mogelijke hardware, software of netwerkstoringen, distribueren oplossingen voor problemen, failovers, upgrades van de database en andere onderhoudstaken. SQL Server-technici hebben geïmplementeerd de bekendste procedures ervoor te zorgen dat alle onderhoudsbewerkingen wordt uitgevoerd in minder dan 0,01% tijd van de levensduur van uw database. Deze architectuur is ontworpen om ervoor te zorgen dat vastgelegde gegevens nooit kwijt is en dat er onderhoudsbewerkingen worden uitgevoerd zonder gevolgen voor werkbelasting. Er zijn geen onderhoudsvensters of downtimes moeten waarvoor u de werkbelasting stoppen terwijl de database wordt bijgewerkt of onderhouden. Ingebouwde hoge beschikbaarheid in Azure SQL Database zorgt ervoor dat de database kan enkel storingspunt in de softwarearchitectuur niet.

Er zijn twee modellen met hoge beschikbaarheid in Azure SQL toegepast:

- Standard/voor algemeen gebruik-model dat u kunt van 99,99% van de tijd beschikbaar, maar met enkele mogelijke prestatievermindering tijdens onderhoudswerkzaamheden.
- Premium/kritieke bedrijfsmodel waarmee biedt ook 99,99% beschikbaarheid met minimale prestatie-impact op uw werkbelasting, zelfs tijdens onderhoudswerkzaamheden.

Azure upgrades en patches onderliggende besturingssysteem, stuurprogramma's en SQL Server Database Engine transparant met de minimale uitvaltijd voor eindgebruikers. Azure SQL-Database wordt uitgevoerd op de laatste stabiele versie van SQL Server Database Engine en Windows-besturingssysteem en de meeste van de gebruikers niet ziet dat de upgrades continu worden uitgevoerd.

## <a name="standard-availability"></a>Standard beschikbaarheid

Standard beschikbaarheid verwijst naar de SLA van 99,99% dat wordt toegepast in de lagen Basic/Standard/algemeen gebruik. Beschikbaarheid wordt bereikt door een scheiding van reken- en opslaglaag. In het model standard beschikbaarheid hebben we twee lagen:

- Een stateless compute-laag die het proces sqlserver.exe wordt uitgevoerd en bevat alleen tijdelijke en in de cache opgeslagen gegevens (bijvoorbeeld: plancache, buffergroep, kolom-archief van toepassingen). Deze stateless SQL Server-knooppunt wordt beheerd door Azure Service Fabric dat proces initialiseert, bepaalt de status van het knooppunt en failover naar een andere locatie uitvoert, indien nodig.
- Een stateful gegevenslaag met databasebestanden (.mdf/.ldf) die zijn opgeslagen in Azure Premium Storage-schijven. Azure Storage zorgt ervoor dat er worden geen gegevens verloren gaan van een record die in een databasebestand wordt geplaatst. Azure Storage heeft ingebouwde beschikbaarheid/redundantie die ervoor zorgt dat elke record in een logboekbestand of de pagina in bestand behouden blijft, zelfs als SQL Server-proces vastloopt.

Wanneer de database-engine of het besturingssysteem is bijgewerkt, of als een belangrijk probleem wordt gedetecteerd in Sql Server-proces, Azure Service Fabric stateless SQL Server-proces wordt verplaatst naar een andere staatloze compute-knooppunt. Gegevens in Azure Storage-laag wordt niet beïnvloed en gegevens/logboekbestanden zijn gekoppeld aan de nieuwe geïnitialiseerde SQL Server-proces. Verwachte failover-tijd kan worden gemeten in seconden. Dit proces garandeert 99,99% beschikbaarheid, maar dat er bepaalde prestatie-invloeden van zware werkbelasting die worden uitgevoerd vanwege enige tijd en de nieuwe SQL Server-knooppunt van het feit begint met cold cache.

## <a name="premium-availability"></a>Premium-beschikbaarheid

Premium-beschikbaarheid is ingeschakeld in Premium-laag van Azure SQL Database en het is bedoeld voor intensieve werkbelastingen die invloed op de prestaties vanwege de lopende onderhoudsbewerkingen kunnen tolereren.

Azure SQL-database in de premium-model, kan worden geïntegreerd reken- en op één knooppunt. De SQL Server Database Engine-proces- en onderliggende mdf/ldf-bestanden worden geplaatst op hetzelfde knooppunt met lage latentie aan uw workload biedt lokaal gekoppelde SSD-opslag.

Hoge beschikbaarheid is geïmplementeerd met behulp van standaard [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Elke database is een cluster met databaseknooppunten met een primaire database, die toegankelijk is voor de workload van een klant, en een paar secundaire processen met kopieën van gegevens. Het primaire knooppunt pushes voortdurend de wijzigingen op secundaire knooppunten om ervoor te zorgen dat de gegevens beschikbaar op secundaire replica's, zijn als het primaire knooppunt om een bepaalde reden vastloopt. Failover wordt verwerkt door de SQL Server Database Engine – één secundaire replica wordt het primaire knooppunt en een nieuwe secundaire replica is gemaakt om ervoor te zorgen voldoende knooppunten in het cluster. De werkbelasting wordt automatisch omgeleid naar de nieuwe primaire knooppunt. Failover-tijd wordt gemeten in milliseconden en de nieuwe primaire instantie is onmiddellijk gereed is om door te gaan met het uitvoeren van aanvragen.

## <a name="zone-redundant-configuration"></a>Zone-redundante configuratie

De quorum-set replica's voor de lokale opslag-configuraties worden standaard gemaakt in hetzelfde datacenter. Dankzij de introductie van [Azure Availability Zones](../availability-zones/az-overview.md), hebt u de mogelijkheid om de verschillende replica's in de quorum-sets voor verschillende beschikbaarheidszones in dezelfde regio. Om te voorkomen een single point of failure, is de controle-ring ook gedupliceerd in meerdere zones als drie gateway ringen (GW). De routering naar een specifieke gateway ring wordt bepaald door [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Omdat de configuratie van de zone-redundante geen extra databaseredundantie maakt, het gebruik van de Beschikbaarheidszones in de Servicelagen Premium en bedrijfskritiek is beschikbaar op zonder extra kosten. Als u een zone-redundante database selecteert, kunt u uw databases Premium en bedrijfskritiek flexibele aan een veel grotere set van fouten, met inbegrip van datacenter catastrofale uitval, zonder deze te wijzigen van de toepassingslogica. U kunt ook een bestaande Premium en bedrijfskritiek databases of pools converteren naar de configuratie van de zone-redundante.

Omdat de redundante zone quorum-set replica's in verschillende datacenters met een onderlinge afstand heeft, kan de verhoogde netwerklatentie verhoogt de tijd die doorvoeren en dus van invloed zijn op de prestaties van sommige OLTP-workloads. U kunt altijd terugkeren naar de configuratie met één zone door de instelling van de redundantie zone uit te schakelen. Dit proces is een grootte van gegevens en is vergelijkbaar met de normale service level objective (SLO)-update. De database of pool aan het einde van het proces is gemigreerd vanuit een redundante ring zone naar een enkele zone-ring of vice versa.

De zone-redundante-versie van de architectuur voor hoge beschikbaarheid wordt aangegeven door het volgende diagram:
 
![hoge beschikbaarheid architectuur zone-redundant](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Uitschaling lezen
Zoals wordt beschreven, Servicelagen Premium en bedrijfskritiek gebruikmaken van de quorum-sets en altijd op technologie voor hoge beschikbaarheid in één zone en de zone-redundante configuraties. Een van de voordelen van AlwaysOn is dat de replica's altijd de transactioneel consistente status hebben. Omdat de replica's hetzelfde prestatieniveau als de primaire hebben, de toepassing kan profiteren van deze extra capaciteit voor het onderhoud van de alleen-lezen werkbelastingen zonder extra kosten (lezen scale-out). Op deze manier de alleen-lezen query's worden geïsoleerd van de belangrijkste workload voor lezen / schrijven en heeft geen invloed op de prestaties. Lezen van de functie scale-out is bedoeld voor de toepassingen die logisch zijn gescheiden van alleen-lezen-werkbelastingen, zoals analytics en daarom kan gebruikmaken van deze extra capaciteit zonder verbinding te maken met de primaire. 

Voor het gebruik van de functie Read Scale-Out met een bepaalde database, moet u expliciet deze activeren bij het maken van de database of later door het wijzigen van de configuratie met behulp van PowerShell door het aanroepen van de [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) of de [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlets of via de REST-API van Azure Resource Manager met behulp de [Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate) methode.

Nadat Read Scale-Out is ingeschakeld voor een database, toepassingen die verbinding maken met deze database worden omgeleid naar de alleen-lezen-replica of naar een alleen-lezen replica van die database volgens de `ApplicationIntent` eigenschap geconfigureerd in van de toepassing de verbindingsreeks. Voor meer informatie over de `ApplicationIntent` eigenschap, Zie [Toepassingsintentie op te geven](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Als Read Scale-Out is uitgeschakeld of als u de eigenschap ReadScale in een niet-ondersteunde servicelaag instellen, alle verbindingen worden doorgestuurd naar de alleen-lezen replica, onafhankelijk van de `ApplicationIntent` eigenschap.  

> [!NOTE]
> Het is mogelijk te activeren Read Scale-out op een Standard- of een algemeen gebruik-database, zelfs als het leidt niet tot de alleen-lezen routering bedoeld sessie naar een afzonderlijke replica. Dit wordt gedaan ter ondersteuning van bestaande toepassingen die omhoog en omlaag tussen lagen Standard/voor algemeen gebruik en Premium/bedrijfskritiek schalen.  

De functie Read Scale-Out ondersteunt sessieniveauconsistentie. Als de alleen-lezen-sessie opnieuw verbinding maakt nadat een verbindingsfout veroorzaken bij replica niet beschikbaar zijn, kan deze worden omgeleid naar een andere replica. Hoewel dit onwaarschijnlijk is, kan dit resulteren in de verwerking van de gegevensset die is verouderd. Op dezelfde manier als een toepassing schrijft gegevens met behulp van een sessie voor lezen / schrijven en leest direct met behulp van de alleen-lezen-sessie, is het mogelijk dat de nieuwe gegevens niet meteen zichtbaar is.

## <a name="conclusion"></a>Conclusie
Azure SQL Database is nauw geïntegreerd met het Azure-platform en is sterk afhankelijk van de Service Fabric voor foutdetectie en herstel, op Azure Storage-Blobs voor gegevensbescherming en Beschikbaarheidszones voor hogere fouttolerantie. Op hetzelfde moment, maakt Azure SQL-database volledig gebruik van de technologie AlwaysOn-beschikbaarheidsgroep van SQL Server-vak product voor replicatie en failover. De combinatie van deze technologieën kan de toepassingen volledig profiteren van de voordelen van een gemengde opslagmodel en ondersteuning voor de meest veeleisende Sla's. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure-Beschikbaarheidszones](../availability-zones/az-overview.md)
- Meer informatie over [Service Fabric](../service-fabric/service-fabric-overview.md)
- Meer informatie over [met Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 
