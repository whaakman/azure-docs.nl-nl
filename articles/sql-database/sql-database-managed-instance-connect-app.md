---
title: Azure SQL Database beheerd-instantie verbinding toepassing | Microsoft Docs
description: Dit artikel wordt beschreven hoe u uw toepassing in Azure SQL Database-beheerd instantie verbinding maakt.
ms.service: sql-database
author: srdjan-bozovic
manager: cguyer
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 243dfd09df5eaac05608f57a0b2abf6e757f00e8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Verbinding maken met uw toepassing met Azure SQL Database beheerd exemplaar

Vandaag hebt u meerdere opties bij het bepalen hoe en waar u uw toepassing wordt gehost. 
 
U kunt kiezen voor het hosten van de toepassing in de cloud met Azure App Service of een aantal van de Azure-netwerk (VNet) geïntegreerd opties zoals Azure App Service-omgeving, virtuele Machine virtuele-Machineschaalset. U kan ook hybride cloud benadering en houden van uw toepassingen on-premises. 
 
Welke optie u hebt gekozen, kunt u deze naar een exemplaar beheerd (preview).  

![Hoge beschikbaarheid](./media/sql-database-managed-instance/application-deployment-topologies.png)  

## <a name="connect-an-application-inside-the-same-vnet"></a>Verbinding maken met een toepassing binnen hetzelfde VNet 

Dit scenario is de eenvoudigste. Virtuele machines binnen het VNet kunnen rechtstreeks met elkaar verbinden zelfs als deze zich in verschillende subnetten. Dat betekent dat hoeft u alleen verbinding maken met de toepassing in een omgeving voor Azure-toepassing of virtuele Machine op de juiste wijze de verbindingsreeks instellen.  
 
Als u niet kunt van de verbinding maken, Controleer of er een Netwerkbeveiligingsgroep ingesteld op het subnet van de toepassing. In dit geval moet u uitgaande verbinding op de SQL-poort 1433, evenals 11000 12000 reeks poorten voor de omleiding van openen. 

## <a name="connect-an-application-inside-a-different-vnet"></a>Verbinding maken met een toepassing in een andere VNet 

Dit scenario is iets meer complexe omdat exemplaar beheerd privé IP-adres in zijn eigen VNet heeft. Als u wilt verbinden, moet een toepassing toegang tot het VNet waarop beheerde exemplaar wordt geïmplementeerd. Ja, u moet eerst een verbinding tussen de toepassing en het exemplaar beheerd VNet te maken. De VNets hoeft te worden in volgorde voor dit scenario werkt hetzelfde abonnement. 
 
Er zijn twee opties voor het verbinden van VNets: 
- [Azure Virtual Network-peering](../virtual-network/virtual-network-peering-overview.md) 
- VNet-naar-VNet-VPN-gateway ([Azure-portal](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md)) 
 
De peering optie is de voorkeur omdat peering gebruikmaakt van het Microsoft-backbone-netwerk, vanuit het perspectief van de connectiviteit, er is geen merkbaar verschil in latentie tussen virtuele machines in VNet peer is ingesteld en in hetzelfde VNet. VNet-peering is beperkt tot de netwerken in dezelfde regio, hoewel regio-overschrijdende peering is ingeschakeld in bepaalde gebieden als voorbeeld.  
 
> [!IMPORTANT]
> VNet-peerings regio-overschrijdende gemaakt wellicht niet dezelfde mate van beschikbaarheid en betrouwbaarheid als peerings in een versie van de algemene beschikbaarheid. VNet-peerings kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-regio's. Voor de meest recente meldingen op beschikbaarheid en de status van deze functie, Controleer de [Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network) pagina updates. 

## <a name="connect-an-on-premises-application"></a>Verbinding maken met een on-premises toepassing 

Beheerde exemplaar kan alleen worden benaderd via een particulier IP-adres. Als u wilt toegang hebt tot het on-premises, moet u een Site-naar-Site-verbinding tussen de toepassing en het exemplaar beheerd VNet maken. 
 
Er zijn twee opties verbinding maken tussen lokale en Azure VNet: 
- Site-naar-Site VPN-verbinding ([Azure-portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)) 
- [ExpressRoute](../expressroute/expressroute-introduction.md) verbinding  
 
Als u hebt de on-premises naar Azure-verbinding is ingesteld en u kan geen verbinding met beheerde-exemplaar maken, controleert u of uw firewall open uitgaande verbinding op de SQL-poort 1433, evenals 11000 12000 reeks poorten voor de omleiding van heeft. 

## <a name="connect-an-azure-app-service-hosted-application"></a>Verbinding maken met een toepassing met Azure App Service die wordt gehost 

Beheerde exemplaar kan alleen worden benaderd via een particulier IP-adres zodat deze toegang tot van Azure App Service u eerst moet een verbinding tussen de toepassing en het exemplaar beheerd VNet te maken. Zie [uw app integreren met een Azure-netwerk](../app-service/web-sites-integrate-with-vnet.md).  
 
Zie voor het oplossen van problemen [probleemoplossing vnet's en toepassingen](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Als een verbinding kan niet worden gemaakt, probeert u [synchroniseren van de netwerkconfiguratie](sql-database-managed-instance-sync-network-configuration.md). 
 
Een speciaal geval van het Azure App Service verbinden met beheerde-exemplaar is wanneer u geïntegreerde dat Azure App Service met een netwerk peer is ingesteld in beheerde exemplaar VNet. Deze aanvraag moet de volgende configuratie wordt ingesteld: 

- Beheerde exemplaar VNet mag geen gateway  
- Beheerde exemplaar VNet moet hebben gebruik externe gateways optie is ingesteld 
- VNet brengen moet toestaan gateway onderweg optie zijn ingesteld 
 
Dit scenario wordt weergegeven in het volgende diagram:

![geïntegreerde app-peering](./media/sql-database-managed-instance/integrated-app-peering.png)
 
## <a name="connect-an-application-on-the-developers-box"></a>Verbinding maken met een toepassing in het vak voor ontwikkelaars 

Beheerde exemplaar kan alleen worden benaderd via een particulier IP-adres dus om te kunnen toegang krijgen vanaf uw developer-vak, moet u eerst een verbinding tussen uw developer-vak en het exemplaar beheerd VNet te maken.  
 
Een punt-naar-Site-verbinding met een VNet met behulp van systeemeigen Azure certificaat verificatie artikelen configureren ([Azure-portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md)) in detail weergeeft hoe het kan worden uitgevoerd.  

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over beheerde exemplaar [wat is er een exemplaar beheerd](sql-database-managed-instance.md).
- Zie voor een zelfstudie [maken van een exemplaar beheerd](sql-database-managed-instance-tutorial-portal.md).
