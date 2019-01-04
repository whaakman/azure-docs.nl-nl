---
title: Azure virtual network service endpoints (Service-eindpunten voor virtuele netwerken van Azure) | Microsoft Docs
description: Ontdek hoe u directe toegang inschakelt voor Azure-resources via een virtueel netwerk met behulp van service-eindpunten.
services: virtual-network
documentationcenter: na
author: sumeetmittal
manager: narayan
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: sumeet.mittal
ms.custom: ''
ms.openlocfilehash: acb5897825a2a7762869d3ad961dfcdcb87071b4
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540551"
---
# <a name="virtual-network-service-endpoints"></a>Service-eindpunten voor virtueel netwerk

Met service-eindpunten van Virtual Network (VNet) kunt u de privé-adresruimte van uw virtuele netwerk en de identiteit van uw VNet uitbreiden naar Azure-services, via een directe verbinding. Met eindpunten kunt u uw kritieke Azure-serviceresources alleen beveiligen naar uw virtuele netwerken. Verkeer van uw VNet naar de Azure-service blijft altijd in het Microsoft Azure-backbonenetwerk.

Deze functie is beschikbaar voor de volgende Azure-services en regio's:

**Algemeen verkrijgbaar**

- **[Azure Storage](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network)**: Algemeen verkrijgbaar in alle Azure-regio's.
- **[Azure SQL Database](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Algemeen verkrijgbaar in alle Azure-regio's.
- **[Azure SQL Data Warehouse](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Algemeen verkrijgbaar in alle Azure-regio's.
- **[Azure Database for PostgreSQL-server](../postgresql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Algemeen verkrijgbaar in preview in Azure-regio's waar de databaseservice beschikbaar is.
- **[Azure Database for MySQL-server](../mysql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Algemeen verkrijgbaar in preview in Azure-regio's waar de databaseservice beschikbaar is.
- **[Azure Cosmos DB](../cosmos-db/vnet-service-endpoint.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Algemeen verkrijgbaar in alle Azure-regio's met een openbare cloud.
- **[Azure Key Vault](https://blogs.technet.microsoft.com/kv/2018/06/25/announcing-virtual-network-service-endpoints-for-key-vault-preview/)**: Algemeen verkrijgbaar in alle Azure-regio's met een openbare cloud.
- **[Azure Service Bus](../service-bus-messaging/service-bus-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Algemeen verkrijgbaar in alle Azure-regio's met een openbare cloud.
- **[Azure Event Hubs](../event-hubs/event-hubs-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Algemeen verkrijgbaar in alle Azure-regio's met een openbare cloud.

**Preview**

- **[Azure Data Lake Store Gen 1](../data-lake-store/data-lake-store-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Beschikbaar in preview.

Voor recente updates kijkt u op de pagina [Azure Virtual Network Updates](https://azure.microsoft.com/updates/?product=virtual-network) (Updates voor Azure Virtual Network).

## <a name="key-benefits"></a>Belangrijkste voordelen

Service-eindpunten bieden de volgende voordelen:

- **Verbeterde beveiliging voor Azure-serviceresources**: De ruimte met privéadressen voor een VNet kan overlappen en kan dus niet worden gebruikt om het verkeer dat afkomstig is van uw VNet, uniek aan te duiden. Service-eindpunten bieden de mogelijkheid om Azure-serviceresources aan uw virtuele netwerk te koppelen door VNet-identiteit uit te breiden naar de service. Nadat de service-eindpunten in uw virtuele netwerk zijn ingeschakeld, kunt u Azure-serviceresources aan uw virtuele netwerk koppelen door een regel voor virtuele netwerken aan de resources toe te voegen. Dit biedt verbeterde beveiliging doordat de toegang tot deze resources vanaf het openbare internet volledig wordt verwijderd en alleen verkeer vanuit uw virtuele netwerk wordt toegestaan.
- **Optimale routering voor verkeer van de Azure-service vanaf het virtuele netwerk**: Tegenwoordig wordt in routes in uw virtuele netwerk die internetverkeer via on-premises netwerken of virtuele apparaten afdwingen - ook wel geforceerde tunnels genoemd - ook het verkeer van de Azure-service gedwongen om dezelfde route te nemen als het internetverkeer. Service-eindpunten bieden een optimale routering voor Azure-verkeer. 

  Eindpunten zorgen er altijd voor dat serviceverkeer rechtstreeks van uw virtuele netwerk naar de service op het Microsoft Azure-backbone-netwerk gaat. Door het verkeer op het Azure-backbone-netwerk in stand te houden, kunt u het uitgaande internetverkeer vanuit uw virtuele netwerk blijven controleren en bewaken via geforceerd tunnelen, zonder dat dit van invloed is op het serviceverkeer. Meer informatie over [door de gebruiker gedefinieerde routes en geforceerd tunnelen](virtual-networks-udr-overview.md).
- **Eenvoudig te installeren met minder beheeroverhead**: U hebt geen gereserveerde openbare IP-adressen in uw virtuele netwerken meer nodig voor het beveiligen van Azure-resources via IP-firewall. Er zijn geen NAT- of gatewayapparaten vereist voor het instellen van de service-eindpunten. Service-eindpunten worden geconfigureerd met een simpele klik op een subnet. Er is geen extra overhead nodig voor het onderhouden van de eindpunten.

## <a name="limitations"></a>Beperkingen

- De functie is alleen beschikbaar voor virtuele netwerken die zijn geïmplementeerd met behulp van het Azure Resource Manager-implementatiemodel.
- Eindpunten worden ingeschakeld in subnetten die zijn geconfigureerd in virtuele Azure-netwerken. Eindpunten kunnen niet worden gebruikt voor verkeer vanaf uw on-premises netwerk naar Azure-services. Zie [Toegang tot Azure-service vanaf on-premises beveiligen](#securing-azure-services-to-virtual-networks) voor meer informatie
- Voor Azure SQL geldt een service-eindpunt alleen voor Azure-serviceverkeer binnen de regio van een virtueel netwerk. Voor Azure Storage worden eindpunten ter ondersteuning van RA-GRS- en GRS-verkeer ook uitgebreid voor opname van gekoppelde regio's waar het virtuele netwerk is geïmplementeerd. Meer informatie over [gekoppelde regio's in Azure](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).

## <a name="securing-azure-services-to-virtual-networks"></a>Azure-services aan virtuele netwerken koppelen

- Een virtueel netwerkservice-eindpunt biedt de identiteit van het virtuele netwerk voor de Azure-service. Nadat de service-eindpunten in uw virtuele netwerk zijn ingeschakeld, kunt u Azure-serviceresources aan uw virtuele netwerk koppelen door een regel voor virtuele netwerken aan de resources toe te voegen.
- Tegenwoordig maakt Azure-serviceverkeer vanaf een virtueel netwerk gebruik van openbare IP-adressen als IP-bronadressen. Bij gebruik van service-eindpunten schakelt serviceverkeer over op het gebruik van privéadressen van virtuele netwerken als bron-IP-adressen bij het toegang krijgen tot de Azure-service vanuit een virtueel netwerk. Hierdoor hebt u toegang tot de services zonder dat u gereserveerde openbare IP-adressen nodig hebt die worden gebruikt in IP-firewalls.

>[!NOTE]
> Met service-eindpunten worden de IP-bronadressen van de virtuele machines in het subnet voor serviceverkeer overgeschakeld van het gebruik van openbare IPv4-adressen naar IPv4-privéadressen. Bestaande firewallregels voor Azure-service die gebruikmaken van openbare IP-adressen werken na deze overschakeling niet meer. Zorg ervoor dat deze overschakeling is toegestaan op basis van de firewallregels voor Azure-service vóórdat u de service-eindpunten instelt. Er kan ook een tijdelijke onderbreking in het serviceverkeer van dit subnet optreden tijdens het configureren van de service-eindpunten. 
 
- __Toegang tot Azure-services vanaf on-premises beveiligen__:

  Standaard zijn Azure-serviceresources die zijn beveiligd naar virtuele netwerken, niet bereikbaar vanaf on-premises netwerken. Als u verkeer wilt toestaan vanaf on-premises netwerken, moet u ook openbare IP-adressen (doorgaans NAT) vanaf uw on-premises netwerken of ExpressRoute toestaan. Deze IP-adressen kunnen worden toegevoegd via de IP-firewallconfiguratie voor Azure-serviceresources.

  ExpressRoute: Als u [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) gebruikt vanuit uw on-premises netwerk voor openbare peering of Microsoft-peering, moet u de NAT IP-adressen opgeven die worden gebruikt. Voor openbare peering gebruikt elk ExpressRoute-circuit standaard twee NAT IP-adressen. Deze worden toegepast op Azure-serviceverkeer wanneer het verkeer het Microsoft Azure-backbone-netwerk binnenkomt. Voor Microsoft-peering worden de NAT IP-adressen die worden gebruikt opgegeven door de klant of de serviceprovider. Voor toegang tot uw serviceresources moet u deze openbare IP-adressen toestaan in de instelling voor IP-firewall voor de resource. Wanneer u op zoek bent naar de IP-adressen van uw ExpressRoute-circuit voor openbare peering, opent u [een ondersteuningsticket met ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) via de Azure-portal. Meer informatie over [NAT voor openbare peering en Microsoft-peering met ExpressRoute.](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering)

![Azure-services aan virtuele netwerken koppelen](./media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Configuratie

- Service-eindpunten worden geconfigureerd in een subnet van een virtueel netwerk. Eindpunten werken met alle soorten rekenprocessen die worden uitgevoerd in dat subnet.
- U kunt meerdere service-eindpunten voor alle ondersteunde Azure-services (bijvoorbeeld Azure Storage of Azure SQL Database) in een subnet configureren.
- Voor Azure SQL Database moeten virtuele netwerken zich in dezelfde regio bevinden als de Azure-serviceresource. Als u Azure Storage-accounts voor GRS en RA-GRS gebruikt, moet het hoofdaccount zich in dezelfde regio bevinden als het virtuele netwerk. Voor alle andere services kunnen Azure-serviceresources aan een virtueel netwerk in elke regio worden gekoppeld. 
- Het virtuele netwerk waar het eindpunt is geconfigureerd, kan zich in hetzelfde abonnement bevinden als de Azure-serviceresource, maar ook in een ander abonnement. Zie [Inrichten](#Provisioning) voor meer informatie over de benodigde machtigingen voor het instellen van eindpunten en het koppelen van Azure-services.
- Voor ondersteunde services kunt u nieuwe of bestaande resources koppelen aan virtuele netwerken met behulp van service-eindpunten.

### <a name="considerations"></a>Overwegingen

- Nadat een service-eindpunt is ingeschakeld, schakelen de bron-IP-adressen van virtuele machines in het subnet over van het gebruik van openbare IPv4-adressen op het gebruik van het privé-IPv4-adres bij het communiceren met de service op dat subnet. Bestaande open TCP-verbindingen met de service worden hierbij gesloten. Zorg ervoor dat er geen kritieke taken worden uitgevoerd wanneer u een service-eindpunt voor een service voor een subnet in- of uitschakelt. Zorg er ook voor dat uw toepassingen automatisch verbinding kunnen maken met Azure-services nadat de wisseling van IP-adres heeft plaatsgevonden.

  Het wisselen van IP-adres heeft alleen gevolgen voor het serviceverkeer vanuit uw virtuele netwerk. Er zijn geen gevolgen voor ander verkeer van en naar de openbare IPv4-adressen die zijn toegewezen aan uw virtuele machines. Als u voor Azure-services bestaande firewallregels hebt waarin gebruik wordt gemaakt van openbare IP-adressen voor Azure, werken deze regels niet meer nadat is overgeschakeld op privéadressen van virtuele netwerken.
- Voor service-eindpunten blijven DNS-vermeldingen voor Azure-services ongewijzigd en worden deze nog steeds omgezet in openbare IP-adressen die zijn toegewezen aan de Azure-service.

- Netwerkbeveiligingsgroepen (NSG's) met de service-eindpunten:
  - NSG's laten standaard uitgaand internetverkeer toe en dus ook verkeer vanaf uw VNet naar Azure-services. Dit functioneert hetzelfde met service-eindpunten. 
  - Als u al het uitgaande internetverkeer wilt weigeren en alleen het verkeer naar specifieke Azure-services wilt toestaan, kunt u dat doen met behulp van [servicelabels](security-overview.md#service-tags) in uw NSG's. U kunt ondersteunde Azure-services opgeven als bestemming in uw NSG-regels en het onderhoud van het IP-adres bij elk label wordt geregeld door Azure. Zie [Azure-servicelabels voor NSG's](security-overview.md#service-tags) voor meer informatie. 

### <a name="scenarios"></a>Scenario's

- **Virtuele netwerken met peers, verbonden of meerdere virtuele netwerken**: Als u Azure-services wilt koppelen aan meerdere subnetten binnen een virtueel netwerk of aan meerdere virtuele netwerken, kunt u in elk van de subnetten service-eindpunten inschakelen en de Azure-serviceresources vervolgens aan al deze subnetten koppelen.
- **Uitgaand verkeer van een virtueel netwerk naar Azure-services filteren**: Als u het verkeer dat is bestemd voor een Azure-service vanaf het virtuele netwerk, wilt controleren of filteren, kunt u een virtueel netwerkapparaat implementeren binnen het virtuele netwerk. U kunt dan service-eindpunten toepassen op het subnet waarop het virtueel-netwerkapparaat is geïmplementeerd en Azure-serviceresources alleen koppelen aan dit subnet. Dit scenario kan nuttig zijn als u de toegang tot de Azure-service vanuit uw virtuele netwerk wilt beperken tot specifieke Azure-resources, met behulp van virtueel-netwerkapparaatfilters. Zie [Egress with network virtual appliances](/azure/architecture/reference-architectures/dmz/nva-ha#egress-with-layer-7-nvas.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Uitgaand verkeer met virtueel-netwerkapparaten) voor meer informatie.
- **Azure-resources beveiligen naar services die rechtstreeks zijn geïmplementeerd in virtuele netwerken**: Verschillende Azure-services kunnen rechtstreeks worden geïmplementeerd in specifieke subnetten in een virtueel netwerk. U kunt Azure-serviceresources koppelen aan subnetten voor [beheerde services](virtual-network-for-azure-services.md) door een service-eindpunt in te stellen in het subnet van deze beheerde services.
- **Schijfverkeer vanaf een virtuele Azure-machine**: VM-schijfverkeer (inclusief koppelen en loskoppelen, diskIO) voor beheerde/niet-beheerde schijven, wordt niet beïnvloed door routeerwijzigingen op service-eindpunten voor Azure Storage. U kunt de REST-toegang tot pagina-blobs beperken om netwerken te selecteren, via service-eindpunten en [Azure Storage-netwerkregels](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

### <a name="logging-and-troubleshooting"></a>Logboekregistratie en problemen oplossen

Wanneer service-eindpunten zijn geconfigureerd voor een bepaalde service, controleert u of de route voor het service-eindpunt functioneert door het volgende te doen: 
 
- Valideer het IP-bronadres van een serviceaanvraag in servicediagnose. In alle nieuwe aanvragen met service-eindpunten wordt het IP-bronadres voor de aanvraag weergegeven als het privéadres van het virtuele netwerk, toegewezen aan de client die de aanvraag vanuit uw virtuele netwerk uitvoert. Zonder het eindpunt is het adres een openbaar IP-adres van Azure.
- Het weergeven van de effectieve routes in een netwerkinterface in een subnet. De route naar de service:
  - Toont een meer specifieke standaardroute naar de adresvoorvoegselbereiken van elke service
  - Heeft een nextHopType van *VirtualNetworkServiceEndpoint*
  - Geeft aan dat een meer rechtstreekse verbinding met de service actief is, vergeleken met routes met geforceerd tunnelen

>[!NOTE]
> Service-eindpuntroute overschrijft BGP- of UDR-routes voor de overeenkomst met het adresvoorvoegsel van een Azure-service. Meer informatie over [het oplossen van problemen met effectieve routes](diagnose-network-routing-problem.md)

## <a name="provisioning"></a>Inrichten

Service-eindpunten kunnen afzonderlijk op virtuele netwerken worden geconfigureerd door een gebruiker met schrijftoegang tot een virtueel netwerk. Als u Azure-serviceresources aan een VNet wilt koppelen, moet u machtigingen hebben voor *Microsoft.Network/JoinServicetoaSubnet* voor de subnetten die worden toegevoegd. Deze machtiging is standaard opgenomen in de ingebouwde service-beheerdersrollen en kan worden gewijzigd door aangepaste rollen te maken.

Meer informatie over [ingebouwde rollen](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Virtuele netwerken en Azure-serviceresources kunnen binnen hetzelfde abonnement of in verschillende abonnementen aanwezig zijn. Als de virtuele netwerken en Azure-serviceresources in verschillende abonnementen aanwezig zijn, moeten de resources onder dezelfde Active Directory-tenant (AD) vallen. 

## <a name="pricing-and-limits"></a>Prijzen en beperkingen

Er worden geen extra kosten berekend voor het gebruik van de service-eindpunten. Het huidige prijsmodel voor Azure-services (Azure Storage, Azure SQL Database enzovoort) is van toepassing.

Er geldt geen limiet voor het totale aantal service-eindpunten in een virtueel netwerk.

Voor een Azure-serviceresource (zoals een Azure Storage-account) kunnen services beperkingen hebben met betrekking tot het aantal subnetten dat wordt gebruikt voor het koppelen van de resource. Raadpleeg de documentatie voor de verschillende services in [Volgende stappen](#next-steps) voor meer informatie.

## <a name="virtual-network-service-endpoint-policies"></a>Beleid voor service-eindpunten voor Virtual Network 

Met beleid voor service-eindpunten voor Virtual Network kunt u via service-eindpunten verkeer van Virtual Network naar Azure-services filteren, waardoor alleen bepaalde Azure-serviceresources worden toegelaten. Beleid voor service-eindpunten zorgt voor nauwkeurig toegangsbeheer voor verkeer van Virtual Network naar Azure-services. Meer informatie: [Beleid voor service-eindpunten voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview)

## <a name="faqs"></a>Veelgestelde vragen

Raadpleeg de [veelgestelde vragen over service-eindpunten voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#virtual-network-service-endpoints) voor veelgestelde vragen

## <a name="next-steps"></a>Volgende stappen

- Zie [Configure Virtual Network Service Endpoints](tutorial-restrict-network-access-to-resources.md) (Service-eindpunten voor virtuele netwerken configureren) voor meer informatie
- Zie [Secure an Azure Storage account to a virtual network](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Een Azure Storage-account aan een virtueel netwerk koppelen) voor meer informatie
- Zie [Secure an Azure SQL Database to a virtual network](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Een Azure SQL Database aan een virtueel netwerk koppelen) voor meer informatie
- Zie [Secure an Azure SQL Data Warehouse to a virtual network](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json) (Een Azure SQL- datawarehouse aan een virtueel netwerk koppelen) voor meer informatie
- Zie [Azure service integration in virtual networks](virtual-network-for-azure-services.md) (Integratie van Azure-services in virtuele netwerken) voor meer informatie
- Meer informatie over [Beleid voor service-eindpunten voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview)
-  Quickstart: [Azure Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration) als u een service-eindpunt wilt instellen op het subnet van een VNet en een Azure Storage-account op dit subnet wilt beveiligen.

