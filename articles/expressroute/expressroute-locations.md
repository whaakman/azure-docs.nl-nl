---
title: 'Connectiviteitsproviders en -locaties: Azure ExpressRoute | Microsoft Docs'
description: Dit artikel bevat een gedetailleerd overzicht van de locaties waar services worden aangeboden en hoe u verbinding maakt met Azure-regio's. Gesorteerd op connectiviteitsprovider.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2017
ms.author: kaanan
ms.openlocfilehash: fef70ca56761565d7b2fa329a17929b978d54cb8
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="expressroute-partners-and-peering-locations"></a>Partners en peeringlocaties voor ExpressRoute

> [!div class="op_single_selector"]
> * [Locaties per provider](expressroute-locations.md)
> * [Providers per locatie](expressroute-locations-providers.md)


In de tabellen in dit artikel vindt u informatie over ExpressRoute-connectiviteitsproviders, geografische dekking van ExpressRoute, Microsoft Cloud-services die via ExpressRoute worden ondersteund en Expressroute SI's (System Integrator).

## <a name="partners"></a>ExpressRoute-connectiviteitsproviders
ExpressRoute wordt ondersteund in alle Azure-regio's en -locaties. De volgende kaart bevat een lijst van Azure-regio's en ExpressRoute-locaties. ExpressRoute-locaties zijn locaties waarop Microsoft samenwerkt met verschillende serviceproviders.

![Locatiekaart][0]

U hebt toegang tot Azure-services in alle regio's binnen een geopolitieke regio als u bent verbonden met ten minste één ExpressRoute-locatie in die geopolitieke regio.

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Azure-regio's naar ExpressRoute-locaties binnen een geopolitieke regio.
In de volgende tabel vindt u een toewijzing van Azure-regio's aan ExpressRoute-locaties binnen een geopolitieke regio.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Noord-Amerika** |VS - oost, VS - west, VS - oost 2, VS - west 2, VS - midden, Zuid-centraal VS, Noord-centraal VS, West-centraal VS, Canada Centraal, Canada Oost |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silicon Valley, Washington DC, Montreal, Quebec City, Toronto |
| **Zuid-Amerika** |Brazilië - zuid |Sao Paulo |
| **Europa** |Noord-Europa, West-Europa, Verenigd Koninkrijk - west, Verenigd Koninkrijk - zuid |Amsterdam, Dublin, Londen, Newport (Wales), Parijs |
| **Azië** |Oost-Azië, Zuidoost-Azië |Hongkong, Singapore |
| **Japan** |Japan - west, Japan - oost |Osaka, Tokio |
| **Australië** |Australië - zuidoost, Australië - oost |Melbourne, Sydney |
| **India** |India - west, India - midden, India - zuid |Chennai, Mumbai |
| **Zuid-Korea** |Korea Centraal, Korea Zuid |Busan, Seoul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regio's en geopolitieke grenzen voor nationale clouds
De volgende tabel bevat informatie over regio's en geopolitieke grenzen voor nationale clouds.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Cloud van de Amerikaanse overheid** |VS (overheid) - Arizona, VS (overheid) - Iowa, VS (overheid) - Texas, VS (overheid) - Virginia, VS DoD - centraal, VS DoD oost  |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **China** |China Noord, China Oost |Beijing, Shanghai |
| **Duitsland** |Duitsland Centraal, Duitsland Oost |Berlijn, Frankfurt |

Connectiviteit tussen de geopolitieke regio's wordt niet ondersteund op de standaard ExpressRoute-SKU. U moet de invoegtoepassing ExpressRoute Premium inschakelen voor ondersteuning van globale connectiviteit. Connectiviteit met nationale cloudomgevingen wordt niet ondersteund. U kunt met uw connectiviteitsprovider samenwerken als de noodzaak daartoe zich voordoet.

## <a name="locations"></a>Locaties van connectiviteitsproviders

