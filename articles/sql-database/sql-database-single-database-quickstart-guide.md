---
title: Quickstart - Individuele databases beveiligen in Azure SQL Database | Microsoft Docs
description: Informatie over hoe u snel aan de slag gaat met individuele databases in Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: 0dc9d18ea4dae6ea0f45d1a91392eb1994e6e80e
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757184"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Aan de slag met individuele databases in Azure SQL Database

[Een individuele database](sql-database-single-index.yml) is een volledig beheerde PaaS-database-als-een-service (DbaaS) die ideaal is als opslag-engine voor moderne toepassingen in de cloud. In dit gedeelte leert u hoe u snel een individuele SQL Database maakt en configureert.

## <a name="quickstart-overview"></a>Overzicht van quickstarts

Dit gedeelte bevat een overzicht van beschikbare artikelen waarmee u snel aan de slag kunt met individuele databases. In de volgende quickstarts leert u snel een individuele database te maken, een firewallregel voor databaseservers te configureren en vervolgens een database te importeren in de nieuwe individuele database via een `.bacpac`-bestand:

- [Een individuele database maken met Azure Portal](sql-database-single-database-get-started.md).
- Nadat de database is gemaakt, moet u uw [database beveiligen door firewallregels te configureren](sql-database-server-level-firewall-rule.md).
- Als u een bestaande database hebt in een SQL Server die u wilt migreren naar Azure, moet u [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) installeren. Dit analyseert uw databases in een SQL Server en zoekt problemen die de migratie naar een individuele database zouden kunnen blokkeren. Als u geen problemen vindt, kunt u uw database als een `.bacpac`-bestand exporteren en [het bestand importeren met de Azure-portal of SqlPackage](sql-database-import.md).

## <a name="automating-management-operations"></a>Beheerbewerkingen automatiseren

U kunt PowerShell of Azure CLI gebruiken om uw database te maken, configureren en schalen.

- [Een individuele database maken met PowerShell](scripts/sql-database-create-and-configure-database-powershell.md)
- [Een individuele database maken met Azure CLI](scripts/sql-database-create-and-configure-database-cli.md)
- [Een individuele database bijwerken en resources schalen met PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md)
- [Een individuele database bijwerken en resources schalen met Azure CLI](scripts/sql-database-monitor-and-scale-database-cli.md)

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>Migreren naar een individuele database met minimale downtime

Met de artikelen in deze quickstarts kunt u uw database snel maken of importeren in Azure met een `.bacpac`-bestand. `.bacpac`- en `.dacpac`-bestanden zijn echter ontworpen om snel databases te verplaatsen tussen verschillende versies van SQL Server en implementatieopties binnen Azure SQL Database of om continue integratie in uw DevOps-pijplijn te implementeren. Deze methode is echter niet ontworpen voor de migratie van uw productiedatabases met minimale downtime, omdat u moet stoppen met het toevoegen van nieuwe gegevens, wachten tot de export van de brondatabase naar een `.bacpac`-bestand voltooid is en vervolgens wachten totdat het importeren in Azure SQL Database voltooid is. Al dit wachten leidt tot downtime van uw toepassing, met name voor grote databases. Als u uw productiedatabase verplaatst, hebt u waarschijnlijk een betere migratiemethode nodig met minimale downtime. Om de database met minimale downtime te migreren, gebruikt u de [Database Migration Service (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json). Dit wordt bereikt doordat DMS de aan de brondatabase aangebrachte wijzigingen stapsgewijs pusht naar de individuele database die wordt teruggezet. Op deze manier kunt u snel uw toepassing van de bron- naar de doeldatabase overzetten met minimale downtime.

## <a name="next-steps"></a>Volgende stappen

- Zoek een [lijst met ondersteunde functies in Azure SQL Database](sql-database-features.md).
- Leer hoe u uw [database veiliger maakt](sql-database-security-tutorial.md).
- U vindt meer uitgebreide informatie in [Individuele database gebruiken in Azure SQL Database](sql-database-howto-single-database.md).
- Meer voorbeeldscripts zoeken die zijn geschreven in [PowerShell](sql-database-powershell-samples.md) en [Azure CLI](sql-database-cli-samples.md).
- Meer informatie over de [Management-API](sql-database-single-databases-manage.md) die u kunt gebruiken om uw databases te configureren.
