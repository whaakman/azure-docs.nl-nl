---
title: Overzicht van vereisten voor het gebruik van de migratie van Azure databaseservice | Microsoft Docs
description: Meer informatie over een overzicht van de vereisten voor het gebruik van de migratie van Azure databaseservice migraties database uitvoeren.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: 3dc449724e405f83ce976b9f8b01a89c25d693fe
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221247"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Overzicht van vereisten voor het gebruik van de Service Azure Database migreren
Er zijn verschillende vereisten die zijn vereist om ervoor te zorgen dat de Service Azure Database migratie probleemloos bij het uitvoeren van de database-migraties. Sommige van de vereisten van toepassing in alle scenario's (bron-doel paren) die door de service, worden ondersteund, terwijl andere vereisten uniek voor een specifiek scenario zijn.

Vereisten die zijn gekoppeld met behulp van de migratie van Azure databaseservice worden in de volgende secties weergegeven.

## <a name="prerequisites-common-across-migration-scenarios"></a>Vereisten algemene over migratiescenario 's
Vereisten voor Azure migratie databaseservice die voor alle ondersteunde migratiescenario's gelden zijn onder andere het:
- Een VNET maken voor de Azure-Service voor het migreren van Database met behulp van het Azure Resource Manager-implementatiemodel, waardoor site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Netwerkbeveiligingsgroep regels komen de volgende communicatie niet blokkeren 443, 53, 9354 poort, 445, 12000. Zie het artikel voor meer informatie over het Azure VNET NSG wordt verkeer gefilterd [filteren van netwerkverkeer met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Wanneer u een firewallapparaat voor uw databases bron, moet u wellicht toevoegen van firewallregels zodat de Service Azure Database migratie voor toegang tot de bron-databases voor migratie.
- Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Inschakelen van het TCP/IP-protocol is standaard uitgeschakeld tijdens de installatie van SQL Server Express door de instructies in het artikel [in- of uitschakelen van een Server netwerkprotocol](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Vereisten voor de SQL-Server migreren naar Azure SQL Database 
Naast de vereisten voor migratie van Azure databaseservice die gemeenschappelijk voor alle scenario's voor migratie zijn, zijn er ook vereisten die specifiek op een scenario of een andere toepassing.

Wanneer u de migratie van Azure databaseservice SQL Server uitvoert naar Azure SQL Database-migraties, naast de vereisten die gemeenschappelijk voor alle scenario's voor migratie zijn, moet u voor het oplossen van de volgende aanvullende vereisten:

- Geen exemplaar maken van Azure SQL Database-instantie die u doen door de details in het artikel C[maken van een Azure SQL database in de Azure portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- Download en installeer de [gegevens migratie-assistent](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
- Open Windows Firewall zodat de Service Azure Database migratie voor toegang tot de bron van SQL Server, die standaard TCP-poort 1433.
- Als u meerdere benoemde exemplaren van SQL Server met behulp van dynamische poorten worden uitgevoerd, kunt u desgewenst de SQL Browser-Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de migratie van Azure databaseservice verbinding met een benoemd exemplaar op de bron maken kan -Server.
- Maken van een serverniveau [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor de Azure SQL Database-server zodat de migratie van Azure databaseservice toegang hebben tot de doeldatabases. Geef het subnetbereik van de VNET die wordt gebruikt voor de Azure-Service voor het migreren van Database.
- Zorg ervoor dat de referenties waarmee verbinding met SQL Server-bronexemplaar [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) machtigingen.
- Zorg ervoor dat de referenties waarmee verbinding met Azure SQL Database doelexemplaar machtiging beheer DATABASE op de doel-Azure SQL-databases.

   > [!NOTE]
   > Zie voor een volledig overzicht van de vereiste onderdelen voor de migratie van Azure databaseservice migraties van SQL Server uitvoert naar Azure SQL Database gebruiken de zelfstudie [SQL-Server migreren naar Azure SQL Database](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database-managed-instance"></a>Vereisten voor de SQL-Server migreren naar Azure SQL Database-beheerde exemplaar
- Geen exemplaar maken van Azure SQL Database-beheerd instantie aan de hand van de details in het artikel [maken van een Azure SQL Database beheerd-exemplaar in de Azure portal](https://aka.ms/sqldbmi).
- Open uw firewalls om toe te staan van SMB-verkeer op poort 445 voor het bereik op Azure Database migratie Service IP-adres of subnet.
- Open Windows Firewall zodat de Service Azure Database migratie voor toegang tot de bron van SQL Server, die standaard TCP-poort 1433.
- Als u meerdere benoemde exemplaren van SQL Server met behulp van dynamische poorten worden uitgevoerd, kunt u desgewenst de SQL Browser-Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de migratie van Azure databaseservice verbinding met een benoemd exemplaar op de bron maken kan -Server.
- Zorg ervoor dat de aanmeldingen gebruikt voor het verbinding maken met de bron van SQL Server en de doelinstantie van beheerde leden van de serverrol sysadmin.
- Maak een netwerkshare die de migratie van Azure databaseservice gebruiken kunt voor back-up van de brondatabase.
- Zorg ervoor dat het serviceaccount dat de bron met SQL Server-exemplaar heeft schrijfrechten op de netwerkshare die u hebt gemaakt en dat het computeraccount voor de bronserver lees-/ schrijftoegang tot dezelfde share heeft.
- Maak een notitie van een Windows-gebruiker (en wachtwoord) die de machtiging Volledig beheer heeft op de netwerkshare die u eerder hebt gemaakt. De migratie van Azure databaseservice imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar Azure storage-container voor restore-bewerking.
- Maken van een blob-container en de SAS URI ophalen met behulp van de stappen in het artikel [beheren Azure Blob Storage-resources met Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). Zorg ervoor dat alle machtigingen (lezen, schrijven, verwijderen, lijst) in het venster beleid selecteren tijdens het maken van de SAS-URI.

   > [!NOTE]
   > Zie voor een volledig overzicht van de vereiste onderdelen voor de migratie van Azure databaseservice migraties van SQL Server uitvoert naar Azure SQL Database-beheerde exemplaar gebruiken de zelfstudie [SQL-Server migreren naar Azure SQL Database-beheerde exemplaar ](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van de Azure-Database migratieservice en de beschikbaarheid van regionale [wat is de Service Azure Database migratie](dms-overview.md). 