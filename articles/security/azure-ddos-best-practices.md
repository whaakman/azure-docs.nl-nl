---
title: Aanbevolen procedures voor Azure DDoS-bescherming & referentiearchitectuur | Microsoft Docs
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
ms.openlocfilehash: 4b2d785f5b9095a2decfc65ec46808ff6f65c38e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="azure-ddos-protection-best-practices-and-reference-architecture"></a>Azure DDoS-bescherming: Best practices en verwijzen naar architectuur

Dit document is gericht op IT-besluitvormers en beveiligingspersoneel. Bekend bent met Azure, netwerken en beveiliging worden verwacht.

Ontwerpen voor gedistribueerde DOS-aanval (DDoS) vereist tolerantie plannen en ontwerpen voor verschillende modi voor fout. Dit document bevat aanbevolen procedures voor het ontwerpen van toepassingen in Azure voor tolerantie tegen DDoS-aanvallen.

## <a name="types-of-attacks"></a>Typen aanvallen

DDoS is een type aanval gebruikt in een poging om de bronnen van de toepassing. Het doel is van invloed op de beschikbaarheid van de toepassing en de mogelijkheid om legitieme aanvragen te verwerken. Aanvallen worden steeds meer geavanceerde en groter in grootte en impact. DDoS-aanvallen kunnen worden gericht op een willekeurig eindpunt dat openbaar bereikbaar is via het Internet.

Azure biedt continue bescherming tegen DDoS-aanvallen. Deze beveiliging is geïntegreerd in de Azure-platform, standaard en zonder extra kosten. Naast de kern DDoS-bescherming als gegeven van het platform, we hebben ook een nieuwe aanbieding met de naam '[Azure DDoS-bescherming standaard](https://azure.microsoft.com/services/ddos-protection/)', waardoor biedt geavanceerde mogelijkheden voor DDoS risicobeperking tegen netwerkaanvallen en is ter bescherming van uw specifieke Azure-resources automatisch zijn afgestemd. Beveiliging is heel eenvoudig om in te schakelen tijdens het maken van nieuwe virtuele netwerken. Het is ook mogelijk na het eerste maken en vereist geen wijzigingen toepassing of bron.

![](media/azure-ddos-best-practices/image1.png)

DDoS-aanvallen kunnen grote schaal worden ingedeeld in verschillende categorieën drie (3)

### <a name="volumetric-attacks"></a>Breng aanvallen

Breng aanvallen zijn de meest voorkomende typen DDoS-aanval. Breng aanvallen zijn gebruikt om gewelddadige die zijn gericht op de netwerk- en lagen. Ze proberen tot resources dergelijke netwerkkoppelingen. Meerdere geïnfecteerde systemen deze aanvallen meestal gebruikt voor de netwerklagen met een aanzienlijke hoeveelheid verkeer ogenschijnlijk betrouwbare overspoelen. Andere netwerklaagprotocollen zoals Internet Control Message Protocol (ICMP), User Datagram Protocol (UDP) en Transmission Control Protocol (TCP) worden gebruikt in dit type aanval.

