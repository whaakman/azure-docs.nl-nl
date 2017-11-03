---
title: Het configureren van een App Service omgeving v1
description: Configuratie, beheer en controle van de App Service-omgeving v1
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: b5a1da49-4cab-460d-b5d2-edd086ec32f4
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 34fb3f15c03a3d3ef5f0a27081539bf0a6d19c5f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-an-app-service-environment-v1"></a>Configureren van een App Service-omgeving v1

> [!NOTE]
> In dit artikel gaat over de v1 App Service-omgeving.  Er is een nieuwere versie van de App Service-omgeving die eenvoudiger te gebruiken en wordt uitgevoerd op krachtiger infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot de App-serviceomgeving](intro.md).
> 

## <a name="overview"></a>Overzicht
Op hoog niveau, wordt een Azure App Service-omgeving bestaat uit diverse belangrijke onderdelen:

* De rekenresources die worden uitgevoerd in App Service-omgeving gehoste service
* Storage
* Een database
* Een virtueel netwerk Classic(V1) of Resource Manager(V2) Azure (VNet) 
* Een subnet met de App Service-omgeving gehoste service wordt uitgevoerd in het

### <a name="compute-resources"></a>Bronnen berekenen
U gebruikt de rekenresources voor de vier resourcegroepen.  Elke as-omgeving (App Service omgeving) heeft een set van front-ends en drie mogelijke werknemersgroepen. U hoeft niet te gebruiken alle drie werknemersgroepen--als u wilt, kunt u alleen gebruiken een of twee.

De hosts in de resourcegroepen (front-ends en werknemers) zijn niet rechtstreeks toegankelijk is voor tenants. Remote Desktop Protocol (RDP) gebruiken voor verbinding mee te maken, wijzigen de inrichtingen, of is niet fungeren als een beheerder op deze.

