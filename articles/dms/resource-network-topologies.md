---
title: "Topologieën voor Azure SQL DB-beheerde exemplaar migraties met behulp van de migratie van Azure databaseservice netwerk | Microsoft Docs"
description: Meer informatie over de bron en doel-configuraties voor migratie databaseservice.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/06/2018
ms.openlocfilehash: 892cff02b5b70f09236bb37ae786f180ddca9316
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Netwerktopologieën voor Azure SQL DB-beheerde exemplaar migraties met behulp van de Service Azure Database migreren
In dit artikel leert u over verschillende netwerktopologieën Migratieservice voor Azure-Database kunt werken met naadloze migratie-ervaring bieden aan Azure SQL Database-beheerde exemplaar van de on-premises SQL-Servers.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure SQL Database beheerd exemplaar dat is geconfigureerd voor hybride werkbelastingen 
Deze topologie te gebruiken als uw Azure SQL Database beheerd-exemplaar is verbonden met uw on-premises netwerk. Deze aanpak biedt de meest vereenvoudigde netwerkroutering en levert gegevensdoorvoer tijdens de migratie.

![Netwerktopologie voor hybride werkbelastingen](media\resource-network-topologies\hybrid-workloads.png)

**Vereisten**
- In dit scenario wordt de Azure SQL Database beheerd-instantie en het Azure-Database migratieservice-exemplaar worden gemaakt in hetzelfde Azure VNET, maar ze gebruiken verschillende subnetten.  
- Het VNET dat in dit scenario gebruikt is ook verbonden met de on-premises netwerk via ExpressRoute of VPN.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL Database beheerd-instantie geïsoleerd van de on-premises netwerk
Deze topologie gebruiken als uw omgeving een of meer van de volgende scenario's vereist:
- De Azure SQL Database beheerd-instantie is geïsoleerd van de lokale verbinding, maar uw Azure-Database migratieservice-exemplaar is verbonden met de on-premises netwerk.
- Beleid voor rollen gebaseerd toegangsbeheer (RBAC) is voldaan en beperken van gebruikerstoegang tot hetzelfde abonnement die als host voor de Azure SQL Database beheerd-instantie fungeert.
- De VNETs die wordt gebruikt voor Azure SQL Database-beheerd instantie en Azure Database migratieservice zijn in verschillende abonnementen behoren.

![Netwerktopologie voor beheerde exemplaar geïsoleerd van de on-premises netwerk](media\resource-network-topologies\mi-isolated-workload.png)

**Vereisten**
- Het VNET dat Azure Database migratie-Service wordt gebruikt voor dit scenario moet ook worden verbonden met de on-premises netwerk via ExpressRoute of VPN.
- Maak een VNET-netwerk tussen het VNET gebruikt voor Azure SQL Database-beheerd instantie en Database migratie-Service van Azure-peering.


## <a name="cloud-to-cloud-migrations"></a>Cloud naar cloud migraties
Deze topologie te gebruiken als de bron van SQL Server wordt gehost in Azure een virtuele machine.

![Netwerktopologie voor Cloud-naar-Cloud-migraties](media\resource-network-topologies\cloud-to-cloud.png)

**Vereisten**
- Maak een VNET-netwerk tussen het VNET gebruikt voor Azure SQL Database-beheerd instantie en Database migratie-Service van Azure-peering.

## <a name="see-also"></a>Zie ook
- [SQL-Server migreren naar beheerde Azure SQL Database-instantie](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Overzicht van vereisten voor het gebruik van de Service Azure Database migreren](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Een virtueel netwerk maken met Azure Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van de migratie van Azure databaseservice en regionale beschikbaarheid tijdens Public Preview [wat is Azure Database migratie Service Preview](dms-overview.md). 