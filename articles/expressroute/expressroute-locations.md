---
title: ExpressRoute-locaties | Microsoft Docs
description: Dit artikel bevat een gedetailleerd overzicht van de locaties waar services worden aangeboden en hoe u verbinding maakt met Azure-regio&quot;s.
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b2d7ba14c7cbc5c082a5f962559936e1a60827f2
ms.openlocfilehash: fd018ed25e5c309a85bb2078d00f2bb0b504e5d2


---
# <a name="expressroute-partners-and-peering-locations"></a>Partners en peeringlocaties voor ExpressRoute
In de tabellen in dit artikel vindt u informatie over ExpressRoute-connectiviteitsproviders, geografische dekking van ExpressRoute, Microsoft Cloud-services die via ExpressRoute worden ondersteund en Expressroute SI's (System Integrator).

## <a name="a-namepartnersaexpressroute-connectivity-providers"></a><a name="partners"></a>ExpressRoute-connectiviteitsproviders
ExpressRoute wordt ondersteund in alle Azure-regio's en -locaties. De volgende kaart bevat een lijst van Azure-regio's en ExpressRoute-locaties. ExpressRoute-locaties zijn locaties waarop Microsoft samenwerkt met verschillende serviceproviders.

![Locatiekaart][0]

U hebt toegang tot Azure-services in alle regio's binnen een geopolitieke regio als u bent verbonden met ten minste één ExpressRoute-locatie in die geopolitieke regio.

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Azure-regio's naar ExpressRoute-locaties binnen een geopolitieke regio.
In de volgende tabel vindt u een toewijzing van Azure-regio's aan ExpressRoute-locaties binnen een geopolitieke regio.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Noord-Amerika** |VS - oost, VS - west, VS - oost 2, VS - midden, Zuid-centraal VS, Noord-centraal VS, Canada Centraal, Canada Oost |Atlanta, Chicago, Dallas, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal+, Quebec City+, Toronto |
| **Zuid-Amerika** |Brazilië - zuid |Sao Paulo |
| **Europa** |Noord-Europa, West-Europa, Verenigd Koninkrijk - west, Verenigd Koninkrijk - zuid |Amsterdam, Dublin, Londen, Newport (Wales), Parijs |
| **Azië** |Oost-Azië, Zuidoost-Azië |Hongkong, Singapore |
| **Japan** |Japan - west, Japan - oost |Osaka, Tokio |
| **Australië** |Australië - zuidoost, Australië - oost |Melbourne, Sydney |
| **India** |India - west, India - midden, India - zuid |Chennai, Mumbai |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regio's en geopolitieke grenzen voor nationale clouds
De volgende tabel bevat informatie over regio's en geopolitieke grenzen voor nationale clouds.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- | --- |
| **Cloud van de Amerikaanse overheid** |VS (overheid) - Iowa, VS (overheid) - Virginia |Chicago, Dallas, New York, Washington DC |
| **China** |China Noord, China Oost |Beijing, Shanghai |
| **Duitsland** |Duitsland Centraal, Duitsland Oost |Berlijn, Frankfurt |

Connectiviteit tussen de geopolitieke regio's wordt niet ondersteund op de standaard ExpressRoute-SKU. U moet de invoegtoepassing ExpressRoute Premium inschakelen voor ondersteuning van globale connectiviteit. Connectiviteit met nationale cloudomgevingen wordt niet ondersteund. U kunt met uw connectiviteitsprovider samenwerken als de noodzaak daartoe zich voordoet.

