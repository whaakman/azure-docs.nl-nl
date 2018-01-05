---
title: Aandachtspunten voor topologie netwerk bij gebruik van Azure Active Directory-toepassingsproxy | Microsoft Docs
description: Bevat informatie over aandachtspunten voor topologie netwerk bij gebruik van Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: f4ca4856333bf8b10a00952356080ed332dc266b
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Netwerk topologie overwegingen bij het gebruik van Azure Active Directory-toepassingsproxy

Dit artikel wordt uitgelegd aandachtspunten voor topologie netwerk bij gebruik van Azure Active Directory (Azure AD) Application Proxy voor het publiceren en externe toegang tot uw toepassingen.

## <a name="traffic-flow"></a>Netwerkverkeer

Wanneer een toepassing is gepubliceerd via Azure AD-toepassingsproxy, wordt het verkeer van de gebruikers naar de toepassingen loopt via de drie verbindingen:

1. De gebruiker verbinding maakt met de openbare Azure AD-toepassingsproxy service-eindpunt op Azure
2. De service voor toepassingsproxy maakt verbinding met de connector voor toepassingsproxy
3. De Application Proxy connector maakt verbinding met de doeltoepassing

![Diagram van netwerkverkeer van gebruiker aan de doeltoepassing](./media/application-proxy-network-topologies/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Locatie van de tenant en de service voor toepassingsproxy

Wanneer u zich aanmeldt voor een Azure AD-tenant, worden de regio van uw tenant wordt bepaald door het land die u opgeeft. Wanneer u toepassingsproxy inschakelt, worden de exemplaren van de service-toepassingsproxy voor uw tenant geselecteerd of gemaakt in dezelfde regio bevinden als uw Azure AD-tenant of de dichtstbijzijnde regio aan.

Bijvoorbeeld, als uw Azure AD-tenant-gebied de Europese Unie is, gebruiken alle toepassingsproxy-connectors service-exemplaren in Azure-datacenters in de EU. Wanneer uw gebruikers toegang tot toepassingen gepubliceerde, gaat u hun verkeer via de toepassingsproxy service-exemplaren op deze locatie.

## <a name="considerations-for-reducing-latency"></a>Overwegingen voor korte wachttijden

Alle proxyoplossingen introduceren latentie in de netwerkverbinding. Ongeacht welke proxy of VPN-oplossing u als uw oplossing voor externe toegang kiest, bevat deze altijd een reeks servers om de verbinding met binnen uw bedrijfsnetwerk.

Organisaties omvatten server-eindpunten in het perimeternetwerk. Met Azure AD-toepassingsproxy echter loopt netwerkverkeer via de proxy-service in de cloud terwijl de connectors bevinden zich op uw bedrijfsnetwerk. Er is geen perimeternetwerk is vereist.

De volgende secties bevatten aanvullende suggesties hebt voor hulp bij de latentie nog verder te verminderen. 

### <a name="connector-placement"></a>Plaatsing van de connector

Toepassingsproxy kiest de locatie van exemplaren voor u, op basis van de locatie van uw tenant. Echter, krijgt u te bepalen waar u de connector te installeren zodat u de kenmerken van de latentie van het netwerkverkeer definiëren.

Bij het instellen van de service voor toepassingsproxy, kunt u de volgende vragen:

* Waar zich de app?
* Waar bevinden de meeste gebruikers toegang de app tot zich?
* Waar zich de toepassingsproxy-exemplaar
* Hebt u al een speciaal netwerkverbinding met Azure-datacenters instellen, zoals Azure ExpressRoute of een vergelijkbare VPN?

De connector is om te communiceren met zowel Azure als uw toepassingen (stap 2 en 3 in het stroomdiagram verkeer), zodat de plaatsing van de connector is van invloed op de latentie van deze twee verbindingen. Bij het evalueren van de plaatsing van de connector, houd er rekening mee houden de volgende punten:

* Als u het gebruik van Kerberos wilt-beperkte delegatie (KCD) voor eenmalige aanmelding, moet de connector een regel zicht naar een datacenter. Bovendien moet de server-connector worden verbonden met het domein.  
* Bij twijfel door de connector dichter bij de toepassing te installeren.

### <a name="general-approach-to-minimize-latency"></a>Algemene aanpak Latentie minimaliseren

U kunt de latentie van het end-to-end-verkeer minimaliseren door het optimaliseren van elke netwerkverbinding. Elke verbinding kan worden geoptimaliseerd door:

* De afstand tussen de twee einden van de hop wordt verminderd.
* Het kiezen van het juiste netwerk overschrijden. Bijvoorbeeld, zijn een particulier netwerk in plaats van het openbare Internet passeert sneller, als gevolg van specifieke verbindingen.

Als er een specifieke VPN- of ExpressRoute-verbinding tussen Azure en uw bedrijfsnetwerk, kan u wilt gebruiken.

## <a name="focus-your-optimization-strategy"></a>Richt uw strategie voor optimalisatie

Er is weinig die u voor het beheren van de verbinding tussen uw gebruikers en de service voor toepassingsproxy kunt doen. Gebruikers kunnen toegang krijgen tot uw apps via een netwerk thuis, een restaurant of een ander land. In plaats daarvan kunt u de verbindingen van de service-toepassingsproxy met de toepassingsproxy-connectors om de apps te optimaliseren. Neem de volgende patronen in uw omgeving.

### <a name="pattern-1-put-the-connector-close-to-the-application"></a>Patroon 1: De connector dicht bij de toepassing plaatsen

De connector dicht bij de doeltoepassing plaats in het netwerk van de klant. Deze configuratie minimaliseert stap 3 in het diagram topografie omdat de connector en de toepassing sluiten. 

Als uw connector een regel zicht naar de domeincontroller moet, zijn dit patroon is het voordeliger. De meeste van onze klanten gebruiken dit patroon omdat deze geschikt is voor de meeste scenario's. Dit patroon kan ook worden gecombineerd met patroon 2 voor het optimaliseren van verkeer tussen de service en de connector.

### <a name="pattern-2-take-advantage-of-expressroute-with-public-peering"></a>Patroon 2: Profiteren van ExpressRoute met openbare peering

Als u ExpressRoute instellen met openbare peering hebt, kunt u de snellere ExpressRoute-verbinding voor verkeer tussen de toepassingsproxy en de connector. De connector is nog steeds op het netwerk dicht bij de app.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>Patroon 3: Profiteren van ExpressRoute met persoonlijke peering

Als u een speciale VPN of ExpressRoute instellen met persoonlijke peering tussen Azure en uw bedrijfsnetwerk, hebt u een andere optie. In deze configuratie is doorgaans het virtuele netwerk in Azure beschouwd als een uitbreiding van het bedrijfsnetwerk. U kunt dus de connector te installeren in het datacenter Azure en nog steeds voldoen aan de eisen van de lage latentie van de connector voor app-verbinding.

Latentie is niet beschadigd omdat verkeer via een speciale verbinding stroomt. Verbeterde latentie voor toepassingsproxy-serviceconnector wordt ook ophalen omdat de connector is geïnstalleerd in een Azure-datacenter dicht bij de locatie van uw Azure AD-tenant.

![Diagram van de connector is geïnstalleerd in een Azure-datacenter](./media/application-proxy-network-topologies/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Andere benaderingen

Hoewel de focus van dit artikel de plaatsing van de connector wordt, kunt u ook de plaatsing van de toepassing betere latentie kenmerken wijzigen.

Steeds meer verplaatst organisaties hun netwerken in gehoste omgevingen. Hierdoor kunnen ze hun apps plaatsen in een gehoste omgeving die ook deel uitmaakt van het bedrijfsnetwerk en nog steeds binnen het domein. In dit geval kunnen de patronen die zijn beschreven in de voorgaande secties worden toegepast op de nieuwe locatie van de toepassing. Als u deze optie overweegt, Zie [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md).

Houd ook rekening met het ordenen van uw connectors met [connector groepen](active-directory-application-proxy-connectors-azure-portal.md) naar doel-apps die zich in verschillende locaties en netwerken. 

## <a name="common-use-cases"></a>Algemene scenario’s

In deze sectie doorlopen we enkele algemene scenario's. Wordt ervan uitgegaan dat de Azure AD-tenant (en dus proxy-service-eindpunt) bevindt zich in de Verenigde Staten (VS). De aandachtspunten besproken in deze gevallen zijn ook van toepassing op andere regio's over de hele wereld gebruik.

Voor deze scenario's, we een 'hop' voor elke verbinding aanroepen en number ze gemakkelijker bespreking:

- **1 hop**: gebruiker kan de service voor toepassingsproxy
- **2 hop**: Application Proxy-service naar de connector voor toepassingsproxy
- **3 hop**: Application Proxy connector aan de doeltoepassing 

### <a name="use-case-1"></a>Voorbeeld 1 gebruiken

**Scenario:** de app is in een organisatie-netwerk in de VS met gebruikers in dezelfde regio. Er bestaat geen ExpressRoute of de VPN-tussen de Azure-datacenter en het bedrijfsnetwerk.

**Aanbeveling:** patroon volgen 1, uitgelegd in de vorige sectie. Voor verbeterde latentie, overweeg het gebruik van ExpressRoute, indien nodig.

Dit is een eenvoudig patroon. U optimaliseren hops 3 door het plaatsen van de connector in de buurt van de app. Dit is ook een natuurlijke keuze, omdat de connector wordt doorgaans geïnstalleerd met zicht regel voor de app en het datacenter KCD bewerkingen uit te voeren.

![Diagram van gebruikers, proxy-connector en app, bevinden zich allemaal in de Verenigde Staten](./media/application-proxy-network-topologies/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Voorbeeld 2 gebruiken

**Scenario:** de app is in het netwerk van een organisatie in de VS met gebruikers globaal verspreid. Er bestaat geen ExpressRoute of de VPN-tussen de Azure-datacenter en het bedrijfsnetwerk.

**Aanbeveling:** patroon volgen 1, uitgelegd in de vorige sectie. 

Opnieuw is het algemene patroon optimaliseren hops 3, waar u de connector in de buurt van de app. Er is geen hops 3 doorgaans dure, als dit allemaal binnen dezelfde regio. Hop 1 kan echter zijn duurder afhankelijk van waar de gebruiker zich bevindt, omdat gebruikers overal ter wereld moeten toegang hebben tot de toepassingsproxy-exemplaar in de Verenigde Staten. Hierbij moet worden opgemerkt dat een proxy-oplossing heeft dezelfde kenmerken met betrekking tot gebruikers globaal wordt verspreid.

![Diagram die laat zien dat gebruikers globaal worden verdeeld, maar de proxyserver, de connector en de app in de Verenigde Staten zijn](./media/application-proxy-network-topologies/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Gebruiksvoorbeeld 3

**Scenario:** de app is in het netwerk van een organisatie in de Verenigde Staten. ExpressRoute met openbare peering bestaat tussen Azure en het bedrijfsnetwerk.

**Aanbeveling:** patronen 1 en 2, uitgelegd in de vorige sectie volgen.

Schakel eerst de connector zo dicht mogelijk bij de app. Het systeem worden vervolgens automatisch de ExpressRoute gebruikt voor hop 2. 

Als de ExpressRoute-koppeling openbare peering, loopt door het verkeer tussen de proxy- en de connector over die koppeling. Latentie heeft een hop 2 is geoptimaliseerd.

![Diagram van ExpressRoute tussen de proxy- en -connector](./media/application-proxy-network-topologies/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Gebruiksvoorbeeld 4

**Scenario:** de app is in het netwerk van een organisatie in de Verenigde Staten. ExpressRoute met persoonlijke peering bestaat tussen Azure en het bedrijfsnetwerk.

**Aanbeveling:** Volg patroon, 3, uitgelegd in de vorige sectie.

De connector in de Azure-datacenter die is verbonden met het bedrijfsnetwerk via ExpressRoute privépeering plaatsen. 

De connector kan worden geplaatst in het Azure datacenter. Omdat de connector nog steeds een zicht regel aan de toepassing en het datacenter tot en met het particuliere netwerk is, blijft hops 3 geoptimaliseerd. Bovendien wordt hop 2 verder geoptimaliseerd.

![Diagram van de connector in een Azure-datacenter en ExpressRoute tussen de connector en de app](./media/application-proxy-network-topologies/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Gebruiksvoorbeeld 5

**Scenario:** de app is in een organisatie-netwerk in de Europese Unie, met de toepassingsproxy-exemplaar en de meeste gebruikers in de Verenigde Staten.

**Aanbeveling:** plaatst u de connector in de buurt van de app. Omdat VS gebruikers van een toepassingsproxy-exemplaar dat is allemaal in dezelfde regio gebruikmaken, is hop 1 niet te duur. Hops 3 is geoptimaliseerd. Overweeg het gebruik van ExpressRoute hop 2 optimaliseren. 

![Diagram van de gebruikers- en proxy in de Verenigde Staten, met de connector en de app in de EU](./media/application-proxy-network-topologies/application-proxy-pattern5b.png)

U kunt ook rekening houden met behulp van een andere variant in deze situatie. Als de meeste gebruikers in de organisatie in de Verenigde Staten, dan waarschijnlijk die hebben breidt uw netwerk naar de Verenigde Staten ook. De connector plaats in de Verenigde Staten en gebruikt u de regel toegewezen interne bedrijfsnetwerk bevinden naar de toepassing in de EU. Deze manier hops 2 en 3 worden geoptimaliseerd.

![Diagram van de gebruikers-, proxy- en -connector in de VS, app in de EU](./media/application-proxy-network-topologies/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Volgende stappen

- [Toepassingsproxy inschakelen](active-directory-application-proxy-enable.md)
- [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang inschakelen](application-proxy-enable-remote-access-sharepoint.md)
- [Oplossen van problemen met toepassingsproxy](active-directory-application-proxy-troubleshoot.md)
