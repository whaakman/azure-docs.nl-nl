---
title: Topologieën voor Azure SQL Database-beheerd instantie migraties met behulp van de migratie van Azure databaseservice netwerk | Microsoft Docs
description: Meer informatie over de bron en doel-configuraties voor migratie databaseservice.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 9fcee103854209016d73e29b598c9f33d35c4b6c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316864"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Netwerktopologieën voor Azure SQL DB-beheerde exemplaar migraties met behulp van de Service Azure Database migreren
Dit artikel bevat verschillende netwerktopologieën maken waarin de Azure-Service voor het migreren van Database werken kunt met een uitgebreide migratie-ervaring bieden van on-premises SQL-Servers aan Azure SQL Database-beheerd instantie.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure SQL Database beheerd exemplaar dat is geconfigureerd voor hybride werkbelastingen 
Deze topologie te gebruiken als uw Azure SQL Database beheerd-exemplaar is verbonden met uw on-premises netwerk. Deze aanpak biedt de meest vereenvoudigde netwerkroutering en levert gegevensdoorvoer tijdens de migratie.

![Netwerktopologie voor hybride werkbelastingen](media\resource-network-topologies\hybrid-workloads.png)

**Vereisten**
- In dit scenario wordt de Azure SQL Database beheerd-instantie en het Azure-Database migratieservice-exemplaar worden gemaakt in hetzelfde Azure VNET, maar ze gebruiken verschillende subnetten.  
- Het VNET dat in dit scenario gebruikt is ook verbonden met de on-premises netwerk via een [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL Database beheerd-instantie geïsoleerd van de on-premises netwerk
Deze topologie gebruiken als uw omgeving een of meer van de volgende scenario's vereist:
- De Azure SQL Database beheerd-instantie is geïsoleerd van de lokale verbinding, maar uw Azure-Database migratieservice-exemplaar is verbonden met de on-premises netwerk.
- Als het beleid voor rollen gebaseerd toegangsbeheer (RBAC) is voldaan en moet u de gebruikers om toegang tot hetzelfde abonnement beperken die als host fungeert voor de Azure SQL Database beheerd-instantie.
- De VNETs gebruikt voor de Azure SQL Database beheerd en de Azure-Service voor het migreren van Database zich in verschillende abonnementen behoren.

![Netwerktopologie voor beheerde exemplaar geïsoleerd van de on-premises netwerk](media\resource-network-topologies\mi-isolated-workload.png)

**Vereisten**
- Het VNET dat Azure Database migratie-Service wordt gebruikt voor dit scenario moet ook worden verbonden met de on-premises netwerk via een (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Instellen van [VNET netwerk peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) tussen het VNET gebruikt voor Azure SQL Database-beheerde exemplaar en de Azure-Service voor het migreren van Database.


## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Cloud-naar-cloud migraties: gedeelde VNET

Deze topologie gebruiken als de bron van SQL Server wordt gehost in een Azure VM en hetzelfde VNET met beheerde-exemplaar van Azure SQL Database en de Azure-Service voor het migreren van Database deelt.

![Netwerktopologie voor Cloud-naar-Cloud-migratie met een gedeelde vnet](media\resource-network-topologies\cloud-to-cloud.png)

**Vereisten**
- Er is geen aanvullende vereisten.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Cloud voor cloud-migraties: geïsoleerd VNET

Deze topologie gebruiken als uw omgeving een of meer van de volgende scenario's vereist:
- De Azure SQL Database beheerd-instantie is in een geïsoleerde VNET ingericht.
- Als het beleid voor rollen gebaseerd toegangsbeheer (RBAC) is voldaan en moet u de gebruikers om toegang tot hetzelfde abonnement beperken die als host fungeert voor de Azure SQL Database beheerd-instantie.
- De VNETs die wordt gebruikt voor Azure SQL Database-beheerde exemplaar en de migratie van Azure databaseservice zijn in verschillende abonnementen behoren.

![Netwerktopologie voor Cloud-naar-Cloud-migraties met een geïsoleerd vnet](media\resource-network-topologies\cloud-to-cloud-isolated.png)

**Vereisten**
- Instellen van [VNET netwerk peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) tussen het VNET gebruikt voor Azure SQL Database-beheerde exemplaar en de Azure-Service voor het migreren van Database.


## <a name="see-also"></a>Zie ook
- [SQL-Server migreren naar beheerde Azure SQL Database-instantie](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Overzicht van vereisten voor het gebruik van de Service Azure Database migreren](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Een virtueel netwerk maken met Azure Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een overzicht van de migratie van Azure databaseservice en regionale beschikbaarheid tijdens Public Preview [wat is Azure Database migratie Service Preview](dms-overview.md). 