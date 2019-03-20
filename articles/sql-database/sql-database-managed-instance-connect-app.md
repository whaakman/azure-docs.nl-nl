---
title: Azure SQL Database Managed Instance-toepassing verbinden met | Microsoft Docs
description: In dit artikel wordt beschreven hoe u verbinding maken met uw toepassing naar Azure SQL Database Managed Instance.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 11/09/2018
ms.openlocfilehash: 9d47e3fb821e9ad64beecd62c274a33e7fa14041
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57887320"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Verbinding maken tussen uw toepassing en het beheerde exemplaar van Azure SQL Database

Nu hebt u meerdere opties bij het bepalen hoe en waar u uw toepassing wordt gehost. 
 
U kunt kiezen voor het hosten van de toepassing in de cloud met Azure App Service of een aantal van de Azure-netwerk (VNet) geïntegreerd opties, zoals Azure App Service-omgeving, virtuele Machine, virtuele-Machineschaalset opgehaald. U kunt ook hybride cloud-benadering en uw toepassingen on-premises houden. 
 
De keuze die u hebt gemaakt, kunt u deze naar een beheerd exemplaar.  

![hoge beschikbaarheid](./media/sql-database-managed-instance/application-deployment-topologies.png)  
## <a name="connect-an-application-inside-the-same-vnet"></a>Verbinding maken met een toepassing in hetzelfde VNet 

Dit scenario is de eenvoudigste. Virtuele machines binnen het VNet worden rechtstreeks met elkaar verbonden, zelfs als deze zich in verschillende subnetten. Dit betekent dat dat is alles wat u nodig voor de toepassing in een omgeving voor Azure-toepassingen of de virtuele Machine verbinden met de verbindingsreeks op de juiste wijze instellen.  
 
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

## <a name="connect-an-application-on-the-developers-box"></a>Verbinding maken met een toepassing in het vak voor ontwikkelaars

Voor het beheerde exemplaar kan alleen worden verkregen via een privé IP-adres dus als u wilt openen vanaf uw developer-box, moet u eerst een verbinding tussen uw developer-box en het beheerde exemplaar VNet te maken. Om dit te doen, een punt-naar-Site-verbinding met een VNet met behulp van systeemeigen Azure certificaatverificatie te configureren. Zie voor meer informatie, [configureren van een punt-naar-site-verbinding verbinding maken met een Azure SQL Database Managed Instance vanaf on-premises computer](sql-database-managed-instance-configure-p2s.md).

## <a name="connect-from-on-premises-with-vnet-peering"></a>Verbinding maken vanaf on-premises met VNet-peering
Een ander scenario geïmplementeerd door klanten is waarop het VPN-gateway is geïnstalleerd in een apart virtueel netwerk en een abonnement van de ene host Managed Instance. De twee virtuele netwerken worden vervolgens gekoppeld. Het volgende voorbeeld-Architectuurdiagram ziet hoe dit kan worden geïmplementeerd.

![VNet-peering](./media/sql-database-managed-instance-connect-app/vnet-peering.png)

Zodra u de basisinfrastructuur instellen hebt, moet u een instelling te wijzigen zodat de VPN-Gateway kunt zien de IP-adressen in het virtuele netwerk dat als host fungeert voor het beheerde exemplaar. Om dit te doen, breng de volgende zeer specifieke wijzigingen onder de **Peering instellingen**.
1.  In het VNet die als host fungeert voor de VPN-gateway, gaat u naar **Peerings**, klikt u vervolgens met het beheerde exemplaar gekoppeld VNet-verbinding, en klik vervolgens op **Gatewayoverdracht toestaan**.
2.  In het VNet die als host fungeert voor het beheerde exemplaar, gaat u naar **Peerings**, klikt u vervolgens op de VPN-Gateway gekoppeld VNet-verbinding, en klik vervolgens op **externe gateways gebruiken**.

## <a name="connect-an-azure-app-service-hosted-application"></a>Verbinding maken met een toepassing met Azure App Service die wordt gehost 

Voor het beheerde exemplaar kan alleen worden verkregen via een privé IP-adres om te kunnen openen vanaf Azure App Service moet u eerst een verbinding tussen de toepassing en het beheerde exemplaar VNet te maken. Zie [uw app integreren met een Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).  
 
