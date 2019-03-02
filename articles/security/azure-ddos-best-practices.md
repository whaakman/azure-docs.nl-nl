---
title: Azure DDoS Protection best practices en referentiearchitecturen | Microsoft Docs
description: Meer informatie over hoe u gegevens voor logboekregistratie diep om inzicht te krijgen over uw toepassing kunt gebruiken.
services: security
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: barclayn
ms.openlocfilehash: 11f3dcefd283ada00e915c2d6cb8abf654590ec1
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57247338"
---
# <a name="azure-ddos-protection-best-practices-and-reference-architectures"></a>Azure DDoS Protection: Aanbevolen procedures en referentiearchitecturen

Dit artikel is bedoeld voor IT-besluitvormers en personeel. Er wordt verwacht dat u bekend met Azure bent, netwerken en beveiliging.

Ontwerpen voor distributed denial of service (DDoS) vereist tolerantie plannen en ontwerpen voor tal van foutmodi. Dit artikel bevat aanbevolen procedures voor het ontwerpen van toepassingen in Azure voor bescherming tegen DDoS-aanvallen.

## <a name="types-of-attacks"></a>Typen aanvallen

DDoS is een soort aanval waarmee wordt geprobeerd te weinig toepassingsresources. Het doel is van invloed op de beschikbaarheid van de toepassing en de mogelijkheid voor het verwerken van legitieme aanvragen. Aanvallen worden steeds meer geavanceerde en grotere in omvang en impact. DDoS-aanvallen kunnen worden gericht op elk eindpunt dat openbaar bereikbaar is via internet.

Azure biedt continue bescherming tegen DDoS-aanvallen. Deze beveiliging is geïntegreerd in het Azure-platform, standaard en zonder extra kosten. 

