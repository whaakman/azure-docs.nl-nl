---
title: Veelgestelde vragen over het gebruik van de migratie van Azure databaseservice | Microsoft Docs
description: Meer informatie over veelgestelde vragen over het gebruik van de migratie van Azure databaseservice migraties database uitvoeren.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/25/2018
ms.openlocfilehash: 87bd27147d20fec8c5839b744d70f215e2c1ec47
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="faq-about-using-the-azure-database-migration-service"></a>Veelgestelde vragen over het gebruik van de Service Azure Database migreren
Dit artikel worden enkele veelgestelde vragen over het gebruik van de Service Azure Database migratie samen met verwante antwoorden.

### <a name="q-what-is-azure-database-migration-service"></a>Q. Wat is Azure Database migratieservice?
De Azure-Service voor het migreren van Database is een volledig beheerde service die is ontworpen om in te schakelen naadloze migraties uit meerdere databasebronnen naar Azure Data platforms met minimale downtime. De service is momenteel in de openbare Preview met ontwikkelingsinspanningen is gericht op:
- Betrouwbaarheid en prestaties.
- Iteratieve toevoeging van de bron-doel-paren.
- Blijvende investeringen in wrijving gratis migraties.

### <a name="q-what-source-target-pairs-does-the-azure-database-migration-service-currently-support"></a>Q. Welke paren bron doel de Service Azure Database migratie ondersteunt momenteel?
De service in de openbare Preview ondersteunt momenteel migraties van SQL Server naar Azure SQL Database en gaat u naar de Azure-portal nu aan de slag met de Azure-Service voor het migreren van Database voor dit scenario. Andere paren bron doel, zoals SQL Server naar Azure SQL Database-beheerde exemplaar en Oracle naar Azure SQL Database zijn beschikbaar via een beperkte Private Preview. Aanmelden voor de gelegenheid om deel te nemen in de beperkte Private Preview van deze scenario's [hier](https://sqldatabase-migrationpreview.azurewebsites.net/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-other-microsoft-database-migration-tools-such-as-the-database-migration-assistant-dma-or-sql-server-migration-assistant-ssma"></a>Q. Hoe Azure Database migratie Service vergelijken met andere Microsoft-database hulpprogramma's voor migratie zoals Database migratie-assistent (DMA) of SQL Server Migration Assistant (SSMA)?
De Azure-Service voor het migreren van Database is de voorkeursmethode voor databasemigratie naar Microsoft Azure op schaal. Voor meer informatie over hoe de Azure-Service voor het migreren van Database met andere Microsoft vergelijkt-hulpprogramma's voor migratie van de database en voor aanbevelingen voor het gebruik van de service voor verschillende scenario's, Zie het blog boeken [verschillen Microsoft Database Hulpprogramma's voor migratie en Services](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-the-azure-migrate-offering"></a>Q. Hoe Azure Database migratie Service vergelijken met het migreren van de Azure-aanbod?
De Service voor het migreren van Azure helpt bij de migratie van on-premises virtuele machines naar Azure IaaS. De service beoordeelt de geschiktheid van de migratie en het formaat op basis van prestaties en kosten schattingen voor het uitvoeren van uw on-premises virtuele machines in Azure biedt. Azure migreren is nuttig voor lift en shift migraties van on-premises werkbelastingen op basis van VM virtuele Azure IaaS-machines. Echter, in tegenstelling tot de Azure-Database migratie-Service Azure migreren is niet een gespecialiseerde migratie databaseservice aanbieding voor Azure PaaS relationele database-platforms zoals Azure SQL Database of SQL Azure of Azure SQL Database-beheerd instantie.

### <a name="q-what-is-a-summary-of-the-steps-required-to-use-the-azure-database-migration-service-to-perform-a-database-migration"></a>Q. Wat is een overzicht van de vereiste stappen voor het gebruik van de Service van Azure Database migratie uitvoeren van de databasemigratie van een?
Tijdens een migratie typische, eenvoudige database u:
1.  Maak een doel-databases.
2.  Het schema databases migreren met behulp van de [Database migratie-assistent](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
3.  Geen exemplaar maken van de Service Azure Database migratie.
4.  Maak een geven de bron-databases, doel-databases en tabellen voor het migreren van migratieproject.
5.  Start de volledige belasting.
6.  Kies de volgende validatie.
7.  Voer een handmatige overschakeling van uw productieomgeving naar de nieuwe cloud-gebaseerde database. 

### <a name="q-what-are-the-prerequisites-for-using-the-azure-database-migration-service"></a>Q. Wat zijn de vereisten voor het gebruik van de Service Azure Database migreren?
Er zijn verschillende vereisten die zijn vereist om ervoor te zorgen dat de Service Azure Database migratie probleemloos bij het uitvoeren van de database-migraties. Sommige van de vereisten van toepassing in alle scenario's (bron-doel paren) die door de service, worden ondersteund, terwijl andere vereisten uniek voor een specifiek scenario zijn.
Vereisten voor Azure migratie databaseservice die voor alle ondersteunde migratiescenario's gelden zijn onder andere het:
- Een VNET maken voor de Azure-Service voor het migreren van Database met behulp van het Azure Resource Manager-implementatiemodel, waardoor site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Netwerkbeveiligingsgroep regels komen de volgende communicatie niet blokkeren 443, 53, 9354 poort, 445, 12000. Zie het artikel voor meer informatie over het Azure VNET NSG wordt verkeer gefilterd [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Wanneer u een firewallapparaat voor uw databases bron, moet u wellicht toevoegen van firewallregels zodat de Service Azure Database migratie voor toegang tot de bron-databases voor migratie.
 
Zie voor een lijst van alle vereisten die zijn vereist voor het specifieke migratiescenario's met behulp van de migratie van Azure databaseservice concurreren, de verwante zelfstudies in de Azure-Service voor het migreren van Database [documentatie](https://docs.microsoft.com/en-us/azure/dms/dms-overview) op Docs.Microsoft.com.

### <a name="q-how-do-i-find-the-ip-address-for-the-azure-database-migration-service-so-that-i-can-create-an-allow-list-for-the-firewall-rules-used-to-access-my-source-database-for-migration"></a>Q. Hoe vind ik het IP-adres voor de Azure-Service voor het migreren van Database zodat ik kan een lijst voor de firewall-regels gebruikt voor toegang tot mijn brondatabase voor migratie maken?
U moet wellicht toevoegen van firewallregels zodat de Service Azure Database migratie tot toegang tot de brondatabase voor migratie. Het IP-adres voor de service dynamisch is, maar als u Express Route gebruikt, wordt dit adres privé toegewezen door uw bedrijfsnetwerk. De eenvoudigste manier om het juiste IP-adres zoeken in dezelfde resourcegroep als uw ingerichte Azure-databaseservice migratie resource te vinden van de netwerkinterface gekoppelde identificeren. De naam van de bron van de netwerkinterface wordt meestal begint met het voorvoegsel NIC en gevolgd door een unieke teken en het volgnummer, voorbeeld NIC jj6tnztnmarpsskr82rbndyp. Als u deze interface netwerkbron selecteert, ziet u het IP-adres dat moet worden opgenomen in de lijst op de resource-overzicht Azure portal-pagina.

U moet mogelijk ook de bron van de poort die SQL Server naar de lijst met toegestane luistert opnemen. Standaard is poort 1433, maar de bron van SQL Server kan worden geconfigureerd om te luisteren op ook andere poorten. In dit geval moet u deze poorten op de lijst ook opnemen. U kunt de poort die SQL Server luistert naar een dynamische Beheerweergave query bepalen:

```sql
    SELECT DISTINCT 
        local_tcp_port 
    FROM sys.dm_exec_connections 
    WHERE local_tcp_port IS NOT NULL
```
U kunt ook de poort die SQL Server luistert door in het foutenlogboek van SQL Server te bepalen:

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on' 
    GO
```

### <a name="q-are-there-any-recommendations-for-optimizing-the-performance-of-the-azure-database-migration-service"></a>Q. Zijn er geen aanbevelingen voor het optimaliseren van de prestaties van de Service Azure Database migreren?
U kunt enkele doen om de databasemigratie van uw met de service te versnellen:
- De multi CPU algemeen doel prijscategorie gebruiken bij het maken van uw service-exemplaar zodat de service om te profiteren van meerdere vcpu's voor garandeert en snellere overdracht van gegevens.
- Tijdelijk opschaling van de doelinstantie van uw Azure SQL Database aan de laag Premium SKU tijdens de bewerking van de migratie gegevens minimaliseren Azure SQL Database beperking die mogelijk van invloed zijn activiteiten van bestandsoverdracht gegevens bij gebruik van lager niveau SKU's.

### <a name="q-how-do-i-set-up-an-azure-virtual-network"></a>Q. Hoe stel ik een Azure Virtual Network
Bij meerdere Microsoft-zelfstudies die u u bij het proces helpt kunnen voor het instellen van een Azure-VNET, de officiële documentatie wordt weergegeven in het artikel [Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview).

### <a name="q-where-can-i-leave-feedback-about-the-azure-database-migration-service"></a>Q. Waar kan ik feedback geven over de migratie van Azure databaseservice?
Wij willen graag van u. Stuur alle feedback en / ideeën hebt over de Service Azure Database migratie via User Voice [hier](https://feedback.azure.com/forums/906100-azure-database-migration-service).

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van de migratie van Azure databaseservice en regionale beschikbaarheid tijdens Public Preview [wat is Azure Database migratie Service Preview](dms-overview.md). 