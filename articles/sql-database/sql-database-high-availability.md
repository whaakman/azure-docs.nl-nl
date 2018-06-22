---
title: Hoge beschikbaarheid - service van Azure SQL Database | Microsoft Docs
description: Meer informatie over de functies en mogelijkheden voor hoge beschikbaarheid van Azure SQL Database-service
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: jovanpop
ms.reviewer: carlrab, sashan
ms.openlocfilehash: 4e1963e97a7458db8badb63e28dbc3d215ad88b2
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309627"
---
# <a name="high-availability-and-azure-sql-database"></a>Hoge beschikbaarheid en Azure SQL-Database

Azure SQL-Database is de maximaal beschikbare database Platform als een Service die zorgt ervoor dat uw database van is en de actieve 99,99% tijd, zonder dat u over het onderhouden en downtime. Dit is een volledig beheerde SQL Server Database Engine-proces gehost in de Azure-cloud die ervoor zorgt dat uw SQL Server-database altijd bijgewerkt/hersteld is zonder uw workload. Azure SQL-Database kunnen snel herstellen zelfs in de meest kritieke omstandigheden ervoor te zorgen dat uw gegevens altijd beschikbaar is.

Azure-platform is volledig beheert van elke Azure SQL Database en garandeert geen verlies van gegevens en een hoog percentage van de beschikbaarheid van gegevens. Azure verwerkt automatisch patchen, back-ups, replicatie, foutdetectie, onderliggende potentiële hardware, software of netwerkstoringen, implementatie oplossingen voor problemen, failovers, upgrades van de database en overige onderhoudstaken. SQL Server-engineers hebben geïmplementeerd de bekendste procedures ervoor te zorgen dat de onderhoudswerkzaamheden zijn voltooid in minder dan 0,01% tijd van de levensduur van uw database. Deze architectuur is ontworpen om ervoor te zorgen dat doorgevoerd gegevens nooit verloren en wordt dat onderhoudsbewerkingen worden uitgevoerd zonder de werkbelasting. Er zijn geen onderhoudsvensters of downtimes moeten waarvoor u de werkbelasting niet meer bij de database is bijgewerkt of onderhouden. Ingebouwde hoge beschikbaarheid in Azure SQL Database zorgt ervoor dat de database kan nooit worden enkel storingspunt in de architectuur van de software.

Er zijn twee hoge beschikbaarheid modellen in Azure SQL toegepast:

- Standaard/algemeen doel model waarmee 99,99% beschikbaarheid van maar een aantal mogelijke vermindering in prestaties tijdens onderhoudsactiviteiten.
- Premium/kritieke bedrijfsmodel waarmee biedt ook 99,99% beschikbaarheid met minimale prestatie-invloed van uw werkbelasting zelfs tijdens onderhoudsactiviteiten.

Azure upgrades en -patches onderliggende besturingssysteem, stuurprogramma's en SQL Server Database Engine transparant met de minimale uitvaltijd voor eindgebruikers. Azure SQL-Database wordt uitgevoerd op de meest recente stabiele versie van SQL Server Database Engine en het Windows-besturingssysteem en de meeste van de gebruikers niet ziet dat de upgrades continu worden uitgevoerd.

## <a name="standard-availability"></a>Standaard-beschikbaarheid

Standaard beschikbaarheid verwijst naar SLA van 99,99% dat in de lagen Standard/Basic/algemeen doel wordt toegepast. Beschikbaarheid wordt bereikt door scheiding van de berekenings- en lagen. In het model standaard beschikbaarheid hebben we twee lagen:

- Een stateless berekeningslaag dat alleen tijdelijke en in de cache opgeslagen gegevens (bijvoorbeeld – plancache, buffergroep en kolom store pool) bevat en dat het proces sqlserver.exe wordt uitgevoerd. Dit staatloze SQL Server-knooppunt wordt beheerd door de Azure Service Fabric dat proces initialiseert, controleert de status van het knooppunt en failover naar een andere locatie uitvoert, indien nodig.
- Een stateful laag met de databasebestanden (.mdf/.ldf) die zijn opgeslagen in Azure Premium-opslag-schijven. Azure-opslag zorgt ervoor dat er worden geen verlies van gegevens van een record die in een databasebestand wordt geplaatst. Azure-opslag heeft ingebouwde beschikbaarheid/gegevensredundantie dat ervoor zorgt dat elke record in het logboekbestand of de pagina in bestand wordt behouden zelfs als de SQL Server-proces is vastgelopen.