Naast de kern DDoS-bescherming in het platform, [Azure DDoS Protection Standard](https://azure.microsoft.com/services/ddos-protection/) biedt geavanceerde mogelijkheden voor DDoS risicobeperking tegen netwerkaanvallen. Het automatisch afgestemd op ter bescherming van uw specifieke Azure-resources. Beveiliging is heel eenvoudig om in te schakelen tijdens het maken van nieuwe virtuele netwerken. Het kan ook worden uitgevoerd na het maken en vereist geen wijzigingen toepassing of resource.

![De rol van Azure DDoS Protection om klanten en een virtueel netwerk te beschermen tegen kwaadwillende personen](media/azure-ddos-best-practices/image1.png)

DDoS-aanvallen kunnen worden ingedeeld in drie categorieën: Breng, het protocol en de resource.

### <a name="volumetric-attacks"></a>Breng aanvallen

Breng aanvallen zijn de meest voorkomende type DDoS-aanval. Breng aanvallen zijn brute-force gebruikt om die zijn gericht op het netwerk en transport-lagen. Ze proberen te weinig resources, zoals netwerkkoppelingen. 

Deze aanvallen maken vaak gebruik van meerdere geïnfecteerde systemen te overspoelen de netwerklagen met schijnbaar legitieme verkeer. Ze netwerklaag protocollen, zoals Internet Control Message Protocol (ICMP), User Datagram Protocol (UDP) en Transmission Control Protocol (TCP) gebruiken.

De meest gebruikte netwerklaag DDoS-aanvallen zijn TCP SYN vollopen ICMP echo, dat wordt overspoeld, DNS, UDP en NTP versterking aanvallen. Dit type aanval kan niet alleen voor de service, maar ook worden onderbroken als een smokescreen voor kwaadwillend en meer gerichte binnendringing in het netwerk worden gebruikt. Een voorbeeld van een recente Breng aanval is de [Memcached misbruik](https://www.wired.com/story/github-ddos-memcached/) die GitHub beïnvloed. Deze aanval gericht UDP-poort 11211 en 1.35 Tb/s van aanval volume gegenereerd.

### <a name="protocol-attacks"></a>Protocol aanvallen

Protocol aanvallen protocollen voor doel-toepassing. Ze proberen te gebruiken om de beschikbare resources in infrastructuurapparaten, zoals firewalls, toepassingsservers, en load balancers. Protocol aanvallen gebruik van pakketten die zijn onjuist gevormd of afwijkingen van protocol bevatten. Deze aanvallen worden uitgevoerd door het verzenden van een groot aantal openstaande aanvragen servers en andere communicatieapparaten te beantwoorden en op een reactie van het pakket wachten. Het doel wordt geprobeerd om te reageren op de openstaande aanvragen, waardoor uiteindelijk het systeem vastlopen.

De meest voorkomende voorbeeld van een protocol op basis van DDoS-aanval is TCP SYN overspoelen. In deze aanval probeert een opeenvolging van TCP SYN aanvragen blijvend wordt overbelast een doel. Het doel is om het doel reageert niet. De 2016 Dyn onderbreking, afgezien van een toepassingslaag aanval, bestond uit TCP SYN wordt overspoeld die gerichte poort 53 van Dyn van DNS-servers.

### <a name="resource-attacks"></a>Resource-aanvallen

Resource-aanvallen als doel de toepassingslaag. Deze activeren back-end-processen in een poging om een systeem blijvend wordt overbelast. Resource-aanvallen misbruik verkeer dat normale uiterlijk, maar dat uitvoert van CPU-intensieve query's voor de server. De hoeveelheid verkeer die nodig zijn te weinig resources is lager dan die van het andere type aanvallen. Het verkeer in een resource-aanval is te onderscheiden van legitieme verkeer, waardoor het moeilijk is om te detecteren. De meest voorkomende resource-aanvallen zijn op HTTP/HTTPS- en DNS-services.

## <a name="shared-responsibility-in-the-cloud"></a>Gedeelde verantwoordelijkheid in de cloud

Een ingrijpende strategie helpt bestrijding de toenemende verschillende en het raffinement van aanvallen. De beveiliging is een gedeelde verantwoordelijkheid tussen de klant en Microsoft. Microsoft, wordt deze een [gedeelde verantwoordelijkheid model](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). De volgende afbeelding ziet u deze deling van de verantwoordelijkheid van:

![De verantwoordelijkheden van de klant en van Azure](media/azure-ddos-best-practices/image2.png)

Azure-klanten profiteren van het controleren van best practices van Microsoft en het bouwen van wereldwijd gedistribueerde toepassingen die zijn ontworpen en getest is mislukt.

## <a name="fundamental-best-practices"></a>Fundamentele aanbevolen procedures

De volgende secties bevatten richtlijnen voor het bouwen van robuuste DDoS services op Azure.

### <a name="design-for-security"></a>Ontwerpen voor beveiliging

Zorg ervoor dat beveiliging een prioriteit gedurende de hele levenscyclus van een toepassing, van ontwerp en implementatie tot implementatie en bewerkingen. Toepassingen kunnen fouten optreden die een relatief laag volume aan aanvragen voor het gebruik van een heel van resources, wat resulteert in een serviceonderbreking toestaan hebben. 

Ter bescherming van een service die wordt uitgevoerd op Microsoft Azure, moet u een goed begrip van de toepassingsarchitectuur van uw hebt en zich richten op de [vijf pijlers van softwarekwaliteit](https://docs.microsoft.com/azure/architecture/guide/pillars).
Typische verkeer volumes, moet u weten het model connectiviteit tussen de toepassing en andere toepassingen en de service-eindpunten die zichtbaar zijn voor het openbare internet.

Ervoor te zorgen dat een toepassing is flexibel genoeg om af te handelen van een denial of service die gericht op de toepassing zelf het belangrijkste is. Beveiliging en privacy zijn ingebouwd in het Azure-platform, vanaf de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). De SDL-adressen van beveiliging in elke ontwikkelingsfase en zorgt ervoor dat Azure voortdurend bijgewerkt wordt zodat deze nog beter beveiligen.

### <a name="design-for-scalability"></a>Ontwerpen voor schaalbaarheid

Schaalbaarheid is hoe goed een systeem kan afhandelen van toegenomen belasting. Moet u uw toepassingen te ontwerpen [horizontaal schalen](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) om te voldoen aan de vraag van een versterkt belasting, specifiek in het geval van een DDoS-aanval. Als uw toepassing afhankelijk van één exemplaar van een service is, maakt het een single point of failure. Meerdere exemplaren wordt ingericht, kunt u uw systeem beter bestand is tegen en beter schaalbaar.

Voor [Azure App Service](../app-service/app-service-value-prop-what-is.md), selecteer een [App Service-plan](../app-service/overview-hosting-plans.md) die meerdere exemplaren biedt. Voor Azure Cloud Services, configureert u elk van de rollen gebruiken [meerdere exemplaren](../cloud-services/cloud-services-choose-me.md). Voor [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), zorg ervoor dat de architectuur van uw virtuele machine (VM) meer dan één virtuele machine bevat en dat elke virtuele machine wordt opgenomen in een [beschikbaarheidsset](../virtual-machines/virtual-machines-windows-manage-availability.md). Wordt u aangeraden [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor automatisch te schalen.

### <a name="defense-in-depth"></a>Verdediging

Het idee achter verdediging is het risico's beheren met behulp van diverse defensieve strategieën. Stapelen beveiligingen in een toepassing vermindert de kans dat een geslaagde aanval. Het is raadzaam dat u beveiligde ontwerpen voor uw toepassingen wilt implementeren met behulp van de ingebouwde mogelijkheden van het Azure-platform.

Bijvoorbeeld, het risico van aanvallen neemt toe met de grootte (*-surface area*) van de toepassing. U kunt de surface area beperken door met behulp van opname in de whitelist moeten sluiten de beschikbaar gemaakte IP-adresruimte en poorten die nodig zijn niet op de load balancers te hebben geluisterd ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) en [Azure Application Gateway](../application-gateway/application-gateway-create-probe-portal.md)). [Netwerkbeveiligingsgroepen (nsg's)](../virtual-network/security-overview.md) zijn van een andere manier om de kwetsbaarheid voor aanvallen verminderen.
U kunt [servicetags](../virtual-network/security-overview.md#service-tags) en [toepassingsbeveiligingsgroepen](../virtual-network/security-overview.md#application-security-groups) complexiteit voor het maken van beveiligingsregels en netwerkbeveiliging configureren als een natuurlijk verlengstuk van de structuur van een toepassing te minimaliseren.

U moet implementeren in Azure-services een [virtueel netwerk](../virtual-network/virtual-networks-overview.md) indien mogelijk. Met deze procedure kunt service-resources om te communiceren via privé-IP-adressen. Azure-serviceverkeer vanaf een virtueel netwerk maakt gebruik van openbare IP-adressen als bron-IP-adressen standaard. Met behulp van [service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) gaat het verkeer van de service voor het gebruik van privéadressen van virtuele netwerken als de bron-IP-adressen wanneer ze toegang wilt de Azure-service vanuit een virtueel netwerk tot krijgen.

Vaak zien we klanten on-premises bronnen ophalen aangevallen, samen met hun bronnen in Azure. Als u een on-premises omgeving verbinding naar Azure, wordt u aangeraden dat u de blootstelling van on-premises bronnen met het openbare internet minimaliseren. U kunt de schaal en geavanceerde DDoS protection-mogelijkheden van Azure door uw bekende openbare entiteiten in Azure implementeren. Omdat deze openbaar toegankelijke entiteiten vaak een doel voor DDoS-aanvallen zijn, ze te plaatsen in Azure hebben minder invloed op uw on-premises resources.

## <a name="azure-offerings-for-ddos-protection"></a>Azure-producten voor DDoS protection

Azure heeft twee DDoS-service-aanbiedingen die bescherming tegen netwerkaanvallen (laag 3 en 4): DDoS Protection Basic en DDoS-bescherming standaard. 

### <a name="ddos-protection-basic"></a>DDoS Protection Basic

Basisbeveiliging is geïntegreerd in de Azure standaard zonder extra kosten. De schaal en de capaciteit van de wereldwijd geïmplementeerde Azure-netwerk biedt beveiliging tegen veelvoorkomende netwerklaag aanvallen via verkeer altijd bewakings- en realtime risicobeperking. DDoS Protection Basic vereist geen gebruikerswijzigingen-configuratie of de toepassing. DDoS Protection Basic beschermt alle Azure-services, met inbegrip van PaaS-services zoals Azure DNS.

![Weergave van de Azure-netwerk, met de tekst 'algemene DDoS risicobeperking aanwezigheid"en 'Toonaangevende DDoS-risicobeperking capaciteit' toewijzen](media/azure-ddos-best-practices/image3.png)

DDoS-basisbeveiliging in Azure bestaat uit zowel software en hardware-onderdelen. Een software-controlelaag bepaalt wanneer, waar en welk type verkeer moet worden aangestuurd door hardware-apparaten die u analyseren en verwijderen van verkeer van de aanval. Het besturingselement vlak maakt deze beslissing op basis van een hele infrastructuur DDoS Protection *beleid*. Dit beleid is statisch instellen en universeel toegepast op alle Azure-klanten.

Bijvoorbeeld, het DDoS Protection-beleid geeft aan welk volume van netwerkverkeer via de beveiliging moet worden *geactiveerd.* (Dat wil zeggen, van de tenant-verkeer moet worden gerouteerd via apparaten scrubbing.) Het beleid wordt bepaald hoe de reinigingstaak apparaten moeten *beperken* de aanval.

De service Azure DDoS Protection Basic is gericht op beveiliging van de infrastructuur en beveiliging van de Azure-platform. Het beperkt verkeer wanneer het groter is dan het tarief dat is dus belangrijk dat dit mogelijk invloed op meerdere klanten in een multitenant-omgeving. Biedt geen waarschuwingen of per klant aangepast beleid.

### <a name="ddos-protection-standard"></a>DDoS Protection Standard

Standard protection biedt verbeterde functies voor DDoS-risicobeperking. Het automatisch afgestemd op ter bescherming van uw specifieke Azure-resources in een virtueel netwerk. Beveiliging is eenvoudig om in te schakelen op een nieuwe of bestaande virtuele netwerk en hiervoor geen wijzigingen toepassing of resource. Heeft diverse voordelen ten opzichte van de basic-service, met inbegrip van logboekregistratie, waarschuwingen en telemetrie. De volgende gedeelten worden de belangrijkste functies van de service Azure DDoS Protection Standard.

#### <a name="adaptive-real-time-tuning"></a>Adaptieve realtime afstemmen

De service Azure DDoS Protection Basic helpt klanten te beschermen en te voorkomen dat gevolgen voor andere klanten. Bijvoorbeeld, als een service wordt ingericht voor een typische volume van legitieme inkomend verkeer die kleiner is dan de *triggerfrequentie* van het hele infrastructuur DDoS Protection-beleid, bijvoorbeeld een DDoS-aanval op de betreffende klant resources worden verzonden opgemerkt. Meer over het algemeen de toepassingsspecifieke-gedrag van tenants en de complexiteit van de recente aanvallen met (bijvoorbeeld meerdere vector DDoS) aanroepen voor per klant, aangepaste beveiligingsbeleid voor apps. De service worden deze aanpassen met behulp van twee insights:

- Automatische leren van verkeerspatronen per klant (per IP) voor laag 3 en 4.

- Fout-positieven, overweegt de schaal van Azure kan het een aanzienlijke hoeveelheid verkeer kunnen worden opgevangen minimaliseren.

![Diagram van de DDoS Protection standaard werking, met 'Genereren van het beleid' omcirkeld](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring-and-alerting"></a>DDoS Protection telemetrie, bewaking en waarschuwingen

DDoS Protection standaard wordt aangegeven dat uitgebreide telemetrie via [Azure Monitor](../azure-monitor/overview.md) voor de duur van een DDoS-aanval. U kunt waarschuwingen voor het gebruik van de Azure Monitor-metrische gegevens die gebruikmaakt van DDoS-beveiliging configureren. U kunt logboekregistratie integreren met Splunk (Azure Event Hubs), logboeken van Azure Monitor en Azure Storage voor geavanceerde analyse via de Azure Monitor Diagnostics-interface.

##### <a name="ddos-mitigation-policies"></a>Beleid voor DDoS-risicobeperking

Selecteer in de Azure portal, **Monitor** > **metrische gegevens**. In de **metrische gegevens** deelvenster, selecteert u de resourcegroep, selecteer een resourcetype van **openbaar IP-adres**, en selecteer uw Azure openbare IP-adres. DDoS-metrische gegevens zijn zichtbaar in de **beschikbare metrische gegevens** deelvenster.

DDoS Protection Standard geldt drie autotuned risicobeperking beleidsregels (TCP SYN, TCP en UDP) voor elke openbare IP-adres van de beveiligde bron in het virtuele netwerk waarvoor DDoS ingeschakeld. U kunt de drempels beleid weergeven door het selecteren van de metrische gegevens **inkomende pakketten voor het activeren van DDoS-risicobeperking**.

![Beschikbare metrische gegevens en een grafiek met metrische gegevens](media/azure-ddos-best-practices/image7.png)

De drempelwaarden beleid worden automatisch geconfigureerd via machine learning-netwerkverkeer profilering. DDoS-Risicobeperking voor een IP-adres aangevallen vindt alleen plaats wanneer de drempelwaarde voor het beleid is overschreden.

##### <a name="metric-for-an-ip-address-under-ddos-attack"></a>Metrische gegevens voor een IP-adres onder DDoS-aanval

Als het openbare IP-adres ligt onder de aanval, de waarde voor de metrische gegevens **onder DDoS-aanvallen of niet** gewijzigd op 1 wanneer DDoS Protection risicobeperking op het verkeer van de aanval uitvoert.

!['Onder DDoS aanval of niet' metrische gegevens en grafiek](media/azure-ddos-best-practices/image8.png)

Het is raadzaam om een waarschuwing configureren op deze metrische gegevens. U gaat vervolgens een melding wanneer er een actieve DDoS-risicobeperking uitgevoerd op uw openbare IP-adres.

Zie voor meer informatie, [beheren Azure DDoS Protection Standard met behulp van de Azure-portal](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="web-application-firewall-for-resource-attacks"></a>Web application firewall voor de resource-aanvallen

Specifiek zijn voor aanvallen van de resource op het niveau van de toepassing, moet u een web application firewall (WAF) om u te helpen webtoepassingen te beveiligen. Een WAF inspecteert inkomend webverkeer te blokkeren van de SQL-injecties, cross-site scripting, DDoS, en andere Routeringsmogelijkheden voor laag 7-aanvallen. Azure biedt [WAF als een functie van Application Gateway](../application-gateway/application-gateway-web-application-firewall-overview.md) voor gecentraliseerde beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen. Er zijn andere WAF-aanbiedingen beschikbaar van Azure-partners die mogelijk beter geschikt is voor uw behoeften via de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1).

Zelfs firewalls voor webtoepassingen zijn vatbaar voor aanvallen uitputting van Breng en status. Wordt aangeraden het inschakelen van DDoS Protection standaard op het virtuele netwerk van de WAF om te beschermen tegen Breng en protocol aanvallen. Zie voor meer informatie de [DDoS Protection referentiearchitecturen](#ddos-protection-reference-architectures) sectie.

### <a name="protection-planning"></a>Beveiliging plannen

Plannen en voorbereiden, zijn essentieel om te begrijpen hoe een systeem wordt uitgevoerd tijdens een DDoS-aanval. Het ontwerpen van een plan voor incidentbeheer respons is onderdeel van deze inspanning.

Als u DDoS Protection Standard hebt, zorg ervoor dat deze ingeschakeld op het virtuele netwerk van internetgerichte eindpunten. DDoS-waarschuwingen configureren, kunt u voortdurend bekijken voor potentiële aanvallen op uw infrastructuur. 

U moet uw toepassingen onafhankelijk bewaken. Begrijp het normale gedrag van een toepassing. Voorbereiden om te fungeren als de toepassing niet gedraagt is zoals verwacht tijdens een DDoS-aanval.

#### <a name="testing-through-simulations"></a>Testen via simulaties

Het is raadzaam om te testen van uw veronderstellingen over hoe uw services op een aanval reageren zullen door periodieke simulaties uit te voeren. Valideren dat uw services of toepassingen werken blijven zoals verwacht en er geen onderbreking van de gebruikerservaring is tijdens het testen. Hiaten een technologie- en het proces vanuit het oogpunt van en opnemen in de strategie voor DDoS-antwoord. Het is raadzaam dat u deze tests uitvoert in faseringsomgevingen of tijdens een niet-piekuren om te beperken de gevolgen voor de productie-omgeving.

We hebben een partnerschap aangegaan met [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) aan het bouwen van een interface waarin Azure-klanten verkeer op basis van openbare eindpunten met DDoS Protection is ingeschakeld voor simulaties kunnen genereren. U kunt de [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) simulatie op:

- Controleer hoe Azure DDoS Protection helpt uw Azure-resources beveiligen tegen DDoS-aanvallen.

- Optimaliseer het incidentreactieproces terwijl onder DDoS-aanval.

- Documenteer DDoS-naleving.

- Uw netwerk beveiligingsteams trainen.

Cyberbeveiliging vereist constante innovatie in defense. Azure DDoS Standard protection is een status-of-the-art aanbieding met een effectieve oplossing steeds complexer DDoS-aanvallen te beperken.

## <a name="components-of-a-ddos-response-strategy"></a>Onderdelen van een strategie voor DDoS-antwoord

Een DDoS-aanval die is doorgaans gericht op Azure-resources vereist zo weinig mogelijk tussenkomst vanuit het oogpunt van een gebruiker. Nog steeds, DDoS met risicobeperking als onderdeel van een strategie voor respons op incidenten te minimaliseren de gevolgen voor zakelijke continuïteit.

### <a name="microsoft-threat-intelligence"></a>Bedreigingsinformatie van Microsoft

Microsoft heeft een uitgebreide threat intelligence virtueel netwerk. Dit netwerk maakt gebruik van de collectieve kennis van een uitgebreide beveiligingscommunity die ondersteuning biedt voor Microsoft online services, Microsoft-partners en relaties in de beveiligingscommunity internet. 

Microsoft ontvangt als een provider voor kritieke infrastructuur, eerdere waarschuwingen over bedreigingen. Microsoft verzamelt informatie over bedreigingen vanuit de online services en van het globale klantenbestand. Microsoft bevat alle informatie over deze bedreigingen weer in de Azure DDoS Protection-producten.

De Microsoft Digital Crimes Unit (DCU) voert ook, aanstootgevende strategieën tegen botnets. Botnets vormen een gemeenschappelijke bron van de opdracht en controle voor DDoS-aanvallen.

### <a name="risk-evaluation-of-your-azure-resources"></a>Risico-evaluatie van uw Azure-resources

Het is belangrijk om inzicht in het bereik van het risico van een DDoS-aanval regelmatig. Af en zelf: 
- Welke nieuwe resources voor openbaar beschikbare Azure beveiliging nodig?

- Is er een single point of failure wordt in de service? 

- Hoe kunnen services worden geïsoleerd om te beperken de gevolgen van een aanval terwijl nog steeds beschikbaar maken van services voor geldige klanten uit?

- Zijn er virtuele netwerken wanneer DDoS Protection standaard moet worden ingeschakeld, maar is niet? 

- Mijn services actief/actief zijn met failover in meerdere regio's?

### <a name="customer-ddos-response-team"></a>Klant DDoS-beveiligingsteam

Het maken van een DDoS-response-team is een belangrijke stap bij het reageren op een aanval snel en efficiënt. Identificeer contactpersonen in uw organisatie die planning en uitvoering leidinggeven zullen. Dit team DDoS-antwoord moet grondig inzicht in de service Azure DDoS Protection Standard. Zorg ervoor dat het team kunt identificeren en aanvallen direct opgelost door te coördineren met interne en externe klanten, met inbegrip van het team van Microsoft ondersteuning.

Voor uw team DDoS-antwoord wordt u aangeraden dat u oefeningen als een normaal onderdeel van de beschikbaarheid van de service en het plannen van de bedrijfscontinuïteit te waarborgen gebruiken. Deze oefeningen moeten bevatten schaal testen.

### <a name="alerts-during-an-attack"></a>Waarschuwingen tijdens een aanval

Azure DDoS Protection standaard identificeert en beperkt DDoS-aanvallen zonder tussenkomst van de gebruiker. Als u wilt een melding ontvangen wanneer er een actieve beperking op een beveiligde openbare IP-adres, kunt u [een waarschuwing configureren](../virtual-network/ddos-protection-manage-portal.md) op de metriek **onder DDoS-aanvallen of niet**. U kunt waarschuwingen maken voor de andere DDoS metrische gegevens om te begrijpen van de schaal van de aanval, verkeer wordt verwijderd en andere gegevens.

#### <a name="when-to-contact-microsoft-support"></a>Wanneer u contact op met Microsoft ondersteuning

- Tijdens een DDoS-aanval merkt u dat de prestaties van de beveiligde resource ernstige problemen gedegradeerd, of de resource niet beschikbaar is.

- U denkt dat de service DDoS Protection is niet gedraagt zoals verwacht. 

  De DDoS Protection-service wordt gestart risicobeperking alleen als de metrische waarde **beleid voor het activeren van DDoS-risicobeperking (SYN TCP/TCP/UDP)** lager is dan het verkeer dat is ontvangen op de beveiligde openbare IP-adresresource.

- U bent een virale gebeurtenis zullen uw netwerkverkeer aanzienlijk meer planning.

- Een actor heeft om te starten van een DDoS-aanval op uw resources in het gedrang komen.

Voor aanvallen waarvoor een ernstige impact op bedrijf, maak een ernst A [ondersteuningsticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>Stappen na aanval

Het is altijd een goede werkwijze een postmortem na een aanval en de strategie voor DDoS-antwoord naar wens aanpassen. Dingen om te overwegen:

- Is er een onderbreking van de service of de gebruiker optreden omdat er geen van de schaalbare architectuur?

- Welke toepassingen of services die het meest?

- Hoe effectief de strategie voor DDoS-antwoord is en hoe kan deze worden verbeterd?

Als u vermoedt dat je nog geen een DDoS-aanval dat, escaleren via uw normale Azure-ondersteuningskanalen.

## <a name="ddos-protection-reference-architectures"></a>DDoS Protection-referentiearchitecturen

DDoS Protection Standard is ontworpen [voor services die zijn geïmplementeerd in een virtueel netwerk](../virtual-network/virtual-network-for-azure-services.md). Voor andere services, de DDoS Protection Basic standaardservice is van toepassing. De volgende referentiearchitecturen zijn geordend op scenario's met de architectuur-patronen die zijn gegroepeerd.

### <a name="virtual-machine-windowslinux-workloads"></a>De werkbelasting van virtuele machine (Windows/Linux)

#### <a name="application-running-on-load-balanced-vms"></a>Toepassing die wordt uitgevoerd op virtuele machines met load balancing

Deze referentiearchitectuur toont een set bewezen procedures voor het uitvoeren van meerdere Windows-VM's in een schaalset achter een load balancer, beschikbaarheid en schaalbaarheid te verbeteren. Deze architectuur kan worden gebruikt voor elke staatloze werkbelasting, zoals een webserver.

![Diagram van de referentiearchitectuur voor een toepassing die wordt uitgevoerd op virtuele machines met load balancing](media/azure-ddos-best-practices/image9.png)

In deze architectuur wordt een werkbelasting verdeeld over meerdere VM-exemplaren. Er is een enkel openbaar IP-adres en internetverkeer wordt gedistribueerd naar de virtuele machines via een load balancer. DDoS Protection standaard is ingeschakeld op het virtuele netwerk van de load balancer van Azure (internet) met het openbare IP-adres dat is gekoppeld aan deze.

De load balancer verdeelt binnenkomende internetaanvragen de VM-exemplaren. Virtuele-machineschaalsets kunnen het aantal virtuele machines kunnen worden geschaald in- of uitgeschaald handmatig of automatisch op basis van vooraf gedefinieerde regels. Dit is belangrijk als de resource voor DDoS-aanval. Zie voor meer informatie over deze referentiearchitectuur [in dit artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm).

#### <a name="application-running-on-windows-n-tier"></a>Toepassing die wordt uitgevoerd op Windows-N-aantal lagen

Er zijn veel manieren om een architectuur met meerdere lagen te implementeren. Het volgende diagram toont een typische drie lagen web-App. Deze architectuur is gebaseerd op het artikel [met load balancing-VM's voor schaalbaarheid en beschikbaarheid uitvoeren](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). De web- en bedrijfslagen gebruiken VM's met taakverdeling.

![Diagram van de referentiearchitectuur voor een toepassing die wordt uitgevoerd op Windows-N-aantal lagen](media/azure-ddos-best-practices/image10.png)

In deze architectuur DDoS Protection standaard ingeschakeld op het virtuele netwerk. Alle openbare IP-adressen in het virtuele netwerk ophalen DDoS-bescherming voor laag 3 en 4. Implementeer voor beveiliging op laag 7, Application Gateway in de WAF-SKU. Zie voor meer informatie over deze referentiearchitectuur [in dit artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier).

#### <a name="paas-web-application"></a>PaaS-webtoepassing

Deze referentiearchitectuur toont een Azure App Service-toepassing in één regio uitgevoerd. Deze architectuur laat zien dat een set bewezen procedures voor een webtoepassing die gebruikmaakt van [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/) en [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).
Een stand-by-regio is ingesteld voor failover-scenario's.

![Diagram van de referentiearchitectuur voor een PaaS-webtoepassing](media/azure-ddos-best-practices/image11.png)

Met Azure Traffic Manager worden binnenkomende aanvragen gerouteerd naar de Application Gateway in een van de regio's. Normaliter worden aanvragen gerouteerd naar de Application Gateway in de actieve regio. Als deze regio niet beschikbaar is, failover-schakeling Traffic Manager aan Application Gateway in de stand-by-regio.

Al het verkeer van internet dat is bestemd voor de webtoepassing wordt doorgestuurd naar de [openbaar IP-adres van Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) via Traffic Manager. In dit scenario wordt de app-service (web-app) zelf is niet rechtstreeks extern gerichte en worden beschermd met Application Gateway. 

Het is raadzaam dat u de Application Gateway WAF-SKU configureert (modus voorkomen) om te beveiligen tegen aanvallen van laag 7 (HTTP/HTTPS/WebSocket). Bovendien web-apps zijn geconfigureerd voor [accepteert alleen verkeer van de toepassingsgateway](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) IP-adres.

Zie voor meer informatie over deze referentiearchitectuur [in dit artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region).

### <a name="mitigation-for-non-web-paas-services"></a>Risicobeperking voor niet-PaaS-services

#### <a name="hdinsight-on-azure"></a>HDInsight op Azure

Deze referentiearchitectuur toont DDoS Protection standaard voor het configureren van een [Azure HDInsight-cluster](https://docs.microsoft.com/azure/hdinsight/). Zorg ervoor dat het HDInsight-cluster is gekoppeld aan een virtueel netwerk en dat de DDoS Protection is ingeschakeld op het virtuele netwerk.

!['HDInsight' en 'Geavanceerd' deelvensters met instellingen voor virtueel netwerk](media/azure-ddos-best-practices/image12.png)

![Selectie voor het inschakelen van DDoS Protection](media/azure-ddos-best-practices/image13.png)

In deze architectuur wordt verkeer dat bestemd is voor het HDInsight-cluster vanaf het internet worden doorgestuurd naar het openbare IP-adres dat is gekoppeld aan de load balancer van HDInsight-gateway. De gateway load balancer stuurt het verkeer naar de hoofdknooppunten of de worker-knooppunten rechtstreeks. Omdat de DDoS Protection standaard is ingeschakeld op het virtuele netwerk van HDInsight, krijgen alle openbare IP-adressen in het virtuele netwerk DDoS-bescherming voor laag 3 en 4. Deze referentiearchitectuur kan worden gecombineerd met de N-Tier en referentiearchitecturen voor meerdere regio's.

Zie voor meer informatie over deze verwijzende architectuur, de [Azure HDInsight uitbreiden met behulp van een Azure Virtual Network](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) documentatie.


> [!NOTE]
> Azure App Service Environment voor PowerApps of API management in een virtueel netwerk met een openbaar IP-adres zijn beide niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

* [Productpagina van Azure DDoS Protection](https://azure.microsoft.com/services/ddos-protection/)

* [Azure DDoS Protection-blog](https://aka.ms/ddosblog)

* [Documentatie voor Azure DDoS Protection](../virtual-network/ddos-protection-overview.md)