U kunt resource pool hoeveelheid en grootte instellen. In een as-omgeving hebt u vier grootteopties, die zijn gelabeld P1 via P4. Zie voor meer informatie over deze grootten en de prijscategorie [App Service-prijzen](https://azure.microsoft.com/pricing/details/app-service/).
De hoeveelheid of de grootte wijzigen wordt een schaalaanpassing genoemd.  Slechts één schaalaanpassing kan worden uitgevoerd op een tijdstip.

**FrontPage-ends**: de front-ends zijn de HTTP/HTTPS-eindpunten voor de apps die zijn ondergebracht in uw as-omgeving. U kunt werkbelastingen niet uitvoeren in de front-ends.

* Een as-omgeving begint met twee P2s die voldoende is voor ontwikkel-/ testwerkbelastingen en op laag niveau productie-workloads. Wij raden P3s voor normaal te zwaar productieworkloads.
* Normaal tot zware productieworkloads, wordt u aangeraden dat u ten minste vier P3s hebt om te controleren of er zijn onvoldoende front-ends gepland onderhoud plaatsvindt. Gepland onderhoudsactiviteiten verschijnt omlaag één front-end tegelijk. Hierdoor worden de algemene beschikbare front-capaciteit tijdens onderhoudsactiviteiten.
* Front-ends kunnen een uur duren om in te richten. 
* Voor verdere schaal aan te passen, moet u het CPU-percentage, geheugenpercentage en actieve aanvragen metrische gegevens voor de front-toepassingen bewaken. Als het percentage CPU of geheugen dan 70 procent wanneer P3s wordt uitgevoerd, voegt u meer front-ends toe. Als het gemiddelde van de waarde van de actieve aanvragen uit te 15.000 op 20.000 aanvragen per front-end, moet u ook meer front-ends toevoegen. De algemene doelstelling is het houden van CPU en geheugen percentages onderstaande 70% en actieve aanvragen gemiddelde naar hieronder 15.000 aanvragen per front end wanneer u P3s uitvoert.  

**Werknemers**: de werknemers zijn waar uw apps daadwerkelijk worden uitgevoerd. Wanneer u uw App Service-abonnementen opschalen, die gebruikmaakt van werknemers in de bijbehorende worker-groep.

* U kunt geen werknemers onmiddellijk toevoegen. Ze kunnen een uur duren om in te richten.
* De grootte van een berekeningsresource voor elke groep schalen duurt < 1 uur per updatedomein. Er zijn 20 update domeinen in een as-omgeving. Als u de schaal van de compute-grootte van een werknemersgroep met 10 exemplaren hebt aangepast, kan het tot 10 uur duren.
* Als u de grootte van de rekenresources die worden gebruikt in een werknemersgroep wijzigt, wordt u koude startprocedures van de apps die worden uitgevoerd in die worker-groep.

Er is de snelste manier om de grootte van de compute resource van een werknemersgroep waarop alle apps niet wijzigen naar:

* Het aantal werknemers 2 terugschroeven.  De minimale schaal omlaag grootte in de portal is 2. Het duurt enkele minuten uw exemplaren ongedaan gemaakt. 
* Selecteer de nieuwe compute-grootte en het aantal exemplaren. Hier kunt duurt het tot 2 uur.

Als uw apps een groter formaat van de compute-resource vereist, kan niet u profiteren van de vorige instructies. U kunt in plaats van het wijzigen van de grootte van de worker-groep die fungeert als voor deze apps host vullen van een andere worker-groep met werknemers van de gewenste grootte en uw apps overstappen naar die groep.

* De extra exemplaren van de grootte van de benodigde berekeningen in een andere worker-groep maken. Hiermee wordt een uur duren om te voltooien.
* Toewijzen van uw App Service-abonnementen die als host van de apps die een groter formaat naar de meest recent geconfigureerde worker-groep nodig. Dit is een snelle bewerking moet minder dan een minuut in beslag nemen.  
* De eerste worker-groep terugschroeven als u niet meer nodig hebt die ongebruikte exemplaren. Deze bewerking duurt een paar minuten.

**Automatisch schalen**: een van de hulpprogramma's die u helpen kunnen bij het beheren van uw verbruik compute automatisch wordt geschaald. U kunt automatisch schalen voor front-end- of worker-groepen. U kunt doen zoals verhoging van uw exemplaren van elk type groep in de ochtend en verminderen het 's avonds. Of misschien kunt u instanties toevoegen wanneer het aantal werknemers die beschikbaar in een werknemersgroep zijn zakt onder een bepaalde drempelwaarde komt.

Als u instellen van regels voor automatisch schalen rond compute resource pool metrische gegevens wilt, houd rekening met de tijd die inrichten, zijn vereist. Zie voor meer informatie over automatisch schalen App Service-omgevingen [configureren voor automatisch schalen in een App-serviceomgeving][ASEAutoscale].

### <a name="storage"></a>Storage
Elke as-omgeving is geconfigureerd met 500 GB aan opslagruimte. Deze ruimte wordt gebruikt voor de apps in de as-omgeving. Deze opslagruimte deel uitmaakt van de as-omgeving en kan momenteel niet worden omgezet voor het gebruik van uw opslagruimte. Als u aanpassingen aan uw virtuele netwerkroutering of beveiliging aanbrengen, moet u nog steeds toegang tot Azure Storage--toestaan of de as-omgeving, kan niet werken.

### <a name="database"></a>Database
De database bevat de informatie die definieert de omgeving, evenals de details over de apps die erin worden uitgevoerd. Dit is een deel van het abonnement voor Azure ondergebracht. Het is niet iets dat u hebt een directe mogelijkheid om te bewerken. Als u aanpassingen aan uw virtuele netwerkroutering of beveiliging aanbrengen, moet u nog steeds toegang tot SQL Azure--toestaan of de as-omgeving, kan niet werken.

### <a name="network"></a>Netwerk
Het VNet dat wordt gebruikt met uw as-omgeving kan worden één die u hebt aangebracht tijdens het maken van de as-omgeving of die u hebt aangebracht tevoren. Wanneer u het subnet tijdens het maken van de as-omgeving maakt, wordt deze gedwongen de as-omgeving zich in dezelfde resourcegroep bevinden als het virtuele netwerk. Als u de resourcegroep die wordt gebruikt door uw as-omgeving anders dan die van uw VNet, moet u uw as-omgeving met een resource manager-sjabloon maken.

Er zijn enkele beperkingen op het virtuele netwerk dat wordt gebruikt voor een as-omgeving:

* Het virtuele netwerk moet een regionaal virtueel netwerk.
* Er moet een subnet met 8 of meer adressen waarbij het as-omgeving is geïmplementeerd.
* Nadat een subnet wordt gebruikt voor het hosten van een as-omgeving, kan niet het adresbereik van het subnet worden gewijzigd. Daarom is het raadzaam het subnet bevat ten minste 64 adressen zodat een eventuele toekomstige groei van de as-omgeving.
* Er is niets anders in het subnet, maar de as-omgeving.

In tegenstelling tot de gehoste service die de as-omgeving, bevat de [virtueel netwerk] [ virtualnetwork] en subnet onder het Gebruikersbeheer zijn.  U kunt uw virtuele netwerk via het virtuele netwerk-gebruikersinterface of PowerShell beheren.  Een as-omgeving kan worden geïmplementeerd in een klassiek of Resource Manager VNet.  De portal en API-ervaringen enigszins verschillen tussen het klassieke en het Resource Manager VNets zijn, maar de ervaring van de as-omgeving is hetzelfde.

Het VNet dat wordt gebruikt voor het hosten van een as-omgeving kunt beide particuliere RFC1918 IP-adressen of openbare IP-adressen kunnen worden gebruikt.  Als u wilt gebruiken van een IP-adresbereik dat niet wordt gedekt door RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) moet u uw VNet en het subnet moet worden gebruikt door uw as-omgeving voor het maken van de as-omgeving maken.

