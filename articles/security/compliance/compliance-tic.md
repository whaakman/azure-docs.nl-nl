---
title: Richtlijnen voor vertrouwde verbindingen via Internet voor Azure
description: Vertrouwde verbindingen via Internet richtlijnen voor Azure en SaaS-services
services: security
author: dlapiduz
ms.assetid: 09511e03-a862-4443-81ac-ede815bdaf25
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: dlap
ms.openlocfilehash: f5efeabf3cf6d52f74aa2d064dc4c67c877d34e5
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55751919"
---
# <a name="trusted-internet-connections-guidance"></a>Richtlijnen voor vertrouwde verbindingen via Internet

In dit artikel bevat informatie over hoe overheidsinstellingen Microsoft Azure Government kunnen gebruiken om u te helpen bij het bereiken van naleving van het initiatief vertrouwde Internet verbindingen (TIC). Het artikel wordt beschreven hoe u Azure Government gebruiken in Azure-infrastructuur as a service (IaaS) en de Azure-platform als een service (PaaS)-aanbiedingen.

## <a name="trusted-internet-connections-overview"></a>Overzicht van vertrouwde verbindingen via Internet

Het doel van het initiatief boter is om te optimaliseren en te standaardiseren, de beveiliging van afzonderlijke externe netwerkverbindingen die worden gebruikt door federale instanties. Het beleid wordt beschreven in de [Office van beheer- en Budget (OMB) Memo M-08-05](https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy2008/m08-05.pdf).

In November 2007 de OMB tot stand gebracht boter wordt perimeterbeveiliging federale netwerk en de functies voor respons op incidenten te verbeteren. TIC is ontworpen voor netwerk-analyses van al het verkeer binnenkomende en uitgaande .gov om specifieke handtekeningen en patroon op basis van gegevens te identificeren. TIC kunt verwerven op basis gedragsafwijkingen, zoals botnet activiteit. Instanties zijn verplicht om te consolideren hun externe netwerkverbindingen en al het verkeer routeren via indringing detectie en preventie van apparaten EINSTEIN genoemd. De apparaten die worden gehost op een beperkt aantal netwerkeindpunten die worden aangeduid als _vertrouwde verbindingen via internet_.

Het doel van TIC is voor overheidsinstellingen weten:
- Wie komt in het netwerk (gemachtigd of niet-geautoriseerde)?
- Wanneer is mijn netwerk toegankelijk en waarom?
- Welke bronnen worden geopend?

Alle Bureau externe verbindingen moeten momenteel routeren via een boter OMB goedgekeurd. Federale instanties zijn vereist voor deelname aan het programma TIC als een boter Access Provider (TICAP), of door het contracteren van services met een van de belangrijkste laag 1 internet serviceproviders. Deze providers worden aangeduid als beheerd vertrouwde Internet Protocol Service (MTIPS)-providers. TIC bevat verplichte essentiële mogelijkheden die worden uitgevoerd door het Bureau en MTIPS-provider. In de huidige versie van TIC, de EINSTEIN versie 2 inbraakdetectie en EINSTEIN versie 3 versnelde (3A), inbraakdetectiehandtekeningen preventie apparaten worden geïmplementeerd op elke TICAP en MTIPS. Het Bureau stelt een "Memo van inzicht in' met de afdeling van binnenlandse beveiliging (DHS) om te implementeren EINSTEIN mogelijkheden voor federale systemen.

DHS vereist als onderdeel van haar verantwoordelijkheid om de .gov-netwerk te beschermen, de onbewerkte gegevens-feeds van Bureau net stroomgegevens voor het afstemmen van incidenten in de onderneming federale en analyses uitvoeren met behulp van speciale hulpprogramma's. DHS routers bieden de mogelijkheid voor het verzamelen van IP-netwerkverkeer als deze binnengaat of een interface verlaat. Netwerkbeheerders kunnen analyseren van de stroomgegevens net om te bepalen van de bron en bestemming van het verkeer, de klasse van de service, enzovoort. NET stroomgegevens wordt beschouwd als 'gegevens niet-inhoud' die vergelijkbaar is met de header, bron-IP, doel-IP, enzovoort. Gegevens van de niet-inhoud kunt DHS voor meer informatie over de inhoud: die is wat en hoe lang doen.

