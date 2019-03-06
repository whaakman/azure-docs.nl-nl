---
title: Veelgestelde vragen over het gebruik van de Azure Database Migration Service | Microsoft Docs
description: Meer informatie over veelgestelde vragen over het gebruik van de Azure Database Migration Service om uit te voeren van de databasemigraties.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/05/2019
ms.openlocfilehash: f66babf5ee72876692a5ae1b371f811534db6875
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57452323"
---
# <a name="faq-about-using-the-azure-database-migration-service"></a>Veelgestelde vragen over het gebruik van de Azure Database Migration Service
In dit artikel geeft een lijst met veelgestelde vragen over het gebruik van de Azure Database Migration Service, samen met verwante antwoorden.

### <a name="q-what-is-azure-database-migration-service"></a>V. Wat is Azure Database Migration Service?
Azure Database Migration Service is een volledig beheerde service die is ontworpen om in te schakelen naadloze migratie van meerdere databasebronnen naar Azure Data platforms met minimale downtime. De service is momenteel algemeen beschikbaar, met continue ontwikkelingsinspanningen gericht op:
- Betrouwbaarheid en prestaties.
- Iteratieve toevoeging van bron-doelparen.
- Voortdurende investeringen in migraties te leren kennen.

### <a name="q-what-source-target-pairs-does-the-azure-database-migration-service-currently-support"></a>V. Welke bron-doelparen Azure Database Migration Service momenteel ondersteunt?
De service ondersteunt momenteel een verscheidenheid aan scenario's voor migratie. Zie het artikel voor een volledige lijst met de status van elke beschikbare migratiescenario, [Status van het migratiescenario's ondersteund door de Azure Database Migration Service](https://docs.microsoft.com/azure/dms/resource-scenario-status). Andere scenario's voor migratie zijn beperkte Preview-versie en vereisen een nominatie via de DMS-Preview-site verzenden. Zie voor een volledige lijst van de scenario's in de beperkte Preview-versie en om u te registreren om deel te nemen in een van deze aanbiedingen, de [DMS voorbeeldsite](https://aka.ms/dms-preview/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-other-microsoft-database-migration-tools-such-as-the-database-migration-assistant-dma-or-sql-server-migration-assistant-ssma"></a>V. Hoe laat de Azure Database Migration Service zich vergelijken met andere Microsoft hulpprogramma's voor databasemigratie, zoals de Database Migration Assistant (DMA) of SQL Server Migration Assistant (SSMA)?
Azure Database Migration Service is de voorkeursmethode voor databasemigratie naar Microsoft Azure op schaal. Voor meer informatie over hoe de Azure Database Migration Service met andere Microsoft vergelijkt-hulpprogramma's voor migratie van de database en voor aanbevelingen over het gebruik van de service voor verschillende scenario's, Zie het blog-boeken [differentiëren de Database van Microsoft Hulpprogramma's voor migratie en Services](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-the-azure-migrate-offering"></a>V. Hoe verhoudt Azure Database Migration Service zich op de aanbieding Azure Migrate?
De Service Azure Migrate helpt bij de migratie van on-premises virtuele machines naar Azure IaaS. De service beoordeelt de geschiktheid voor migratie en prestatie gebaseerde schaling en kostenramingen voor het uitvoeren van uw on-premises virtuele machines in Azure biedt. Azure Migrate is handig voor lift-and-shift-migraties van on-premises workloads naar Azure IaaS VM's op basis van een virtuele machine. Echter, in tegenstelling tot de Azure Database Migration Service, Azure Migrate is niet een gespecialiseerde database Migratieservice voor Azure PaaS relationele database-platforms zoals Azure SQL Database of SQL Azure of Azure SQL Database Managed Instance biedt.

### <a name="q-what-versions-of-sql-server-does-the-azure-database-migration-service-support-as-a-source"></a>V. Welke versies van SQL Server ondersteunt de Azure Database Migration Service als een bron?
Bij het migreren van SQL Server, de Azure Database Migration Service biedt ondersteuning voor SQL Server 2005 via SQL Server 2017.

### <a name="q-what-is-a-summary-of-the-steps-required-to-use-the-azure-database-migration-service-to-perform-a-database-migration"></a>V. Wat is een overzicht van de vereiste stappen voor het gebruik van de Azure Database Migration Service om uit te voeren van de migratie van een database?
Tijdens een typische, eenvoudige databasemigratie u:
1.  Maak een doel-database (s).
2.  De database (s)-schema migreren met behulp van de [Database Migration Assistant](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
3.  De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
4.  Een migratieproject opgeven van de brondatabase (s), doel-database (s) en te migreren tabellen maken.
5.  Start de volledige belasting.
6.  Kies de volgende validatie.
7.  Voer een handmatige overschakeling van de productie-omgeving naar de nieuwe cloud-gebaseerde database. 

### <a name="q-what-are-the-prerequisites-for-using-the-azure-database-migration-service"></a>V. Wat zijn de vereisten voor het gebruik van de Azure Database Migration Service?
Er zijn verschillende vereisten die zijn vereist om ervoor te zorgen dat de Azure Database Migration Service probleemloos werkt bij het uitvoeren van de databasemigraties. Sommige van de vereisten van toepassing in alle scenario's (bron-doelparen) die wordt ondersteund door de service, terwijl andere vereiste onderdelen uniek voor een specifiek scenario zijn.
Vereisten voor Azure Database Migration Service die betrekking hebben op alle ondersteunde migratiescenario's zijn onder andere het:
- Maak een VNET voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Dit geeft site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Network Security Group-regels de volgende communicatiepoorten niet blokkeren: 443, 53, 9354, 445, 12000. Zie voor meer informatie over verkeer filteren van Azure VNET NSG het artikel [Netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor de Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.
 
Zie voor een lijst van alle vereisten voor migratie van specifieke scenario's met behulp van de Azure Database Migration Service deelname, de gerelateerde zelfstudies in de Azure Database Migration Service [documentatie](https://docs.microsoft.com/azure/dms/dms-overview) op Docs.Microsoft.com.

### <a name="q-how-do-i-find-the-ip-address-for-the-azure-database-migration-service-so-that-i-can-create-an-allow-list-for-the-firewall-rules-used-to-access-my-source-database-for-migration"></a>V. Hoe vind ik het IP-adres voor de Azure Database Migration Service zodat ik kan een acceptatielijst voor de firewall-regels gebruikt voor toegang tot mijn brondatabase voor migratie maken?
U wilt toevoegen van firewallregels zodat de Azure Database Migration Service toegang tot uw brondatabase voor migratie. Het IP-adres voor de service dynamisch is, maar als u Expressroute gebruikt, wordt dit adres privé toegewezen door uw bedrijfsnetwerk. De eenvoudigste manier om het juiste IP-adres, dat het is om te zoeken in dezelfde resourcegroep als uw ingerichte Azure Database Migration Service-resource te vinden van de bijbehorende netwerkinterface identificeren. Doorgaans de naam van de resource van de netwerkinterface begint met het voorvoegsel van de NIC en gevolgd door een unieke teken en volgnummer, bijvoorbeeld NIC-jj6tnztnmarpsskr82rbndyp. Als u dit netwerkinterface resource selecteert, ziet u het IP-adres dat moet worden opgenomen in de acceptatielijst op de resource-overzicht van Azure portal-pagina.

U moet mogelijk ook om op te nemen van de bron van de poort die SQL Server op de acceptatielijst luistert. Standaard's deze poort 1433, maar de bron die SQL Server kan worden geconfigureerd om te luisteren op ook andere poorten. In dit geval moet u deze poorten op de lijst ook opnemen. U kunt de poort op die SQL Server luistert op met behulp van een dynamische Beheerweergave query bepalen:

```sql
    SELECT DISTINCT 
        local_tcp_port 
    FROM sys.dm_exec_connections 
    WHERE local_tcp_port IS NOT NULL
```
U kunt ook de poort op die SQL Server luistert door het opvragen van de SQL Server-foutenlogboek bepalen:

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on' 
    GO
```

### <a name="q-are-there-any-recommendations-for-optimizing-the-performance-of-the-azure-database-migration-service"></a>V. Zijn er geen aanbevelingen voor het optimaliseren van de prestaties van de Azure Database Migration Service?
U kunt een aantal dingen om de databasemigratie van uw met behulp van de service sneller te doen:
- Gebruik de meerdere CPU prijzen categorie voor algemeen gebruik bij het maken van uw service-exemplaar zodat de service om te profiteren van meerdere vcpu's voor parallelle uitvoering en snellere overdracht van gegevens.
- Tijdelijk schaal van uw Azure SQL Database-doelexemplaar naar de laag Premium SKU tijdens de bewerking van de migratie van gegevens te minimaliseren, Azure SQL Database-beperking die mogelijk van invloed op activiteiten van bestandsoverdracht gegevens bij het gebruik van lager niveau SKU's.

### <a name="q-how-do-i-set-up-an-azure-virtual-network"></a>V. Hoe stel ik een Azure Virtual Network
Tijdens het meerdere Microsoft-zelfstudies die u bij het proces helpen kunnen van het instellen van een Azure-VNET, de officiële documentatie wordt weergegeven in het artikel [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

### <a name="q-why-is-my-azure-database-migration-service-unavailable-or-stopped"></a>V. Waarom wordt mijn Azure Database Migration Service niet beschikbaar of is gestopt?
Als de gebruiker expliciet het Azure Database Migration Service (DMS stopt) of als de service niet actief gedurende een periode van 24 uur is, wordt de service zich in een gestopte of automatisch de status onderbroken ingeschakeld. In elk geval wordt is de service niet beschikbaar en in een gestopte status.  Als u wilt hervatten active migraties, door de service opnieuw te starten.

### <a name="q-where-can-i-leave-feedback-about-the-azure-database-migration-service"></a>V. Waar kan ik feedback geven over de Azure Database Migration Service?
We horen graag van u. Stuur alle feedback en / ideeën die u hebt over de Azure Database Migration Service via User Voice, [hier](https://feedback.azure.com/forums/906100-azure-database-migration-service), of neem contact op met het team via [Azure Databasemigraties vragen](mailto:AskAzureDatabaseMigrations@service.microsoft.com).

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van de Azure Database Migration Service en de regionale beschikbaarheid [wat is de Azure Database Migration Service](dms-overview.md). 