## <a name="a-namelocationsaconnectivity-provider-locations"></a><a name="locations"></a>Locaties van connectiviteitsproviders
> [!div class="op_single_selector"]
> * [Locaties per provider](expressroute-locations.md#locations)
> * [Providers per locatie](expressroute-locations-providers.md#locations)
>
>

### <a name="production-azure"></a>Productie-Azure
| **Serviceprovider** | **Microsoft Azure** | **Office 365 en CRM Online** | **Locaties** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Ondersteund |Ondersteund |Melbourne, Sydney |
| **[Aryaka Networks](http://www.aryaka.com/)** |Ondersteund |Ondersteund |Amsterdam, Dallas, Silicon Valley, Singapore, Tokio, Washington DC |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Dallas, Londen, Silicon Valley, Singapore, Sydney, Washington DC |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Ondersteund |Ondersteund |Toronto |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Ondersteund |Ondersteund |Amsterdam, Hongkong, Londen, Silicon Valley, Singapore, Sydney, Tokio, Washington DC |
| **[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)** |Binnenkort beschikbaar |Binnenkort beschikbaar |Silicon Valley |
| **China Telecom Global** |Ondersteund |Niet ondersteund |Hongkong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Ondersteund |Ondersteund |Dallas, Montreal+, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Ondersteund |Ondersteund |Amsterdam, Dublin, Londen, Tokio |
| **Comcast** |Ondersteund |Ondersteund |Chicago, Silicon Valley, Washington DC |
| **Console**| Ondersteund | Ondersteund |Silicon Valley |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Ondersteund |Ondersteund |Los Angeles |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Ondersteund |Ondersteund |Amsterdam, Atlanta, Chicago, Dallas, Hongkong, Londen, Los Angeles, Melbourne, New York, Osaka, Parijs+, Sao Paulo, Seattle, Silicon Valley, Singapore, Sydney, Tokio, Toronto, Washington DC |
| **euNetworks** |Ondersteund |Ondersteund |Amsterdam |
| **GÉANT** |Ondersteund |Ondersteund |Amsterdam |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Ondersteund |Ondersteund |Osaka, Tokio |
| **[InterCloud](https://www.intercloud.com/)** |Ondersteund |Ondersteund |Amsterdam, Londen, Singapore, Washington DC |
| **Internet Solutions - Cloud Connect** |Ondersteund |Ondersteund |Amsterdam, Londen |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)** |Ondersteund |Ondersteund |Amsterdam, Londen, Parijs |
| **Jisc** |Ondersteund |Ondersteund |Londen |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Dallas, Las Vegas+, Londen, Seattle, Silicon Valley, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Ondersteund |Ondersteund |Dallas, Hongkong, Las Vegas, Los Angeles, Melbourne, New York, Seattle, Singapore, Sydney, Toronto, Washington DC |
| **MTN** |Ondersteund |Ondersteund |Londen |
| **[Next Generation Data](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Ondersteund |Ondersteund |Newport (Wales) |
| **NEXTDC** |Ondersteund |Ondersteund |Melbourne, Sydney |
| **NTT Communications** |Ondersteund |Ondersteund |Londen, Los Angeles, Osaka, Singapore, Tokio, Washington DC |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Ondersteund |Ondersteund |Amsterdam, Hongkong, Londen, Silicon Valley, Singapore, Sydney, Washington DC |
| **PCCW Global Limited** |Ondersteund |Ondersteund |Hongkong |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Ondersteund |Ondersteund |Chennai |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Ondersteund |Ondersteund |Singapore |
| **Softbank** |Ondersteund |Ondersteund |Osaka, Tokio |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Ondersteund |Ondersteund |Amsterdam, Chennai, Hongkong, Londen, Mumbai, Silicon Valley, Singapore, Washington DC |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Ondersteund |Ondersteund |Amsterdam, Dublin, Londen |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Ondersteund |Ondersteund |Sao Paulo |
| **Telenor** |Ondersteund |Ondersteund |Amsterdam, Londen |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Ondersteund |Ondersteund |Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Ondersteund |Ondersteund |Amsterdam, Hongkong, Londen, Silicon Valley, Singapore, Sydney, Tokio, Washington DC |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Ondersteund |Niet ondersteund |Londen |
| **[Zayo Group](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Ondersteund |Ondersteund |Chicago, Los Angeles, New York, Silicon Valley Toronto, Washington DC |

 **+** betekent binnenkort beschikbaar

### <a name="national-cloud-environment"></a>Nationale cloudomgeving

### <a name="us-government-cloud"></a>Cloud van de Amerikaanse overheid
| **Serviceprovider** | **Microsoft Azure** | **Office 365** | **Locaties** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Ondersteund |Ondersteund |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Ondersteund |Ondersteund |Chicago, Dallas, New York, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Ondersteund |Ondersteund |Chicago, New York+, Washington DC |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Ondersteund |Ondersteund |Chicago, Dallas, New York, Washington DC |

### <a name="china"></a>China
| **Serviceprovider** | **Microsoft Azure** | **Office 365** | **Locaties** |
| --- | --- | --- | --- |
| **China Telecom** |Ondersteund |Niet ondersteund |Beijing, Shanghai |

Zie [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/) voor meer informatie.

### <a name="germany"></a>Duitsland
| **Serviceprovider** | **Microsoft Azure** | **Office 365** | **Locaties** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Ondersteund |Niet ondersteund |Berlijn+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Ondersteund |Niet ondersteund |Frankfurt |
| **e-shelter** |Ondersteund |Niet ondersteund |Berlijn |
| **Interxion** |Ondersteund |Niet ondersteund |Frankfurt |

## <a name="a-namenonpartnersaconnectivity-through-service-providers-not-listed"></a><a name="nonpartners"></a>Connectiviteit via niet vermelde serviceproviders
Als uw connectiviteitsprovider niet wordt vermeld in de vorige secties, kunt u alsnog verbinding maken.

* Neem contact op met uw connectiviteitsprovider om na te gaan of ze zijn verbonden met een van de exchange-punten in de bovenstaande tabel. Via de volgende koppelingen vindt u meer informatie over services die door de exchange-providers worden verstrekt. Verschillende connectiviteitsproviders zijn al verbonden met Ethernet-exchange-punten.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Vraag uw connectiviteitsprovider om uw netwerk uit te breiden tot de gewenste peeringlocatie.
  * Vergewis u ervan dat de connectiviteitsprovider uw connectiviteit uitbreidt op een maximaal beschikbare manier, zodat er geen storingspunten zijn.
* Vraag een ExpressRoute-circuit aan met het exchange-punt wanneer uw connectiviteitsprovider verbinding maakt met Microsoft.
  * Volg de stappen in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Een ExpressRoute-circuit maken) om connectiviteit in te stellen.

| **Connectiviteitsprovider** | **Exchange** | **Locaties** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapore |
| **[Arteria-Net](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokio |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Londen |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | Londen, Slough |
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix |New York, Washington DC |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix |Chicago, Silicon Valley, Washington DC |
| **[XO Communications](http://www.xo.com/)** |Equinix |Silicon Valley |
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |


## <a name="expressroute-system-integrators"></a>ExpressRoute-SI's
Het inschakelen van particuliere connectiviteit conform uw specifieke behoeften kan lastig zijn, al naargelang de schaal van uw netwerk. U kunt alle SI's uit de volgende tabel gebruiken om u te helpen met de voorbereidingen voor ExpressRoute.

| **System integrator** | **Continent** |
| --- | --- |
| **[Avanade Inc.](http://www.avanade.com/)** |Azië, Europa, VS |
| **[Dotnet Solutions](http://www.dotnetsolutions.co.uk/)** |Europa |
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** |VS |
| **[Nelite](http://nelite.com/)** |Europa |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** |Azië |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** |VS |
| **[Project Leadership](http://www.projectleadership.net/azure)** |VS |


## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatiekaart"



<!--HONumber=Dec16_HO3-->