Het initiatief omvat ook beleidsregels voor veiligheid, richtlijnen en frameworks die wordt ervan uitgegaan on-premises infrastructuur dat. Zoals overheidsinstanties naar de cloud verplaatsen om kosten te besparen, operationele efficiëntie en innovatie te bereiken, kan de vereisten van de implementatie van TIC netwerkverkeer kunnen vertragen. De snelheid en flexibiliteit met welke government gebruikers toegang hun gegevens cloud-gebaseerde tot is daardoor beperkt.

## <a name="azure-networking-options"></a>Azure netwerkopties

Er zijn drie manieren verbinding maken met Azure-services:

- Directe verbinding met internet: Verbinding maken met Azure-services rechtstreeks via een open internetverbinding. Het medium en de verbinding zijn openbaar. Toepassings- en versleuteling op blokniveau transport zijn leveranciers vertrouwen privacy te waarborgen. Bandbreedte wordt beperkt door van een site-verbinding met internet. Meer dan één actieve provider gebruiken om ervoor te zorgen tolerantie.
- Virtueel particulier netwerk (VPN): Verbinding maken met uw Azure-netwerk privé met behulp van een VPN-gateway.
Het medium is openbaar, omdat deze over van een site internet-verbinding wordt verzonden, maar de verbinding is versleuteld in een tunnel privacy te waarborgen. Bandbreedte is beperkt, afhankelijk van de VPN-apparaten en de gekozen configuratie. Azure point-to-site-verbindingen zijn meestal beperkt tot 100 Mbps en site-naar-site-verbindingen zijn beperkt tot 1,25 Gbps.
- Azure ExpressRoute: ExpressRoute is een directe verbinding met Microsoft-services. Omdat de verbinding is via een geïsoleerde Fibre channel, kan de verbinding zijn openbaar of privé zijn afhankelijk van de configuratie die wordt gebruikt. De bandbreedte is meestal beperkt tot maximaal 10 Gbps.

Er zijn verschillende manieren om te voldoen aan de vereisten boter bijlage H (overwegingen voor Cloud) zoals opgegeven in het Ministerie van binnenlandse beveiliging van "Vertrouwde Internet-verbindingen (TIC) referentiedocument architectuur, versie 2.0." In dit artikel richtlijnen DHS boter wordt aangeduid als **TIC 2.0**.

Om in te schakelen van de verbinding van de **afdeling of Agency (D/A)** naar Azure of Office 365, zonder het routeren van verkeer via de TIC D/A de D/A moet gebruiken een gecodeerde tunnel of een specifieke verbinding met de cloudserviceprovider (CSP). De CSP-services kunnen controleren of de verbinding met de activa D/A cloud wordt niet aangeboden aan het openbare internet voor directe Bureau personeel toegang.

Office 365 is compatibel met TIC 2.0 bijlage H met behulp van een van beide ExpressRoute met [Microsoft-Peering](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings) ingeschakeld of een internetverbinding waarmee alle verkeer wordt versleuteld met behulp van TLS 1.2. D/A eindgebruikers helpen bij het netwerk D/een kunt maken verbinding via hun bureau Netwerk- en boter-infrastructuur via internet. Alle externe internettoegang tot Office 365 is geblokkeerd en wordt doorgestuurd via de instantie. De D/A kan ook verbinding maken met Office 365 via een ExpressRoute-verbinding met Microsoft-Peering (een type van de openbare peering) ingeschakeld.  

Voor Azure, de tweede optie (VPN) en de derde optie (ExpressRoute) kunnen voldoen aan deze vereisten wanneer ze worden gebruikt in combinatie met services die de toegang tot internet beperken.

![Microsoft vertrouwde verbinding met internet (TIC)](media/tic-diagram-a.png)

## <a name="azure-infrastructure-as-a-service-offerings"></a>Azure-infrastructuur als een service-aanbiedingen

