---
title: Overzicht van vereisten voor het gebruik van de migratie van Azure databaseservice | Microsoft Docs
description: Meer informatie over een overzicht van de vereisten voor het gebruik van de migratie van Azure databaseservice migraties database uitvoeren.
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
ms.openlocfilehash: a103bb4802bc4a20b6d82f0c6bb427133d9e5643
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Overzicht van vereisten voor het gebruik van de Service Azure Database migreren
Er zijn verschillende vereisten die zijn vereist om ervoor te zorgen dat de Service Azure Database migratie probleemloos bij het uitvoeren van de database-migraties. Sommige van de vereisten van toepassing in alle scenario's (bron-doel paren) die door de service, worden ondersteund, terwijl andere vereisten uniek voor een specifiek scenario zijn.

Vereisten die zijn gekoppeld met behulp van de migratie van Azure databaseservice worden in de volgende secties weergegeven.

## <a name="prerequisites-common-across-migration-scenarios"></a>Vereisten algemene over migratiescenario 's
Vereisten voor Azure migratie databaseservice die voor alle ondersteunde migratiescenario's gelden zijn onder andere het:
- Een VNET maken voor de Azure-Service voor het migreren van Database met behulp van het Azure Resource Manager-implementatiemodel, waardoor site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Netwerkbeveiligingsgroep regels komen de volgende communicatie niet blokkeren 443, 53, 9354 poort, 445, 12000. Zie het artikel voor meer informatie over het Azure VNET NSG wordt verkeer gefilterd [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Wanneer u een firewallapparaat voor uw databases bron, moet u wellicht toevoegen van firewallregels zodat de Service Azure Database migratie voor toegang tot de bron-databases voor migratie.

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Vereisten voor de SQL-Server migreren naar Azure SQL Database 
Naast de vereisten voor migratie van Azure databaseservice die gemeenschappelijk voor alle scenario's voor migratie zijn, zijn er ook vereisten die specifiek op een scenario of een andere toepassing.

Wanneer u de migratie van Azure databaseservice SQL Server uitvoert naar Azure SQL Database-migraties, naast de vereisten die gemeenschappelijk voor alle scenario's voor migratie zijn, moet u voor het oplossen van de volgende aanvullende vereisten:
- Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Inschakelen van het TCP/IP-protocol is standaard uitgeschakeld tijdens de installatie van SQL Server Express door de instructies in het artikel [in- of uitschakelen van een Server netwerkprotocol](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Geen exemplaar maken van Azure SQL Database-instantie die u doen door de details in het artikel C[maken van een Azure SQL database in de Azure portal](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal).
- Download en installeer de [gegevens migratie-assistent](https://www.microsoft.com/en-us/download/details.aspx?id=53595) v3.3 of hoger.
- Open Windows firewall zodat de Service Azure Database migratie voor toegang tot de bron-databases.
- Maken van een serverniveau [firewallregel](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) voor de Azure SQL Database-server zodat de migratie van Azure databaseservice toegang hebben tot de doeldatabases. Geef het subnetbereik van de VNET die wordt gebruikt voor de Azure-Service voor het migreren van Database.
- Zorg ervoor dat de referenties waarmee verbinding met SQL Server-bronexemplaar [CONTROL SERVER](https://docs.microsoft.com/en-us/sql/t-sql/statements/grant-server-permissions-transact-sql) machtigingen.
- Zorg ervoor dat de referenties waarmee verbinding met Azure SQL Database doelexemplaar machtiging beheer DATABASE op de doel-Azure SQL-databases.

   > [!NOTE]
   > Zie voor een volledig overzicht van de vereiste onderdelen voor de migratie van Azure databaseservice migraties van SQL Server uitvoert naar Azure SQL Database gebruiken de zelfstudie [SQL-Server migreren naar Azure SQL Database](https://docs.microsoft.com/en-us/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van de migratie van Azure databaseservice en regionale beschikbaarheid tijdens Public Preview [wat is Azure Database migratie Service Preview](dms-overview.md). 