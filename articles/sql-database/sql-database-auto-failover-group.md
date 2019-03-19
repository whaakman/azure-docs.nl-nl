---
title: Failover-groepen - Azure SQL Database | Microsoft Docs
description: Automatische failover-groepen is een SQL-Database-functie waarmee u voor het beheren van replicatie en automatische / gecoördineerde failover van een groep databases op een SQL-Database-server of alle databases in het beheerde exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 7bfed1144ebfc69ed51b7bbc1adf78538ed28425
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57861074"
---
# <a name="use-auto-failover-groups-to-enable-transparent-and-coordinated-failover-of-multiple-databases"></a>Automatische failover-groepen gebruiken voor het inschakelen van transparante en gecoördineerd failover van meerdere databases

Automatische failover-groepen is een SQL-Database-functie waarmee u voor het beheren van replicatie en failover van een groep databases op een SQL-Database-server of alle databases in een beheerd exemplaar naar een andere regio (momenteel in openbare preview voor beheerd exemplaar). Hierbij de dezelfde onderliggende technologie als [actieve geo-replicatie](sql-database-active-geo-replication.md). U kunt failover handmatig starten of kunt u het delegeren aan de service SQL Database op basis van een door de gebruiker gedefinieerd beleid. De laatste optie kunt u meerdere verwante databases in een secundaire regio wordt automatisch hersteld na een onherstelbare fout of andere niet-geplande gebeurtenis optreedt die in het volledige of gedeeltelijke verlies van beschikbaarheid van de service SQL Database in de primaire regio resulteert. Bovendien kunt u de leesbare secundaire databases voor de offload van alleen-lezen querywerkbelastingen. Omdat automatische failover-groepen hebben betrekking op meerdere databases, moeten deze databases worden geconfigureerd op de primaire server. Primaire en secundaire servers voor de databases in de failovergroep moeten zich in hetzelfde abonnement. Automatische failovergroepen ondersteuning bieden voor replicatie van alle databases in de groep met slechts één secundaire server in een andere regio.

> [!NOTE]
> Bij het werken met één of gegroepeerde databases op een SQL-Database-server en u wilt dat meerdere secundaire replica's in de dezelfde of verschillende regio's, gebruikt u [actieve geo-replicatie](sql-database-active-geo-replication.md).

Wanneer u gebruikmaakt van automatische failovergroepen met automatische failover-beleid, een storing voordoet die gevolgen heeft voor een of meer van de databases in de Groepsresultaten van de in automatische failover. Bovendien, automatische failover-groepen bieden de lezen / schrijven en alleen-lezen-listener-eindpunten die blijven ongewijzigd tijdens failovers. Of u nu handmatige of automatische failover-activering, verandert failover alle secundaire databases in de groep in de primaire. Nadat de databasefailover is voltooid, wordt de DNS-record wordt automatisch bijgewerkt om te leiden van de eindpunten naar de nieuwe regio. Zie voor de specifieke RPO en RTO gegevens [overzicht van bedrijfscontinuïteit](sql-database-business-continuity.md).

Wanneer u gebruikmaakt van automatische failovergroepen met automatische failover-beleid, een storing voordoet die gevolgen heeft voor databases in de SQL Database-server of beheerd exemplaar resultaten in automatische failover. U kunt beheren via automatische failover:

- [Azure Portal](sql-database-implement-geo-distributed-database.md)
- [PowerShell: Failover-groep](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [REST API: Failovergroep](https://docs.microsoft.com/rest/api/sql/failovergroups).

Controleer of dat de verificatievereisten voor uw server en database zijn geconfigureerd op de nieuwe primaire na een failover. Zie voor meer informatie, [SQL Database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md).

Voor het bereiken van echte zakelijke continuïteit, toe te voegen databaseredundantie tussen datacenters is slechts een deel van de oplossing. Herstellen van een toepassing (service) end-to-end nadat een onherstelbare fout is vereist voor herstel van alle onderdelen die deel uitmaken van de service en afhankelijke services. Voorbeelden van deze onderdelen zijn de clientsoftware (bijvoorbeeld een browser met een aangepaste JavaScript), web-front-ends, opslag en DNS. Het is essentieel dat alle onderdelen die tolerant omgaan met de dezelfde fouten en beschikbaar is in de beoogde hersteltijd (RTO) van uw toepassing. Daarom moet u voor het identificeren van alle afhankelijke services en informatie over de garanties en mogelijkheden bieden. Vervolgens moet u de juiste stappen om ervoor te zorgen dat uw servicefuncties tijdens de failover van de services waarvan deze afhankelijk is, nemen. Zie voor meer informatie over het ontwerpen van oplossingen voor herstel na noodgevallen, [oplossingen voor Cloudopslag ontwerpen voor Disaster Recovery met behulp van actieve geo-replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="auto-failover-group-terminology-and-capabilities"></a>Automatische failover-groep terminologie en mogelijkheden

- **Failover-groep**

  Een failovergroep is een groep databases die worden beheerd door één SQL Database-server of in een beheerd exemplaar van één die fungeren als kan failover als één eenheid naar een andere regio in het geval alle of een primaire databases niet beschikbaar vanwege een storing in de primaire regio.

  - **SQL Database-servers**

     Met SQL Database-servers, kunnen sommige of alle van de databases op één SQL Database-server in een failovergroep worden geplaatst. Een SQL Database-server ondersteunt ook meerdere failovergroepen op een enkele SQL-Database-server.

  - **Beheerde exemplaren**
  
     Een failovergroep bevat alle gebruikersdatabases in het beheerde exemplaar met beheerd exemplaar, en daarom een beheerd exemplaar biedt alleen ondersteuning voor één failovergroep.

- **Primary**

  De SQL Database-server of een beheerd exemplaar dat als host fungeert voor de primaire databases in de failovergroep.

- **Secondary**

  De SQL Database-server of een beheerd exemplaar dat als host fungeert voor de secundaire databases in de failovergroep. De secundaire server kan niet in dezelfde regio als de primaire.

- **Individuele databases aan de failovergroep toe te voegen**

  U kunt verschillende individuele databases op dezelfde SQL-Database-server in dezelfde failover-groep plaatsen. Als u een individuele database aan de failovergroep toevoegt, wordt automatisch een secundaire database met behulp van dezelfde grootte edition en compute op de secundaire server gemaakt.  U hebt opgegeven die server bij de failovergroep is gemaakt. Als u een database die al een secundaire database in de secundaire server toevoegt, wordt deze koppeling voor geo-replicatie wordt overgenomen door de groep. Wanneer u een database die al een secundaire database in een server die geen deel uitmaakt van de failovergroep toevoegt, wordt een nieuwe secundaire gemaakt in de secundaire server.
  
> [!IMPORTANT]
  > In een beheerd exemplaar, worden alle gebruikersdatabases worden gerepliceerd. Een subset van gebruikersdatabases voor de replicatie kan niet worden opgenomen in de failovergroep.

- **Databases in elastische pool toevoegen aan de failovergroep**

  U kunt alle of enkele databases in een elastische pool in dezelfde failover-groep plaatsen. Als de primaire database zich in een elastische pool, wordt de secundaire server wordt automatisch gemaakt in de elastische pool met dezelfde naam (secundaire groep). U moet ervoor zorgen dat de secundaire server een elastische pool met de exacte naam en voldoende beschikbare capaciteit bevat voor het hosten van de secundaire databases die door de failovergroep wordt gemaakt. Als u een database in de groep die al een secundaire database in de secundaire groep toevoegt, wordt deze koppeling voor geo-replicatie wordt overgenomen door de groep. Wanneer u een database die al een secundaire database in een server die geen deel uitmaakt van de failovergroep toevoegt, wordt een nieuwe secundaire gemaakt in de secundaire groep.
  
  - **Failover-listener voor lezen / schrijven**

  Een DNS CNAME-record gemaakt die naar de huidige primaire URL verwijst. Hierdoor kan de SQL-toepassingen voor lezen / schrijven transparant opnieuw verbinding maken met de primaire database als de primaire gewijzigd na een failover.

  - **SQL Database-server DNS CNAME-record voor lezen / schrijven-listener**

     Op een SQL-Database-server, de DNS-CNAME-record voor de failovergroep die naar de huidige primaire URL verwijst heeft een onjuiste indeling als `failover-group-name.database.windows.net`.

  - **Beheerd exemplaar DNS CNAME-record voor lezen / schrijven-listener**

     In een Managed Instance, de DNS-CNAME-record voor de failovergroep die naar de huidige primaire URL verwijst heeft een onjuiste indeling als `failover-group-name.zone_id.database.windows.net`.

- **Failover-groep alleen-lezen-listener**

  Een DNS CNAME-record gemaakt die verwijst naar de alleen-lezen-listener die naar de URL van de secundaire server verwijst. Hierdoor kan de alleen-lezen SQL-toepassingen op transparante wijze verbinding maken met de secundaire server met behulp van de opgegeven regels voor taakverdeling.

  - **SQL Database-server DNS CNAME-record voor alleen-lezen-listener**

     Op een SQL-Database-server, de DNS-CNAME-record voor de alleen-lezen-listener die naar de URL van de secundaire verwijst is opgemaakt als `failover-group-name.secondary.database.windows.net`.

  - **Beheerd exemplaar DNS CNAME-record voor alleen-lezen-listener**

     In een Managed Instance, de DNS-CNAME-record voor de alleen-lezen-listener die naar de URL van de secundaire verwijst is opgemaakt als `failover-group-name.zone_id.database.windows.net`.

- **Automatische failover-beleid**

  Een failovergroep is standaard geconfigureerd met een automatische failover-beleid. De service SQL Database wordt de failover geactiveerd nadat de fout wordt gedetecteerd en de respijtperiode is verlopen. Het systeem moet verifiëren dat de onderbreking kan niet worden verholpen door de ingebouwde [infrastructuur met hoge beschikbaarheid van de service SQL Database](sql-database-high-availability.md) vanwege de schaal van de impact. Als u wilt voor het beheren van de werkstroom van de toepassing, kunt u automatische failover uitschakelen.

- **Alleen-lezen-failover-beleid**

  De failover van de listener voor de alleen-lezen is standaard uitgeschakeld. Het zorgt ervoor dat de prestaties van de primaire wordt niet negatief beïnvloed wanneer de secundaire offline is. Echter, het betekent ook dat de alleen-lezen-sessies wordt pas weer verbinding maken, totdat de secundaire server is hersteld. Als u downtime voor de alleen-lezen-sessies niet kan tolereren en OK tijdelijk de primaire te gebruiken voor zowel alleen-lezen als lezen / schrijven-verkeer ten koste van de afname van de mogelijke prestaties van de primaire, kunt u failover voor de alleen-lezen-listener inschakelen. In dat geval wordt het alleen-lezen-verkeer automatisch omgeleid naar de primaire als de secundaire server niet beschikbaar is.

- **Geplande failover**

   Geplande failover uitvoert volledige synchronisatie tussen primaire en secundaire databases voordat u de secundaire switches naar de primaire rol. Dit garandeert een zonder verlies van gegevens. Geplande failover wordt gebruikt in de volgende scenario's:

  - Herstelanalyses (DR) in productie uitvoeren wanneer het verlies van gegevens niet geaccepteerd wordt
  - Aan de databases te verplaatsen naar een andere regio
  - Retourneert de databases naar de primaire regio wanneer de onderbreking is verholpen (failback).

- **Niet-geplande failover**

   Niet-geplande of geforceerde failover wordt onmiddellijk de secundaire naar de primaire rol zonder een synchronisatie met de primaire verandert. Met deze bewerking leidt tot verlies van gegevens. Niet-geplande failover wordt gebruikt als een methode voor het herstellen tijdens storingen wanneer de primaire niet toegankelijk is. Als de oorspronkelijke primaire database weer online is, wordt automatisch opnieuw verbinding te maken zonder synchronisatie en een nieuwe secundaire geworden.

- **Handmatige failover**

  U kunt failover handmatig starten op elk gewenst moment, ongeacht de configuratie van de automatische failover. Als automatische failover-beleid niet is geconfigureerd, is handmatige failover vereist in de failovergroep naar de secundaire server-databases te herstellen. Geforceerde of beschrijvende failover (met volledige gegevenssynchronisatie), kunt u starten. De laatste kan worden gebruikt om aan de primaire te verplaatsen naar de secundaire regio. Wanneer failover is voltooid, worden de DNS-records automatisch bijgewerkt om te controleren of de verbinding met de nieuwe primaire

- **Respijtperiode verlies van gegevens**

  Omdat de primaire en secundaire databases worden gesynchroniseerd met behulp van asynchrone replicatie, kan de failover leiden tot verlies van gegevens. U kunt de automatische failover-beleid om de tolerantie van uw toepassing tot verlies van gegevens weer te geven. Door het configureren van **GracePeriodWithDataLossHours**, kunt u bepalen hoe lang het systeem moet wachten voordat u begint de failover die waarschijnlijk zal resultaat gegevens verloren gaan.

- **Meerdere failover-groepen**

  U kunt meerdere failover-groepen voor de dezelfde combinatie van servers voor het beheren van de schaal van failovers configureren. Elke groep failover-schakeling onafhankelijk van elkaar. Als uw toepassing met meerdere tenants maakt gebruik van elastische pools, kunt u deze mogelijkheid om primaire en secundaire databases in elke groep. Op deze manier die u de impact van een storing tot slechts de helft van de tenants beperken kunt.

  > [!IMPORTANT]
  > Beheerd exemplaar biedt geen ondersteuning voor meerdere failover-groepen.
  
## <a name="permissions"></a>Machtigingen
Machtigingen voor een failovergroep worden beheerd via [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md). De [Inzender voor SQL Server](../role-based-access-control/built-in-roles.md#sql-server-contributor) rol bevat alle benodigde machtigingen om failover-groepen te beheren. 

### <a name="create-failover-group"></a>Failover-groep maken
Als u wilt een failovergroep maakt, moet u toegang voor RBAC schrijven naar de primaire en secundaire servers, en tot alle databases in de failovergroep. Voor een beheerd exemplaar, moet u RBAC schrijftoegang heeft tot zowel de primaire en secundaire beheerd exemplaar, maar machtigingen voor afzonderlijke databases zijn niet relevant omdat beheerd exemplaar voor afzonderlijke databases kunnen niet worden toegevoegd aan of verwijderd uit een failovergroep. 

### <a name="update-a-failover-group"></a>Een failovergroep bijwerken
Voor het bijwerken van een failovergroep, moet u RBAC schrijftoegang tot de failovergroep en alle databases op de huidige primaire server of het beheerde exemplaar.  

### <a name="failover-a-failover-group"></a>Failover van een failovergroep
Voor een failover-groep met failover, moet RBAC schrijftoegang tot de failovergroep op de nieuwe primaire server of beheerd exemplaar. 

## <a name="best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools"></a>Aanbevolen procedures van het gebruik van failover-groepen met individuele databases en elastische pools

De automatische failover-groep moet worden geconfigureerd op de primaire SQL-Database-server en de App verbinden met de secundaire SQL-Database-server in een andere Azure-regio.  De groepen kunnen u alle of enkele databases opnemen in deze servers. Het volgende diagram illustreert een typische configuratie van een geografisch redundante cloudtoepassing met meerdere databases en automatische-failovergroep.

![automatische failover](./media/sql-database-auto-failover-group/auto-failover-group.png)

Bij het ontwerpen van een service met zakelijke continuïteit in gedachten, volgt u deze algemene richtlijnen:

- **Een of meerdere failovergroepen gebruiken voor het beheren van failovers van meerdere databases**

  Een of meer failover-groepen kunnen worden gemaakt tussen twee servers in verschillende regio's (primaire en secundaire servers). Elke groep kan een of meer databases die worden hersteld als een eenheid in het geval alle of een primaire databases niet beschikbaar vanwege een storing in de primaire regio bevatten. De failovergroep wordt geo-secundaire database gemaakt met de dezelfde servicedoelstelling als de primaire. Als u een bestaande geo-replicatie-relatie aan de failovergroep toevoegt, zorg er dan voor dat de geo-secundaire is geconfigureerd met de dezelfde servicelaag en de grootte van de compute als de primaire.

- **Gebruik van lezen / schrijven-listener voor OLTP-werkbelasting**

  Bij het uitvoeren van OLTP-bewerkingen, gebruik `failover-group-name.database.windows.net` als de server de URL en de verbindingen automatisch worden doorgestuurd naar de primaire. Deze URL wordt niet gewijzigd na de failover. Houd er rekening mee dat de failover omvat het bijwerken van die de DNS-record, zodat de clientverbindingen worden omgeleid naar de nieuwe primaire pas nadat de client-DNS-cache vernieuwd wordt.

- **Gebruik van alleen-lezen-listener voor de werkbelasting van alleen-lezen**

  Als u een logisch geïsoleerde alleen-lezen-werkbelasting die is gevoelig voor bepaalde veroudering van de gegevens hebt, kunt u de secundaire database in de toepassing. Gebruik voor alleen-lezen-sessies, `failover-group-name.secondary.database.windows.net` als de server de URL en de verbinding automatisch omgeleid naar de secundaire server. Het is ook raadzaam dat u aan te in de verbindingsreeks kunt u lezen wat lezen geven met behulp van **ApplicationIntent = alleen-lezen**.

- **Worden voorbereid voor verslechtering van prestaties**

  SQL failover beslissing is onafhankelijk van de rest van de toepassing of andere services die worden gebruikt. De toepassing kan worden "gemengde" met bepaalde onderdelen in één regio en sommige in een andere. Zorg ervoor dat de implementatie van de redundante toepassing in de DR-regio om te voorkomen dat de degradatie, en volgt u deze [richtlijnen voor beveiliging van het netwerk](#failover-groups-and-network-security).

  > [!NOTE]
  > De toepassing in de DR-regio heeft geen om een andere verbindingsreeks te gebruiken.  

- **Voorbereiden op het verlies van gegevens**

  Als er een storing wordt gedetecteerd, SQL wacht gedurende de periode die u hebt opgegeven door **GracePeriodWithDataLossHours**. De standaardwaarde is 1 uur. Als u geen enkele gegevens verloren gaan, zorg ervoor dat ingesteld **GracePeriodWithDataLossHours** op een groot genoeg getal, zoals 24 uur. Gebruik handmatige voor de failovergroep voor failback van de secundaire naar de primaire.

> [!IMPORTANT]
> Elastische pools met 800 of minder dtu's en meer dan 250 databases met behulp van geo-replicatie mogelijk problemen met inbegrip van meer geplande failovers en verminderde prestaties.  Deze problemen zijn vaker optreden voor intensieve werkbelastingen schrijven wanneer geo-replicatie-eindpunten zijn ver uit elkaar liggen op basis van Geografie, of wanneer er meerdere secundaire eindpunten worden gebruikt voor elke database.  Symptomen van deze problemen worden aangegeven wanneer de vertraging van geo-replicatie na verloop van tijd toeneemt.  Deze vertraging kan worden bewaakt met behulp van [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Als deze problemen optreden, zijn oplossingen verhogen van het aantal pool-dtu's of verlaag het aantal databases in dezelfde groep geo-replicatie.

## <a name="best-practices-of-using-failover-groups-with-managed-instances"></a>Aanbevolen procedures van het gebruik van failover-groepen met beheerde instanties

De automatische failover-groep moet worden geconfigureerd op de primaire instantie en verbindt dit met de secundaire-instantie in een andere Azure-regio.  Alle databases in het exemplaar wordt gerepliceerd naar de secundaire-exemplaar. Het volgende diagram illustreert een typische configuratie van een beheerd exemplaar en automatische-failovergroep met geografisch redundante cloudtoepassing.

![automatische failover](./media/sql-database-auto-failover-group/auto-failover-group-mi.png)

> [!IMPORTANT]
> Automatische failover-groepen voor Managed Instance is in openbare preview.

Als uw toepassing gebruikmaakt van beheerd exemplaar als de gegevenslaag, volgt u deze algemene richtlijnen bij het ontwerpen voor bedrijfscontinuïteit:

- **De secundaire exemplaar in de dezelfde DNS-zone als het primaire exemplaar maken**

  Wanneer een nieuw exemplaar wordt gemaakt, wordt een unieke id automatisch gegenereerd als de DNS-Zone en opgenomen in de DNS-naam van het exemplaar. Een meerdere domeinen (SAN)-certificaat voor dit exemplaar is ingericht met het SAN-veld in de vorm van `zone_id.database.windows.net`. Dit certificaat kan worden gebruikt voor verificatie van de clientverbindingen met een exemplaar in de dezelfde DNS-zone. Om ervoor te zorgen van niet-onderbroken verbinding met het primaire exemplaar na een failover zowel de primaire en secundaire moet exemplaren in dezelfde DNS-zone. Wanneer uw toepassing gereed voor productie-implementatie is, het maken van een secundair exemplaar in een andere regio en zorg ervoor dat de DNS-zone met het primaire exemplaar deelt. Dit wordt gedaan door op te geven een `DNS Zone Partner` optionele parameter met de Azure-portal, PowerShell of de REST-API.

  Zie voor meer informatie over het maken van de secundaire-exemplaar in de dezelfde DNS-zone als het primaire exemplaar [failover-groepen beheren met beheerde instanties (preview)](#powershell-managing-failover-groups-with-managed-instances-preview).

- **Replicatieverkeer tussen de twee exemplaren inschakelen**

  Omdat elk exemplaar is geïsoleerd in een eigen VNet, moet twee richtingen verkeer tussen deze VNets worden toegestaan. Zie [Azure VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)

- **Configureren van een failovergroep voor het beheren van failovers van volledige-exemplaar**

  De failovergroep beheert u de failover van alle databases in het exemplaar. Wanneer een groep is gemaakt, wordt elke database in het exemplaar worden automatisch geo-replicatie naar de secundaire-exemplaar. U kunt failover-groepen niet gebruiken voor het initiëren van een gedeeltelijke failover van een subset van de databases.

  > [!IMPORTANT]
  > Als een database is verwijderd uit de primaire instantie, deze ook verwijderd automatisch op de secundaire geo-instantie.

- **Gebruik van lezen / schrijven-listener voor OLTP-werkbelasting**

  Bij het uitvoeren van OLTP-bewerkingen, gebruik `failover-group-name.zone_id.database.windows.net` als de server de URL en de verbindingen automatisch worden doorgestuurd naar de primaire. Deze URL wordt niet gewijzigd na de failover. De failover omvat het bijwerken van de DNS-record, zodat de clientverbindingen worden omgeleid naar de nieuwe primaire pas nadat de client-DNS-cache vernieuwd wordt. Omdat de secundaire-exemplaar de DNS-zone met de primaire deelt, kunnen de clienttoepassing opnieuw tot stand te brengen met hetzelfde SAN-certificaat is mogelijk.

- **Rechtstreeks verbinding maken met secundaire geo-replicatie voor alleen-lezen query 's**

  Als u een logisch geïsoleerde alleen-lezen-werkbelasting die is gevoelig voor bepaalde veroudering van de gegevens hebt, kunt u de secundaire database in de toepassing. Gebruiken om rechtstreeks verbinding maken met de secundaire geo-replicatie, `server.secondary.zone_id.database.windows.net` als de server-URL en de verbinding rechtstreeks naar de secundaire geo-replicatie wordt gemaakt.

  > [!NOTE]
  > In bepaalde Servicelagen, Azure SQL Database ondersteunt het gebruik van [alleen-lezen replica's](sql-database-read-scale-out.md) laden saldo alleen-lezen querywerkbelastingen met behulp van de capaciteit van een alleen-lezen replica en het gebruik van de `ApplicationIntent=ReadOnly` parameter in de verbinding tekenreeks. Wanneer u een secundaire geo-replicatie hebt geconfigureerd, kunt u deze mogelijkheid verbinding maken met een alleen-lezen replica op de primaire locatie of in de geografisch gerepliceerde locatie.
  > - Gebruiken voor verbinding met een alleen-lezen replica op de primaire locatie, `failover-group-name.zone_id.database.windows.net`.
  > - Gebruiken voor verbinding met een alleen-lezen replica op de secundaire locatie, `failover-group-name.secondary.zone_id.database.windows.net`.

- **Worden voorbereid voor verslechtering van prestaties**

  SQL failover beslissing is onafhankelijk van de rest van de toepassing of andere services die worden gebruikt. De toepassing kan worden "gemengde" met bepaalde onderdelen in één regio en sommige in een andere. Zorg ervoor dat de implementatie van de redundante toepassing in de DR-regio om te voorkomen dat de degradatie, en volgt u deze [richtlijnen voor beveiliging van het netwerk](#failover-groups-and-network-security).

- **Voorbereiden op het verlies van gegevens**

  Als er een storing wordt gedetecteerd, SQL wordt lezen / schrijven-failover geactiveerd als er geen gegevens verloren gaan naar het beste van onze kennis. Anders, dat wordt gewacht totdat de periode die u hebt opgegeven door `GracePeriodWithDataLossHours`. Als u hebt opgegeven `GracePeriodWithDataLossHours`, worden voorbereid op verlies van gegevens. Tijdens onderbrekingen, omdat hiermee Azure in het algemeen beschikbaar. Als u geen enkele gegevens verloren gaan, zorg er dan voor dat GracePeriodWithDataLossHours ingesteld op een groot genoeg getal, zoals 24 uur.

  De DNS-update van de listener voor lezen / schrijven gebeurt onmiddellijk nadat de failover wordt gestart. Met deze bewerking wordt niet leiden tot verlies van gegevens. Het proces van het schakelen tussen databaserollen kan echter maximaal vijf minuten onder normale omstandigheden duren. Totdat deze is voltooid, wordt nog steeds enkele databases in de nieuwe primaire instantie worden alleen-lezen. Als failover wordt gestart met behulp van PowerShell zijn de gehele bewerking is synchroon. Als deze is gestart met behulp van de Azure portal wordt de gebruikersinterface van de voltooiingsstatus aangegeven. Als deze is gestart met behulp van de REST-API, gebruikt u standaard Azure Resource Manager de polling-mechanisme voor het bewaken van de functie voor naverwerkingsbeheer.

  > [!IMPORTANT]
  > Gebruik voor de failovergroep handmatige voorverkiezingen uit te brengen om terug te gaan naar de oorspronkelijke locatie. Wanneer de onderbreking waardoor de failover is verholpen, kunt u uw primaire databases verplaatsen naar de oorspronkelijke locatie. Hiervoor moet u de handmatige failover van de groep te starten.

## <a name="failover-groups-and-network-security"></a>Failover-groepen en -netwerkbeveiliging

Voor sommige toepassingen die de beveiligingsregels vereisen dat de netwerktoegang tot de gegevenslaag is beperkt tot een specifiek onderdeel of -onderdelen, zoals een virtuele machine, web service, enzovoort. Deze vereiste geeft enkele uitdagingen voor zakelijke continuïteit ontwerp en het gebruik van de failover-groepen. U moet rekening houden met de volgende opties bij het implementeren van dergelijke beperkte toegang.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Met behulp van failover-groepen en regels voor virtueel netwerk

Als u [Virtual Network-service-eindpunten en regels](sql-database-vnet-service-endpoint-rule-overview.md) om toegang te beperken tot uw SQL-database, houd er rekening mee dat elke Virtual Network service-eindpunt is van toepassing op slechts één Azure-regio. Het eindpunt is niet ingeschakeld voor andere regio's communicatie accepteert van het subnet. Daarom worden alleen de clienttoepassingen die zijn geïmplementeerd in dezelfde regio kunnen verbinden met de primaire database. Nadat de failover resulteert in de SQL-client-sessies omgeleid naar een server in een andere regio (secundair), mislukt deze sessies als afkomstig is van een client buiten deze regio. Om die reden kan niet het beleid automatische failover worden ingeschakeld als de deelnemende servers zijn opgenomen in de regels van het Virtueelnetwerk. Ter ondersteuning van handmatige failover, de volgende stappen uit:

1. Inrichten van de redundante exemplaren van de front-end-onderdelen van uw toepassing in de secundaire regio (webservice, virtuele machines enz.)
2. Configureer de [virtuele netwerkregels](sql-database-vnet-service-endpoint-rule-overview.md) afzonderlijk voor de primaire en secundaire server
3. Schakel de [front-failover met behulp van een configuratie van Traffic manager](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4. Handmatige failover geïnitieerd wanneer de onderbreking is gedetecteerd. Deze optie is geoptimaliseerd voor de toepassingen waarvoor consistente latentie tussen de front-end en de gegevenslaag en biedt ondersteuning voor herstel wanneer front-end, gegevenslaag of beide worden beïnvloed door de storing.

> [!NOTE]
> Als u de **alleen-lezen-listener** voor het verdelen van de werkbelasting van een alleen-lezen, zorg ervoor dat deze workload wordt uitgevoerd in een virtuele machine of een andere bron in de secundaire regio, zodat deze verbinding met de secundaire database maken kan.

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Met behulp van failover-groepen en firewallregels voor SQL database

Als uw bedrijfscontinuïteitsplan failover met behulp van groepen met automatische failover vereist, kunt u toegang tot uw SQL-database met behulp van de traditionele firewallregels beperken.  Ter ondersteuning van automatische failover, de volgende stappen uit:

1. [Een openbaar IP-adres maken](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)
2. [Maken van een openbare load balancer](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer) en het openbare IP-adres toewijzen aan deze.
3. [Maak een virtueel netwerk en de virtuele machines](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) voor uw front-end-onderdelen
4. [Netwerkbeveiligingsgroep maken](../virtual-network/security-overview.md) en binnenkomende verbindingen te configureren.
5. Zorg ervoor dat de uitgaande verbindingen geopend met Azure SQL database zijn met behulp van 'Sql' [servicetag](../virtual-network/security-overview.md#service-tags).
6. Maak een [SQL database-firewallregel](sql-database-firewall-configure.md) waarmee inkomend verkeer van het openbare IP-adres dat u in stap 1 hebt gemaakt.

Zie voor meer informatie over op het configureren van uitgaande toegang en welke IP-adres in de firewallregels [Load balancer uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md).

De bovenstaande configuratie zorgt ervoor dat de automatische failover wordt de verbindingen van de front-endcomponenten niet geblokkeerd en wordt ervan uitgegaan dat de toepassing de langere latentie tussen de front-end en de gegevenslaag kan tolereren.

> [!IMPORTANT]
> Om te garanderen van bedrijfscontinuïteit voor regionale storingen moet u ervoor zorgen dat geografische redundantie voor zowel de front-end-onderdelen en de databases.

## <a name="enabling-geo-replication-between-managed-instances-and-their-vnets"></a>Geo-replicatie tussen beheerde instanties en hun VNets inschakelen

Bij het instellen van een failover-groepen tussen primaire en secundaire beheerde instanties in twee verschillende regio's, wordt elk exemplaar is geïsoleerd met behulp van een onafhankelijke VNet. Als u wilt toestaan replicatieverkeer tussen deze vnet's volgen, zorg ervoor dat wordt deze voorwaarden voldaan:

1. De twee beheerde exemplaren moeten zich in verschillende Azure-regio's.
2. Uw secundaire moet leeg zijn (geen gebruikersdatabases).
3. De primaire en secundaire beheerde instanties moeten zich in dezelfde resourcegroep bevinden.
4. De vnet's die de beheerde-instanties deel uit van moet zijn verbonden maken via een [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md). Wereldwijde VNet-Peering wordt niet ondersteund.
5. De twee beheerd exemplaar VNets geen overlappende IP-adressen.
6. U moet het instellen van uw Netwerkbeveiligingsgroep groepen (NSG) dergelijke die 5022 poort en het bereik van 11000 ~ 12000 zijn open binnenkomende en uitgaande voor verbindingen van de andere beheerd exemplaar gestart subnet. Dit is om replicatieverkeer tussen de exemplaren

    > [!IMPORTANT]
    > Onjuist geconfigureerde NSG security regels leidt tot kopieerbewerkingen vastgelopen database.

7. U moet voor het configureren van DNS-zone partner op secundaire exemplaar. Een DNS-zone is een eigenschap van een beheerd exemplaar. Staat voor het deel van de hostnaam die volgt op de naam van de Managed Instance en voorafgaat aan de `.database.windows.net` voorvoegsel. Het wordt als willekeurige tekenreeks gegenereerd tijdens het maken van de eerste Managed Instance in elk VNet. De DNS-zone kan niet worden gewijzigd na het maken van een beheerd exemplaar en alle beheerde instanties binnen hetzelfde subnet delen dezelfde waarde voor de DNS-zone. Voor beheerd exemplaar voor failover instellen, moet de primaire Managed Instance en de secundaire Managed Instance dezelfde waarde voor de DNS-zone delen. U doet dit door de parameter DnsZonePartner op te geven bij het maken van de secundaire Managed Instance. De DNS-zone partner eigenschap definieert het beheerd exemplaar voor het delen van een failovergroep exemplaar met. Door te geven in de resource-id van een ander beheerd exemplaar als de invoer van DnsZonePartner, neemt het beheerde exemplaar wordt gemaakt over dezelfde DNS-zone-waarde van de partner voor het beheerde exemplaar.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Het upgraden of downgraden van een primaire database

U kunt upgraden en downgraden van een primaire database op een andere compute-grootte (in dezelfde servicelaag, niet tussen algemeen gebruik en bedrijfskritiek) zonder te verbreken alle secundaire databases. Bij een upgrade uitvoert, wordt het aanbevolen dat u eerst een upgrade alle secundaire databases uitvoert en werk vervolgens de primaire. Wanneer de Downgrade uitvoert, de volgorde omgekeerd: eerst downgraden van de primaire en vervolgens gebruik maken van alle secundaire databases. Wanneer u upgraden en downgraden van de database naar een andere service-laag, worden deze aanbeveling wordt afgedwongen.

Deze reeks wordt aanbevolen speciaal om te voorkomen dat het probleem waarbij de secundaire server op een lagere SKU wordt overbelast en moet opnieuw geseede tijdens een upgrade of downgrade. U kunt dit probleem ook voorkomen door de primaire ten koste van invloed op alle werkbelastingen voor lezen / schrijven op basis van de primaire alleen-lezen. 

> [!NOTE]
> Het verdient aanbeveling niet downgraden van de secundaire database als u een secundaire database als onderdeel van de configuratie van de failover gemaakt. Dit is om te controleren of dat uw gegevenslaag heeft onvoldoende capaciteit voor het verwerken van uw reguliere werkbelasting nadat failover is geactiveerd.

## <a name="preventing-the-loss-of-critical-data"></a>Zo wordt voorkomen dat het verlies van kritieke gegevens

Doorlopend kopiëren gebruikt vanwege de hoge latentie voor wide area network, een asynchroon replicatiemechanisme. Asynchrone replicatie maakt enig gegevensverlies onvermijdelijk als er een fout optreedt. Sommige toepassingen vereisen echter zonder verlies van gegevens. Ter bescherming van deze essentiële updates, een ontwikkelaar kan aanroepen de [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) system procedure onmiddellijk na het doorvoeren van de transactie. Aanroepen van **sp_wait_for_database_copy_sync** de aanroepende thread geblokkeerd totdat de laatst vastgelegde transactie is verzonden naar de secundaire database. Echter, het wacht niet totdat de verzonden transacties worden opnieuw afgespeeld en doorgevoerd op de secundaire server. **sp_wait_for_database_copy_sync** is afgestemd op de koppeling van een specifieke doorlopend kopiëren. Elke gebruiker met de rechten van de verbinding met de primaire database, kunt deze procedure aanroept.

> [!NOTE]
> **sp_wait_for_database_copy_sync** wordt voorkomen dat gegevens verloren gaan na een failover, maar is geen garantie voor volledige synchronisatie voor leestoegang. De vertraging is veroorzaakt door een **sp_wait_for_database_copy_sync** procedureaanroep kan aanzienlijk zijn en is afhankelijk van de grootte van het transactielogboek op het moment van de aanroep.

## <a name="failover-groups-and-point-in-time-restore"></a>Failover-groepen maken en terugzetten van de punt-in-time

Zie voor meer informatie over het gebruik van point in time restore met failovergroepen [punt in tijd herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="programmatically-managing-failover-groups"></a>Failover-groepen programmatisch te beheren

Zoals eerder besproken automatische failover-groepen en actieve kan geo-replicatie ook worden beheerd via een programma met behulp van Azure PowerShell en de REST-API. De volgende tabellen beschrijven de reeks opdrachten die beschikbaar zijn. Actieve geo-replicatie bevat een set met Azure Resource Manager-API's voor beheer, met inbegrip van de [REST-API van Azure SQL Database](https://docs.microsoft.com/rest/api/sql/) en [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview). Deze API's vereisen het gebruik van resourcegroepen en ondersteuning voor beveiliging op basis van rollen (RBAC). Zie voor meer informatie over het implementeren van toegang tot rollen [toegangsbeheer](../role-based-access-control/overview.md).

### <a name="powershell-manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>PowerShell: Failover van de SQL-database met individuele databases en elastische pools beheren

| Cmdlet | Description |
| --- | --- |
| [New-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasefailovergroup) |Met deze opdracht wordt een failovergroep gemaakt en geregistreerd op de primaire en secundaire servers|
| [Remove-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqldatabasefailovergroup) | Hiermee verwijdert u de failovergroep van de server en verwijdert alle secundaire databases opgenomen in de groep |
| [Get-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Hiermee haalt de configuratie van de failover |
| [Set-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasefailovergroup) |Hiermee wijzigt u de configuratie van de failovergroep |
| [Switch-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/switch-azsqldatabasefailovergroup) | Triggers failover van de failovergroep naar de secundaire server |
| [Add-AzSqlDatabaseToFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/add-azsqldatabasetofailovergroup)|Een of meer databases toegevoegd aan een Azure SQL Database-failovergroep|
|  | |

> [!IMPORTANT]
> Zie voor een voorbeeld van een script, [configureren en failover een failovergroep voor één database](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md).
>

### <a name="powershell-managing-failover-groups-with-managed-instances-preview"></a>PowerShell: Failover-groepen beheren met beheerde instanties (preview)

#### <a name="install-the-newest-pre-release-version-of-powershell"></a>Installeer de nieuwste versie van de voorlopige versie van PowerShell

1. De PowerShellGet-module bijwerken naar 1.6.5 (of de nieuwste preview-versie). Zie [PowerShell voorbeeldsite](https://www.powershellgallery.com/packages/AzureRM.Sql/4.11.6-preview).

   ```PowerShell
      install-module PowerShellGet -MinimumVersion 1.6.5 -force
   ```

2. Voer de volgende opdrachten in een nieuwe PowerShell-venster:

   ```PowerShell
      import-module PowerShellGet
      get-module PowerShellGet #verify version is 1.6.5 (or newer)
      install-module azurerm.sql -RequiredVersion 4.5.0-preview -AllowPrerelease –Force
      import-module azurerm.sql
   ```

#### <a name="powershell-commandlets-to-create-an-instance-failover-group"></a>PowerShell-commandlets te maken van een failover-instantiegroep

| API | Description |
| --- | --- |
| New-AzSqlDatabaseInstanceFailoverGroup |Met deze opdracht wordt een failovergroep gemaakt en geregistreerd op de primaire en secundaire servers|
| Set-AzSqlDatabaseInstanceFailoverGroup |Hiermee wijzigt u de configuratie van de failovergroep|
| Get-AzSqlDatabaseInstanceFailoverGroup |Hiermee haalt de configuratie van de failover|
| Switch-AzSqlDatabaseInstanceFailoverGroup |Triggers failover van de failovergroep naar de secundaire server|
| Remove-AzSqlDatabaseInstanceFailoverGroup | Hiermee verwijdert u een failovergroep|

### <a name="rest-api-manage-sql-database-failover-groups-with-single-and-pooled-databases"></a>REST-API: SQL database failover-groepen met één en gepoolde databases beheren

| API | Description |
| --- | --- |
| [Maken of bijwerken van de Failovergroep](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | Hiermee maken of bijwerken van een failovergroep |
| [Failover-groep verwijderen](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | Hiermee verwijdert u de failovergroep van de server |
| [Failover (gepland)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | Failover-schakeling van de huidige primaire server naar deze server. |
| [Geforceerde Failover verlies van gegevens toestaan](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) |ails via van de huidige primaire server naar deze server. Met deze bewerking kan leiden tot verlies van gegevens. |
| [Get-Failovergroep](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | Hiermee haalt u een failovergroep. |
| [Lijst met Failover-groepen door Server](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | Geeft een lijst van de failover-groepen in een server. |
| [Failover-groep bijwerken](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | Werkt een failovergroep. |
|  | |

### <a name="rest-api-manage-failover-groups-with-managed-instances-preview"></a>REST-API: Failover-groepen beheren met beheerde instanties (preview)

| API | Description |
| --- | --- |
| [Maken of bijwerken van de Failovergroep](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/createorupdate) | Hiermee maken of bijwerken van een failovergroep |
| [Failover-groep verwijderen](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/delete) | Hiermee verwijdert u de failovergroep van de server |
| [Failover (gepland)](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/failover) | Failover-schakeling van de huidige primaire server naar deze server. |
| [Geforceerde Failover verlies van gegevens toestaan](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/forcefailoverallowdataloss) |ails via van de huidige primaire server naar deze server. Met deze bewerking kan leiden tot verlies van gegevens. |
| [Get-Failovergroep](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/get) | Hiermee haalt u een failovergroep. |
| [Lijst met Failovergroepen - lijst per locatie](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/listbylocation) | Hier worden de failovergroepen op een locatie. |

## <a name="next-steps"></a>Volgende stappen

- Zie voor voorbeelden van scripts:
  - [PowerShell gebruiken voor het configureren van actieve geo-replicatie voor één Azure SQL-database](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [PowerShell gebruiken voor het configureren van actieve geo-replicatie voor een gepoolde Azure SQL-database](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [Een failovergroep configureren en een failover uitvoeren voor een individuele database](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md)
- Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor herstel, [een database herstellen vanuit back-ups service geïnitieerde](sql-database-recovery-using-backups.md).
- Zie voor meer informatie over vereisten voor een nieuwe primaire server en database authentication, [SQL Database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md).