Naleving van beleid met behulp van Azure IaaS boter is relatief eenvoudig omdat Azure-klanten hun eigen virtuele netwerk routeren beheren.

De belangrijkste vereiste om te garanderen van naleving van de referentiearchitectuur boter is om te controleren of dat het virtuele netwerk is een privé-uitbreiding van het netwerk D/A. Moet een _persoonlijke_ extensie, het beleid vereist dat geen verkeer laat uw netwerk, behalve via de on-premises boter-netwerkverbinding. Dit proces staat bekend als _geforceerde tunneling_. BOTER-naleving stuurt het proces al het verkeer van elk systeem in de CSP-omgeving via een on-premises gateway in een organisatienetwerk uit met het internet via de TIC.

Azure IaaS boter-naleving is onderverdeeld in twee belangrijke stappen:

- Stap 1: De configuratie.
- Stap 2: Controle.

### <a name="azure-iaas-tic-compliance-configuration"></a>Azure IaaS boter-naleving: Configuratie

Als u wilt configureren een boter-compatibele architectuur met Azure, moet u eerst te voorkomen dat directe toegang tot het internet met uw virtuele netwerk en vervolgens internetverkeer via de on-premises netwerk.

#### <a name="prevent-direct-internet-access"></a>Voorkomen dat directe toegang tot internet