Omdat deze mogelijkheid wordt de Azure App Service in het virtuele netwerk geplaatst, betekent dit dat de apps die worden gehost in uw as-omgeving hebt nu toegang tot resources die rechtstreeks via ExpressRoute of site-naar-site virtuele particuliere netwerken (VPN's) beschikbaar worden gesteld. De apps die binnen uw App Service-omgeving nodig geen extra functies voor netwerken voor toegang tot resources die beschikbaar zijn voor het virtuele netwerk die als host voor uw App Service-omgeving fungeert. Dit betekent dat u hoeft niet te met VNET-integratie of hybride verbindingen toegang krijgen tot bronnen of niet verbonden met het virtuele netwerk. U kunt beide van deze functies nog steeds gebruiken hoewel voor toegang tot bronnen in netwerken die niet zijn verbonden met het virtuele netwerk.

Bijvoorbeeld, kunt u VNET-integratie te integreren met een virtueel netwerk dat zich in uw abonnement, maar niet is gekoppeld aan het virtuele netwerk dat u uw as-omgeving in. U kunt nog steeds ook hybride verbindingen gebruiken voor toegang tot bronnen die zich in andere netwerken, net zoals u normaal gesproken kunt.  

Als u het virtuele netwerk dat is geconfigureerd met een VPN ExpressRoute hebt, moet u zich bewust zijn van een deel van de routering behoeften met een as-omgeving. Er zijn enkele gebruiker gedefinieerde route (UDR)-configuraties die incompatibel met een as-omgeving zijn. Zie voor meer informatie over het uitvoeren van een as-omgeving in een virtueel netwerk met ExpressRoute [met een App Service-omgeving in een virtueel netwerk met ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Binnenkomend verkeer beveiligen
Er zijn twee primaire methoden voor het beheren van binnenkomend verkeer op de as-omgeving.  U kunt Network Security Groups(NSGs) gebruiken om te bepalen welk IP-adressen heeft toegang tot uw as-omgeving zoals hier wordt beschreven [binnenkomend verkeer in een App-serviceomgeving beheren](app-service-app-service-environment-control-inbound-traffic.md) en u kunt ook uw as-omgeving configureren met een interne Load Balancer(ILB).  Deze functies kunnen ook samen worden gebruikt als u wilt beperken van toegang met nsg's aan uw as ILB-omgeving.

Wanneer u een as-omgeving maakt, maakt deze een VIP-adres in uw VNet.  Er zijn twee VIP typen externe en interne.  Wanneer u een as-omgeving met een externe VIP maakt vervolgens zijn uw apps in uw as-omgeving toegankelijk via internet routeerbare IP-adres. Wanneer u uw as-omgeving intern selecteert, wordt geconfigureerd met een ILB en worden niet rechtstreeks toegankelijk internet.  Een as ILB-omgeving een externe VIP nog steeds vereist, maar wordt alleen gebruikt voor toegang tot Azure beheer en onderhoud.  

Tijdens het maken van de as-ILB omgeving het subdomein dat wordt gebruikt door de as ILB-omgeving te bieden en hebben voor het beheren van uw eigen DNS voor het subdomein dat u opgeeft.  Omdat u de naam van het subdomein u ook moet voor het beheren van het certificaat dat wordt gebruikt voor HTTPS-toegang instellen.  Na het maken van de as-omgeving wordt u gevraagd het certificaat op te geven.  Lees voor meer informatie over het maken en gebruiken van een as-omgeving voor de ILB [met behulp van een interne Load Balancer met een App-serviceomgeving][ILBASE]. 

## <a name="portal"></a>Portal
U kunt beheren en controleren van uw App Service-omgeving met behulp van de gebruikersinterface in de Azure portal. Als u een as-omgeving hebt, klikt u vervolgens bent u waarschijnlijk het symbool App Service op de zijbalk. Dit symbool wordt gebruikt voor het App Service-omgevingen vertegenwoordigen in de Azure-portal:

![Symbool van App Service-omgeving][1]

U opent de gebruikersinterface met een lijst met alle van uw App Service-omgevingen, u kunt het pictogram gebruiken of Selecteer de dubbele punthaak (' > ' symbool) aan de onderkant van de zijbalk App Service-omgevingen te selecteren. Als u een van de ASEs vermeld, opent u de gebruikersinterface die wordt gebruikt om te controleren en beheren.

![Gebruikersinterface voor het controleren en beheren van uw App Service-omgeving][2]

Deze eerste blade ziet u enkele eigenschappen van de as-omgeving, samen met een metrische grafiek per resourcegroep. Sommige eigenschappen die worden weergegeven in de **Essentials** blok ook hyperlinks zijn die is gekoppeld aan deze blade wordt geopend. Bijvoorbeeld, kunt u de **virtueel netwerk** naam om de gebruikersinterface te openen die zijn gekoppeld aan het virtuele netwerk dat u uw as-omgeving wordt uitgevoerd in. **App Service-plannen** en **Apps** elke open bladen die een lijst van deze items die in uw as-omgeving.  

### <a name="monitoring"></a>Bewaking
De grafieken kunnen u verschillende maatstaven voor prestaties in elke resourcegroep. Voor de front-groep, kunt u het gemiddelde CPU en geheugen kunt bewaken. Werknemersgroepen, kunt u de hoeveelheid die wordt gebruikt en de hoeveelheid die beschikbaar is bewaken.

Meerdere App Service plan kunnen maken gebruik van de werknemers in een werknemersgroep. De werkbelasting is niet gedistribueerd op dezelfde manier als met de front-end-servers, zodat de CPU- en geheugengebruik niet bieden veel heeft op het gebied nuttige informatie. Het is belangrijker om bij te houden hoeveel werknemers die u hebt gebruikt en zijn beschikbaar--met name als u dit systeem voor andere gebruikers beheert.  

U kunt ook alle van de metrische gegevens die kunnen worden bijgehouden in de grafieken gebruiken voor het instellen van waarschuwingen. Instellen van waarschuwingen hier werkt op dezelfde manier als elders in App Service. U kunt een waarschuwing instellen vanuit de **waarschuwingen** UI-onderdeel of van dieper ingegaan op parameters gebruikersinterface en het selecteren van **waarschuwing toevoegen**.

![Metrische gegevens gebruikersinterface][3]

De metrische gegevens die alleen zijn besproken, zijn de metrische gegevens van App Service-omgeving. Er zijn ook metrische gegevens die beschikbaar op het niveau van de App Service-plan zijn. Dit is waar controle van CPU en geheugen maakt een groot aantal zin.

Alle van de App Service-abonnementen zijn speciale App Service-abonnementen in een as-omgeving. Dit betekent dat alleen apps die worden uitgevoerd op de hosts toegewezen aan dat de App Service-abonnement de apps in App Service plan zijn. Voor informatie over uw App Service-abonnement, online zetten van uw App Service-abonnement vanuit een van de lijsten in de gebruikersinterface van de as-omgeving of van **bladeren App Service-plannen** (die een lijst met alle mappen).   

### <a name="settings"></a>Instellingen
Binnen de blade as-omgeving bevindt zich een **instellingen** sectie met verschillende belangrijke mogelijkheden:

**Instellingen** > **eigenschappen**: de **instellingen** blade automatisch wordt geopend wanneer u online van de blade van het as-omgeving zetten. Bovenaan staat **eigenschappen**. Er zijn een aantal items hier redundante wilt weergeven op **Essentials**, maar wat is zeer nuttig is **virtueel IP-adres**, evenals **uitgaande IP-adressen**.

![Instellingenblade en eigenschappen][4]

**Instellingen** > **IP-adressen**: wanneer u een IP-Secure Sockets Layer (SSL)-app in uw as-omgeving maken, moet u een SSL IP-adres. Om een ophalen, moet uw as-omgeving SSL IP-adressen waarvan deze eigenaar is die kunnen worden toegewezen. Wanneer een as-omgeving is gemaakt, heeft één SSL IP-adres voor dit doel, maar u kunt meer toevoegen. Er is een kosten voor extra IP SSL adressen, zoals wordt weergegeven in [App Service-prijzen] [ AppServicePricing] (in de sectie op SSL-verbindingen). De aanvullende prijs is de prijs IP SSL.

**Instellingen** > **Front-End-Pool** / **werknemersgroepen**: elk van deze groep resourceblades biedt de mogelijkheid om informatie te bekijken alleen op deze bronnengroep naast het bieden van besturingselementen om te schalen volledig die resourcegroep.  

De base blade voor elke resourcegroep wordt een grafiek met metrische gegevens voor deze resourcegroep. Net als met de grafieken van de blade as-omgeving, kunt u gaat u in de grafiek en waarschuwingen naar wens instellen. Een waarschuwing instellen op de blade as-omgeving voor een specifieke resourcegroep doet hetzelfde als het om dit op de resourcegroep. Uit de worker-groep **instellingen** blade hebt u toegang tot alle Apps of App Service-abonnementen die worden uitgevoerd in deze worker-groep.

![Groepsinstellingen worker gebruikersinterface][5]

### <a name="portal-scale-capabilities"></a>Mogelijkheden van de portal schaal
Er zijn drie schaalbewerkingen:

* Het wijzigen van het aantal IP-adressen in de as-omgeving die beschikbaar voor gebruik van IP SSL zijn.
* Het wijzigen van de grootte van de compute-resource die wordt gebruikt in een resourcegroep.
* Het wijzigen van het aantal rekenresources die worden gebruikt in een resourcegroep, hetzij handmatig, hetzij via automatisch schalen.

Er zijn drie manieren om te bepalen hoeveel servers die u in uw resourcegroepen hebt in de portal:

* Een schaalbewerking van de hoofdblade van de as-omgeving aan de bovenkant. U kunt meerdere scale configuratiewijzigingen aanbrengen op de front- en werkrollen opslaggroepen. Ze worden toegepast als een enkele bewerking.
* Een handmatige schaalaanpassing van de afzonderlijke resourcegroep **Scale** blade die zich onder **instellingen**.
* Automatisch schalen die u tijdens het instellen van de afzonderlijke resourcegroep **Scale** blade.

Sleep de schuifregelaar op de hoeveelheid en sla voor het gebruik van de scale-bewerking op de blade as-omgeving. Deze gebruikersinterface ondersteunt ook de grootte wijzigen.  

![Schaal gebruikersinterface][6]

Als u de mogelijkheden handmatig of automatisch schalen in een specifieke resourcegroep, gaat u naar **instellingen** > **Front-End-Pool** / **werknemersgroepen** zo nodig. Open vervolgens de groep die u wilt wijzigen. Ga naar **instellingen** > **uitschalen** of **instellingen** > **opschalen**. De **uitschalen** blade kunt u de hoeveelheid exemplaar. **Omhoog schalen** kunt u de resourcegrootte van de.  

![Schaalinstellingen gebruikersinterface][7]

## <a name="fault-tolerance-considerations"></a>Overwegingen voor fouttolerantie
U kunt een App Service-omgeving voor het gebruik van maximaal 55 totale rekenresources configureren. Deze 55 rekenresources kan alleen 50 worden gebruikt om werkbelastingen te hosten. De reden hiervoor is tweeledig. Er is minimaal 2 front-rekenresources.  Die laat tot 53 ter ondersteuning van de toewijzing worker-groep. Om fouttolerantie te doen, moet u een extra Reken-bron die is toegewezen op basis van de volgende regels:

* Elke worker-groep moet ten minste 1 extra berekeningsresource die is niet beschikbaar voor een werkbelasting worden toegewezen.
* Wanneer het aantal rekenresources in een werknemersgroep boven een bepaalde waarde gaat, klikt u vervolgens is een andere compute resource vereist voor fouttolerantie. Dit is niet het geval in de front-groep.

Binnen een enkele werknemersgroep zijn de vereisten voor fouttolerantie die voor een bepaalde waarde van X resources toegewezen aan een worker-groep:

* Als X tussen 2 en 20, is de hoeveelheid bruikbare rekenresources die u voor werkbelastingen gebruiken kunt X-1.
* Als X tussen 21 en 40, is de hoeveelheid bruikbare rekenresources die u voor werkbelastingen gebruiken kunt X-2.
* Als X tussen 41 en 53, is de hoeveelheid bruikbare rekenresources die u voor werkbelastingen gebruiken kunt X-3.

De minimale voetafdruk heeft 2-front-endservers en twee 2 werkers beschikken.  Met de bovenstaande instructies vervolgens volgen hier enkele voorbeelden om te verduidelijken:  

* Als u 30 werknemers in een van de toepassingen hebt, kan 28 ze worden gebruikt om werkbelastingen te hosten.
* Als u twee 2 werkers beschikken in een van de toepassingen hebt, kan 1 worden gebruikt om werkbelastingen te hosten.
* Als u 20 werknemers in een van de toepassingen hebt, kan 19 worden gebruikt om werkbelastingen te hosten.  
* Hebt u 21 werknemers in een één groep, kan vervolgens nog enige 19 worden gebruikt om werkbelastingen te hosten.  

Het aspect fouttolerantie is belangrijk, maar moet u er rekening mee houden terwijl u boven bepaalde drempels schaalt. Als u wilt meer capaciteit van 20 exemplaren toe te voegen, ga dan naar 22 of hoger omdat 21 eventuele meer capaciteit niet toevoegen. Hetzelfde geldt gaat boven 40, waarbij het volgende nummer dat wordt toegevoegd capaciteit 42 is.  

## <a name="deleting-an-app-service-environment"></a>Verwijderen van een App Service-omgeving
Als u verwijderen van een App Service-omgeving wilt, gewoon gebruikt u de **verwijderen** actie aan de bovenkant van de blade App Service-omgeving. Als u dit doet, wordt u gevraagd om in te voeren van de naam van uw App Service-omgeving wilt bevestigen dat u echt om dit te doen. Houd er rekening mee dat wanneer u een App Service-omgeving hebt verwijderd, u alle van de inhoud van deze ook verwijderen.  

![Verwijderen van een App Service-omgeving gebruikersinterface][9]  

## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgevingen, Zie [het maken van een App-serviceomgeving](app-service-web-how-to-create-an-app-service-environment.md).

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[Appserviceplans]: ../azure-web-sites-web-hosting-plans-in-depth-overview.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
