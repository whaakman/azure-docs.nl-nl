---
title: Overzicht van vereisten voor het gebruik van de Azure Database Migration Service | Microsoft Docs
description: Meer informatie over een overzicht van de vereisten voor het gebruik van de Azure Database Migration Service om uit te voeren van de databasemigraties.
services: database-migration
author: HJToland3
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: a6dc9b31052e49eb0312006ea40056067e92b5ee
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57790412"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Overzicht van vereisten voor het gebruik van de Azure Database Migration Service
Er zijn verschillende vereisten die zijn vereist om ervoor te zorgen dat de Azure Database Migration Service probleemloos werkt bij het uitvoeren van de databasemigraties. Sommige van de vereisten van toepassing in alle scenario's (bron-doelparen) die wordt ondersteund door de service, terwijl andere vereiste onderdelen uniek voor een specifiek scenario zijn.

Vereisten die zijn gekoppeld aan met behulp van de Azure Database Migration Service worden vermeld in de volgende secties.

## <a name="prerequisites-common-across-migration-scenarios"></a>Vereisten gebruikt voor migratiescenario 's
Vereisten voor Azure Database Migration Service die betrekking hebben op alle ondersteunde migratiescenario's zijn onder andere het:
- Maak een VNET voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Dit geeft site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zorg ervoor dat uw Azure Virtual Network (VNET) Network Security Group-regels de volgende communicatiepoorten niet blokkeren: 443, 53, 9354, 445, 12000. Zie voor meer informatie over verkeer filteren van Azure VNET NSG het artikel [Netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor de Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.
- Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Schakel het TCP/IP-protocol in, dat standaard is uitgeschakeld tijdens de installatie van SQL Server Express, door de instructies in het artikel [In- of uitschakelen van een Server Network Protocol](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure) te volgen.

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Vereisten voor SQL-Server migreren naar Azure SQL Database 
Naast Azure Database Migration Service-vereisten die gemeenschappelijk voor alle scenario's voor migratie zijn, zijn er ook vereisten die specifiek op een scenario of een andere toepassing.

Bij het gebruik van de Azure Database Migration Service om uit te voeren moet SQL Server naar Azure SQL Database-migraties, naast de vereisten die gemeenschappelijk voor alle scenario's voor migratie zijn naar het adres van de volgende aanvullende vereisten:

- Maak een instantie van Azure SQL Database-instantie die u doen door het volgende op de details in het artikel C[maken van een Azure SQL database in Azure portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- Download en installeer de [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
- Stel uw Windows-firewall open voor toegang van de Azure Database Migration Service tot de brondatabase van SQL Server. Standaard verloopt dit via TCP-poort 1433.
- Als u meerdere benoemde SQL Server-exemplaren met behulp van dynamische poorten uitvoert, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
- Maak een [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) op serverniveau voor de Azure SQL Database-server om voor de Azure Database Migration Service toegang tot de doeldatabase toe te staan. Geef het subnetbereik van het VNET op dat wordt gebruikt voor de Azure Database Migration Service.
- Zorg ervoor dat de referenties waarmee verbinding wordt gemaakt met het SQL Server-bronexemplaar [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql)-machtigingen hebben.
- Zorg ervoor dat de referenties waarmee verbinding wordt gemaakt met het Azure SQL Database-doelexemplaar CONTROL DATABASE-machtiging hebben op de Azure SQL-doeldatabases.

   > [!NOTE]
   > Voor een volledige lijst met de vereiste onderdelen voor het gebruik van de Azure Database Migration Service om uit te voeren van migraties van SQL Server naar Azure SQL Database, Zie de zelfstudie [SQL-Server migreren naar Azure SQL Database](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database-managed-instance"></a>Vereisten voor SQL-Server migreren naar Azure SQL Database Managed Instance
- Een exemplaar van Azure SQL Database Managed Instance maken door het volgen van de details in het artikel [maken van een Azure SQL Database Managed Instance in de Azure portal](https://aka.ms/sqldbmi).
- Open uw firewalls om toe te staan van SMB-verkeer op poort 445 voor de Azure Database Migration Service IP-adres of subnet bereik.
- Stel uw Windows-firewall open voor toegang van de Azure Database Migration Service tot de brondatabase van SQL Server. Standaard verloopt dit via TCP-poort 1433.
- Als u meerdere benoemde SQL Server-exemplaren met behulp van dynamische poorten uitvoert, kunt u desgewenst de SQL Browser Service inschakelen en toegang tot de UDP-poort 1434 via uw firewalls toestaan, zodat de Azure Database Migration Service verbinding kan maken met een benoemd exemplaar op uw bronserver.
- Zorg ervoor dat de aanmeldingsreferenties die worden gebruikt voor verbinding met het bronexemplaar van SQL Server en het doelexemplaar van Managed Instance lid zijn van de serverrol sysadmin.
- Maak een netwerkshare die de Azure Database Migration Service kan gebruiken om een back-up te maken van de brondatabase.
- Zorg ervoor dat het serviceaccount van het bronexemplaar van SQL Server schrijfbevoegdheid heeft voor de netwerkshare die u hebt gemaakt en dat het computeraccount voor de bronserver lees-/schrijftoegang heeft tot de share.
- Maak een notitie van een Windows-gebruiker (en wachtwoord) die volledig beheer heeft over de netwerkshare die u eerder hebt gemaakt. De Azure Database Migration Service imiteert de gebruikersreferenties voor het uploaden van de back-upbestanden naar een Azure-opslagcontainer voor herstelbewerkingen.
- Een blob-container maken en de SAS-URI ophalen met behulp van de stappen in het artikel [beheren Azure Blob Storage-resources met Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). Zorg ervoor dat alle machtigingen (lezen, schrijven, verwijderen, lijst) in het venster voor beleid selecteren tijdens het maken van de SAS-URI.

   > [!NOTE]
   > Zie voor een volledige lijst met de vereiste onderdelen voor het gebruik van de Azure Database Migration Service uitvoert migraties van SQL Server naar Azure SQL Database Managed Instance, de zelfstudie [SQL-Server migreren naar Azure SQL Database Managed Instance ](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van de Azure Database Migration Service en de regionale beschikbaarheid [wat is de Azure Database Migration Service](dms-overview.md). 