Zie voor het oplossen van problemen [probleemoplossing vnet's en toepassingen](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Als u geen verbinding kan niet worden gemaakt, probeert u [synchroniseren van de netwerkconfiguratie](sql-database-managed-instance-sync-network-configuration.md). 
 
Een speciaal geval van Azure App Service verbinden met Managed Instance is wanneer u geïntegreerde dat Azure App Service met een netwerk aan elkaar gekoppeld naar beheerd exemplaar VNet. Deze aanvraag moet de volgende configuratie worden ingesteld: 

- Beheerde exemplaar VNet mag geen gateway  
- Beheerde exemplaar VNet moet optieset voor externe gateways gebruiken 
- Gekoppelde VNet moet toestaan gateway doorvoer optie is ingesteld 
 
In dit scenario is geïllustreerd in het volgende diagram:

![geïntegreerde app-peering](./media/sql-database-managed-instance/integrated-app-peering.png)

>[!NOTE]
>De VNet-integratie-functie is niet geïntegreerd voor een app met een VNet met een ExpressRoute-Gateway. De VNet-integratie werkt niet, zelfs als de ExpressRoute-Gateway is geconfigureerd in de co-existentie-modus. Als u nodig hebt voor toegang tot resources via een ExpressRoute-verbinding, kunt u een App Service-omgeving, die wordt uitgevoerd in uw VNet kunt gebruiken.
>
 
## <a name="troubleshooting-connectivity-issues"></a>Het oplossen van problemen met de netwerkverbinding

Voor het oplossen van problemen met de netwerkverbinding, controleert u het volgende:
- Als u geen verbinding maken met beheerd exemplaar van een virtuele machine van Azure binnen het hetzelfde VNet maar een ander subnet, moet u controleren of er een Netwerkbeveiligingsgroep die is ingesteld op VM-subnet dat toegang blokkeert. Daarnaast merk op dat u uitgaande verbindingen op de SQL-poort 1433, evenals poorten openen in bereik van 11000 12000 moet omdat die nodig zijn om verbinding te maken via een omleiding binnen de grenzen van Azure. 
- Zorg ervoor dat de doorgifte van BGP is ingesteld op **ingeschakeld** voor de routetabel die zijn gekoppeld aan het VNet.
- Als u P2S VPN, controleert u de configuratie in de Azure portal om te controleren of **Inkomend/uitgaand verkeer** getallen. Niet-nulwaarde getallen geven aan dat Azure routeren van verkeer naar/vanuit on-premises.

   ![Inkomend/uitgaand verkeer cijfers](./media/sql-database-managed-instance-connect-app/ingress-egress-numbers.png)

- Controleer of de clientcomputer (dat wordt uitgevoerd de VPN-client) routevermeldingen voor alle vnet's die u nodig hebt voor toegang tot heeft. De routes worden opgeslagen in `%AppData%\ Roaming\Microsoft\Network\Connections\Cm\<GUID>\routes.txt`.


   ![route.txt](./media/sql-database-managed-instance-connect-app/route-txt.png)

   Zoals u in deze afbeelding, zijn er twee vermeldingen voor elke VNet die betrokken zijn en een derde vermelding voor het VPN-eindpunt dat is geconfigureerd in de Portal.

   Een andere manier om te controleren of de routes is via de volgende opdracht uit. De uitvoer ziet u de routes naar de verschillende subnetten: 

   ```cmd
   C:\ >route print -4
   ===========================================================================
   Interface List
   14...54 ee 75 67 6b 39 ......Intel(R) Ethernet Connection (3) I218-LM
   57...........................rndatavnet
   18...94 65 9c 7d e5 ce ......Intel(R) Dual Band Wireless-AC 7265
   1...........................Software Loopback Interface 1
   Adapter===========================================================================
   
   IPv4 Route Table
   ===========================================================================
   Active Routes:
   Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0       10.83.72.1     10.83.74.112     35
         10.0.0.0    255.255.255.0         On-link       172.26.34.2     43
     
         10.4.0.0    255.255.255.0         On-link       172.26.34.2     43
   ===========================================================================
   Persistent Routes:
   None
   ```

- Als u VNet-peering, zorgt u ervoor dat u de instructies voor de instelling hebt gevolgd [Gatewayoverdracht toestaan en externe Gateways gebruiken](#connect-from-on-premises-with-vnet-peering). 

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
|SSMS   | 17.8.1 of [hoger](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) |

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is een beheerd exemplaar (preview)?](sql-database-managed-instance.md) voor meer informatie over beheerde exemplaren.
- Zie voor een zelfstudie wordt uitgelegd hoe u een nieuwe Managed Instance maakt u [maken van een beheerd exemplaar](sql-database-managed-instance-get-started.md).
