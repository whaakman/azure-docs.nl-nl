---
title: Azure DDoS-bescherming best practices en verwijzen naar architecturen | Microsoft Docs
description: Meer informatie over hoe u logboekgegevens grondige om inzicht te krijgen over uw toepassing kunt gebruiken.
services: security
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2018
ms.author: barclayn
ms.openlocfilehash: 042dd4876a63e5881e67456b449570b01cb967a5
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="azure-ddos-protection-best-practices-and-reference-architectures"></a>Azure DDoS-bescherming: Best practices en verwijzen naar architecturen

In dit artikel is bedoeld voor IT-besluitvormers en beveiligingspersoneel. Deze verwacht dat u bekend met Azure bent, netwerken en beveiliging.

Ontwerpen voor gedistribueerde DOS-aanval (DDoS) vereist tolerantie plannen en ontwerpen voor verschillende modi voor fout. Dit artikel bevat aanbevolen procedures voor het ontwerpen van toepassingen in Azure voor tolerantie tegen DDoS-aanvallen.

## <a name="types-of-attacks"></a>Typen aanvallen

DDoS is een type aanval waarmee wordt geprobeerd naar bronnen van de toepassing. Het doel is van invloed op de beschikbaarheid van de toepassing en de mogelijkheid om legitieme aanvragen te verwerken. Aanvallen worden steeds meer geavanceerde en groter in grootte en impact. DDoS-aanvallen kunnen worden gericht op een willekeurig eindpunt dat openbaar bereikbaar is via het internet.

Azure biedt continue bescherming tegen DDoS-aanvallen. Deze beveiliging is geïntegreerd in de Azure-platform, standaard en zonder extra kosten. 

