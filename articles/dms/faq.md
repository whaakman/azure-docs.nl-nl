---
title: Veelgestelde vragen over het gebruik van de Azure Database Migration Service | Microsoft Docs
description: Meer informatie over veelgestelde vragen over het gebruik van Azure Database Migration Service om uit te voeren van de databasemigraties.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 07/10/2019
ms.openlocfilehash: 5077539f6f80784f865bd4c1b52e3b4c147107ed
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717999"
---
# <a name="faq-about-using-azure-database-migration-service"></a>Veelgestelde vragen over het gebruik van Azure Database Migration Service

In dit artikel geeft een lijst met veelgestelde vragen over het gebruik van Azure Database Migration Service, samen met verwante antwoorden.

## <a name="overview"></a>Overzicht

**Q. Wat is Azure Database Migration Service?**
Azure Database Migration Service is een volledig beheerde service die is ontworpen om in te schakelen naadloze migratie van meerdere databasebronnen naar Azure Data platforms met minimale downtime. De service is momenteel algemeen beschikbaar, met continue ontwikkelingsinspanningen gericht op:

* Betrouwbaarheid en prestaties.
* Iteratieve toevoeging van bron-doelparen.
* Voortdurende investeringen in migraties te leren kennen.

**Q. Welke bron-/ doelparen Azure Database Migration Service ondersteunt momenteel?**
De service ondersteunt momenteel tal van bron-/ doelparen of migratiescenario's. Zie het artikel voor een volledige lijst met de status van elke beschikbare migratiescenario, [Status van het migratiescenario's ondersteund door de Azure Database Migration Service](https://docs.microsoft.com/azure/dms/resource-scenario-status).

Andere scenario's voor migratie zijn beschikbaar als preview en vereist een nominatie via de DMS-Preview-site verzenden. Zie voor een volledige lijst van de scenario's in de Preview-versie en om u te registreren om deel te nemen in een van deze aanbiedingen, de [DMS voorbeeldsite](https://aka.ms/dms-preview/).

**Q. Welke versies van SQL Server ondersteunt Azure Database Migration Service als een bron?**
Bij het migreren van SQL Server, zijn de ondersteunde gegevensbronnen voor Azure Database Migration Service SQL Server 2005 via SQL Server 2017.

**V: Wanneer u Azure Database Migration Service, wat is het verschil tussen een offline en een online migratie?**
Azure Database Migration Service kunt u offline en online migraties uitvoeren. Met een *offline* migratie, downtime van toepassingen wordt gestart wanneer de migratie begint. Met een *online* migratie uitvaltijd is beperkt tot de tijd voor het overzetten van aan het einde van de migratie. We raden u aan eerst een offlinemigratie te testen om te bepalen of de downtime aanvaardbaar is. Zo niet, voer dan een onlinemigratie uit.

> [!NOTE]
> Met Azure Database Migration Service voor het uitvoeren van een online migratie vereist het maken van een exemplaar op basis van de prijscategorie Premium. Zie de pagina met [prijzen](https://azure.microsoft.com/pricing/details/database-migration/) van Azure Database Migration Service voor meer informatie.

**Q. Hoe laat Azure Database Migration Service zich vergelijken met andere Microsoft hulpprogramma's voor databasemigratie, zoals de Database Migration Assistant (DMA) of SQL Server Migration Assistant (SSMA)?**
Azure Database Migration Service is de voorkeursmethode voor databasemigratie naar Microsoft Azure op schaal. Voor meer informatie over hoe Azure Database Migration Service met andere Microsoft vergelijkt-hulpprogramma's voor migratie van de database en voor aanbevelingen over het gebruik van de service voor verschillende scenario's, Zie het blog-boeken [differentiëren Microsofts databasemigratie Hulpprogramma's en Services](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

**Q. Hoe verhoudt Azure Database Migration Service zich op de aanbieding Azure Migrate?**
Azure Migrate helpt bij de migratie van on-premises virtuele machines naar Azure IaaS. De service beoordeelt de geschiktheid voor migratie en prestatie gebaseerde schaling en kostenramingen voor het uitvoeren van uw on-premises virtuele machines in Azure biedt. Azure Migrate is handig voor lift-and-shift-migraties van on-premises workloads naar Azure IaaS VM's op basis van een virtuele machine. Echter, in tegenstelling tot de Azure Database Migration Service, Azure Migrate is niet een gespecialiseerde database Migratieservice voor Azure PaaS relationele database-platforms zoals Azure SQL Database of Azure SQL Database Managed Instance biedt.

## <a name="setup"></a>Instellen

**Q. Wat zijn de vereisten voor het gebruik van Azure Database Migration Service?**
Er zijn verschillende vereisten die zijn vereist om ervoor te zorgen dat Azure Database Migration Service probleemloos werkt bij het uitvoeren van de databasemigraties. Sommige van de vereisten van toepassing in alle scenario's (bron-doelparen) die wordt ondersteund door de service, terwijl andere vereiste onderdelen uniek voor een specifiek scenario zijn.

Vereisten voor Azure Database Migration Service die betrekking hebben op alle ondersteunde migratiescenario's zijn onder andere het:

* Een VNet maken voor Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [ VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Zorg ervoor dat uw Azure Virtual Network (VNet) netwerkbeveiligingsgroepsregels blokkeren niet de volgende communicatiepoorten 443, 53, 9354, 445, 12000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Wanneer u een apparaat voor een firewall voor de brondatabase (s), moet u mogelijk toevoegen van firewallregels om toe te staan van Azure Database Migration Service voor toegang tot de brondatabase (s) voor de migratie.

Zie voor een lijst van alle vereisten voor migratie van specifieke scenario's met behulp van Azure Database Migration Service deelname, de gerelateerde zelfstudies in de Azure Database Migration Service [documentatie](https://docs.microsoft.com/azure/dms/dms-overview) op docs.microsoft.com.

**Q. Hoe vind ik het IP-adres voor Azure Database Migration Service zodat ik kan een acceptatielijst voor de firewall-regels gebruikt voor toegang tot mijn brondatabase voor migratie maken?**
U wilt toevoegen van firewallregels zodat Azure Database Migration Service toegang tot uw brondatabase voor migratie. Het IP-adres voor de service dynamisch is, maar als u Expressroute gebruikt, wordt dit adres privé toegewezen door uw bedrijfsnetwerk. De eenvoudigste manier om te identificeren van het juiste IP-adres is om te zoeken in dezelfde resourcegroep als uw ingerichte Azure Database Migration Service-resource te vinden van de netwerkinterface gekoppeld. Doorgaans de naam van de resource van de netwerkinterface begint met het voorvoegsel van de NIC en gevolgd door een unieke teken en volgnummer, bijvoorbeeld NIC-jj6tnztnmarpsskr82rbndyp. Als u dit netwerkinterface resource selecteert, ziet u het IP-adres dat moet worden opgenomen in de acceptatielijst op de resource-overzicht van Azure portal-pagina.

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

**Q. Hoe stel ik een Azure Virtual Network**
Tijdens het meerdere Microsoft-zelfstudies die u bij het proces helpen kunnen van het instellen van een Azure-VNET, de officiële documentatie wordt weergegeven in het artikel [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

## <a name="usage"></a>Gebruik

**Q. Wat is een overzicht van de vereiste stappen voor het gebruik van Azure Database Migration Service om uit te voeren van de migratie van een database?**
Tijdens een typische, eenvoudige databasemigratie u:

1. Maak een doel-database (s).
2. Evalueer uw brondatabase (s).
    * Voor migraties van homogene, beoordeling van uw bestaande database (s) met behulp van [DMA](https://www.microsoft.com/download/details.aspx?id=53595).
    * Voor heterogene migraties (van volledige-bronnen), beoordeling van uw bestaande database (s) met [SSMA](https://aka.ms/get-ssma). U kunt ook SSMA gebruiken om te converteren van objecten in de database en het schema migreren naar uw doelplatform.
3. Maak een instantie van Azure Database Migration Service.
4. Maak een migratieproject opgeven van de brondatabase (s), doel-database (s) en de tabellen die u wilt migreren.
5. Start de volledige belasting.
6. Kies de volgende validatie.
7. Voer een handmatige overschakeling van de productie-omgeving naar de nieuwe cloud-gebaseerde database.

## <a name="troubleshooting-and-optimization"></a>Oplossen van problemen en optimalisatie

**Q. Ik ben een migratieproject in DMS instellen en ik ondervind problemen bij het verbinding maken met mijn brondatabase. Wat moet ik doen?**
Als u problemen ondervindt bij het verbinding maken met uw bron-databasesysteem tijdens het werken voor migratie, moet u een virtuele machine maken in het VNet waarmee u uw DMS-instantie hebt ingesteld. In de virtuele machine, zou het mogelijk om uit te voeren van een test connect, zoals een UDL-bestand gebruiken om een verbinding met SQL Server te testen of het downloaden van Robo 3T als u wilt testen van de MongoDB-verbindingen. Als de verbindingstest is geslaagd, al dan niet mogen er een probleem is met het verbinding maken met uw brondatabase. Als de verbindingstest niet slaagt, neem dan contact op met uw netwerkbeheerder.

**Q. Waarom wordt mijn Azure Database Migration Service niet beschikbaar of is gestopt?**
Als de gebruiker expliciet Azure Database Migration Service (DMS stopt) of als de service niet actief gedurende een periode van 24 uur is, wordt de service zich in een gestopte of automatisch de status onderbroken ingeschakeld. In elk geval wordt is de service niet beschikbaar en in een gestopte status.  Als u wilt hervatten active migraties, door de service opnieuw te starten.

**Q. Zijn er geen aanbevelingen voor het optimaliseren van de prestaties van Azure Database Migration Service?**
U kunt een aantal dingen om de databasemigratie van uw met behulp van de service sneller te doen:

* Gebruik de meerdere CPU prijzen categorie voor algemeen gebruik bij het maken van uw service-exemplaar zodat de service om te profiteren van meerdere vcpu's voor parallelle uitvoering en snellere overdracht van gegevens.
* Tijdelijk schaal van uw Azure SQL Database-doelexemplaar naar de laag Premium SKU tijdens de bewerking van de migratie van gegevens te minimaliseren, Azure SQL Database-beperking die mogelijk van invloed op activiteiten van bestandsoverdracht gegevens bij het gebruik van lager niveau SKU's.

## <a name="next-steps"></a>Volgende stappen

Zie het artikel voor een overzicht van de Azure Database Migration Service en de regionale beschikbaarheid [wat is de Azure Database Migration Service](dms-overview.md).