Wanneer het besturingssysteem of database-engine is bijgewerkt of als een kritiek probleem in Sql Server-proces wordt gedetecteerd, Azure Service Fabric staatloze SQL Server-proces wordt verplaatst naar een andere staatloze rekenknooppunt. Gegevens in Azure Storage-laag wordt niet beïnvloed en gegevens/logboekbestanden zijn gekoppeld aan de nieuwe geïnitialiseerde SQL Server-proces. Verwachte failover-tijd kan worden gemeten in seconden. Dit proces wordt gegarandeerd dat 99,99% beschikbaarheid, maar heeft deze mogelijk bepaalde prestaties gevolgen voor zwaar worden belast die worden uitgevoerd vanwege overgangsduur en het nieuwe SQL Server-knooppunt van het feit gestart met koude cache.

## <a name="premium-availability"></a>Premium-beschikbaarheid

Premium-beschikbaarheid is ingeschakeld in Premium-laag van Azure SQL Database en is bedoeld voor intensieve werkbelastingen die invloed op de prestaties vanwege de lopende onderhoudsbewerkingen kunnen tolereren.

In het model premium integreert Azure SQL-database berekeningen en opslag op één knooppunt. Het Database-Engine van SQL Server-proces en onderliggende mdf/ldf-bestanden worden geplaatst op hetzelfde knooppunt met lokaal gekoppelde SSD-opslag bieden lage latentie voor uw workload.