Het meest gebruikt netwerklaag DDoS-aanvallen zijn TCP SYN overspoelen, ICMP echo UDP overspoelen, DNS- en NTP versterkingsfactor aanval. Dit type aanval kan niet alleen voor de service, maar ook verstoren als een smokescreen voor slechte en meer gerichte netwerk inbraakdetectie worden gebruikt. Een voorbeeld van een recente Breng aanval is de [Memcached misbruik](https://www.wired.com/story/github-ddos-memcached/) die van invloed op GitHub. Deze aanval gericht UDP-poort 11211 en 1.35 Tb/s aanval volume gegenereerd.

### <a name="protocol-attacks"></a>Protocol-aanvallen

Protocol aanvallen doel toepassingsprotocollen. Ze proberen te gebruiken om alle beschikbare bronnen in de infrastructuurapparaten, zoals firewalls, toepassingsservers en netwerktaakverdelers. Protocol aanvallen gebruiken pakketten die zijn onjuist gevormd of protocol afwijkingen bevatten. Deze aanvallen werken door te groot aantal openstaande aanvragen, servers en andere communicatie apparaten antwoord en wacht tot de reactie van een pakket verzenden. Het doel probeert om te reageren op de openstaande aanvragen uiteindelijk waardoor het betreffende systeem vastlopen.

De meest voorkomende voorbeeld van een protocol DDoS-aanval is TCP SYN overstroming. In deze aanval is een opeenvolging van aanvragen voor TCP SYN is gericht op een doel en kan worden gebruikt voor het overbelast. Het doel is om het doel niet reageert. De 2016 Dyn onderbreking, behalve als een aanval toepassingslaag bestond ook uit TCP SYN overstromingen die gericht is op poort 53 van Dyn van DNS-servers.

### <a name="resource-attacks"></a>Resource-aanvallen

Resource-aanvallen gericht zijn op de toepassingslaag. Deze activeren back-end-processen in een poging het doelsysteem overbelast. Resource-aanvallen misbruik verkeer die er ongeveer normale uitziet maar dat uitvoert CPU-intensief query's naar de server. De hoeveelheid verkeer die nodig zijn voor de bronnen is relatief lager is dan die van het andere type aanvallen. Het verkeer bij een aanval van de resource is te onderscheiden van legitieme verkeer, waardoor het moeilijk te detecteren. De meest voorkomende resource-aanvallen zijn op HTTP/HTTPS- en DNS-services.

## <a name="shared-responsibility-in-the-cloud"></a>Gedeelde verantwoordelijkheid in de cloud

Een strategie voor de diepgaande verdediging is vereist voor de bestrijding de toenemende typen en verfijning van aanvallen. Beveiliging is een gedeelde verantwoordelijkheid tussen de klant en Microsoft. Microsoft verwijst naar deze als een [gedeelde verantwoordelijkheid model](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). De afbeelding hieronder ziet deze deling van verantwoordelijkheid.

![](media/azure-ddos-best-practices/image2.png)

Azure-klanten profiteren van onze best practices te controleren en bouwen van globaal gedistribueerde toepassingen ontworpen en getest voor mislukte.

## <a name="fundamental-best-practices"></a>Fundamentele aanbevolen procedures

DDoS-aanvallen zijn en toenemen. De volgende sectie bevat richtlijnen om samen te stellen robuuste DDoS-services in Azure.

### <a name="design-for-security"></a>Ontwerpen voor beveiliging

Klanten moeten ervoor zorgen voor beveiliging is een prioriteit gedurende de volledige levenscyclus van een toepassing, in de ontwerp- en implementatiestappen voor implementatie en bewerkingen. Toepassingen kunnen fouten waardoor een relatief lage volume van vervaardigde aanvragen voor het gebruik van een heel van resources, wat resulteert in een onderbreking van deze service hebben. Ter bescherming van een service die wordt uitgevoerd op Microsoft Azure, klanten een goede kennis van de toepassingsarchitectuur moet hebben en moet zich richten op de [5 stijlen van software quality](https://docs.microsoft.com/azure/architecture/guide/pillars).
Klanten moeten weten volumes normale verkeer, het model connectiviteit tussen de toepassing en andere toepassingen en de service-eindpunten blootgesteld aan het openbare Internet.

Bovendien is is een toepassing robuuste voor het afhandelen van denial of service die gericht is op de toepassing zelf gezorgd van essentieel belang. Beveiliging en privacy zijn rechtstreeks ingebouwd in de Azure-platform vanaf de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). De SDL beveiliging adressen aan elke ontwikkelingsfase en zorgt ervoor dat Azure voortdurend wordt bijgewerkt zodat deze nog beter beveiligen.

### <a name="design-for-scalability"></a>Ontwerp voor schaalbaarheid

Schaalbaarheid is de mogelijkheid van een systeem om toegenomen belasting aan te kunnen. Klanten moeten toegang tot hun toepassingen ontwerpen [horizontaal schalen](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) om te voldoen aan de vraag van versterkt belasting, speciaal in geval van een DDoS-aanval. Als uw toepassing afhankelijk van één exemplaar van een service is, maakt het een potentieel risico. Inrichting van meerdere exemplaren verbetert de tolerantie en de schaalbaarheid.

Voor [Azure App Service](../app-service/app-service-value-prop-what-is.md), selecteer een [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) die biedt meerdere exemplaren. Configureer elk van uw rollen te gebruiken voor Azure Cloud Services [meerdere exemplaren](../cloud-services/cloud-services-choose-me.md). Voor [Azure Virtual Machines (VM's)](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), zorg ervoor dat uw VM-architectuur meer dan één virtuele machine bevat en dat elke virtuele machine wordt opgenomen in een [beschikbaarheidsset](../virtual-machines/virtual-machines-windows-manage-availability.md). Wordt u aangeraden [virtuele-Machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor automatisch schalen mogelijkheden.

### <a name="defense-in-depth"></a>Gedegen

Het idee achter gedegen is het risico's beheren met diverse defensive strategieën. Stapelen beveiligingen in een toepassing vermindert de kans op een geslaagde aanval. Het is raadzaam dat klanten beveiligde ontwerpen voor hun toepassingen via de ingebouwde mogelijkheden van het Azure-platform implementeren.

Bijvoorbeeld, verhoogd het risico van aanvallen met de grootte of surface area van de toepassing. Oppervlak vermindering wordt aanbevolen via whitelisting omlaag sluit de blootgestelde IP adres ruimte & luisterende poorten die nodig zijn niet op de load balancers ([ALB](../load-balancer/load-balancer-get-started-internet-portal.md)/[App Gateway](../application-gateway/application-gateway-create-probe-portal.md)) of via [Netwerkbeveiligingsgroepen (NSG).](../virtual-network/virtual-networks-nsg.md)
Kunt u [service labels](/virtual-network/security-overview.md) en [toepassing beveiligingsgroepen](/virtual-network/security-overview.md) complexiteit voor het maken van de regel beveiliging beperken en netwerkbeveiliging configureren als een natuurlijke extensie van de structuur van een toepassing.

Klanten dienen implementeren Azure-services in een [virtueel netwerk](../virtual-network/virtual-networks-overview.md) indien mogelijk. Hierdoor kunnen de bronnen van de service te communiceren via privé IP-adressen. Verkeer van de Azure-service van een virtueel netwerk wordt standaard openbare IP-adressen gebruikt als bron-IP-adressen. Met behulp van [service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) netwerkverkeer voor de service voor het gebruik van virtueel netwerk particuliere adressen als de bron-IP-adressen bij het openen van de Azure-service van een virtueel netwerk wordt overgeschakeld.

Vaak ziet van de klant lokale bronnen ophalen aangevallen samen met hun bronnen in Azure. Als u een on-premises-omgeving naar Azure verbindt, wordt u aangeraden minimaliseert u blootstelling van lokale bronnen aan het openbare Internet. U kunt de schaal en geavanceerde DDoS-bescherming-mogelijkheden van Azure gebruiken door het implementeren van uw bekende openbare entiteiten in Azure. Aangezien deze openbaar toegankelijk entiteiten vaak een doel voor DDoS-aanvallen zijn vermindert als ze in Azure de gevolgen voor uw lokale bronnen.

## <a name="azure-ddos-protection"></a>Azure DDoS-bescherming

Azure heeft twee DDoS serviceaanbiedingen die bescherming tegen aanvallen (laag 3 en 4) - DDoS basic netwerkbeveiliging en standaard DDoS-bescherming bieden. 

### <a name="azure-ddos-basic-protection"></a>Azure basic DDoS-bescherming

Basic-beveiliging is geïntegreerd in de Azure standaard zonder extra kosten. De volledige schaal en de capaciteit van globaal geïmplementeerde netwerk van Azure biedt bescherming tegen veelvoorkomende laag netwerkaanvallen via altijd op de controle- en realtime risicobeperking verkeer. DDoS Basic-bescherming vereist geen wijzigingen van configuratie- of gebruiker. Alle Azure-services waaronder PaaS-services zoals Azure DNS worden beschermd door Basic DDoS-bescherming.

![](media/azure-ddos-best-practices/image3.png)

Basic DDoS-bescherming van Azure bestaat uit software- en hardware-onderdelen. Een besturingselement software vlak bepaalt wanneer, waar en welk type verkeer via hardware-apparaten die analyseren en verwijderen van de aanval verkeer moet worden aangestuurd. Het besturingselement vlak maakt deze beslissing op basis van een infrastructuur wide DDoS-bescherming *beleid*, die is statisch en universeel toegepast op alle Azure-klanten.

Het beleid DDoS-bescherming wordt bijvoorbeeld aangegeven welk verkeersvolume de beveiliging moet worden *geactiveerd* (dat wil zeggen, de tenant-verkeer moet worden gerouteerd via toestellen scrubbing), en vervolgens hoe de scrubbing apparaten moeten *verhelpen* de aanval.

DDoS-bescherming Basic van Azure-service is gericht op beveiliging van de infrastructuur en beveiliging van de Azure-platform. Dit vermindert verkeer wanneer deze groter is dan het tarief dat is dus belangrijk dat deze mogelijk gevolgen hebben voor meerdere klanten in een omgeving met meerdere tenants. Het niet mogelijk om waarschuwingen of per klant aangepast beleid.

### <a name="azure-ddos-standard-protection"></a>Azure standard DDoS-bescherming

Standaard beveiliging biedt verbeterde DDoS risicobeperking functies en automatisch zijn afgestemd ter bescherming van uw specifieke Azure-resources in een virtueel netwerk. De beveiliging is eenvoudig in te schakelen voor een nieuwe of bestaande virtuele netwerk en vereist geen toepassing of wijzigingen in de resourcedefinitie. Heeft verschillende voordelen ten opzichte van de basic service zoals logboekregistratie, waarschuwingen en telemetrie. Onderstaande zijn de belangrijkste differentiators van Azure DDoS-bescherming Standard-service.

#### <a name="adaptive-realtime-tuning"></a>Adaptieve realtime afstemmen

DDoS-bescherming Basic-Azure service beschermt klanten, maar beveiligt alleen om te voorkomen dat van invloed op de andere klanten. Bijvoorbeeld, als een service wordt ingericht voor een typische volume van legitieme inkomend verkeer die kleiner is dan de *trigger snelheid* van de gehele infrastructuur DDoS protection-beleid een DDoS-aanval op de bronnen van de klant kan gaan opgemerkt. Meer in het algemeen de complexe aard van recente aanvallen (bijvoorbeeld meerdere vector DDoS), evenals de toepassingsspecifieke gedrag van tenants aanroep voor de per klant, aangepaste protection-beleid.
Dit wordt gedaan met behulp van twee insights:

1. Automatisch leren van (per-IP) per klant laag-3/4-verkeerspatronen, en
2. Fout-positieven minimaliseren gezien het feit dat is toegestaan aanzienlijke hoeveelheid verkeer kunnen Azure schaal.

![](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring--alerting"></a>DDoS-bescherming telemetrie, bewaking en waarschuwingen

Met DDoS-bescherming standaard, geven we uitgebreide telemetrie via [Azure Monitor](/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) tijdens de duur van een DDoS-aanval. Waarschuwingen kan worden geconfigureerd voor een van de Azure-Monitor metrische gegevens die worden weergegeven door DDoS-bescherming. Logboekregistratie kan worden geïntegreerd met Splunk (Azure Event Hubs), Azure Log Analytics en Azure Storage voor geavanceerde analyse via de interface van de Monitor diagnostische Azure-gegevens.

##### <a name="ddos-mitigation-policies"></a>DDoS risicobeperking beleid

Klik in de Azure-portal op **Monitor** \> **metrische gegevens**. Op de **metrische gegevens** scherm, selecteert u de resourcegroep, brontype van openbare IP-adres en uw Azure openbaar IP-adres. DDoS metrische gegevens zijn zichtbaar in het deelvenster van de beschikbare metrische gegevens.

DDoS-bescherming standaard van toepassing is **drie autotuned risicobeperking beleidsregels (TCP SYN, TCP en UDP)** voor elke openbare IP-adres van de beveiligde bron, in het VNET dat DDoS ingeschakeld is. U kunt de drempels beleid weergeven door de metriek **'Het aantal inkomende pakketten voor het activeren van DDoS risicobeperking'**.

![](media/azure-ddos-best-practices/image7.png)

De drempelwaarden beleid zijn automatisch geconfigureerde via onze machine learning-gebaseerde netwerkverkeer profilering. DDoS Risicobeperking voor een IP-adres aangevallen vindt alleen plaats wanneer de beleid-drempelwaarde is overschreden.

##### <a name="under-ddos-attack"></a>Bij een DDoS-aanval

Als het openbare IP-adres aangevallen is, de metrische waarde voor 'onder DDoS aanval of niet' in-of uitschakelen op 1 als we risicobeperking op het verkeer van de aanval uitvoeren.

![](media/azure-ddos-best-practices/image8.png)

Het is raadzaam om een waarschuwing configureren in deze metriek, om te worden geïnformeerd wanneer er een actieve DDoS risicobeperking uitgevoerd op uw openbare IP-adres.

Zie voor meer informatie [beheren Azure DDoS-bescherming standaard met de Azure portal](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="resource-attacks"></a>Resource-aanvallen

Specifiek zijn voor aanvallen van de resource op de toepassingslaag, klanten Configureer Web Application Firewall (WAF) om te helpen beveiligde webtoepassingen. WAF inspecteert inkomend webverkeer voor het blokkeren van SQL injectie, Cross-Site Scripting DDoS en andere aanvallen laag 7. Azure biedt [WAF als een functie van toepassingsgateway](../application-gateway/application-gateway-web-application-firewall-overview.md) voor gecentraliseerde beveiliging van uw webtoepassingen van algemene aanvallen en beveiligingsproblemen. Er zijn soorten WAF aanbiedingen beschikbaar is via de Azure-partners die mogelijk beter geschikt is voor uw behoeften via de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1).

Zelfs Web Application Firewalls zijn vatbaar voor aanvallen van uitputting van de Breng & status. Het wordt aangeraden inschakelen van de standaard DDoS-beveiliging op het virtuele netwerk WAF te beschermen tegen Breng & protocol aanvallen. Zie de sectie van de architectuur van documentatie voor meer informatie.

### <a name="protection-planning"></a>Beveiliging plannen

Planning en voorbereiding zijn essentieel om te begrijpen hoe een systeem wordt uitgevoerd tijdens een DDoS-aanval. Deze planning en voorbereiding helpt ook bij het ontwerpen van een plan voor respons van incidentbeheer.

Klanten moeten ervoor zorgen dat DDoS-bescherming standaard is ingeschakeld op het virtuele netwerk van internet internetgerichte eindpunten. DDoS-waarschuwingen configureren wordt voorkomen dat een constante gaten te houden op potentiële aanvallen op uw infrastructuur. Klanten moeten hun toepassingen onafhankelijk bewaken. Ze moeten naar de normale werking van de toepassing te begrijpen. Stappen moeten worden genomen als de toepassing niet zoals verwacht tijdens een DDoS-aanval.

#### <a name="ddos-attacks-orchestration"></a>DDoS-aanvallen orchestration

Het is raadzaam uw veronderstellingen over hoe uw services op een aanval reageren voordat dit gebeurt door middel van periodieke simulaties testen. Valideren dat uw services of toepassingen werken blijven zoals verwacht en er geen onderbreking van de gebruikerservaring tijdens het testen. Hiaten uit oogpunt van een technologie- & proces en in de strategie DDoS-antwoord opnemen. Een algemene aanbeveling is voor het uitvoeren van deze tests in testomgevingen of tijdens daluren gevolgen voor de productie-omgeving te minimaliseren.

We hebben samen met [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) voor het bouwen van een interface waarin Azure-klanten verkeer tegen DDoS-bescherming kunnen genereren openbare eindpunten voor simulaties ingeschakeld. [Onderbrekingspunt Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) simulatie kunt u de:

- Valideren van de manier waarop Microsoft Azure DDoS-bescherming uw Azure-resources beschermt tegen DDoS-aanvallen

- Optimaliseren van uw proces voor respons op incidenten terwijl onder DDoS-aanval

- Document DDoS naleving

- Training van uw netwerk beveiliging teams

Cybersecurity is een aanhoudend strijd constante innovatie in verdediging vereisen. Azure standaard DDoS-bescherming is een geavanceerde geavanceerde aanbieding voor klanten met een doeltreffende oplossing om het risico van steeds complexer DDoS-aanvallen te bieden.

## <a name="components-of-a-ddos-response-strategy"></a>Onderdelen van de strategie voor een DDoS-antwoord

In de meeste gevallen wanneer een DDoS-aanval is gericht op Azure-resources, is dit zo weinig mogelijk tussenkomst vanuit het oogpunt van een eindgebruiker is vereist. Toch zorgt opnemen DDoS risicobeperking als onderdeel van de organisatie respons op incidenten strategie ervoor minimale gevolgen voor de bedrijfscontinuïteit.

### <a name="microsoft-threat-intelligence"></a>Microsoft dreigingen

Microsoft heeft een uitgebreide threat intelligence-netwerk dat gebruikmaakt van de collectieve kennis van de beveiligingscommunity van een uitgebreide die ondersteuning biedt voor Microsoft online services, Microsoft-partners en relaties binnen de Internet security-community. Als een provider kritieke infrastructuur Microsoft ontvangt vroegtijdige waarschuwing over bedreigingen, duurt dreigingen geleerd uit andere Microsoft online services en de klant van Microsoft globale base. Alle dreigingen van Microsoft wordt opgenomen in de Azure DDoS-bescherming-producten.

Daarnaast voert van Microsoft Digital Crimes Unit (DCU) aanstootgevende strategieën tegen botnets, een gemeenschappelijke bron van de opdracht en controle voor DDoS-aanvallen.

### <a name="perform-a-risk-evaluation-of-your-azure-resources"></a>Uitvoeren van een risico-evaluatie van uw Azure-resources

het is noodzakelijk om te begrijpen van het bereik van het risico van een DDoS-aanval voortdurend. Klanten moeten periodiek vragen zelf: welke nieuwe openbaar Azure-resources moeten worden beveiligd? Is er één punt van fouten in de service? Hoe kunnen services worden geïsoleerd, zodat de gevolgen van een aanval terwijl nog steeds services beschikbaar voor klanten geldig? Zijn er virtuele netwerken waar DDoS-bescherming standaard moet worden ingeschakeld, maar is niet? Mijn services actief/actief met failover zijn over meerdere regio's?

### <a name="customer-ddos-response-team"></a>DDoS-beveiligingsteam klant

Maken van een team DDoS-antwoord is een belangrijke stap bij effectieve en snelle reactie op een aanval. Klanten moeten verschillende contactpersonen in uw organisatie die wordt overzien planning en uitvoering identificeren. Het antwoord DDoS Team grondig begrip van de Azure DDoS-bescherming standaardservice moet hebben en moet apt in het identificeren van aanvallen coördinatie met verschillende interne en externe klanten, waaronder Microsoft ondersteuningsteam beperkende indien nodig.

Microsoft raadt aan DDoS Response Team plannen en simulatie oefeningen, met inbegrip van de schaal testen als een normaal onderdeel van de beschikbaarheid van de service en het plannen van continuïteit opnemen. 

### <a name="during-an-attack"></a>Tijdens een aanval

Azure DDoS-bescherming standaard wordt het identificeren en verhelpen DDoS-aanvallen zonder tussenkomst van de gebruiker. Als u wilt op de hoogte worden gebracht als er een actieve Risicobeperking voor een beveiligde openbare IP-adres, kunt u [een waarschuwing configureren](../virtual-network/ddos-protection-manage-portal.md) op de metriek 'onder DDoS aanval of niet'. U kunt meer controle en waarschuwingen voor de andere DDoS metrische gegevens om te begrijpen van de schaal van de aanval, verkeer wordt verwijderd, enzovoort.

#### <a name="when-to-contact-microsoft-support"></a>Wanneer u contact opnemen met Microsoft ondersteuning

- Als tijdens een DDoS-aanval u dat merkt de prestaties van de beveiligde bron ernstig is beschadigd of de resource is niet beschikbaar.

- Als u denkt dat de DDoS-bescherming-service is niet werkt dat zoals verwacht. DDoS-bescherming-service zal risicobeperking alleen starten als de onderstaande criteria wordt voldaan:

    - De metrische waarde ' beleid voor het activeren van DDoS risicobeperking (SYN TCP/TCP/UDP) is lager dan het verkeer dat is ontvangen op de beveiligde bron van de openbare IP-adres.

- Als u weet, gaat u een geplande een gebeurtenis die tot een aanzienlijke toename van het netwerkverkeer leiden hebben.

- Als een actor in het gedrang komen heeft een DDoS-aanval op uw resources te starten.

Voor kritieke zakelijke problemen beïnvloeden, maakt u een ernst-A [ondersteunen ticket](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>Stappen voor post-aanval

Het is altijd een goede werkwijze een postmortem doen na een aanval en past u de strategie DDoS-antwoord naar behoefte. Aandachtspunten:

- Is er een onderbreking van de service of door eindgebruikers optreden vanwege een gebrek aan schaalbare architectuur?

- De meeste geleden welke toepassingen of services?

- Hoe effectief is de strategie DDoS-antwoord en hoe kan deze worden verbeterd verder?

Escaleren via uw normale ondersteuning van Azure-kanalen als u vermoedt dat u bent onder een DDoS-aanval is.

## <a name="ddos-protection-reference-architectures"></a>DDoS-bescherming verwijzing architecturen

DDoS-bescherming standaard is ontworpen [voor services die zijn geïmplementeerd in een virtueel netwerk.](../virtual-network/virtual-network-for-azure-services.md) Voor andere services op de standaardwaarde Basic DDoS-bescherming van toepassing. De specifieke verwijzing architecturen onderstaande zijn geordend op scenario's met architectuur patronen die zijn gegroepeerd.

### <a name="virtual-machine-windowslinux-workloads"></a>De werkbelasting van virtuele machine (Windows of Linux)

#### <a name="application-running-on-load-balanced-vms"></a>Een toepassing werklast waarop virtuele machines met gelijke taakverdeling

Deze referentiearchitectuur bevat een reeks beproefde procedures voor het uitvoeren van meerdere virtuele Windows-machines (VM's) in een schaalset achter een load balancer, om de beschikbaarheid en schaalbaarheid te verbeteren. Deze architectuur kan worden gebruikt voor elke staatloze werkbelasting, zoals een webserver.

![](media/azure-ddos-best-practices/image9.png)

In deze architectuur wordt een werkbelasting verdeeld over meerdere VM-exemplaren. Er is één openbaar IP-adres en internetverkeer wordt verdeeld over de virtuele machines met behulp van een load balancer. DDoS-bescherming standaard is ingeschakeld op het virtuele netwerk van de load balancer van Azure (internet) met het openbare IP-adres zijn gekoppeld.

De load balancer verdeelt inkomende aanvragen van Internet naar de VM-exemplaren. VM-schaalsets toestaan dat het aantal virtuele machines worden geschaald in- of handmatig of automatisch op basis van vooraf gedefinieerde regels. Dit is belangrijk als de bron zich onder DDoS-aanval. Neem deze [artikel](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm), voor meer informatie over deze verwijzende architectuur.

#### <a name="applications-running-on-windows-n-tier"></a>Toepassingen die worden uitgevoerd op Windows-N-laag

Er zijn veel manieren om een architectuur met meerdere lagen te implementeren. In het diagram ziet u een kenmerkende webtoepassing met drie lagen. Deze architectuur is gebaseerd op [uitvoeren van virtuele machines voor schaalbaarheid en beschikbaarheid taakverdeling](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). De web- en bedrijfslagen gebruiken VM's met taakverdeling.

![](media/azure-ddos-best-practices/image10.png)

In de bovenstaande architectuur is DDoS-bescherming standaard ingeschakeld op het virtuele netwerk. Alle openbare IP-adressen in het virtuele netwerk krijgen Layer 3/Layer4 DDoS-bescherming. Voor beveiliging van laag 7, moeten Application Gateway in WAF SKU worden geïmplementeerd. Raadpleeg [dit](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier) artikel voor meer informatie over deze verwijzende architectuur.

#### <a name="paas-web-application"></a>PaaS-webtoepassing

Deze verwijzende architectuur ziet u een Azure App Service-toepassing uitgevoerd in één regio. Deze architectuur toont verzameling bewezen voor een webtoepassing die gebruikmaakt van [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/) en [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).
Stand-by-regio is ingesteld voor failover-scenario's.

![](media/azure-ddos-best-practices/image11.png)

Traffic Manager stuurt binnenkomende aanvragen naar de Application Gateway in een van de regio's. Deze stuurt aanvragen voor de toepassingsgateway in actieve regio tijdens normale bewerkingen. Als deze regio niet beschikbaar is, wordt Traffic Manager overgenomen door de toepassingsgateway in stand-by-regio.

Het verkeer van Internet bestemd is voor de webtoepassing wordt doorgestuurd naar de [Application Gateway openbaar IP-adres](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) via de Traffic Manager. In dit scenario wordt de app-service (web-apps) zelf is niet rechtstreeks extern gerichte en wordt beveiligd door Application Gateway. Het verdient aanbeveling voor het configureren van Application Gateway WAF SKU (voorkomen modus) te beschermen tegen aanvallen van laag 7 (HTTPS-HTTP-webpagina Socket). Bovendien de Web-apps zijn geconfigureerd voor [accepteren van verkeer van de toepassingsgateway alleen](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) IP-adres.

Raadpleeg [dit](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region) artikel voor meer informatie over deze verwijzende architectuur.

### <a name="mitigation-for-non-web-paas-services"></a>Risicobeperking voor niet-PaaS-webservices

#### <a name="hdinsight-on-azure"></a>HDInsight op Azure

Deze verwijzende architectuur toont configureren DDoS-bescherming Std. voor [HDInsight-cluster](https://docs.microsoft.com/azure/hdinsight/) op Azure. U moet ervoor zorgen dat de HDInsight-cluster is gekoppeld aan een virtueel netwerk en DDoS-bescherming is ingeschakeld op dit virtuele netwerk.

![](media/azure-ddos-best-practices/image12.png)

![](media/azure-ddos-best-practices/image13.png)

In deze architectuur wordt het verkeer dat is bestemd voor het HDInsight-cluster vanaf het internet worden doorgestuurd naar het openbare IP-adres die zijn gekoppeld aan de load balancer van HDInsight-gateway. De load balancer van gateway stuurt het verkeer naar de hoofdknooppunten of worker-knooppunten rechtstreeks. Opgegeven DDoS-bescherming standaard is ingeschakeld op het virtuele netwerk van HDInsight, alle openbare IP-adressen in het virtuele netwerk krijgt Layer 3/Layer4 DDoS-bescherming. De bovenstaande referentiearchitectuur kan worden gecombineerd met N-laag/meerdere-Region reference-architecturen.

Raadpleeg voor meer informatie over deze verwijzende architectuur [uitbreiden Azure HDInsight met behulp van een Azure Virtual Network](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) documentatie.

### <a name="azure-api-management"></a>Azure API management

Deze verwijzende architectuur beveiligt de openbaar eindpunt van de [API management](../api-management/api-management-key-concepts.md) resource publiceren van API's voor klanten die buiten de organisatie. APIM moeten worden geïmplementeerd in een extern virtueel netwerk DDoS-beveiliging in te schakelen.

![](media/azure-ddos-best-practices/image15.png)

Door het externe virtuele netwerk te configureren, API Management gateway & developer portal toegankelijk zijn vanaf het openbare internet via een openbare load balancer. In deze architectuur is DDoS-bescherming standaard ingeschakeld op het virtuele netwerk APIM extern virtueel netwerk. Verkeer wordt doorgestuurd via internet naar het openbare IP-adres van de APIM dat is beveiligd tegen netwerkaanvallen Layer 3/Layer4. Om te beschermen tegen aanvallen van laag 7 HTTP/HTTPs, kunt u een toepassingsgateway configureren in de modus WAF.

Lijst met extra services die zijn geïmplementeerd in een virtueel netwerk en kan worden geconfigureerd voor DDoS-bescherming standaard wordt bijgehouden [hier](../virtual-network/virtual-network-for-azure-services.md). DDoS-bescherming Standard biedt alleen ondersteuning voor Azure Resource Manager-resources. *Geïnjecteerde as-omgeving (toepassing Service omgeving)-implementatie in een VNET met een openbare IP-adres wordt niet standaard ondersteund.* Dit gedeelte vindt u meer informatie over het beveiligen van de omgeving as-omgeving.

## <a name="next-steps"></a>Volgende stappen

* [Azure DDoS-bescherming-productpagina](https://azure.microsoft.com/services/ddos-protection/)

* [Azure DDoS-bescherming-Blog](http://aka.ms/ddosblog)

* [Documentatie voor Azure DDoS-bescherming ](../virtual-network/ddos-protection-overview.md)
