---
title: Netwerktopologieën voor migraties van Azure SQL Database Managed Instance met Azure Database Migration Service | Microsoft Docs
description: Meer informatie over de configuratie van de bron en doel voor Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/07/2019
ms.openlocfilehash: 74613599903f7cde606295a1e2d9eaaa0924cf50
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808417"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-azure-database-migration-service"></a>Netwerktopologieën voor migraties van Azure SQL database Managed Instance met Azure Database Migration Service

Dit artikel worden verschillende netwerktopologieën maken waarin Azure Database Migration Service werken kunt met een uitgebreide migratie-ervaring bieden van on-premises SQL-Servers naar Azure SQL Database Managed Instance.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure SQL Database Managed Instance geconfigureerd voor hybride werkbelastingen 

Gebruik deze topologie als uw Azure SQL Database Managed Instance is verbonden met uw on-premises netwerk. Deze aanpak biedt de meest eenvoudige netwerkroutering en geeft de maximale doorvoer tijdens de migratie.

![Netwerktopologie voor hybride werkbelastingen](media/resource-network-topologies/hybrid-workloads.png)

**Vereisten**

- In dit scenario, de Azure SQL Database managed instance en de Azure Database Migration Service-exemplaar worden gemaakt in hetzelfde Azure-VNet, maar ze gebruiken verschillende subnetten.  
- Het VNet gebruikt in dit scenario is ook verbonden met de on-premises netwerk met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL Database Managed Instance geïsoleerd van de on-premises netwerk

Deze topologie gebruiken als uw omgeving een of meer van de volgende scenario's vereist:

- De beheerde Azure SQL Database-exemplaar is geïsoleerd van on-premises connectiviteit, maar uw Azure Database Migration Service-exemplaar is verbonden met de on-premises netwerk.
- Als het beleid voor rollen gebaseerd toegangsbeheer (RBAC) is voldaan en u wilt voorkomen dat de gebruikers toegang tot hetzelfde abonnement als dat als host voor het beheerde exemplaar van Azure SQL Database fungeert.
- De VNets die worden gebruikt voor de Azure SQL Database Managed Instance en Azure Database Migration Service zijn in verschillende abonnementen.

![Netwerktopologie voor beheerd exemplaar is geïsoleerd van de on-premises netwerk](media/resource-network-topologies/mi-isolated-workload.png)

**Vereisten**

- Het VNet waarmee Azure Database Migration Service wordt gebruikt voor dit scenario moet ook worden verbonden met de on-premises netwerk met behulp van (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Instellen van [VNet-netwerkpeering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) tussen het VNet gebruikt voor Azure SQL Database managed instance en Azure Database Migration Service.

## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Cloud-naar-cloud-migraties: gedeelde VNet

Gebruik deze topologie als de bron-SQL Server wordt gehost in een Azure-VM en hetzelfde VNET met Azure SQL Database managed instance en Azure Database Migration Service deelt.

![Netwerktopologie voor Cloud-naar-Cloud-migraties met een gedeelde VNet](media/resource-network-topologies/cloud-to-cloud.png)

**Vereisten**

- Er is geen aanvullende vereisten.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Met de cloud voor cloudmigraties: VNet (geïsoleerd)

Deze topologie gebruiken als uw omgeving een of meer van de volgende scenario's vereist:

- Het beheerde exemplaar van Azure SQL Database wordt ingericht in een geïsoleerde VNet.
- Als het beleid voor rollen gebaseerd toegangsbeheer (RBAC) is voldaan en u wilt voorkomen dat de gebruikers toegang tot hetzelfde abonnement als dat als host voor het beheerde exemplaar van Azure SQL Database fungeert.
- De VNets die worden gebruikt voor Azure SQL Database Managed Instance en Azure Database Migration Service zijn in verschillende abonnementen.

![Netwerktopologie voor Cloud-naar-Cloud-migraties met een geïsoleerd VNet](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Vereisten**

- Instellen van [VNet-netwerkpeering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) tussen het VNet gebruikt voor Azure SQL Database managed instance en Azure Database Migration Service.

## <a name="inbound-security-rules"></a>Inkomende beveiligingsregels

| **NAME**   | **POORT** | **PROTOCOL** | **BRON** | **BESTEMMING** | **ACTIE** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Alle      | Alle          | DMS-SUBNET | Alle             | Toestaan      |

## <a name="outbound-security-rules"></a>Uitgaande beveiligingsregels

| **NAME**                  | **POORT**                                              | **PROTOCOL** | **BRON** | **BESTEMMING**           | **ACTIE** | **Reden voor regel**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| management                | 443,9354                                              | TCP          | Alle        | Alle                       | Toestaan      | Management vlak communicatie via servicebus en Azure blob-opslag. <br/>(Als Microsoft-peering is ingeschakeld, moet u mogelijk niet met deze regel.)                                                             |
| Diagnostiek               | 12000                                                 | TCP          | Alle        | Alle                       | Toestaan      | DMS maakt gebruik van deze regel voor het verzamelen van diagnostische gegevens voor het oplossen van problemen.                                                                                                                      |
| SQL-bronserver         | 1433 (of TCP IP-poorten die SQL Server naar luistert) | TCP          | Alle        | On-premises adresruimte | Toestaan      | Connectiviteit van SQL Server-bron van DMS <br/>(Als u site-naar-site-connectiviteit hebt, moet u mogelijk niet met deze regel.)                                                                                       |
| SQL Server-benoemd exemplaar | 1434                                                  | UDP          | Alle        | On-premises adresruimte | Toestaan      | SQL Server-benoemd exemplaar bron connectiviteit van DMS <br/>(Als u site-naar-site-connectiviteit hebt, moet u mogelijk niet met deze regel.)                                                                        |
| SMB-share                 | 445                                                   | TCP          | Alle        | On-premises adresruimte | Toestaan      | SMB-netwerkshare voor DMS voor het opslaan van back-upbestanden voor migraties naar MI Azure SQL Database en SQL-Servers op Azure VM <br/>(Als u site-naar-site-connectiviteit hebt, moet u mogelijk niet met deze regel). |
| DMS_subnet                | Alle                                                   | Alle          | Alle        | DMS_Subnet                | Toestaan      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Zie ook

- [SQL Server migreren naar beheerd exemplaar voor Azure SQL Database](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Overzicht van vereisten voor het gebruik van Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Een virtueel netwerk maken met Azure Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Volgende stappen

- Zie het artikel voor een overzicht van Azure Database Migration Service, [wat is Azure Database Migration Service?](dms-overview.md).
- Zie voor actuele informatie over regionale beschikbaarheid van Azure Database Migration Service, de [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration) pagina.