Hoge beschikbaarheid is geïmplementeerd met behulp van standaard [altijd op beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Elke database is een cluster met databaseknooppunten met een primaire database die toegankelijk is voor de werkbelasting van de klant en enkele secundaire processen met kopieën van gegevens. Het primaire knooppunt duwt voortdurend de wijzigingen aan secundaire knooppunten om ervoor te zorgen dat de gegevens beschikbaar zijn op secundaire replica's is als het primaire knooppunt om welke reden vastloopt. Failover wordt verwerkt door de SQL Server Database Engine – één secundaire replica wordt het primaire knooppunt en een nieuwe secundaire replica wordt gemaakt om ervoor te zorgen voldoende knooppunten in het cluster. De werkbelasting wordt automatisch omgeleid naar de nieuwe primaire knooppunt. Failover-tijd wordt gemeten in milliseconden en de nieuwe primaire instantie is onmiddellijk gereed om te blijven voldoen aan aanvragen.

## <a name="zone-redundant-configuration-preview"></a>Zone-redundante configuratie (preview)

Standaard worden de quorum-set replica's voor de lokale opslagconfiguraties gemaakt in hetzelfde datacenter. Dankzij de introductie van [Azure beschikbaarheid Zones](../availability-zones/az-overview.md), hebt u de mogelijkheid om de andere replica's in het quorum-stelt aan andere beschikbaarheid zones in dezelfde regio. Om te voorkomen een potentieel risico, is de besturingselement-ring ook gedupliceerd in meerdere zones als drie gateway ringen (GW). De routering naar een specifieke gateway ring wordt beheerd door [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM). Omdat de configuratie van de zone-redundante geen extra databaseredundantie, het gebruik van beschikbaarheid Zones in de Premium maakt of Business-kritische (preview) servicecategorieën beschikbaar is zonder extra kosten. Als u een zone redundant database selecteert, kunt u uw Premium of Business-kritische (preview) flexibel omgaan met een veel grotere set van fouten, met inbegrip van datacenter catastrofale uitval, zonder eventuele wijzigingen van de toepassingslogica databases. U kunt ook alle bestaande Premium of kritieke zakelijke databases of groepen (preview) converteren naar de zone-redundante configuratie.

Omdat de redundante zone quorum-set replica's in verschillende datacenters met sommige afstand tussen deze twee heeft, mag de toegenomen netwerklatentie vergroot de toegewezen tijd en dus invloed op de prestaties van sommige OLTP-werkbelastingen. Altijd kunt u terugkeren naar de configuratie met één zone door het uitschakelen van de instelling van de redundantie van de zone. Dit proces heeft een grootte van gegevens en is vergelijkbaar met de reguliere service level objective (SLO)-update. De database of de groep van toepassingen aan het einde van het proces is gemigreerd uit een zone redundant-ring naar een enkele zone ring of vice versa.

> [!IMPORTANT]
> Zone-redundante databases en elastische pools zijn momenteel alleen ondersteund in de Premium servicecategorie. Tijdens de openbare preview, back-ups en audit records worden opgeslagen in de opslag van de RA-GRS en daarom mogelijk niet automatisch beschikbaar in het geval van een onderbreking van de hele zone. 

De zone redundant versie van de architectuur van hoge beschikbaarheid wordt door het volgende diagram geïllustreerd:
 
![hoge beschikbaarheid architectuur zoneredundant](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Scale-out lezen
Zoals wordt beschreven, service Premium en Business-kritische (preview) lagen hefboomwerking quorum-stelt en altijd op technologie voor hoge beschikbaarheid in één zone en zone redundant configuraties. Een van de voordelen van AlwasyON is de replica's zijn altijd de transactioneel consistent is. Omdat de replica's hetzelfde prestatieniveau als het primaire hebben, de toepassing kan profiteren van deze extra capaciteit voor het onderhoud van de alleen-lezen werkbelastingen zonder extra kosten (lezen scale-out). Op deze manier de alleen-lezen-query's zijn geïsoleerd van de belangrijkste lezen-schrijven werkbelasting en niet van invloed op de prestaties. Lees de functie scale-out is bedoeld voor de toepassingen die logisch zijn alleen-lezen werkbelastingen zoals analytics gescheiden en daarom kan gebruikmaken van deze extra capaciteit zonder verbinding met de primaire. 

Als u wilt de lezen Scale-Out-functie met een bepaalde database gebruiken, moet u expliciet deze activeren bij het maken van de database of later door het wijzigen van de configuratie met behulp van PowerShell door aan te roepen de [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) of de [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlets of via de REST-API van Azure Resource Manager met behulp van de [Databases - maken of bijwerken](/rest/api/sql/databases/createorupdate) methode.

Nadat lezen Scale-Out is ingeschakeld voor een database, verbinding maken met database toepassingen wordt omgeleid naar de alleen-lezen-replica of naar een alleen-lezen replica van die database volgens de `ApplicationIntent` eigenschap geconfigureerd in de toepassing verbindingstekenreeks. Voor informatie over de `ApplicationIntent` eigenschap, Zie [Toepassingsintentie geven](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Als lezen Scale-Out is uitgeschakeld of u de eigenschap ReadScale in een niet-ondersteunde servicelaag instellen, alle verbindingen worden omgeleid naar de alleen-lezen-replica, onafhankelijk van de `ApplicationIntent` eigenschap.  

> [!NOTE]
> Het is mogelijk te activeren lezen Scale-out op een standaard of een database voor algemene doeleinden, ondanks dat niet resulteert in de alleen-lezen routering sessie naar een afzonderlijke replica bedoeld. Dit wordt gedaan ter ondersteuning van bestaande toepassingen waarvoor omhoog en omlaag geschaald tussen de lagen Standard/algemeen doel- en Premium/Business-kritische.  

De functie lezen Scale-Out ondersteunt sessie niveau consistentie. Als de alleen-lezen-sessie opnieuw verbinding maakt nadat een verbindingsfout veroorzaken bij replica niet beschikbaar zijn, kan deze worden omgeleid naar een andere replica. Terwijl onwaarschijnlijk, kan dit leiden tot het verwerken van de gegevensset die is verouderd. Op dezelfde manier als een toepassing schrijft gegevens via een alleen-lezen-sessie en leest onmiddellijk met behulp van de alleen-lezen-sessie, is het mogelijk dat de nieuwe gegevens niet direct zichtbaar is.

## <a name="conclusion"></a>Conclusie
Azure SQL Database is nauw geïntegreerd met het Azure-platform en maximaal afhankelijk is van het Service Fabric voor foutdetectie en herstel van Azure Storage-Blobs voor gegevensbescherming en beschikbaarheid Zones voor hogere fouttolerantie. Op hetzelfde moment Azure SQL database volledig maakt gebruik van de AlwaysOn-beschikbaarheidsgroep-technologie van SQL Server-product vak voor replicatie en failover. De combinatie van deze technologieën kan de toepassingen volledig de voordelen van een gemengde opslagmodel en ondersteuning van de zwaarste serviceovereenkomsten. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure beschikbaarheid Zones](../availability-zones/az-overview.md)
- Meer informatie over [Fabric-Service](../service-fabric/service-fabric-overview.md)
- Meer informatie over [met Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) 