Naast de kern DDoS-bescherming van het platform [Azure DDoS-bescherming standaard](https://azure.microsoft.com/services/ddos-protection/) biedt geavanceerde mogelijkheden voor DDoS risicobeperking tegen netwerkaanvallen. Er wordt automatisch zijn afgestemd ter bescherming van uw specifieke Azure-resources. Beveiliging is heel eenvoudig om in te schakelen tijdens het maken van nieuwe virtuele netwerken. Het is ook mogelijk na het maken en vereist geen wijzigingen toepassing of bron.

![De rol van Azure DDoS-bescherming bij het beschermen van klanten en een virtueel netwerk van een aanvaller](media/azure-ddos-best-practices/image1.png)

DDoS-aanvallen kunnen worden ingedeeld in drie categorieën: Breng, het protocol en de resource.

### <a name="volumetric-attacks"></a>Breng aanvallen

Breng aanvallen zijn de meest voorkomende type DDoS-aanval. Breng aanvallen zijn gebruikt om gewelddadige die zijn gericht op de netwerk- en lagen. Ze willen bronnen zoals netwerkkoppelingen. 

Deze aanvallen maken vaak gebruik van meerdere geïnfecteerde systemen voor de netwerklagen met ogenschijnlijk betrouwbare verkeer overspoelen. Ze netwerklaag protocollen, zoals Internet Control Message Protocol (ICMP), User Datagram Protocol (UDP) en Transmission Control Protocol (TCP) gebruiken.

De meest gebruikte netwerklaag DDoS-aanvallen zijn TCP SYN overspoelen, ICMP echo, UDP overspoelen, DNS, en de versterkingsfactor NTP-aanvallen. Dit type aanval kan niet alleen voor de service, maar ook verstoren als een smokescreen voor slechte en meer gerichte netwerk inbraakdetectie worden gebruikt. Een voorbeeld van een recente Breng aanval is de [Memcached misbruik](https://www.wired.com/story/github-ddos-memcached/) die van invloed op GitHub. Deze aanval gericht UDP-poort 11211 en 1.35 Tb/s aanval volume gegenereerd.

### <a name="protocol-attacks"></a>Protocol-aanvallen

Protocol aanvallen doel toepassingsprotocollen. Ze proberen te gebruiken om alle beschikbare bronnen in de infrastructuurapparaten, zoals firewalls, toepassingsservers, en netwerktaakverdelers. Protocol aanvallen gebruiken pakketten die zijn onjuist gevormd of protocol afwijkingen bevatten. Deze aanvallen werken door bestoken van openstaande aanvragen servers en andere communicatieapparaten beantwoorden en op een antwoord van het pakket wachten. Het doel wordt geprobeerd om te reageren op de openstaande aanvragen, waardoor uiteindelijk het systeem vastlopen.

De meest voorkomende voorbeeld van een protocol DDoS-aanval is TCP SYN overstroming. In deze aanval probeert een opeenvolging van aanvragen voor TCP SYN overbelast een doel. Het doel is om het doel niet reageert. De 2016 Dyn onderbreking, afgezien van een aanval toepassingslaag, bestond uit TCP SYN wordt overspoeld die gerichte poort 53 van Dyn van DNS-servers.

### <a name="resource-attacks"></a>Resource-aanvallen

Resource-aanvallen gericht zijn op de toepassingslaag. Deze activeren back-end-processen in een poging om een systeem overbelast. Resource-aanvallen misbruik verkeer die er ongeveer normale uitziet maar dat uitvoert CPU-intensief query's naar de server. De hoeveelheid verkeer die nodig zijn voor de bronnen is lager dan die van het andere type aanvallen. Het verkeer bij een aanval van de resource is te onderscheiden van legitieme verkeer, waardoor het moeilijk te detecteren. De meest voorkomende resource-aanvallen zijn op HTTP/HTTPS- en DNS-services.

## <a name="shared-responsibility-in-the-cloud"></a>Gedeelde verantwoordelijkheid in de cloud

Een strategie defense-in-depth helpt bestrijding de toenemende diverse en verfijning van aanvallen. Beveiliging is een gedeelde verantwoordelijkheid tussen de klant en Microsoft. Microsoft noemt dit een [gedeelde verantwoordelijkheid model](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). De volgende afbeelding ziet u deze deling van verantwoordelijkheid:

![De verantwoordelijkheden van de klant en Azure](media/azure-ddos-best-practices/image2.png)

Azure-klanten profiteren van de hand van best practices van Microsoft en bouwen van globaal gedistribueerde toepassingen die zijn ontworpen en getest voor fout.

## <a name="fundamental-best-practices"></a>Fundamentele aanbevolen procedures

De volgende secties bevatten richtlijnen voor het bouwen van DDoS robuuste services in Azure.

### <a name="design-for-security"></a>Ontwerpen voor beveiliging

Zorg ervoor dat beveiliging een prioriteit gedurende de volledige levenscyclus van een toepassing, in de ontwerp- en implementatiestappen voor implementatie en bewerkingen. Toepassingen kunnen fouten waardoor een relatief lage volume van aanvragen voor het gebruik van een heel van resources, wat resulteert in een onderbreking van deze service hebben. 

Ter bescherming van een service die wordt uitgevoerd op Microsoft Azure, moet u een goede kennis van de toepassingsarchitectuur van uw hebben en zich richten op de [vijf stijlen van software quality](https://docs.microsoft.com/azure/architecture/guide/pillars).
Typische verkeer volumes, moet u weten het model connectiviteit tussen de toepassing en andere toepassingen en de service-eindpunten die zijn blootgesteld aan het openbare internet.

Ervoor zorgen dat een toepassing robuuste voor het afhandelen van denial of service die gericht op de toepassing zelf is zeer belangrijk. Beveiliging en privacy zijn ingebouwd in de Azure-platform vanaf de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). De SDL beveiliging adressen aan elke ontwikkelingsfase en zorgt ervoor dat Azure voortdurend wordt bijgewerkt zodat deze nog beter beveiligen.

### <a name="design-for-scalability"></a>Ontwerp voor schaalbaarheid

Schaalbaarheid is hoe goed een systeem kan afhandelen van toegenomen belasting. U moet uw toepassingen en ontwerpen [horizontaal schalen](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) om te voldoen aan de vraag van een versterkt belasting, speciaal in het geval van een DDoS-aanval. Als uw toepassing afhankelijk van één exemplaar van een service is, maakt het een potentieel risico. Inrichting van meerdere exemplaren, kunt u uw systeem meer flexibiliteit en meer schaalbare.

Voor [Azure App Service](../app-service/app-service-value-prop-what-is.md), selecteer een [App Service-abonnement](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) die biedt meerdere exemplaren. Configureer elk van uw rollen te gebruiken voor Azure Cloud Services [meerdere exemplaren](../cloud-services/cloud-services-choose-me.md). Voor [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), zorg ervoor dat de architectuur van de virtuele machine (VM) meer dan één virtuele machine bevat en dat elke virtuele machine wordt opgenomen in een [beschikbaarheidsset](../virtual-machines/virtual-machines-windows-manage-availability.md). Wordt u aangeraden [virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor automatisch schalen mogelijkheden.

### <a name="defense-in-depth"></a>Gedegen

Het idee achter gedegen is het risico's beheren met behulp van diverse defensive strategieën. Stapelen beveiligingen in een toepassing vermindert de kans op een geslaagde aanval. Het is raadzaam dat u veilig ontwerpen voor uw toepassingen implementeren met behulp van de ingebouwde mogelijkheden van het Azure-platform.

Bijvoorbeeld, het risico van aanvallen verhoogd met de grootte (*oppervlak*) van de toepassing. U kunt het oppervlak verkleinen door whitelisting te sluiten blootgestelde IP-adresruimte en poorten die niet nodig op de load balancers zijn luisteren ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) en [Azure Application Gateway](../application-gateway/application-gateway-create-probe-portal.md)). [Netwerkbeveiligingsgroepen (nsg's)](../virtual-network/virtual-networks-nsg.md) zijn van een andere manier om de kwetsbaarheid te beperken.
Kunt u [service labels](/virtual-network/security-overview.md) en [toepassing beveiligingsgroepen](/virtual-network/security-overview.md) complexiteit voor het maken van de beveiligingsregels voor verbindingen en netwerkbeveiliging configureren als een natuurlijke extensie van de structuur van een toepassing te minimaliseren.

U moet Azure services implementeren in een [virtueel netwerk](../virtual-network/virtual-networks-overview.md) indien mogelijk. Op deze manier kunt serviceresources te communiceren via privé IP-adressen. Verkeer van de Azure-service van een virtueel netwerk wordt standaard openbare IP-adressen gebruikt als bron-IP-adressen. Met behulp van [service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) netwerkverkeer voor de service voor het gebruik van virtueel netwerk particuliere adressen als de bron-IP-adressen wanneer ze toegang de Azure-service van een virtueel netwerk tot je wordt overgeschakeld.

Vaak zien we klanten lokale bronnen ophalen aangevallen samen met hun bronnen in Azure. Als u een on-premises omgeving verbinding naar Azure, wordt u aangeraden minimaliseert u blootstelling van lokale bronnen aan het openbare internet. U kunt de schaal en geavanceerde DDoS-bescherming-mogelijkheden van Azure gebruiken door het implementeren van uw bekende openbare entiteiten in Azure. Omdat deze openbaar toegankelijk entiteiten vaak een doel voor DDoS-aanvallen zijn, vermindert zodat ze Azure de gevolgen voor uw lokale bronnen.

## <a name="azure-offerings-for-ddos-protection"></a>Azure-aanbiedingen voor DDoS-bescherming

Azure heeft twee DDoS serviceaanbiedingen ter bescherming tegen netwerkaanvallen (laag 3 en 4): DDoS beveiliging basis en standaard van DDoS-bescherming. 

### <a name="ddos-protection-basic"></a>DDoS-bescherming-basis

Basic-beveiliging is geïntegreerd in de Azure standaard zonder extra kosten. De schaal en de capaciteit van het globaal geïmplementeerde Azure-netwerk biedt bescherming tegen algemene netwerklaag aanvallen door middel van het controle- en realtime risicobeperking altijd aan verkeer. DDoS-bescherming Basic is vereist dat er geen gebruikerswijzigingen configuratie of toepassing. DDoS-bescherming Basic beschermt alle Azure-services, waaronder PaaS-services zoals Azure DNS.

![Weergave van het Azure-netwerk worden toegewezen met de tekst "globale DDoS risicobeperking aanwezigheid' en 'Voorloopspaties DDoS risicobeperking capaciteit'](media/azure-ddos-best-practices/image3.png)

Basic DDoS-bescherming in Azure bestaat uit software- en hardware-onderdelen. Een besturingselement software vlak bepaalt wanneer, waar en welk type verkeer via hardware-apparaten die analyseren en verwijderen van de aanval verkeer moet worden aangestuurd. Het besturingselement vlak maakt deze beslissing op basis van een infrastructuur wide DDoS-bescherming *beleid*. Dit beleid is statisch ingesteld en universeel toegepast op alle Azure-klanten.

Het beleid DDoS-bescherming wordt bijvoorbeeld aangegeven welk verkeersvolume de beveiliging moet worden *geactiveerd.* (Dat wil zeggen, de tenant-verkeer moet worden gerouteerd via het reinigen van apparaten.) Het beleid wordt bepaald hoe de reinigen toestellen moeten *verhelpen* de aanval.

De service Azure DDoS-bescherming Basic is gericht op beveiliging van de infrastructuur en beveiliging van de Azure-platform. Dit vermindert verkeer wanneer deze groter is dan het tarief dat is dus belangrijk dat dit mogelijk invloed op meerdere klanten in een multitenant-omgeving. Het niet mogelijk om waarschuwingen of per klant aangepast beleid.

### <a name="ddos-protection-standard"></a>DDoS-bescherming-standaard

Standaard-beveiliging biedt verbeterde DDoS risicobeperking functies. Er wordt automatisch zijn afgestemd ter bescherming van uw specifieke Azure-resources in een virtueel netwerk. Beveiliging is eenvoudig in te schakelen voor een nieuwe of bestaande virtuele netwerk en vereist geen wijzigingen van de toepassing of bron. Heeft verschillende voordelen ten opzichte van de basic service, inclusief logboekregistratie, waarschuwingen en telemetrie. De volgende secties worden de belangrijkste functies van de service Azure DDoS-bescherming standaard.

#### <a name="adaptive-real-time-tuning"></a>Adaptieve realtime afstemmen

De Azure DDoS-bescherming Basic service helpt bij het beveiligen van klanten en te voorkomen dat gevolgen voor andere klanten. Bijvoorbeeld, als een service wordt ingericht voor een typische volume van legitieme inkomend verkeer die kleiner is dan de *trigger snelheid* van het hele infrastructuur DDoS-bescherming-beleid, moet een DDoS-aanval op de bronnen van de klant wellicht opgemerkt. Meer in het algemeen de complexiteit van recente aanvallen (bijvoorbeeld meerdere vector DDoS) en het gedrag toepassingsspecifieke van tenants aanroep voor de per klant, aangepaste protection-beleid. De service kunnen worden behaald deze aanpassing met behulp van twee insights:

- Automatische leren van verkeerspatronen per klant (per-IP) voor laag 3 en 4.

- Fout-positieven, met inbegrip van de schaal van Azure kan het een aanzienlijke hoeveelheid verkeer kunnen minimaliseren.

![Diagram van hoe DDoS-bescherming Standard werkt, met 'beleid generatie, omcirkeld](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring-and-alerting"></a>DDoS-bescherming telemetrie, bewaken en waarschuwen

DDoS-bescherming standaard beschrijft de uitgebreide telemetrie via [Azure Monitor](/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) voor de duur van een DDoS-aanval. U kunt waarschuwingen voor een van de Azure-Monitor metrische gegevens die gebruikmaakt van DDoS-bescherming configureren. U kunt logboekregistratie integreren met Splunk (Azure Event Hubs), Azure Log Analytics en Azure Storage voor geavanceerde analyse via de interface van de Monitor diagnostische Azure-gegevens.

##### <a name="ddos-mitigation-policies"></a>DDoS risicobeperking beleid

Selecteer in de Azure-portal **Monitor** > **metrische gegevens**. In de **metrische gegevens** deelvenster selecteert u de resourcegroep, selecteer een resourcetype **openbaar IP-adres**, en selecteer uw Azure openbaar IP-adres. DDoS metrische gegevens zijn zichtbaar in de **beschikbare metrische gegevens** deelvenster.

DDoS-bescherming standaard geldt drie autotuned risicobeperking beleidsregels (TCP SYN, TCP en UDP) voor elke openbare IP-adres van de beveiligde bron, in het virtuele netwerk die DDoS ingeschakeld heeft. U kunt de drempels beleid weergeven door de metriek **inkomende pakketten voor het activeren van DDoS risicobeperking**.

![Beschikbare metrische gegevens en metrische gegevens grafiek](media/azure-ddos-best-practices/image7.png)

De drempelwaarden beleid zijn automatisch geconfigureerde via machine learning-netwerkverkeer profilering. DDoS Risicobeperking voor een IP-adres aangevallen vindt alleen plaats wanneer de beleid-drempelwaarde is overschreden.

##### <a name="metric-for-an-ip-address-under-ddos-attack"></a>Metrische gegevens voor een IP-adres onder DDoS-aanval

Als de openbare IP-adres onder aanvallen, de waarde voor de metriek **onder DDoS-aanvallen of niet** verandert op 1 als DDoS-bescherming risicobeperking op het verkeer van de aanval uitvoert.

!['Onder DDoS aanval of niet' metrische gegevens en de grafiek](media/azure-ddos-best-practices/image8.png)

Het is raadzaam om een waarschuwing configureren op deze metrische gegevens. U moet vervolgens worden gewaarschuwd wanneer er een actieve DDoS risicobeperking uitgevoerd op uw openbare IP-adres.

Zie voor meer informatie [beheren Azure DDoS-bescherming standaard met de Azure portal](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="web-application-firewall-for-resource-attacks"></a>Web application firewall voor de resource-aanvallen

Specifiek zijn voor aanvallen op de toepassingslaag resource, moet u configureren web application firewall (WAF) te beveiligde webtoepassingen. Een WAF inspecteert inkomend webverkeer naar SQL-injectie blokkeren, scripts, DDoS en andere aanvallen laag 7 op meerdere sites. Azure biedt [WAF als een functie van toepassingsgateway](../application-gateway/application-gateway-web-application-firewall-overview.md) voor gecentraliseerde beveiliging van uw webtoepassingen van algemene aanvallen en beveiligingsproblemen. Er zijn andere WAF aanbiedingen beschikbaar is via de Azure-partners die mogelijk beter geschikt is voor uw behoeften via de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1).

Zelfs web application firewalls zijn vatbaar voor aanvallen van uitputting van de Breng en status. Het wordt aangeraden DDoS-beveiliging standaard ingeschakeld op het virtuele netwerk WAF om te beschermen tegen Breng en protocol aanvallen. Zie voor meer informatie de [DDoS-bescherming verwijzing architecturen](#ddos-protection-reference-architectures) sectie.

### <a name="protection-planning"></a>Beveiliging plannen

Planning en voorbereiding zijn essentieel om te begrijpen hoe een systeem wordt uitgevoerd tijdens een DDoS-aanval. Het ontwerpen van een plan voor respons incidentbeheer maakt deel uit van deze inspanningen.

Als u hebt een DDoS-bescherming Standard, zorg ervoor dat ingeschakeld op het virtuele netwerk van internetgerichte eindpunten. DDoS-waarschuwingen configureren, kunt u voortdurend gecontroleerd op potentiële aanvallen op uw infrastructuur. 

U moet uw toepassingen onafhankelijk bewaken. Inzicht in de normale werking van een toepassing. Voorbereiden om te fungeren als de toepassing niet zoals verwacht tijdens een DDoS-aanval.

#### <a name="testing-through-simulations"></a>Testen via simulaties

Het is raadzaam uw veronderstellingen over hoe uw services op een aanval reageert door middel van periodieke simulaties testen. Valideren dat uw services of toepassingen werken blijven zoals verwacht en er geen onderbreking van de gebruikerservaring tijdens het testen. Hiaten uit oogpunt van een technologie- en het proces en deze implementeren in de strategie DDoS-antwoord. Het is raadzaam dat u deze tests uitvoert in faseringsomgevingen of tijdens daluren te minimaliseren, de impact op de productieomgeving.

We hebben samen met [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) voor het bouwen van een interface waarin Azure-klanten verkeer tegen openbare eindpunten DDoS-bescherming is ingeschakeld voor simulaties kunnen genereren. U kunt de [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) simulatie naar:

- Controleer hoe Azure DDoS-bescherming beschermt u uw Azure-resources tegen DDoS-aanvallen.

- Optimaliseren van uw proces voor respons op incidenten terwijl onder DDoS-aanval.

- Documenteer DDoS-compatibiliteit.

- Training van uw netwerk beveiliging teams.

Cybersecurity vereist constante innovatie in verdediging. Azure DDoS Standard-bescherming is een status-van-de nieuwste met een doeltreffende oplossing om het risico van steeds complexer DDoS-aanvallen te bieden.

## <a name="components-of-a-ddos-response-strategy"></a>Onderdelen van de strategie voor een DDoS-antwoord

Een DDoS-aanval die meestal gericht op Azure-resources vereist zo weinig mogelijk tussenkomst uit oogpunt van de gebruiker. Toch helpt opnemen DDoS risicobeperking als onderdeel van een strategie voor respons op incidenten de gevolgen voor de bedrijfscontinuïteit te minimaliseren.

### <a name="microsoft-threat-intelligence"></a>Microsoft dreigingen

Microsoft heeft een uitgebreide threat intelligence-netwerk. Dit netwerk maakt gebruik van de collectieve kennis van de beveiligingscommunity van een uitgebreide die ondersteuning biedt voor Microsoft online services, Microsoft-partners en relaties binnen de internet security-community. 

Microsoft ontvangt als een provider kritieke infrastructuur vroegtijdige waarschuwing over bedreigingen. Microsoft verzamelt dreigingen uit de online services en globale klantendatabase. Microsoft bevat alle deze dreigingen terug in de Azure DDoS-bescherming-producten.

De Microsoft Digital Crimes Unit (DCU) voert ook aanstootgevende strategieën tegen botnets. Botnets zijn gemeenschappelijke bron van de opdracht en controle van DDoS-aanvallen.

### <a name="risk-evaluation-of-your-azure-resources"></a>Risico-evaluatie van uw Azure-resources

het is noodzakelijk om te begrijpen van het bereik van het risico van een DDoS-aanval voortdurend. Periodiek afvragen: 
- Welke nieuwe openbaar Azure-resources beveiliging nodig?

- Is er één enkel storingspunt in de service? 

- Hoe kunnen services worden geïsoleerd, zodat de gevolgen van een aanval terwijl nog steeds services beschikbaar voor klanten geldig?

- Zijn er virtuele netwerken waar DDoS-bescherming standaard moet worden ingeschakeld, maar is niet? 

- Mijn services actief/actief met failover zijn over meerdere regio's?

### <a name="customer-ddos-response-team"></a>DDoS-beveiligingsteam klant

Maken van een team DDoS-antwoord is een belangrijke stap in het reageren op een aanval snel en efficiënt. Identificeer contactpersonen in uw organisatie die wordt overzien planning en uitvoering. Dit team DDoS-antwoord moet de Azure DDoS-bescherming standaardservice grondig begrijpen. Zorg ervoor dat het team kunt identificeren en beperken van een aanval door samenwerking met interne en externe klanten, met inbegrip van het team van Microsoft ondersteuning.

U wordt aangeraden dat u oefeningen als een normale deel van de beschikbaarheid van de service en de continuïteit planning gebruiken voor uw team DDoS-antwoord. Deze oefeningen bevatten schaal testen.

### <a name="alerts-during-an-attack"></a>Waarschuwingen tijdens een aanval

Azure DDoS-bescherming standaard identificeert en vermindert DDoS-aanvallen zonder tussenkomst van de gebruiker. Als u wilt op de hoogte worden gebracht als er een actieve Risicobeperking voor een beveiligde openbare IP-adres, kunt u [een waarschuwing configureren](../virtual-network/ddos-protection-manage-portal.md) op de metriek **onder DDoS-aanvallen of niet**. U kunt waarschuwingen voor de andere DDoS metrische gegevens om te begrijpen van de schaal van de aanval, verkeer wordt verwijderd en andere gegevens maken.

#### <a name="when-to-contact-microsoft-support"></a>Wanneer u contact opnemen met Microsoft ondersteuning

- Tijdens een DDoS-aanval u dat de prestaties van de beveiligde bron ernstig gedegradeerd, of de resource niet beschikbaar is.

- U denkt dat de DDoS-bescherming-service is niet werkt zoals verwacht. 

  De DDoS-bescherming-service wordt gestart risicobeperking alleen als de metrische waarde **beleid voor het activeren van DDoS risicobeperking (SYN TCP/TCP/UDP)** lager is dan het verkeer dat is ontvangen op de beveiligde openbare IP-resource.

- U bent van plan een zeer snelle gebeurtenis die wordt het netwerkverkeer aanzienlijk toenemen.

- Een actor heeft in het gedrang komen een DDoS-aanval op uw resources te starten.

Voor aanvallen die een invloed bedrijfskritieke maken een ernst-A [ondersteunen ticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>Stappen na aanval

Het is altijd een goede werkwijze een postmortem doen na een aanval en pas de strategie DDoS-antwoord naar wens aan. Aandachtspunten:

- Is er een onderbreking van de service of de gebruiker optreden vanwege een gebrek aan schaalbare architectuur?

- De meeste geleden welke toepassingen of services?

- Hoe effectief is de strategie DDoS-antwoord en hoe kan deze worden verbeterd?

Als u vermoedt dat u bent onder een DDoS-aanval, escaleren via uw normale kanalen voor ondersteuning van Azure.

## <a name="ddos-protection-reference-architectures"></a>DDoS-bescherming verwijzing architecturen

DDoS-bescherming standaard is ontworpen [voor services die zijn geïmplementeerd in een virtueel netwerk](../virtual-network/virtual-network-for-azure-services.md). Voor andere services, is de standaard DDoS-bescherming Basic service van toepassing. De volgende verwijzing architecturen zijn geordend op scenario's met architectuur patronen die zijn gegroepeerd.

### <a name="virtual-machine-windowslinux-workloads"></a>De werkbelasting van virtuele machine (Windows of Linux)

#### <a name="application-running-on-load-balanced-vms"></a>Toepassing die wordt uitgevoerd op virtuele machines Netwerktaakverdeling

Deze verwijzende architectuur wordt een reeks bewezen voor het uitvoeren van meerdere Windows virtuele machines in een schaal ingesteld achter een load balancer, beschikbaarheid en schaalbaarheid te verbeteren. Deze architectuur kan worden gebruikt voor elke staatloze werkbelasting, zoals een webserver.

![Diagram van de referentiearchitectuur voor een toepassing die wordt uitgevoerd op virtuele machines Netwerktaakverdeling](media/azure-ddos-best-practices/image9.png)

In deze architectuur wordt een werkbelasting verdeeld over meerdere VM-exemplaren. Er is één openbaar IP-adres en internetverkeer wordt gedistribueerd naar de virtuele machines via een load balancer. DDoS-bescherming standaard is ingeschakeld op het virtuele netwerk van de load balancer van Azure (internet) met het openbare IP-adres zijn gekoppeld.

De load balancer verdeelt inkomende aanvragen van internet naar de VM-exemplaren. Virtuele-machineschaalsets toestaan dat het aantal virtuele machines worden geschaald in- of handmatig of automatisch op basis van vooraf gedefinieerde regels. Dit is belangrijk als de bron zich onder DDoS-aanval. Zie voor meer informatie over deze verwijzende architectuur [in dit artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm).

#### <a name="application-running-on-windows-n-tier"></a>Toepassing die wordt uitgevoerd op Windows-N-laag

Er zijn veel manieren om een architectuur met meerdere lagen te implementeren. Het volgende diagram toont een typische drie lagen-webtoepassing. Deze architectuur is gebaseerd op het artikel [uitvoeren van virtuele machines voor schaalbaarheid en beschikbaarheid taakverdeling](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). De web- en bedrijfslagen gebruiken VM's met taakverdeling.

![Diagram van de referentiearchitectuur voor een toepassing die wordt uitgevoerd op Windows-N-laag](media/azure-ddos-best-practices/image10.png)

In deze architectuur is DDoS-bescherming standaard ingeschakeld op het virtuele netwerk. Alle openbare IP-adressen in het virtuele netwerk ophalen DDoS-bescherming voor laag 3 en 4. Voor beveiliging van laag 7, Application Gateway in de SKU WAF te implementeren. Zie voor meer informatie over deze verwijzende architectuur [in dit artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier).

#### <a name="paas-web-application"></a>PaaS-webtoepassing

Deze verwijzende architectuur ziet u een Azure App Service-toepassing uitgevoerd in één regio. Deze architectuur wordt een reeks bewezen voor een webtoepassing die gebruikmaakt van [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/) en [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).
Een stand-by-regio is ingesteld voor failover-scenario's.

![Diagram van de referentiearchitectuur voor een PaaS-webtoepassing](media/azure-ddos-best-practices/image11.png)

Azure Traffic Manager stuurt binnenkomende aanvragen naar de Application Gateway in een van de regio's. Deze stuurt aanvragen aan de toepassingsgateway in het actieve gebied tijdens normale bewerkingen. Als deze regio niet beschikbaar is, wordt Traffic Manager overgenomen door aan de toepassingsgateway in stand-by-regio.

Al het verkeer van het internet bestemd is voor de webtoepassing wordt doorgestuurd naar de [Application Gateway openbaar IP-adres](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) via Traffic Manager. In dit scenario wordt de app-service (web-app) zelf is niet rechtstreeks extern gerichte en wordt beveiligd door Application Gateway. 

Het is raadzaam dat u de Application Gateway-SKU WAF configureert (voorkomen modus) te beschermen tegen aanvallen van laag 7 (HTTP/HTTPS/WebSocket). Bovendien web-apps zijn geconfigureerd voor [accepteren alleen verkeer van de toepassingsgateway](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) IP-adres.

Zie voor meer informatie over deze verwijzende architectuur [in dit artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region).

### <a name="mitigation-for-non-web-paas-services"></a>Risicobeperking voor niet-PaaS-webservices

#### <a name="hdinsight-on-azure"></a>HDInsight op Azure

Deze verwijzende architectuur toont DDoS-bescherming standaard voor het configureren van een [Azure HDInsight-cluster](https://docs.microsoft.com/azure/hdinsight/). Zorg ervoor dat het HDInsight-cluster is gekoppeld aan een virtueel netwerk en dat DDoS-bescherming is ingeschakeld op het virtuele netwerk.

!['HDInsight' en 'Geavanceerd' deelvensters met virtuele-netwerkinstellingen](media/azure-ddos-best-practices/image12.png)

![Selectie voor het inschakelen van DDoS-bescherming](media/azure-ddos-best-practices/image13.png)

In deze architectuur wordt verkeer dat is bestemd voor het HDInsight-cluster vanaf het internet worden doorgestuurd naar het openbare IP-adres die zijn gekoppeld aan de load balancer van HDInsight-gateway. De load balancer van gateway stuurt het verkeer naar de hoofdknooppunten of worker-knooppunten rechtstreeks. Omdat DDoS-bescherming standaard is ingeschakeld op het virtuele netwerk van HDInsight, krijgen alle openbare IP-adressen in het virtuele netwerk DDoS-bescherming voor laag 3 en 4. Deze verwijzende architectuur kan worden gecombineerd met de lagen en meerdere landen/regio verwijzing architecturen.

Zie voor meer informatie over deze verwijzende architectuur, het [uitbreiden Azure HDInsight met behulp van een Azure Virtual Network](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) documentatie.

### <a name="azure-api-management"></a>Azure API Management

Deze verwijzende architectuur beschermt de openbaar eindpunt van de [Azure API Management](../api-management/api-management-key-concepts.md) resource publiceren van API's voor klanten buiten de organisatie. API Management implementeren in een extern virtueel netwerk DDoS-beveiliging in te schakelen.

![Diagram van de referentiearchitectuur voor API Management](media/azure-ddos-best-practices/image15.png)

Wanneer u de extern virtueel netwerk configureert, zijn de API Management-gateway en developer-portal toegankelijk is vanaf het openbare internet via een openbare load balancer. In deze architectuur is DDoS-bescherming standaard ingeschakeld op de extern virtueel netwerk voor API Management. Verkeer wordt gerouteerd via internet naar het openbare IP-adres van API Management, die is beveiligd tegen netwerkaanvallen op laag 3 en 4. Om te beschermen tegen aanvallen van laag 7 HTTP/HTTPS, kunt u Application Gateway configureren in de modus WAF.

Zie voor een lijst van extra services die zijn geïmplementeerd in een virtueel netwerk en kunnen worden geconfigureerd voor DDoS-bescherming standaard [in dit artikel](../virtual-network/virtual-network-for-azure-services.md). DDoS-bescherming standaard ondersteunt alleen Azure Resource Manager-resources. 

> [!NOTE]
> Geïnjecteerde implementatie van App Service-omgeving voor PowerApps in een virtueel netwerk met een openbare IP-adres wordt niet standaard ondersteund. Zie voor meer informatie over het beveiligen van App Service-omgeving in deze sectie.

## <a name="next-steps"></a>Volgende stappen

* [Azure DDoS-bescherming-productpagina](https://azure.microsoft.com/services/ddos-protection/)

* [Azure DDoS-bescherming-blog](http://aka.ms/ddosblog)

* [Documentatie voor Azure DDoS-bescherming](../virtual-network/ddos-protection-overview.md)