Azure IaaS-netwerken wordt uitgevoerd via virtuele netwerken die bestaan uit subnetten waaraan de network interface controller (NIC's) van virtuele machines gekoppeld zijn.

Er is het eenvoudigste scenario boter-naleving kunt ondersteunen om te bevestigen dat een virtuele machine of een verzameling van virtuele machines, kan geen verbinding met externe bronnen maken. Het verbreken van de externe netwerken garandeert met behulp van netwerkbeveiligingsgroepen (nsg's). Nsg's gebruiken om verkeer naar een of meer NIC's of subnetten in het virtuele netwerk beheren. Een netwerkbeveiligingsgroep bevat toegangsbeheerregels waarmee verkeer wordt toegestaan of geweigerd op basis van verkeersrichting, protocol, bronadres en poort en doeladres en poort. U kunt de regels van een NSG op elk gewenst moment wijzigen en wijzigingen worden toegepast op alle bijbehorende instanties. Zie voor meer informatie over het maken van een NSG, [netwerkverkeer filteren met een netwerkbeveiligingsgroep](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

#### <a name="force-internet-traffic-through-an-on-premises-network"></a>Internetverkeer via een on-premises netwerk

Azure maakt automatisch systeemroutes en wijst de routes toe aan elk subnet in een virtueel netwerk. U kunt maken of verwijderen, maar u kunt bepaalde systeemroutes met aangepaste routes overschrijven. Azure maakt standaardsysteemroutes voor elk subnet. Wanneer u specifieke mogelijkheden van Azure, voegt Azure optionele standaardroutes op specifieke subnetten of elk subnet. Dit type Routering zorgt ervoor dat:
- Verkeer dat bestemd binnen het virtuele netwerk is blijft in het virtuele netwerk.
- IANA aangewezen privéadresruimten zoals 10.0.0.0/8 zijn verwijderd, tenzij ze zijn opgenomen in de adresruimte van het virtuele netwerk.
- "Laatste redmiddel" routering van 0.0.0.0/0 aan de internet-eindpunt voor virtueel netwerk.

![TIC geforceerde tunneling](media/tic-diagram-c.png)

Al het verkeer dat het virtuele netwerk moet routeren via de on-premises-verbinding, om ervoor te zorgen dat alle verkeer de TIC D/A passeert. U maakt aangepaste routes door gebruiker gedefinieerde routes maken of door het uitwisselen van routes tussen de gateway van uw on-premises netwerk en een Azure VPN-gateway Border Gateway Protocol (BGP). Zie voor meer informatie over de gebruiker gedefinieerde routes, [routering van verkeer van virtuele netwerken: Gebruiker gedefinieerde routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined). Zie voor meer informatie over het BGP [routering van verkeer van virtuele netwerken: Border Gateway Protocol](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol).

#### <a name="add-user-defined-routes"></a>Toevoegen van de gebruiker gedefinieerde routes

Als u een op route gebaseerde virtuele netwerkgateway gebruikt, kunt u geforceerde tunneling in Azure. Toevoegen van een gebruiker gedefinieerde route (UDR) die Hiermee stelt u 0.0.0.0/0 verkeer routeren naar een **volgende hop** van uw virtuele netwerkgateway. Azure bepaalt de volgorde van gebruiker gedefinieerde routes via het systeem gedefinieerde routes. Alle niet-virtuele netwerkverkeer wordt verzonden naar uw virtuele netwerkgateway die u kunt vervolgens het verkeer naar on-premises te routeren. Nadat u de UDR gedefinieerd, koppelt u de route met bestaande subnetten of nieuwe subnetten in alle virtuele netwerken in uw Azure-omgeving.

![gebruiker gedefinieerde routes en ticket m.](media/tic-diagram-d.png)

#### <a name="use-the-border-gateway-protocol"></a>Gebruik de Border Gateway Protocol

Als u ExpressRoute of de gateway van een virtueel netwerk BGP is ingeschakeld, is BGP de beste manier om routes te adverteren. Voor een BGP aangekondigd route 0.0.0.0/0, ExpressRoute- en BGP-compatibele virtuele netwerkgateways Zorg ervoor dat de standaard-route van toepassing op alle subnetten in uw virtuele netwerken.

### <a name="azure-iaas-tic-compliance-auditing"></a>Azure IaaS boter-naleving: Controleren

Azure biedt verschillende manieren boter naleving controleren.

#### <a name="view-effective-routes"></a>Effectieve routes weergeven

Bevestig dat de standaardroute wordt doorgegeven met inachtneming van de _effectieve routes_ voor een bepaalde virtuele machine, een specifieke NIC of een gebruiker gedefinieerde routetabel in [de Azure-portal](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#diagnose-using-azure-portal) of in [ Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-powershell#diagnose-using-powershell). De **effectieve Routes** weergeven van de betreffende gebruiker gedefinieerde routes, BGP routes en systeemroutes die betrekking hebben op de relevante entiteit aangekondigd, zoals wordt weergegeven in de volgende afbeelding:

![Effectieve routes](media/tic-screen-1.png)

> [!NOTE]
> U kunt de effectieve routes niet weergeven voor een NIC, tenzij de NIC gekoppeld aan een actieve virtuele machine is.

#### <a name="use-azure-network-watcher"></a>Gebruik Azure Network Watcher

Azure Network Watcher biedt verschillende hulpprogramma's voor het TIC naleving controleren. Zie voor meer informatie, [in dit overzicht over Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).

##### <a name="capture-network-security-group-flow-logs"></a>Stroomlogboeken van netwerk vastleggen 

Gebruik van Network Watcher om vast te leggen van de netwerk-stroomlogboeken die wijzen op de metagegevens die wordt omgeven door IP-verkeer. De logboeken van de stroom netwerk bevatten de bron- en -adressen van doelen en andere gegevens. U kunt deze gegevens gebruiken met de logboeken van de virtuele netwerkgateway, on-premises apparaten of de boter, om te controleren dat al het verkeer on-premises routes van de rand. 

## <a name="azure-platform-as-a-service-offerings"></a>Azure-platform als een service-aanbiedingen

Azure PaaS-services, zoals Azure Storage zijn toegankelijk via een via internet bereikbaar-URL. Iedereen met goedgekeurde referenties hebben toegang tot de resource, zoals een storage-account vanaf elke locatie zonder het doorlopen van een boter. Daarom sluiten veel overheidsklanten ten onrechte dat Azure PaaS-services niet compatibel zijn met boter-beleid. Veel Azure PaaS-services kunnen worden compatibel zijn met boter-beleid. Een service is compatibel wanneer de architectuur hetzelfde als de boter-compatibele IaaS-omgeving is ([zoals eerder beschreven](https://docs.microsoft.com/azure/security/compliance/compliance-tic#azure-infrastructure-as-a-service-offerings)) en de service is gekoppeld aan een Azure-netwerk.

Wanneer Azure PaaS-services zijn geïntegreerd met een virtueel netwerk, is de service is privé toegankelijk is vanaf dit virtuele netwerk. U kunt toepassen om aangepaste routering voor 0.0.0.0/0 via de gebruiker gedefinieerde routes of BGP. Aangepaste routering, zorgt u ervoor dat alle voor internet bestemde verkeer routeert on-premises naar de boter passeren. Azure-services integreren met virtuele netwerken met behulp van de volgende patronen:

- **Een toegewezen exemplaar van een service implementeren**: Er zijn een toenemend aantal PaaS-services kan worden geïmplementeerd als toegewezen instanties met virtuele NAS-eindpunten. U kunt een App Service Environment voor PowerApps implementeren in de modus 'Geïsoleerd' om toe te staan de netwerkeindpunt om te worden beperkt tot een virtueel netwerk. De App Service-omgeving kunnen veel Azure PaaS-services, zoals Azure Web Apps, Azure API Management en Azure Functions kunt hosten.
- **Gebruik virtual network-service-eindpunten**: Een toenemend aantal PaaS-services kunt de optie voor het verplaatsen van hun eindpunt naar een virtueel netwerk privé IP-adres in plaats van een openbaar adres.

Services die ondersteuning bieden voor implementatie van toegewezen instanties in een virtueel netwerk of het gebruik van service-eindpunten vanaf mei 2018, worden vermeld in de volgende tabellen.

> [!Note]
> De beschikbaarheidsstatus komt overeen met de Azure-services die verkrijgbaar zijn. De status van de beschikbaarheid voor Azure-services in Azure Government is in behandeling.

### <a name="support-for-service-endpoints"></a>Ondersteuning voor service-eindpunten

|Service                        |Beschikbaarheid      |
|-------------------------------|------------------|
|Azure Key Vault                | Beperkte Preview-versie  |
|Azure Cosmos DB                | Beperkte Preview-versie  |
|Identiteiten              | Beperkte Preview-versie  |
|Azure Data Lake                | Beperkte Preview-versie  |
|Azure Database for PostgreSQL  | Beperkte Preview-versie  |
|Azure Database for MySQL       | Beperkte Preview-versie  |
|Azure SQL Data Warehouse       | Openbare preview   |
|Azure SQL Database             | Algemene beschikbaarheid (GA) |
|Azure Storage                  | Algemene beschikbaarheid               |

### <a name="support-for-virtual-network-injection"></a>Ondersteuning voor virtueel netwerk-injectie

|Service                               |Beschikbaarheid      |
|--------------------------------------|------------------|
|Azure SQL Database Managed Instance   | Openbare preview   |
|Azure Kubernetes Service (AKS)        | Openbare preview   |
|Azure Service Fabric                  | Algemene beschikbaarheid               |
|Azure API Management                  | Algemene beschikbaarheid               |
|Azure Active Directory                | Algemene beschikbaarheid               |
|Azure Batch                           | Algemene beschikbaarheid               |
|App Service-omgeving               | Algemene beschikbaarheid               |
|Azure Cache voor Redis                     | Algemene beschikbaarheid               |
|Azure HDInsight                       | Algemene beschikbaarheid               |
|Schaalset voor virtuele machines             | Algemene beschikbaarheid               |
|Azure Cloud Services                  | Algemene beschikbaarheid               |


### <a name="virtual-network-integration-details"></a>Details van de integratie van virtueel netwerk

Het volgende diagram toont de algemene netwerkstroom voor toegang tot PaaS-services. Toegang wordt van virtueel netwerk-injectie en virtueel netwerk-servicetunneling weergegeven. Zie voor meer informatie over service-netwerkgateways, virtuele netwerken en servicetags, [netwerk en beveiligingsgroepen voor toepassing: Servicetags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

![PaaS-connectiviteitsopties voor ticket m.](media/tic-diagram-e.png)

1. Een persoonlijke verbinding wordt gemaakt naar Azure met behulp van ExpressRoute. ExpressRoute-privépeering met geforceerde tunnels wordt gebruikt om alle virtuele netwerkverkeer afdwingen via ExpressRoute en back-ups maken met on-premises. Microsoft-Peering is niet vereist.
2. Azure VPN-Gateway, wanneer gebruikt in combinatie met ExpressRoute- en Microsoft-Peering kunt end-to-end IPsec-versleuteling tussen het virtuele netwerk van de klant en de rand van het on-premises overlay. 
3. De netwerkverbinding met het virtuele netwerk van de klant wordt beheerd met behulp van NSGs waarmee klanten voor het toestaan/weigeren op basis van IP, poort en protocol.
4. Het virtuele netwerk van de klant is een uitbreiding naar de PaaS-service door het maken van een service-eindpunt voor de service van de klant.
5. Het PaaS-service-eindpunt is beveiligd op **standaard Alles weigeren** en alleen toegang toestaan vanaf de opgegeven subnetten binnen het virtuele netwerk van de klant. De standaardwaarde weigert ook inclusief verbindingen die afkomstig van het internet zijn.
6. Andere Azure-services die nodig hebt voor toegang tot resources binnen het virtuele netwerk van de klant moet:  
   - Rechtstreeks geïmplementeerd in het virtuele netwerk.
   - Selectief toegestaan, op basis van de richtlijnen van de betreffende Azure-service.

#### <a name="option-a-deploy-a-dedicated-instance-of-a-service-virtual-network-injection"></a>Optie A: Een toegewezen exemplaar van een service (virtueel netwerk injectie) implementeren

Virtueel netwerk injectie kan klanten selectief toegewezen instanties van een bepaald Azure-service, zoals HDInsight, in hun eigen virtuele netwerk te implementeren. Service-exemplaren zijn geïmplementeerd in een toegewezen subnet in het virtuele netwerk van een klant. Virtueel netwerk injectie biedt toegang tot serviceresources via de niet-routeerbare internetadressen. On-premises instanties gebruik ExpressRoute of een site-naar-site-VPN rechtstreeks toegang krijgen tot exemplaren van de service via de adresruimte van virtueel netwerk, in plaats van een firewall op openbare internet-adresruimte te openen. Wanneer een toegewezen exemplaar wordt gekoppeld aan een eindpunt, kunt u de dezelfde strategieën voor IaaS boter-naleving. Standaardroutering zorgt ervoor dat internetverkeer wordt omgeleid naar een virtuele netwerkgateway die gekoppeld voor on-premises. U kunt verder binnenkomende en uitgaande toegang tot en met nsg's voor het opgegeven subnet beheren.

![Overzicht van Virtual network-injectie](media/tic-diagram-f.png)

#### <a name="option-b-use-virtual-network-service-endpoints-service-tunnel"></a>Optie B: Gebruik virtual network-service-eindpunten (servicetunnel)

Een toenemend aantal multitenant Azure-services bieden "service-eindpunten." Service-eindpunten zijn een alternatieve methode voor het integreren van Azure-netwerk. Virtual network-service-eindpunten Breid uw IP-adresruimte van virtueel netwerk en de identiteit van het virtuele netwerk naar de service via een directe verbinding. Verkeer van het virtuele netwerk naar de Azure-service blijft altijd in de Azure-backbone-netwerk. 

Nadat u een service-eindpunt voor een service hebt ingeschakeld, gebruikt u de beleidsregels die worden weergegeven door de service-verbindingen voor de service beperken tot dit virtuele netwerk. Toegangscontroles worden afgedwongen in het platform door de Azure-service. Toegang tot een vergrendelde resource wordt alleen verleend als de aanvraag afkomstig is van het toegestane virtuele netwerk of subnet of van de twee IP-adressen die worden gebruikt voor identificatie van uw on-premises-verkeer als u ExpressRoute gebruikt. Gebruik deze methode om effectief te voorkomen dat binnenkomend en uitgaand verkeer rechtstreeks verlaten van de PaaS-service.

![Overzicht van de service-eindpunten](media/tic-diagram-g.png)

## <a name="cloud-native-tools-for-network-situational-awareness"></a>Cloud-eigen hulpprogramma's voor het netwerk verzamelen awareness

Azure biedt cloud-eigen hulpprogramma's om ervoor te zorgen dat u het verzamelen bewustzijn die vereist zijn om te begrijpen van de verkeersstromen van uw netwerk hebt. De hulpprogramma's zijn niet vereist voor de naleving boter-beleid. De hulpprogramma's voor kunnen uw beveiligingsmogelijkheden aanzienlijk verbeteren.

### <a name="azure-policy"></a>Azure Policy

[Azure Policy](https://azure.microsoft.com/services/azure-policy/) is een Azure-service die uw organisatie biedt betere mogelijkheid om te controleren en afdwingen van nalevingsinitiatieven. Azure Policy is momenteel beschikbaar in openbare preview in Azure-services die verkrijgbaar zijn. Azure Policy is nog niet beschikbaar in Azure Government. Klanten kunnen plannen en hun Azure Policy-regels nu testen om ervoor te zorgen boter naleving in de toekomst. 

Azure Policy is gericht op het abonnementsniveau. De service biedt een centrale interface waar u de taken voor naleving, waaronder kunt uitvoeren:
- Initiatieven beheren
- Beleidsdefinities configureren
- Naleving controleren
- Afdwingen van naleving
- Uitzonderingen beheren

Beheerders kunnen hun eigen aangepaste definities samen met veel ingebouwde definities, met behulp van eenvoudige JSON-sjablonen definiëren. Microsoft raadt aan om de prioriteit van de controle op naleving, waar mogelijk.

Het volgende voorbeeldsbeleid kunnen worden gebruikt voor boter naleving scenario's:

|Beleid  |Voorbeeldscenario  |Template  |
|---------|---------|---------|
|Afgedwongen door gebruiker gedefinieerde routetabel. | Zorg ervoor dat de standaardroute op alle virtuele netwerken naar een goedgekeurde virtuele netwerkgateway verwijst voor de routering naar on-premises.    | Aan de slag met dit [sjabloon](https://docs.microsoft.com/azure/azure-policy/scripts/no-user-def-route-table). |
|Audit als Network Watcher is niet ingeschakeld voor een regio.  | Zorg ervoor dat de Network Watcher is ingeschakeld voor alle regio's gebruikt.  | Aan de slag met dit [sjabloon](https://docs.microsoft.com/azure/azure-policy/scripts/net-watch-not-enabled). |
|NSG x in elk subnet.  | Zorg ervoor dat er een NSG (of een set van goedgekeurde nsg's) met internetverkeer geblokkeerd wordt toegepast op alle subnetten in elk virtueel netwerk. | Aan de slag met dit [sjabloon](https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-subnet). |
|NSG x op elke NIC wordt gebruikt. | Zorg ervoor dat er een NSG met internetverkeer geblokkeerd wordt toegepast op alle NIC's op alle virtuele machines. | Aan de slag met dit [sjabloon](https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-nic). |
|Gebruik een virtueel netwerk van goedgekeurde voor netwerkinterfaces van virtuele machines.  | Zorg ervoor dat alle NIC's in een goedgekeurde-netwerk. | Aan de slag met dit [sjabloon](https://docs.microsoft.com/azure/azure-policy/scripts/use-approved-vnet-vm-nics). |
|Toegestane locaties. | Zorg ervoor dat alle resources worden geïmplementeerd in de regio's met compatibele virtuele netwerken en configuratie van Network Watcher.  | Aan de slag met dit [sjabloon](https://docs.microsoft.com/azure/azure-policy/scripts/allowed-locs). |
|Niet toegestane resourcetypen, zoals **PublicIPs**. | De implementatie van resourcetypen waarvoor geen een plan naleving verbieden. Dit beleid gebruiken om te voorkomen dat de implementatie van resources voor openbaar IP-adres. Terwijl de NSG-regels kunnen worden gebruikt voor het effectief geblokkeerd binnenkomend verkeer van internet, vermindert zo wordt voorkomen dat het gebruik van openbare IP-adressen verder de kwetsbaarheid voor aanvallen.   | Aan de slag met dit [sjabloon](https://docs.microsoft.com/azure/azure-policy/scripts/not-allowed-res-type).  |

### <a name="network-watcher-traffic-analytics"></a>Network Watcher-traffic analytics

Network Watcher [traffic analytics](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) stroom logboekgegevens en andere logboeken voor een overzicht op hoog niveau van het netwerkverkeer in beslag neemt. De gegevens zijn handig voor het controleren van boter-naleving en voor het identificeren van knelpunten. Het dashboard op hoog niveau kunt u snel de virtuele machines die communiceren met internet en een gerichte lijst voor de routering van boter scherm.

![Verkeersanalyse](media/tic-traffic-analytics-1.png)

Gebruik de **geografische kaart** snel identificeren van de waarschijnlijke fysieke bestemmingen van internetverkeer voor uw virtuele machines. U kunt identificeren en verdachte locaties of patroon wijzigingen sorteren:

![Geografische kaart](media/tic-traffic-analytics-2.png)

Gebruik de **virtuele netwerken-topologie** naar de enquête snel bestaande virtuele netwerken:

![Topology Netwerktoewijzingen](media/tic-traffic-analytics-3.png)

### <a name="network-watcher-next-hop-tests"></a>Network Watcher volgende hop-tests

Netwerken in regio's die worden bewaakt met Network Watcher kunnen volgende hop-tests uitvoeren. In de Azure-portal, kunt u een bron en bestemming voor een voorbeeld van netwerk-stroom voor Network Watcher in voor het oplossen van de volgende hop-bestemming. Deze test uitvoeren op virtuele machines en adressen van de voorbeeld-internet om te controleren of dat de volgende hop-bestemming is de verwachte virtuele netwerkgateway.

![Volgende hop-tests](media/tic-network-watcher.png)

## <a name="conclusions"></a>Conclusies

U kunt eenvoudig toegang tot Microsoft Azure, Office 365 en Dynamics 365 om te voldoen aan de richtlijnen TIC 2.0 bijlage H, als geschreven en gedefinieerde mei 2018 configureren. Microsoft herkent dat de richtlijnen TIC onderhevig aan wijzigingen is. De allerbeste Microsoft om te voldoen aan de richtlijnen tijdig terwijl nieuwe richtlijnen wordt vrijgegeven klanten te helpen.

## <a name="appendix-trusted-internet-connections-patterns-for-common-workloads"></a>Bijlage: Vertrouwde verbindingen via Internet patronen voor veelvoorkomende workloads

| Categorie | Workload | IaaS | Toegewezen PaaS / Virtual network-injectie  | Service-eindpunten  |
|---------|---------|---------|---------|--------|
| Compute | Azure virtuele Linux-machines | Ja | | |
| Compute | Windows virtuele machines van Azure | Ja | | |
| Compute | Virtuele-machineschaalsets | Ja | | |
| Compute | Azure Functions | | App Service-omgeving | |
| Web en mobiel | Interne webtoepassingen | | App Service-omgeving| |
| Web en mobiel | Interne mobiele toepassing | | App Service-omgeving | |
| Web en mobiel | API-Apps | | App Service-omgeving | |
| Containers | Azure Container Service | | | Ja |
| Containers | Azure Kubernetes Service (AKS) \* | | | Ja |
| Database | Azure SQL Database | | Beheerd exemplaar voor Azure SQL Database \* | Azure SQL |
| Database | Azure Database for MySQL | | | Ja |
| Database | Azure Database for PostgreSQL | | | Ja |
| Database | Azure SQL Data Warehouse | | | Ja |
| Database | Azure Cosmos DB | | | Ja |
| Database | Azure Cache voor Redis | | Ja | |
| Storage | Azure Blob Storage | Ja | | |
| Storage | Azure Files | Ja | | |
| Storage | Azure Queue storage | Ja | | |
| Storage | Azure Table Storage | Ja | | |
| Storage | Azure Disk-opslag | Ja | | |

\* Openbare preview-versie in Azure Government, mei 2018.
