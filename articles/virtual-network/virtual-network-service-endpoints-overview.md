---
title: Azure virtual network service endpoints (Service-eindpunten voor virtuele netwerken van Azure) | Microsoft Docs
description: Ontdek hoe u directe toegang inschakelt voor Azure-resources via een virtueel netwerk met behulp van service-eindpunten.
services: virtual-network
documentationcenter: na
author: anithaa
manager: narayan
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: anithaa
ms.custom: 
ms.openlocfilehash: 7b5675dacd1d9effd73f3bc51ea4efc0ea6be029
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="virtual-network-service-endpoints-preview"></a>Virtual Network Service Endpoints (preview)

Met service-eindpunten van Virtual Network (VNet) kunt u de privé-adresruimte van uw virtuele netwerk en de identiteit van uw VNet uitbreiden naar Azure-services, via een directe verbinding. Met eindpunten kunt u uw kritieke Azure-serviceresources alleen beveiligen naar uw virtuele netwerken. Verkeer van uw VNet naar de Azure-service blijft altijd in het Microsoft Azure-backbonenetwerk.

Deze functie is beschikbaar als preview voor de volgende Azure-services en regio's:

- **Azure Storage**: alle regio's in de openbare cloud van Azure.
- **Azure SQL**: alle regio's in de openbare cloud van Azure.

