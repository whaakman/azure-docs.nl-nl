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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 297c51cca31d981ffadd20cccce1cd3dd69dc81c
ms.contentlocale: nl-nl
ms.lasthandoff: 09/25/2017

---

# <a name="virtual-network-service-endpoints-preview"></a>Virtual Network Service Endpoints (preview)

Met service-eindpunten van Virtual Network (VNet) kunt u de privé-adresruimte van uw virtuele netwerk en de identiteit van uw VNet uitbreiden naar Azure-services, via een directe verbinding. Met eindpunten kunt u uw kritieke Azure-serviceresources alleen beveiligen naar uw virtuele netwerken. Verkeer van uw VNet naar de Azure-service blijft altijd in het Microsoft Azure-backbonenetwerk.

Deze functie is beschikbaar als __preview__ voor de volgende Azure-services en regio's:

__Azure Storage__: WestCentralUS, WestUS2, EastUS, WestUS, AustraliaEast, AustraliaSouthEast.

__Azure SQL Database__: WestCentralUS, WestUS2, EastUS.

Voor recente updates over de preview kijkt u op de pagina [Azure Virtual Network Updates](https://azure.microsoft.com/updates/?product=virtual-network).

>[!NOTE]
Tijdens de preview heeft de functie mogelijk niet dezelfde beschikbaarheid en betrouwbaarheid als functies die al algemeen beschikbaar zijn. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="key-benefits"></a>Belangrijkste voordelen

Service-eindpunten bieden de volgende voordelen:

- __Verbeterde beveiliging voor uw Azure-serviceresources__

  Met service-eindpunten kunnen Azure-serviceresources worden beveiligd naar uw virtuele netwerk. Dit biedt extra beveiliging doordat de toegang tot het openbare internet op deze resources volledig wordt verwijderd en alleen verkeer vanaf uw virtuele netwerk wordt toegestaan.

- __Optimale routering voor verkeer van de Azure-service vanaf uw virtuele netwerk__

  Tegenwoordig wordt in routes in uw VNet die internetverkeer via on-premises netwerken of virtuele apparaten afdwingen, ook wel geforceerde tunnels genoemd, ook het verkeer van de Azure-service gedwongen om dezelfde route te nemen als het internetverkeer. Service-eindpunten bieden een optimale routering voor Azure-verkeer. 

  Eindpunten zorgen altijd dat serviceverkeer rechtstreeks van uw VNet naar de service op het Microsoft Azure-backbonenetwerk gaat. Hierdoor kunt u doorgaan met het controleren en bewaken van uitgaand internetverkeer vanaf uw VNets, via geforceerde tunnels, zonder gevolgen voor het serviceverkeer. Meer informatie over [door de gebruiker gedefinieerde routes en geforceerde tunnels](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- __Eenvoudig te installeren met minder beheeroverhead__

  U hebt geen gereserveerde openbare IP-adressen in uw VNets meer nodig voor het beveiligen van Azure-resources via IP-firewall. Er zijn geen NAT- of gatewayapparaten vereist voor het instellen van de service-eindpunten. Service-eindpunt kunnen worden geconfigureerd met een simpele klik op een subnet. Er is geen extra overhead voor het onderhouden van de eindpunten.

## <a name="limitations"></a>Beperkingen

- De functie is alleen beschikbaar voor VNets die zijn geïmplementeerd met behulp van het Azure Resource Manager-implementatiemodel.
- Eindpunten zijn ingeschakeld op subnetten die zijn geconfigureerd in Azure VNets. Eindpunten kunnen niet worden gebruikt voor verkeer van uw on-premises netwerk naar Azure-services. Zie [Toegang tot Azure-service vanaf on-premises beveiligen](#securing-azure-services-to-virtual-networks) voor meer informatie.
- Service-eindpunt geldt alleen voor verkeer van Azure-service binnen de regio van het VNet. Voor Azure Storage moet ook het gekoppelde gebied worden opgenomen waar het VNet wordt geïmplementeerd, ter ondersteuning van RA-GRS- en GRS-verkeer. Meer informatie over [gekoppelde gebieden in Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions).

## <a name="securing-azure-services-to-virtual-networks"></a>Azure-services naar virtuele netwerken beveiligen

- VNet-service-eindpunt voorziet de Azure-service van de identiteit van uw VNet. Zodra de service-eindpunten zijn ingeschakeld in uw VNet, kunt u Azure-serviceresources naar uw VNet beveiligen door een regel voor virtuele netwerken toe te voegen aan de resources.
- Tegenwoordig gebruikt het verkeer van de Azure-service vanaf uw VNet openbare IP-adressen als bron-IP-adressen. Door de service-eindpunten schakelt het serviceverkeer naar het gebruik van VNet-privéadressen als bron-IP-adressen bij het krijgen van toegang tot de Azure-service vanaf uw VNet. Hierdoor hebt u toegang tot de services zonder dat u gereserveerde openbare IP-adressen nodig hebt die worden gebruikt in IP-firewalls.
- __Toegang tot Azure-services vanaf on-premises beveiligen__:
 
   Standaard zijn Azure-serviceresources die zijn beveiligd naar virtuele netwerken, niet bereikbaar vanaf on-premises netwerken. Als u verkeer wilt toestaan vanaf on-premises netwerken, moet u ook NAT IP-adressen vanaf uw on-premises netwerken of ExpressRoute-circuits toestaan. NAT IP-adressen kunnen worden toegevoegd via een IP-firewall-configuratie voor Azure-serviceresources.
  
   __ExpressRoute__: 

    Als u [ExpressRoute](/azure/expressroute/expressroute-introduction) (ER) gebruikt vanaf uw on-premises netwerk, gebruikt elk Expressroute-circuit twee NAT IP-adressen, die worden toegepast op Azure-serviceverkeer wanneer het verkeer het Microsoft Azure-backbonenetwerk binnengaat.  Voor toegang tot uw serviceresources moet u die twee IP-adressen toestaan in de instelling voor IP-firewall.  Wanneer u op zoek bent naar de IP-adressen van uw ExpressRoute-circuit, opent u [een ondersteuningsticket met ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) via de Azure Portal.

![Azure Services naar virtuele netwerken beveiligen](media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Configuratie

- Service-eindpunten worden geconfigureerd op een subnet in een VNet. Eindpunten werken met alle soorten rekenprocessen die worden uitgevoerd in dat subnet.
- Slechts één service-eindpunt kan worden ingeschakeld voor een bepaalde service van een subnet. U kunt meerdere service-eindpunten voor alle ondersteunde Azure-services (Say, Storage, Sql) op een subnet configureren.
- VNets moeten zich in dezelfde regio bevinden als de Azure-serviceresource. Voor Azure Storage, wanneer gebruik wordt gemaakt van GRS en RA-GRS-accounts, moet het hoofdaccount zich in dezelfde regio bevinden als het VNet.
- De VNet waar het eindpunt is geconfigureerd, kan zich bevinden in hetzelfde abonnement als of een ander abonnement dan de Azure-serviceresource. Zie [Inrichten](#Provisioning) hieronder voor meer informatie over de benodigde machtigingen voor het instellen van eindpunten en het beveiligen van Azure-services.
- Voor ondersteunde services kunt u nieuwe of bestaande resources naar VNets beveiligen met behulp van service-eindpunten.

### <a name="considerations"></a>Overwegingen

- Wanneer een service-eindpunt wordt ingeschakeld, schakelen de bron-IP-adressen van virtuele machines in het subnet over van het gebruik van openbare IPv4-adressen naar het gebruik van hun privé-IPv4-adres bij het communiceren met de service op dat subnet.
  - Bestaande open TCP-verbindingen met de service worden hierbij gesloten. Zorg ervoor dat er geen kritieke taken worden uitgevoerd wanneer u een service-eindpunt voor een service voor een subnet in- of uitschakelt. Zorg ook dat uw toepassingen automatisch verbinding kunnen maken met Azure-services nadat deze wisseling van IP-adres heeft plaatsgevonden.
  - Het wisselen van IP-adres heeft alleen gevolgen voor het serviceverkeer vanaf uw VNet. Er zijn geen gevolgen voor ander verkeer van en naar openbare IPV4-adressen die zijn toegewezen aan uw virtuele machines.
  - Als u voor Azure-services bestaande firewallregels hebt waarin gebruik wordt gemaakt van openbare IP-adressen voor Azure, werken deze regels niet meer nadat is overgeschakeld op privé-VNet-adressen.
- Voor service-eindpunten blijven DNS-vermeldingen voor Azure-services ongewijzigd en worden deze nog steeds omgezet in IP-adressen toegewezen aan de Azure-service.
- Netwerkbeveiligingsgroepen (NSG's) met de service-eindpunten:
  - NSG's laten standaard uitgaand internetverkeer toe en dus ook verkeer vanaf uw VNet naar Azure-services. Dit functioneert hetzelfde met service-eindpunten.
  - Als u al het uitgaande internetverkeer wilt weigeren en alleen het verkeer naar specifieke Azure-services wilt toestaan, kunt u doen met behulp van __Azure-servicelabels__ in uw NSG's. U kunt ondersteunde Azure-services opgeven als bestemming in uw NSG-regels en het onderhoud van het IP-adres bij elk label wordt geregeld door Azure. Zie [Azure-servicelabels voor NSG's](https://aka.ms/servicetags) voor meer informatie.

### <a name="scenarios"></a>Scenario's
- Peer-VNets, verbonden VNets of meerdere VNets:

Als u Azure-services wilt beveiligen naar meerdere subnetten binnen een VNet of op meerdere VNets, kunt u onafhankelijk service-eindpunten op elk van deze subnetten inschakelen en Azure-serviceresources beveiligen naar alle deze subnetten.

- Uitgaand verkeer van VNet tot Azure-services filteren:

Als u het verkeer dat is bestemd voor een Azure-service vanaf het virtuele netwerk wilt controleren of het filteren, kunt u een NVA (Network Virtual Appliance) binnen dat VNet implementeren. U kunt dan service-eindpunten toepassen op het subnet waarop de NVA is geïmplementeerd en Azure-serviceresources alleen naar dit subnet beveiligen. Dit scenario kan nuttig zijn als u de toegang tot de Azure-service vanaf uw VNet wilt beperken tot specifieke Azure-resources, met behulp van NVA-filters. Lees voor meer informatie het artikel [egress with NVAs (Uitgaand verkeer met NVA's)](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/nva-ha#egress-with-layer-7-nvas).

- Azure-resources beveiligen naar services die rechtstreeks zijn geïmplementeerd in VNets:

Verschillende Azure-services kunnen rechtstreeks worden geïmplementeerd in specifieke subnetten in uw VNets. U kunt Azure-serviceresources beveiligen naar subnetten voor [beheerde services](virtual-network-for-azure-services.md) door een service-eindpunt in te stellen op het subnet voor beheerde services.

### <a name="logging-and-troubleshooting"></a>Logboekregistratie en problemen oplossen

Wanneer service-eindpunten zijn geconfigureerd voor een bepaalde service, controleert u of de route voor het service-eindpunt functioneert door het volgende te doen: 

- __Controleer de diagnostische gegevens van de Azure-service__: u kunt bevestigen of de aanvraag voor toegang afkomstig is van uw privénetwerk, dat wil zeggen uw VNet, door de bron-IP van een serviceaanvraag in de diagnostische gegevens over de service te valideren. In alle nieuwe aanvragen met de service-eindpunten wordt de bron-IP voor de aanvraag weergegeven als het VNet-privéadres, toegewezen aan de client die de aanvraag vanaf uw VNet uitvoert. Zonder het eindpunt is dit adres een openbaar IP-adres van Azure.

- __Effectieve routes__ op een NIC in het subnet geven een meer specifieke 'standaardroute' weer voor het adresvoorvoegselbereik van die service. De route heeft een nextHopType als 'VirtualNetworkServiceEndpoint'. Deze route geeft aan dat een meer rechtstreekse verbinding met de service actief is, vergeleken met routes met geforceerde tunnels.

>[!NOTE]
Service-eindpuntroute overschrijft BGP- of UDR-routes voor de adresvoorvoegselovereenkomst, als de Azure-service. Meer informatie over [het oplossen van problemen met effectieve routes](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#using-effective-routes-to-troubleshoot-vm-traffic-flow)

## <a name="provisioning"></a>Inrichten

Service-eindpunten kunnen afzonderlijk op virtuele netwerken worden geconfigureerd door een gebruiker met schrijftoegang tot het virtuele netwerk.

Als u Azure-serviceresources naar een VNet wilt beveiligen, moet u machtigingen hebben voor Microsoft.Network/JoinServicetoaSubnet voor de subnetten die worden toegevoegd. Deze machtiging is standaard opgenomen in de ingebouwde service-beheerdersrollen en kan worden gewijzigd door aangepaste rollen te maken.

Meer informatie over [ingebouwde rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles).

VNets en Azure-serviceresources kunnen in hetzelfde abonnement of in verschillende abonnementen zitten. Als de VNet en Azure-serviceresources in verschillende abonnementen zitten, moeten de resources tijdens deze previewperiode onder dezelfde Active Directory-tenant (AD) zitten. 

## <a name="pricing-and-limits"></a>Prijzen en beperkingen

Er worden geen extra kosten berekend voor het gebruik van de service-eindpunten. Het huidige prijsmodel voor Azure-services (Azure Storage, Azure SQL Database) is van toepassing.

Er geldt geen limiet voor het totale aantal service-eindpunten in een VNet.

Voor een Azure-serviceresource (zoals Storage-account) kunnen services beperkingen hebben met betrekking tot het aantal subnetten dat wordt gebruikt voor het beveiligen van de resource. Raadpleeg de documentatie voor verschillende services in [Volgende stappen](#next%20steps) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [het configureren van VNet-service-eindpunten](virtual-network-service-endpoints-configure.md)
- Meer informatie over [het beveiligen van Azure Storage-accounts naar virtuele netwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security)
- Meer informatie over [het beveiligen van Azure SQL Database naar virtuele netwerken](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview)
- [Overzicht van de integratie van Azure-service voor virtuele netwerken](virtual-network-for-azure-services.md)