De volgende tabel toont de locaties per serviceprovider. Als u alle beschikbare providers per locatie wilt weergeven, raadpleegt u [Serviceproviders per locatie](expressroute-locations-providers.md#locations).


### <a name="production-azure"></a>Productie-Azure
| **Serviceprovider** | **Microsoft Azure** | **Voor Office 365 en Dynamics 365** | **Locaties** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Ondersteund |Ondersteund |Melbourne, Sydney |
| **[Airtel](http://www.airtel.in/business/connexion)** | Ondersteund | Ondersteund | Chennai, Mumbai |
| **[Aryaka Networks](http://www.aryaka.com/)** |Ondersteund |Ondersteund |Amsterdam, Dallas, Hongkong, Silicon Valley, Singapore, Tokio, Washington DC |
| **[Ascenty Data Centers](https://ascenty.com/solucoes/conectividade-e-interconexoes/Microsoft-express-route/)** |Binnenkort beschikbaar |Binnenkort beschikbaar |Sao Paulo |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Dallas, Londen, Silicon Valley, Singapore, Sydney, Tokio, Toronoto, Washington DC |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Ondersteund |Ondersteund |Montreal, Toronto |
| **[British Telecom](http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** |Ondersteund |Ondersteund |Amsterdam, Hongkong, Londen, Silicon Valley, Singapore, Sydney, Tokio, Washington DC |
| **[C3ntro](https://c3ntro.com/)** |Binnenkort beschikbaar |Binnenkort beschikbaar |Miami |
| **[CenturyLink](http://www.centurylink.com/business/enterprise/services/data-network/mpls-vpn.html)** |Binnenkort beschikbaar |Binnenkort beschikbaar |Silicon Valley |
| **China Telecom Global** |Ondersteund |Niet ondersteund |Hongkong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Ondersteund |Ondersteund |Dallas, Montreal, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Ondersteund |Ondersteund |Amsterdam, Dublin, Londen, Parijs, Tokio |
| **[Comcast](https://business.comcast.com/landingpage/microsoft-azure)** |Ondersteund |Ondersteund |Chicago, Silicon Valley, Washington DC |
| **[Console](https://www.consoleconnect.com/partners/cloudsaas/)**| Ondersteund | Ondersteund |Silicon Valley, Toronto |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Ondersteund |Ondersteund |Chicago, Denver, Los Angeles, New York, Silicon Valley, Washinton DC |
| **Epsilon Global Communications** |Ondersteund |Ondersteund |Singapore |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Ondersteund |Ondersteund |Amsterdam, Atlanta, Chicago, Dallas, Hong Kong, Londen, Los Angeles, Melbourne, New York, Osaka, Parijs, Sao Paulo, Seattle, Silicon Valley, Singapore, Sydney, Tokio, Toronto, Washington DC |
| **euNetworks** |Ondersteund |Ondersteund |Amsterdam |
| **GÉANT** |Ondersteund |Ondersteund |Amsterdam |
| **[Global Cloud Xchange (GCX)] (http://globalcloudxchange.com/cloud-platform/cloud-x-fusion/cloud-x-fusion-for-azure/)** | Ondersteund| Ondersteund | Chennai, Mumbai |
| **[InterCloud](https://www.intercloud.com/)** |Ondersteund |Ondersteund |Amsterdam, Londen, Parijs, Singapore, Washington DC |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Ondersteund |Ondersteund |Osaka, Tokio |
| **Internet Solutions - Cloud Connect** |Ondersteund |Ondersteund |Amsterdam, Londen |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Ondersteund |Ondersteund |Amsterdam, Dublin, Londen, Parijs |
| **Jisc** |Ondersteund |Ondersteund |Londen |
| **KINX** |Ondersteund |Ondersteund |Seoul |
| **[KPN](http://www.kpn.com/cloudconnect)** | Ondersteund | Ondersteund | Amsterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Dallas, Las Vegas, Londen, Sao Paulo, Seattle, Silicon Valley, Singapore, Washington DC |
| **LG CNS** |Ondersteund |Ondersteund |Busan, Seoul |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Ondersteund |Ondersteund |Amsterdam, Atlanta, Chicago, Dallas, Hong Kong, Londen, Los Angeles, Melbourne, New York, Osaka, Parijs, Sao Paulo, Seattle, Silicon Valley, Singapore, Sydney, Tokio, Toronto, Washington DC |
| **MTN** |Ondersteund |Ondersteund |Londen |
| **[Neutrona Networks](http://www.neutrona.com/index.php/services#cloud-connect)** |Ondersteund |Ondersteund |Miami, Sao Paulo |
| **[Next Generation Data](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Ondersteund |Ondersteund |Newport (Wales) |
| **NEXTDC** |Ondersteund |Ondersteund |Melbourne, Sydney |
| **[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)** |Ondersteund |Ondersteund |Londen, Los Angeles, Osaka, Singapore, Tokio, Washington DC |
| **[NTT SmartConnect](http://cloud.nttsmc.com/cxc/azure.html)** |Ondersteund |Ondersteund |Osaka |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Ondersteund |Ondersteund |Amsterdam, Hong Kong, Londen, Parijs, Silicon Valley, Singapore, Sydney, Washington DC |
| **PCCW Global Limited** |Ondersteund |Ondersteund |Hongkong SAR |
| **Sejong Telecom** |Ondersteund |Ondersteund |Seoul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Ondersteund |Ondersteund |Chennai, Mumbai |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Ondersteund |Ondersteund |Singapore |
| **[Softbank](http://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Ondersteund |Ondersteund |Osaka, Tokio |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Ondersteund |Ondersteund |Amsterdam, Chennai, Hongkong, Londen, Mumbai, Silicon Valley, Singapore, Washington DC |
| **[TeleCity Group](http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** |Ondersteund |Ondersteund |Amsterdam, Dublin, Londen |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Ondersteund |Ondersteund |Amsterdam, Sao Paulo |
| **[Telehouse - KDDI](http://www.telehouse.net/solutions/cloud-services/cloud-link)** |Ondersteund |Ondersteund |Londen |
| **Telenor** |Ondersteund |Ondersteund |Amsterdam, Londen |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Ondersteund |Ondersteund |Melbourne, Sydney |
| **[Telus](http://www.telus.com)** |Ondersteund |Ondersteund |Toronto |
| **[UOLDIVEO](http://www.uoldiveo.com.br/solucoes/cloud.html#rmcl)** |Ondersteund |Ondersteund |Sao Paulo |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Dallas, Hongkong, Londen, Silicon Valley, Singapore, Sydney, Tokio, Washington DC |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Ondersteund |Niet ondersteund |Londen |
| **[Zayo Group](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Ondersteund |Ondersteund |Chicago, Dallas+, Londen+, Los Angeles, New York, Silicon Valley, Toronto, Washington DC |

 **+** betekent binnenkort beschikbaar

### <a name="national-cloud-environment"></a>Nationale cloudomgeving

### <a name="us-government-cloud"></a>Cloud van de Amerikaanse overheid
| **Serviceprovider** | **Microsoft Azure** | **Office 365** | **Locaties** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Ondersteund |Ondersteund |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Ondersteund |Ondersteund |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Ondersteund |Ondersteund |Chicago, New York+, Silicon Valley, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Ondersteund | Ondersteund | Chicago, Dallas |
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
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Ondersteund |Niet ondersteund |Berlijn |
| **Interxion** |Ondersteund |Niet ondersteund |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Ondersteund  | Niet ondersteund | Berlijn |
| **T-Systems** |Ondersteund |Niet ondersteund |Berlijn |

## <a name="connectivity-through-exchange-providers"></a>Connectiviteit via Exchange-providers

Als uw connectiviteitsprovider niet wordt vermeld in de vorige secties, kunt u alsnog verbinding maken.

* Neem contact op met uw connectiviteitsprovider om na te gaan of ze zijn verbonden met een van de exchange-punten in de bovenstaande tabel. Via de volgende koppelingen vindt u meer informatie over services die door de exchange-providers worden verstrekt. Verschillende connectiviteitsproviders zijn al verbonden met Ethernet-exchange-punten.
  * [Cologix](http://www.cologix.com/)
  * [Console](https://www.consoleconnect.com/partners/cloudsaas/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/products/interconnection/cloud-connect/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Vraag uw connectiviteitsprovider om uw netwerk uit te breiden tot de gewenste peeringlocatie.
  * Vergewis u ervan dat de connectiviteitsprovider uw connectiviteit uitbreidt op een maximaal beschikbare manier, zodat er geen storingspunten zijn.
* Vraag een ExpressRoute-circuit aan met het exchange-punt wanneer uw connectiviteitsprovider verbinding maakt met Microsoft.
  * Volg de stappen in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Een ExpressRoute-circuit maken) om connectiviteit in te stellen.

## <a name="connectivity-through-additional-service-providers"></a>Connectiviteit via additionele serviceproviders

| **Connectiviteitsprovider** | **Exchange** | **Locaties** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapore |
| **[Airgate Technologies, Inc.](http://airgate.ca/cloud-express/)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington DC |
| **[Arteria Networks Corporation](https://arteria-net.com/business/service/cloud_access/sca/)** |Equinix |Tokio |
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | Londen |
| **[BroadBand Tower, Inc.](http://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokio |
| **[C3ntro Telecom](http://www.c3ntro.com/data/cloud-conectivity/)** | Equinix, Megaport | Dallas |
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Dallas, Silicon Valley, Washington DC | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](http://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | London, Singapore, Washington DC |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Londen |
| **[Fastweb S.p.A](http://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[Gtt Communications Inc](https://www.gtt.net/wp-content/uploads/2017/04/EtherCloud-Data-Sheet.pdf)** |Equinix | Washington DC |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | Londen, Slough |
| **LGA Telecom** |Equinix |Singapore|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Chicago, New York, Washington DC |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hongkong SAR |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[NexGen Networks](http://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | Londen |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |  
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montreal, Toronto |
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | Londen | 
| **[Telia](https://www.telia.se/foretag/losningar/produkter-tjanster/datanet)** | Equinix | Amsterdam |
| **[ThinkTel](http://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapore |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silicon Valley, Washington DC |
| **Zain** |Equinix |Londen|
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Equinix | Montreal, Toronto |

## <a name="connectivity-through-datacenter-providers"></a>Connectiviteit via datacenterproviders
| **Provider** | **Exchange** |
| --- | --- |
| **[Cyrus One](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | Megaport |
| **[EdgeConnex](http://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport |
| **[RagingWire Data Centers](http://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | Console |
| **[T5 Datacenters](http://t5datacenters.com/network-cloud-connect/)** | Console |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Connectiviteit via nationale onderzoeks- en onderwijsnetwerken (NREN)

| **Provider**|
| --- |
| **AARNET**| 
| **DeIC, through GÉANT**|
| **GARR via GÉANT**|
| **GÉANT**|
| **HEAnet via GÉANT**|
| **JISC**|
| **RedIRIS via GÉANT**|
| **SINET**|
| **Surfnet via GÉANT**|

* Als uw connectiviteitsprovider hier niet wordt vermeld, controleert u of ze zijn verbonden aan een van de hierboven vermelde ExpressRoute Exchange-Partners.

## <a name="expressroute-system-integrators"></a>ExpressRoute-SI's
Het inschakelen van particuliere connectiviteit conform uw specifieke behoeften kan lastig zijn, al naargelang de schaal van uw netwerk. U kunt alle SI's uit de volgende tabel gebruiken om u te helpen met de voorbereidingen voor ExpressRoute.

| **System integrator** | **Continent** |
| --- | --- |
| **[Altogee](http://www.altogee.be/expressroute)** | Europa |
| **[Avanade Inc.](http://www.avanade.com/)** | Azië, Europa, Noord-Amerika, Zuid-Amerika |
| **[Bright Skies GmbH](http://bskies.io/expressroute)** | Europa
| **[Ensyst](http://www.ensyst.com.au)** | Azië
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | Noord-Amerika |
| **[FlexManage](http://www.flexmanage.com/cloud)** | Noord-Amerika |
| **[Inframon](http://www.inframon.com/partner/microsoft/)** | Europa |
| **[The IT Consultancy Group](http://itconsult.com.au/microsoft-expressroute)** | Australië |
| **[MOQdigital](http://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Australië |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Europa (Duitsland) |
| **[Nelite](http://nelite.com/)** | Europa |
| **[Nieuwe handtekening](https://www.newsignature.co.uk/)** | Europa |
| **[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Azië |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Europa |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Noord-Amerika |
| **[Presidio](http://info.presidio.com/microsoft-azure-expressroute)** | Noord-Amerika |
| **[sol-tec](http://www.sol-tec.com/Technologies)** | Europa |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Australië |


## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatiekaart"