Voor recente updates over de preview kijkt u op de pagina [Azure Virtual Network Updates](https://azure.microsoft.com/updates/?product=virtual-network) (Updates voor Azure Virtual Network).

>[!NOTE]
> Tijdens de preview heeft de functie mogelijk niet dezelfde beschikbaarheid en betrouwbaarheid als functies die al algemeen beschikbaar zijn. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="key-benefits"></a>Belangrijkste voordelen

Service-eindpunten bieden de volgende voordelen:

- **Verbeterde beveiliging voor uw Azure-serviceresources**: met service-eindpunten kunnen Azure-serviceresources op veilige wijze worden gekoppeld aan uw virtuele netwerk. Het koppelen van serviceresources aan een virtueel netwerk biedt extra beveiliging doordat de toegang tot resources via het openbare internet volledig wordt verwijderd en alleen verkeer vanaf uw virtuele netwerk wordt toegestaan.
- **Optimaal routeren van Azure-serviceverkeer vanaf uw virtuele netwerk**: tegenwoordig wordt in routes op uw virtuele netwerk die internetverkeer naar uw on-premises netwerken en/of virtuele apparaten afdwingen (ook wel geforceerd tunnelen genoemd), ook het verkeer van de Azure-service gedwongen om dezelfde route te nemen als het internetverkeer. Service-eindpunten bieden een optimale routering voor Azure-verkeer. 

  Eindpunten zorgen er altijd voor dat serviceverkeer rechtstreeks van uw virtuele netwerk naar de service op het Microsoft Azure-backbone-netwerk gaat. Door het verkeer op het Azure-backbone-netwerk in stand te houden, kunt u het uitgaande internetverkeer vanuit uw virtuele netwerk blijven controleren en bewaken via geforceerd tunnelen, zonder dat dit van invloed is op het serviceverkeer. Meer informatie over [door de gebruiker gedefinieerde routes en geforceerd tunnelen](virtual-networks-udr-overview.md).
- **Eenvoudig te installeren met minder beheeroverhead**: u hebt geen gereserveerde, openbare IP-adressen meer nodig in uw virtuele netwerken om Azure-resources via een IP-firewall te beveiligen. Er zijn geen NAT- of gatewayapparaten vereist voor het instellen van de service-eindpunten. Service-eindpunten worden geconfigureerd met een simpele klik op een subnet. Er is geen extra overhead nodig voor het onderhouden van de eindpunten.

## <a name="limitations"></a>Beperkingen

- De functie is alleen beschikbaar voor virtuele netwerken die zijn geïmplementeerd met behulp van het Azure Resource Manager-implementatiemodel.
- Eindpunten worden ingeschakeld in subnetten die zijn geconfigureerd in virtuele Azure-netwerken. Eindpunten kunnen niet worden gebruikt voor verkeer vanaf uw on-premises netwerk naar Azure-services. Zie [Toegang tot Azure-service vanaf on-premises beveiligen](#securing-azure-services-to-virtual-networks) voor meer informatie
- Een service-eindpunt geldt alleen voor Azure-serviceverkeer binnen de regio van een virtueel netwerk. Ter ondersteuning van RA-GRS- en GRS-verkeer voor Azure Storage worden eindpunten uitgebreid voor opname van gekoppelde regio's waar het virtuele netwerk is geïmplementeerd. Meer informatie over [gekoppelde gebieden in Azure](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).

## <a name="securing-azure-services-to-virtual-networks"></a>Azure-services aan virtuele netwerken koppelen

- Een virtueel netwerkservice-eindpunt biedt de identiteit van het virtuele netwerk voor de Azure-service. Nadat de service-eindpunten in uw virtuele netwerk zijn ingeschakeld, kunt u Azure-serviceresources aan uw virtuele netwerk koppelen door een regel voor virtuele netwerken aan de resources toe te voegen.
- Tegenwoordig maakt Azure-serviceverkeer vanaf een virtueel netwerk gebruik van openbare IP-adressen als IP-bronadressen. Bij gebruik van service-eindpunten schakelt serviceverkeer over op het gebruik van privéadressen van virtuele netwerken als bron-IP-adressen bij het toegang krijgen tot de Azure-service vanuit een virtueel netwerk. Hierdoor hebt u toegang tot de services zonder dat u gereserveerde openbare IP-adressen nodig hebt die worden gebruikt in IP-firewalls.
- __Toegang tot Azure-services vanaf on-premises beveiligen__:

  Standaard zijn Azure-serviceresources die zijn beveiligd naar virtuele netwerken, niet bereikbaar vanaf on-premises netwerken. Als u verkeer wilt toestaan vanaf on-premises netwerken, moet u ook openbare IP-adressen (doorgaans NAT) vanaf uw on-premises netwerken of ExpressRoute toestaan. Deze IP-adressen kunnen worden toegevoegd via de IP-firewallconfiguratie voor Azure-serviceresources.

  ExpressRoute: als u [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) gebruikt vanuit uw on-premises netwerk voor openbare peering of Microsoft-peering, moet u de NAT IP-adressen opgeven die worden gebruikt. Voor openbare peering gebruikt elk ExpressRoute-circuit standaard twee NAT IP-adressen. Deze worden toegepast op Azure-serviceverkeer wanneer het verkeer het Microsoft Azure-backbone-netwerk binnenkomt. Voor Microsoft-peering worden de NAT IP-adressen die worden gebruikt opgegeven door de klant of de serviceprovider. Voor toegang tot uw serviceresources moet u deze openbare IP-adressen toestaan in de instelling voor IP-firewall voor de resource. Wanneer u op zoek bent naar de IP-adressen van uw ExpressRoute-circuit, opent u [een ondersteuningsticket met ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) via de Azure Portal. Meer informatie over [NAT voor openbare peering en Microsoft-peering met ExpressRoute.](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering)

![Azure-services aan virtuele netwerken koppelen](./media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Configuratie

- Service-eindpunten worden geconfigureerd in een subnet van een virtueel netwerk. Eindpunten werken met alle soorten rekenprocessen die worden uitgevoerd in dat subnet.
- Slechts één service-eindpunt kan worden ingeschakeld voor een bepaalde service van een subnet. U kunt meerdere service-eindpunten voor alle ondersteunde Azure-services (bijvoorbeeld Azure Storage of Azure SQL Database) in een subnet configureren.
- Virtuele netwerken moeten zich in dezelfde regio bevinden als de Azure-serviceresource. Als u Azure Storage-accounts voor GRS en RA-GRS gebruikt, moet het hoofdaccount zich in dezelfde regio bevinden als het virtuele netwerk.
- Het virtuele netwerk waar het eindpunt is geconfigureerd, kan zich in hetzelfde abonnement bevinden als de Azure-serviceresource, maar ook in een ander abonnement. Zie [Inrichten](#Provisioning) voor meer informatie over de benodigde machtigingen voor het instellen van eindpunten en het koppelen van Azure-services.
- Voor ondersteunde services kunt u nieuwe of bestaande resources koppelen aan virtuele netwerken met behulp van service-eindpunten.

### <a name="considerations"></a>Overwegingen

- Nadat een service-eindpunt is ingeschakeld, schakelen de bron-IP-adressen van virtuele machines in het subnet over van het gebruik van openbare IPv4-adressen op het gebruik van het privé-IPv4-adres bij het communiceren met de service op dat subnet. Bestaande open TCP-verbindingen met de service worden hierbij gesloten. Zorg ervoor dat er geen kritieke taken worden uitgevoerd wanneer u een service-eindpunt voor een service voor een subnet in- of uitschakelt. Zorg er ook voor dat uw toepassingen automatisch verbinding kunnen maken met Azure-services nadat de wisseling van IP-adres heeft plaatsgevonden.

  Het wisselen van IP-adres heeft alleen gevolgen voor het serviceverkeer vanuit uw virtuele netwerk. Er zijn geen gevolgen voor ander verkeer van en naar de openbare IPv4-adressen die zijn toegewezen aan uw virtuele machines. Als u voor Azure-services bestaande firewallregels hebt waarin gebruik wordt gemaakt van openbare IP-adressen voor Azure, werken deze regels niet meer nadat is overgeschakeld op privéadressen van virtuele netwerken.
- Voor service-eindpunten blijven DNS-vermeldingen voor Azure-services ongewijzigd en worden deze nog steeds omgezet in openbare IP-adressen die zijn toegewezen aan de Azure-service.
- Netwerkbeveiligingsgroepen (NSG's) met de service-eindpunten:
  - NSG's laten standaard uitgaand internetverkeer toe en dus ook verkeer vanaf uw VNet naar Azure-services. Dit functioneert hetzelfde met service-eindpunten. 
  - Als u al het uitgaande internetverkeer wilt weigeren en alleen het verkeer naar specifieke Azure-services wilt toestaan, kunt u doen met behulp van __Azure-servicelabels__ in uw NSG's. U kunt ondersteunde Azure-services opgeven als bestemming in uw NSG-regels en het onderhoud van het IP-adres bij elk label wordt geregeld door Azure. Zie [Azure-servicelabels voor NSG's](https://aka.ms/servicetags) voor meer informatie. 

### <a name="scenarios"></a>Scenario's

- **Virtuele netwerken met peers, verbonden of meerdere virtuele netwerken**: als u Azure-services wilt koppelen aan meerdere subnetten binnen een virtueel netwerk of aan meerdere virtuele netwerken, kunt u in elk van de subnetten service-eindpunten inschakelen en de Azure-serviceresources vervolgens aan al deze subnetten koppelen.
- **Filteren van uitgaand verkeer vanuit een virtueel netwerk naar Azure-services**: als u het verkeer vanuit een virtueel netwerk naar een Azure-service wilt inspecteren of filteren, kunt u binnen het virtuele netwerk een virtueel-netwerkapparaat implementeren. U kunt dan service-eindpunten toepassen op het subnet waarop het virtueel-netwerkapparaat is geïmplementeerd en Azure-serviceresources alleen koppelen aan dit subnet. Dit scenario kan nuttig zijn als u de toegang tot de Azure-service vanuit uw virtuele netwerk wilt beperken tot specifieke Azure-resources, met behulp van virtueel-netwerkapparaatfilters. Zie [Egress with network virtual appliances](/azure/architecture/reference-architectures/dmz/nva-ha#egress-with-layer-7-nvas.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Uitgaand verkeer met virtueel-netwerkapparaten) voor meer informatie.
- **Rechtstreekse implementatie van Azure-resources naar services in virtuele netwerken**: diverse Azure-services kunnen rechtstreeks in specifieke subnetten in een virtueel netwerk worden geïmplementeerd. U kunt Azure-serviceresources koppelen aan subnetten voor [beheerde services](virtual-network-for-azure-services.md) door een service-eindpunt in te stellen in het subnet van deze beheerde services.

### <a name="logging-and-troubleshooting"></a>Logboekregistratie en problemen oplossen

Wanneer service-eindpunten zijn geconfigureerd voor een bepaalde service, controleert u of de route voor het service-eindpunt functioneert door het volgende te doen: 
 
- Valideer het IP-bronadres van een serviceaanvraag in servicediagnose. In alle nieuwe aanvragen met service-eindpunten wordt het IP-bronadres voor de aanvraag weergegeven als het privéadres van het virtuele netwerk, toegewezen aan de client die de aanvraag vanuit uw virtuele netwerk uitvoert. Zonder het eindpunt is het adres een openbaar IP-adres van Azure.
- Het weergeven van de effectieve routes in een netwerkinterface in een subnet. De route naar de service:
  - Toont een meer specifieke standaardroute naar de adresvoorvoegselbereiken van elke service
  - Heeft een nextHopType van *VirtualNetworkServiceEndpoint*
  - Geeft aan dat een meer rechtstreekse verbinding met de service actief is, vergeleken met routes met geforceerd tunnelen

>[!NOTE]
> Service-eindpuntroute overschrijft BGP- of UDR-routes voor de overeenkomst met het adresvoorvoegsel van een Azure-service. Meer informatie over [het oplossen van problemen met effectieve routes](virtual-network-routes-troubleshoot-portal.md#using-effective-routes-to-troubleshoot-vm-traffic-flow)

## <a name="provisioning"></a>Inrichten

Service-eindpunten kunnen afzonderlijk op virtuele netwerken worden geconfigureerd door een gebruiker met schrijftoegang tot een virtueel netwerk. Als u Azure-serviceresources aan een VNet wilt koppelen, moet u machtigingen hebben voor *Microsoft.Network/JoinServicetoaSubnet* voor de subnetten die worden toegevoegd. Deze machtiging is standaard opgenomen in de ingebouwde service-beheerdersrollen en kan worden gewijzigd door aangepaste rollen te maken.

Meer informatie over [ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Virtuele netwerken en Azure-serviceresources kunnen binnen hetzelfde abonnement of in verschillende abonnementen aanwezig zijn. Als de virtuele netwerken en Azure-serviceresources in verschillende abonnementen aanwezig zijn, moeten de resources tijdens de previewperiode onder dezelfde Active Directory-tenant (AD) vallen. 

## <a name="pricing-and-limits"></a>Prijzen en beperkingen

Er worden geen extra kosten berekend voor het gebruik van de service-eindpunten. Het huidige prijsmodel voor Azure-services (Azure Storage, Azure SQL Database) is van toepassing.

Er geldt geen limiet voor het totale aantal service-eindpunten in een virtueel netwerk.

Voor een Azure-serviceresource (zoals een Azure Storage-account) kunnen services beperkingen hebben met betrekking tot het aantal subnetten dat wordt gebruikt voor het koppelen van de resource. Raadpleeg de documentatie voor de verschillende services in [Volgende stappen](#next-steps) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie [Configure Virtual Network Service Endpoints](virtual-network-service-endpoints-configure.md) (Service-eindpunten voor virtuele netwerken configureren) voor meer informatie
- Zie [Secure an Azure Storage account to a virtual network](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Een Azure Storage-account aan een virtueel netwerk koppelen) voor meer informatie
- Zie [Secure an Azure SQL Database to a virtual network](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Een Azure SQL Database aan een virtueel netwerk koppelen) voor meer informatie
- Zie [Azure service integration in virtual networks](virtual-network-for-azure-services.md) (Integratie van Azure-services in virtuele netwerken) voor meer informatie
-  Snel starten: [Azure Resource Manager-sjabloon](https://azure.microsoft.com/en-us/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration) als u een service-eindpunt wilt instellen op het subnet van een VNet en het Azure Storage-account op dat subnet wilt beveiligen.

