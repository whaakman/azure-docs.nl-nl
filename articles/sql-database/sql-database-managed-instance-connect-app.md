---
title: Azure SQL Database Managed Instance-toepassing verbinden met | Microsoft Docs
description: In dit artikel wordt beschreven hoe u verbinding maken met uw toepassing naar Azure SQL Database Managed Instance.
ms.service: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.custom: managed instance
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 82e8836892b033ccbb3c3ad9806257348afe3702
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818399"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Verbinding maken tussen uw toepassing en het beheerde exemplaar van Azure SQL Database

Nu hebt u meerdere opties bij het bepalen hoe en waar u uw toepassing wordt gehost. 
 
U kunt kiezen voor het hosten van de toepassing in de cloud met Azure App Service of een aantal van de Azure-netwerk (VNet) geïntegreerd opties, zoals Azure App Service-omgeving, virtuele Machine, virtuele-Machineschaalset opgehaald. U kunt ook hybride cloud-benadering en uw toepassingen on-premises houden. 
 
De keuze die u hebt gemaakt, kunt u deze naar een beheerd exemplaar (preview).  

![hoge beschikbaarheid](./media/sql-database-managed-instance/application-deployment-topologies.png)  

## <a name="connect-an-application-inside-the-same-vnet"></a>Verbinding maken met een toepassing in hetzelfde VNet 

Dit scenario is de eenvoudigste. Virtuele machines binnen het VNet worden rechtstreeks met elkaar verbonden, zelfs als deze zich in verschillende subnetten. Dit betekent dat dat is alles wat u nodig voor de toepassing in een omgeving voor Azure-toepassingen of de virtuele Machine verbinden met de verbindingsreeks op de juiste wijze instellen.  
 
In het geval u de verbinding te maken kan, moet u controleren of er een Netwerkbeveiligingsgroep die is ingesteld op het subnet van de toepassing. In dit geval moet u uitgaande verbindingen op de SQL-poort 1433, evenals 11000 12000 bereik van poorten voor de omleiding van openen. 

## <a name="connect-an-application-inside-a-different-vnet"></a>Verbinding maken met een toepassing in een ander VNet 

In dit scenario is iets wat ingewikkelder omdat Managed Instance privé IP-adres in een eigen VNet is. Als u wilt verbinden, moet een toepassing toegang tot het VNet waarop Managed Instance wordt geïmplementeerd. Ja, u moet eerst een verbinding tussen de toepassing en het beheerde exemplaar VNet te maken. De VNets hoeven niet te worden in hetzelfde abonnement voor dit scenario te werken. 
 
Er zijn twee opties voor het verbinden van vnet's: 
- [Azure Virtual Network-peering](../virtual-network/virtual-network-peering-overview.md) 
- VNet-naar-VNet-VPN-gateway ([Azure-portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md)) 
 
De peering-optie is de voorkeur omdat peering maakt gebruik van het Microsoft-backbone-netwerk, vanuit het perspectief van de connectiviteit, wordt er geen merkbaar verschil in latentie tussen virtuele machines in gekoppelde VNet en in hetzelfde VNet. VNet-peering is beperkt tot de netwerken in dezelfde regio.  
 
> [!IMPORTANT]
> VNet peering scenario voor het beheerde exemplaar is beperkt tot de netwerken in dezelfde regio vanwege [beperkingen van de wereldwijde vnet-peering](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints). 

## <a name="connect-an-on-premises-application"></a>Verbinding maken met een on-premises toepassing 

Beheerd exemplaar kan alleen worden geopend via een particulier IP-adres. Voor toegang tot deze van on-premises, moet u een Site-naar-Site-verbinding tussen de toepassing en het VNet beheerd exemplaar maken. 
 
Er zijn twee opties verbinding maken tussen on-premises en Azure-VNet: 
- Site-naar-Site VPN-verbinding ([Azure-portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)) 
- [ExpressRoute](../expressroute/expressroute-introduction.md) verbinding  
 
Als hebt u on-premises naar Azure-verbinding is gemaakt en u kunt geen verbinding met Managed Instance maken, controleert u of uw firewall open uitgaande verbinding op SQL-poort 1433, evenals 11000 12000 bereik van poorten voor de omleiding van heeft. 

## <a name="connect-an-azure-app-service-hosted-application"></a>Verbinding maken met een toepassing met Azure App Service die wordt gehost 

Voor het beheerde exemplaar kan alleen worden verkregen via een privé IP-adres om te kunnen openen vanaf Azure App Service moet u eerst een verbinding tussen de toepassing en het beheerde exemplaar VNet te maken. Zie [uw app integreren met een Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).  
 
Zie voor het oplossen van problemen [probleemoplossing vnet's en toepassingen](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Als u geen verbinding kan niet worden gemaakt, probeert u [synchroniseren van de netwerkconfiguratie](sql-database-managed-instance-sync-network-configuration.md). 
 
Een speciaal geval van Azure App Service verbinden met Managed Instance is wanneer u geïntegreerde dat Azure App Service met een netwerk aan elkaar gekoppeld naar beheerd exemplaar VNet. Deze aanvraag moet de volgende configuratie worden ingesteld: 

- Beheerde exemplaar VNet mag geen gateway  
- Beheerde exemplaar VNet moet optieset voor externe gateways gebruiken 
- Gekoppelde VNet moet toestaan gateway doorvoer optie is ingesteld 
 
In dit scenario is geïllustreerd in het volgende diagram:

![geïntegreerde app-peering](./media/sql-database-managed-instance/integrated-app-peering.png)
 
## <a name="connect-an-application-on-the-developers-box"></a>Verbinding maken met een toepassing in het vak voor ontwikkelaars 

Voor het beheerde exemplaar kan alleen worden verkregen via een privé IP-adres dus als u wilt openen vanaf uw developer-box, moet u eerst een verbinding tussen uw developer-box en het beheerde exemplaar VNet te maken.  
 
Een punt-naar-Site-verbinding met een VNet met behulp van systeemeigen Azure-certificaatverificatie verificatie artikelen configureren ([Azure-portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md)) in detail weergeeft hoe het kan worden uitgevoerd. 

## <a name="required-versions-of-drivers-and-tools"></a>Vereiste versies van stuurprogramma's en hulpprogramma 's

De volgende minimale versies van de hulpprogramma's en stuurprogramma's worden aanbevolen als u wilt verbinding maken met Managed Instance:

| Stuurprogramma/hulpprogramma | Versie |
| --- | --- |
|.NET Framework | 4.6.1 (of .NET Core) | 
|ODBC-stuurprogramma    | v17 |
|PHP-stuurprogramma | 5.2.0 |
|JDBC-stuurprogramma    | 6.4.0 |
|Node.js-stuurprogramma | 2.1.1 |
|OLEDB-stuurprogramma   | 18.0.2.0 |
|SSMS   | 17.8.1 of [hoger](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) |

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Managed Instance [wat is een beheerd exemplaar](sql-database-managed-instance.md).
- Zie voor een zelfstudie wordt uitgelegd hoe u een nieuwe Managed Instance maakt u [maken van een beheerd exemplaar](sql-database-managed-instance-get-started.md).
