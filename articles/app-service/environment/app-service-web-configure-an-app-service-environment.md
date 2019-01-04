---
title: Het configureren van een App Service Environment v1 - Azure
description: Configuratie, beheer en bewaking van de App Service Environment v1
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: b5a1da49-4cab-460d-b5d2-edd086ec32f4
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 5c0b4117f6e7b48dce1746ad6eb3dbe29c0d16af
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53723209"
---
# <a name="configuring-an-app-service-environment-v1"></a>Configureren van een App Service Environment v1

> [!NOTE]
> In dit artikel gaat over de App Service Environment v1.  Er is een nieuwere versie van de App Service Environment die gebruiksvriendelijker en wordt uitgevoerd op een krachtigere infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot App Service Environment](intro.md).
> 

## <a name="overview"></a>Overzicht
Op hoog niveau, wordt een Azure App Service-omgeving bestaat uit verschillende belangrijke onderdelen:

* COMPUTE-resources die worden uitgevoerd in de App Service Environment gehoste service
* Storage
* Een database
* Een virtueel netwerk Classic(V1) of Resource Manager(V2) Azure (VNet) 
* Een subnet met de App Service Environment gehoste-service wordt uitgevoerd op het

### <a name="compute-resources"></a>Rekenresources
U gebruikt de compute-resources voor uw vier resourcegroepen.  Elke App Service Environment (ASE) is een set van front-ends en drie mogelijke werknemersgroepen. U hoeft niet te gebruiken van alle drie werknemersgroepen--als u wilt, kunt u alleen gebruiken één of twee.

De hosts in de resourcegroepen (front-ends en werkrollen) zijn niet rechtstreeks toegankelijk is voor tenants. U kunt Remote Desktop Protocol (RDP) Maak er verbinding mee, wijzigt de inrichtingen gebruiken of fungeren als een beheerder zijn van deze.

U kunt resource pool hoeveelheid en grootte instellen. In een as-omgeving hebt u vier grootteopties, die zijn gelabeld P1 tot en met P4. Zie voor meer informatie over deze grootten en bijbehorende prijzen [App Service-prijzen](https://azure.microsoft.com/pricing/details/app-service/).
De hoeveelheid of het formaat wijzigen, heet een schaalbewerking.  Slechts één schaalbewerking kan worden uitgevoerd op een tijdstip.

**Front-ends**: De front-ends zijn de HTTP/HTTPS-eindpunten voor uw apps die zijn ondergebracht in de as-omgeving. U kunt werkbelastingen niet uitvoeren in de front-ends.

* Een as-omgeving begint met twee P2s, die is voldoende voor werkbelastingen voor ontwikkelen/testen en op laag niveau productieworkloads. Wij raden P3s voor normaal naar zware productieworkloads.
* Normaal tot zware productieworkloads, wordt u aangeraden dat u ten minste vier P3s hebt om te controleren of er zijn voldoende front-ends uitgevoerd als gepland onderhoud plaatsvindt. Geplande onderhoudsactiviteiten uitvallen een front-end in op een tijdstip. Dit vermindert het totale beschikbare front-end capaciteit tijdens onderhoudswerkzaamheden.
* Front-ends kunnen een uur duren om in te richten. 
* Voor verdere schaal te passen, moet u het CPU-percentage, geheugenpercentage en actieve aanvragen metrische gegevens voor de front-end-pool controleren. Als het percentage CPU of geheugen meer dan 70 procent bij het uitvoeren van P3s, Voeg meer front-ends. Als het gemiddelde van de waarde van de actieve aanvragen naar 15.000 tot 20.000 aanvragen per front-end, moet u ook meer front-ends toevoegen. De algemene doelstelling onderstaande percentages van CPU en geheugen houden 70% is en actieve aanvragen gemiddelde naar hieronder 15.000 aanvragen per front end wanneer u P3s uitvoert.  

**Werknemers**: De werknemers zijn waar uw apps daadwerkelijk worden uitgevoerd. Wanneer u uw App Service-plannen opschalen, die gebruikmaakt van werknemers in de groep met werkrollen gekoppeld.

* U kunt geen werknemers direct toevoegen. Ze kunnen een uur duren om in te richten.
* De grootte van een compute-resource voor een pool schalen duurt < 1 uur per updatedomein. Er zijn 20 updatedomeinen in een as-omgeving. Als u de compute-grootte van een groep met werkrollen met 10 exemplaren geschaald, kan het tot 10 uur duren.
* Als u de grootte van de compute-resources die worden gebruikt in een groep met werkrollen wijzigt, wordt u ervoor zorgen dat de koude start van de apps die worden uitgevoerd in die groep met werkrollen.

Er is de snelste manier om de grootte van de compute resource van een groep met werkrollen waarop alle apps niet wijzigen naar:

* Het aantal werknemers 2 verkleinen.  De minimale neerschalen grootte in de portal is 2. Het duurt een paar minuten toewijzing van uw instanties. 
* Selecteer de nieuwe compute-grootte en het aantal exemplaren. Hier duurt het maximaal twee uur om te voltooien.

Als uw apps een groter formaat van de compute-resource is vereist, kan niet u profiteren van de vorige richtlijnen. In plaats van de grootte van de groep met werkrollen die als host voor deze apps fungeert te wijzigen, kunt u een andere groep met werkrollen met werknemers van de gewenste grootte invullen en die uw apps overzetten naar die groep.

* De extra exemplaren van de grootte van de benodigde rekenkracht maken in een andere groep met werkrollen. Hiermee wordt een uur duren om uit te voeren.
* Uw App Service-plannen die als host van de apps die een groter formaat naar de meest recent geconfigureerde worker-groep moeten toewijzen. Dit is een snelle bewerking moet minder dan een minuut in beslag nemen.  
* De eerste groep met werkrollen verkleinen als u niet meer nodig hebt die niet-gebruikte instanties. Met deze bewerking duurt een paar minuten om te voltooien.

**Automatisch schalen**: Een van de hulpprogramma's die u helpen kunnen bij het beheren van het gebruik van de compute-resources is automatisch schalen. U kunt automatisch schalen voor front-end- of worker-groepen. U kunt dingen doen zoals toename van de exemplaren van elk type groep in de ochtend en deze in de avonduren is gepland te verminderen. Of misschien kunt u exemplaren toevoegen wanneer het aantal werknemers die beschikbaar in een groep met werkrollen zijn onder een bepaalde drempelwaarde komt.

Als u wilt instellen van de regels voor automatisch schalen om compute resource pool metrische gegevens, en houd rekening met de tijd die de inrichting is vereist. Zie voor meer informatie over automatisch schalen App Service-omgevingen, [configureren voor automatisch schalen in een App Service Environment][ASEAutoscale].

### <a name="storage"></a>Storage
Elke as-omgeving is geconfigureerd met 500 GB aan opslagruimte. Deze ruimte wordt gebruikt voor alle apps in de as-omgeving. Deze opslagruimte is een onderdeel van de as-omgeving en momenteel niet voor het gebruik van uw opslagruimte kan niet worden overgeschakeld. Als u wijzigingen aan uw virtuele netwerkroutering of veiligheid aanbrengt, moet u wel wilt toestaan dat toegang tot Azure Storage - of de as-omgeving kan niet worden gebruikt.

### <a name="database"></a>Database
De database bevat de informatie dat de omgeving, evenals de details over de apps die worden uitgevoerd binnen het definieert. Dit is ook een deel van het abonnement Azure bewaard. Het is niet iets dat u hebt een directe mogelijkheid om te bewerken. Als u wijzigingen aan uw virtuele netwerkroutering of veiligheid aanbrengt, moet u dat nog steeds toegang tot SQL Azure, of de as-omgeving kan niet worden gebruikt.

### <a name="network"></a>Netwerk
Het VNet dat wordt gebruikt met de as-omgeving kan één die u tijdens het maken van de as-omgeving hebt gemaakt of die vooraf zijn. Wanneer u het subnet tijdens het maken van as-omgeving maakt, zorgt deze ervoor dat de as-omgeving zich in dezelfde resourcegroep bevinden als het virtuele netwerk. Als u de resourcegroep die wordt gebruikt door de as-omgeving anders dan die van uw VNet, moet u uw ASE maken met een resource manager-sjabloon.

Er zijn enkele beperkingen voor het virtuele netwerk dat wordt gebruikt voor een as-omgeving:

* Het virtuele netwerk moet een regionaal virtueel netwerk.
* Er moet een subnet met 8 of meer adressen waar de as-omgeving is geïmplementeerd.
* Nadat een subnet wordt gebruikt voor het hosten van een as-omgeving, kan het adresbereik van het subnet kan niet worden gewijzigd. Daarom is het raadzaam dat het subnet bevat ten minste 64-mailadressen op voor een eventuele toekomstige groei van de as-omgeving.
* Er is niets anders in het subnet, maar de as-omgeving.

In tegenstelling tot de gehoste service die de as-omgeving, bevat de [virtueel netwerk] [ virtualnetwork] en subnet vallen onder controle van de gebruiker.  U kunt uw virtuele netwerk via het virtuele netwerk-gebruikersinterface of PowerShell beheren.  Een as-omgeving kan worden geïmplementeerd in een klassieke of Resource Manager VNet.  De portal en API-ervaringen verschillen enigszins tussen klassieke en Resource Manager VNets, maar de ervaring van de as-omgeving is hetzelfde.

Het VNet dat wordt gebruikt voor het hosten van een as-omgeving kunt beide persoonlijke RFC1918 IP-adressen of het openbare IP-adressen kunt gebruiken.  Als u wilt gebruiken van een IP-adresbereik dat niet wordt gedekt door RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) moet u uw VNet en subnet moet worden gebruikt door de as-omgeving voor het maken van ASE maken.

Omdat deze mogelijkheid wordt de Azure App Service in uw virtuele netwerk geplaatst, betekent dit dat uw apps die worden gehost in de as-omgeving hebt nu toegang tot resources die beschikbaar zijn via ExpressRoute of site-naar-site virtuele particuliere netwerken (VPN's) rechtstreeks. De apps die in uw App Service Environment zijn vereist geen aanvullende functies voor netwerkbeheer voor toegang tot resources die beschikbaar zijn voor het virtuele netwerk die als host voor uw App Service-omgeving fungeert. Dit betekent dat u hoeft niet aan het VNET-integratie of hybride verbindingen gebruiken om te krijgen tot bronnen of niet verbonden met het virtuele netwerk. U kunt beide van deze functies nog steeds gebruiken al voor toegang tot resources in netwerken die niet zijn verbonden met het virtuele netwerk.

Bijvoorbeeld, kunt u VNET-integratie om te integreren met een virtueel netwerk dat is in uw abonnement, maar is niet verbonden met het virtuele netwerk waarin de as-omgeving zich bevindt. U kunt nog steeds ook hybride verbindingen gebruiken voor toegang tot resources die zich in andere netwerken, net zoals u normaal gesproken kunt.  

Als u uw virtuele netwerk dat is geconfigureerd met een ExpressRoute-VPN hebt, moet u rekening houden met enkele van de routering behoeften met een as-omgeving. Er zijn enkele gebruiker gedefinieerde route (UDR)-configuraties die niet compatibel met een as-omgeving zijn. Zie voor meer informatie over het uitvoeren van een as-omgeving in een virtueel netwerk met ExpressRoute [die een App Service Environment worden uitgevoerd in een virtueel netwerk met ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Binnenkomend verkeer beveiligen
Er zijn twee primaire methoden voor het beheren van inkomend verkeer op de as-omgeving.  Kunt u Network Security Groups(NSGs) om te bepalen welke IP adressen toegang krijgen tot uw ASE zoals hier wordt beschreven [inkomend verkeer in een App Service Environment beheren](app-service-app-service-environment-control-inbound-traffic.md) en u kunt ook uw ASE met een interne Load Balancer configureren (ILB).  Deze functies kunnen ook samen worden gebruikt als u wilt toegang beperken met nsg's aan uw ILB as-omgeving.

Wanneer u een as-omgeving maakt, wordt een VIP-adres gemaakt in uw VNet.  Er zijn twee VIP-typen, externe en interne.  Wanneer u een as-omgeving met een extern VIP-adres maakt vervolgens is uw apps in de as-omgeving toegankelijk via internet routeerbare IP-adres. Wanneer u uw ASE intern selecteert, wordt geconfigureerd met een ILB en wordt niet rechtstreeks toegankelijk is via internet.  Een ILB as-omgeving nog steeds een extern VIP-adres is vereist, maar wordt alleen gebruikt voor toegang tot Azure beheer en onderhoud.  

Tijdens het maken van de ILB as-omgeving bieden het subdomein dat wordt gebruikt door de ILB as-omgeving en heeft voor het beheren van uw eigen DNS voor het subdomein dat u opgeeft.  Omdat u de naam van het subdomein u ook moet voor het beheren van het certificaat wordt gebruikt voor HTTPS-toegang instellen.  Nadat de as-omgeving is gemaakt wordt u gevraagd het certificaat op te geven.  Lees voor meer informatie over het maken en gebruiken van een ILB as-omgeving [met behulp van een interne Load Balancer met een App Service Environment][ILBASE]. 

## <a name="portal"></a>Portal
U kunt beheren en controleren van uw App Service-omgeving met behulp van de gebruikersinterface in Azure portal. Als u een as-omgeving hebt, klikt u vervolgens kunt u waarschijnlijk om te zien van het symbool voor App Service op de zijbalk. Dit symbool wordt gebruikt voor App Service-omgevingen in Azure portal:

![Symbool van App Service-omgeving][1]

Om te openen in de gebruikersinterface met een lijst met al uw App Service-omgevingen, kunt u het pictogram of de pijl-omlaag selecteren (' > ' symbool) aan de onderkant van de zijbalk Selecteer App Service-omgevingen. U selecteert een van de as-omgevingen die worden vermeld, opent u de gebruikersinterface die wordt gebruikt om te controleren en beheren.

![Gebruikersinterface voor het controleren en beheren van uw App Service Environment][2]

Deze eerste blade ziet u enkele eigenschappen van de as-omgeving, samen met een grafiek met metrische gegevens per resourcegroep. Enkele van de eigenschappen die worden weergegeven in de **Essentials** blok zijn ook hyperlinks die de die is gekoppeld aan deze blade wordt geopend. Bijvoorbeeld, kunt u de **Virtueelnetwerk** naam om de gebruikersinterface te openen die zijn gekoppeld aan het virtuele netwerk dat in de as-omgeving wordt uitgevoerd. **App Service-plannen** en **Apps** elke open blades met deze items in de as-omgeving.  

### <a name="monitoring"></a>Bewaking
De grafieken kunnen u tal van metrische gegevens voor prestaties in elke resourcegroep. Voor de front-end-groep, kunt u het gemiddelde CPU- en geheugengebruik bewaken. Controleer voor de worker-groepen, de hoeveelheid die wordt gebruikt en de hoeveelheid die beschikbaar is.

Meerdere App Service plannen kunnen maken gebruik van de werknemers in een groep met werkrollen. De werkbelasting is niet gedistribueerd op dezelfde manier als met de front-end-servers, zodat de CPU- en geheugengebruik niet bieden veel heeft op het gebied nuttige informatie. Het is belangrijker om bij te houden hoeveel werknemers die u hebt gebruikt en beschikbaar zijn met name als u dit systeem voor gebruik door anderen beheert.  

U kunt ook alle van de metrische gegevens die kunnen worden bijgehouden in de grafieken gebruiken voor het instellen van waarschuwingen. Instellen van waarschuwingen hier werkt op dezelfde manier als elders in App Service. U kunt een waarschuwing instellen vanuit de **waarschuwingen** UI-onderdeel of van analyseren in alle metrische gegevens over de gebruikersinterface en te selecteren **waarschuwing toevoegen**.

![Metrische gegevens over de gebruikersinterface][3]

De metrische gegevens die alleen zijn besproken, zijn de metrische gegevens van App Service-omgeving. Er zijn ook metrische gegevens die beschikbaar op het niveau van de App Service-plan zijn. Dit is waar bewaking van CPU en geheugen vormen een logische combinatie.

In een as-omgeving worden alle van de App Service-abonnementen toegewezen App Service-plannen. Dit betekent dat de enige apps die worden uitgevoerd op de hosts toegewezen aan de App Service-plan zijn de apps in het App Service-plan. Voor informatie over uw App Service-plan, Open uw App Service-plan uit een van de lijsten in de gebruikersinterface van de as-omgeving of van **bladeren App Service-plannen** (die een lijst met al deze).   

### <a name="settings"></a>Instellingen
In de blade as-omgeving, er is een **instellingen** sectie waarin verschillende belangrijke mogelijkheden:

**Instellingen voor** > **eigenschappen**: De **instellingen** blade wordt automatisch geopend als u uw ASE-blade. Aan de bovenkant is **eigenschappen**. Er zijn een aantal items in hier die redundante aan wat u ziet in **Essentials**, maar wat is zeer nuttig is **virtuele IP-adres**, evenals **uitgaande IP-adressen**.

![Instellingenblade en eigenschappen][4]

**Instellingen voor** > **IP-adressen**: Wanneer u een IP-Secure Sockets Layer (SSL)-app in de as-omgeving maakt, moet u een IP SSL-adres. Als u wilt aanvragen, moet uw ASE IP SSL-adressen die het eigendom zijn van die kunnen worden toegewezen. Wanneer een as-omgeving is gemaakt, wordt er één IP SSL-adres voor dit doel, maar u meer kunt toevoegen. Er is een post voor extra IP SSL-adressen, zoals wordt weergegeven in [App Service-prijzen] [ AppServicePricing] (in de sectie over SSL-verbindingen). De prijs voor extra is de prijs IP SSL.

**Instellingen voor** > **Front-Endpool** / **werknemersgroepen**: Elk van deze groep resourceblades biedt de mogelijkheid om informatie zien alleen in die resourcegroep, naast het leveren van besturingselementen om te schalen volledig die resourcegroep.  

De basis-blade voor elke resourcegroep bevat een grafiek met metrische gegevens voor deze resourcegroep. Net als met de grafieken op de blade as-omgeving u gaat u naar de grafiek en instellen van waarschuwingen naar wens. Een waarschuwing instellen op de blade as-omgeving voor een specifieke resourcegroep doet hetzelfde als het doen van de resourcegroep. In de groep met werkrollen **instellingen** blade, hebt u toegang tot alle Apps of App Service-abonnementen die worden uitgevoerd in deze groep met werkrollen.

![Instellingen van werknemer UI][5]

### <a name="portal-scale-capabilities"></a>Mogelijkheden voor portal
Er zijn drie schaalbewerkingen:

* Het wijzigen van het aantal IP-adressen in de as-omgeving die beschikbaar voor gebruik van IP SSL zijn.
* De grootte van de compute-resource die wordt gebruikt in een resourcegroep te wijzigen.
* Het wijzigen van het aantal rekenresources die worden gebruikt in een resourcegroep, hetzij handmatig, hetzij via automatisch schalen.

Er zijn drie manieren om te bepalen hoeveel servers die u in uw resourcegroepen hebt in de portal:

* Een schaalbewerking van de hoofdblade van de as-omgeving aan de bovenkant. U kunt meerdere schaal configuratiewijzigingen aanbrengen aan de front-end- en worker-groepen. Ze worden toegepast als één bewerking.
* Een bewerking handmatig schalen van de afzonderlijke resourcegroep **schaal** blade deze bevindt zich onder **instellingen**.
* Automatisch schalen die u tijdens het instellen van de afzonderlijke resourcegroep **schaal** blade.

Sleep de schuifregelaar in de hoeveelheid die u wilt gebruiken en opslaan voor het gebruik van de schaalbewerking op de blade van de as-omgeving. Deze gebruikersinterface biedt ook ondersteuning voor de grootte te wijzigen.  

![Schaal UI][6]

Voor het gebruik van de mogelijkheden handmatig of automatisch schalen in een specifieke resourcegroep, gaat u naar **instellingen** > **Front-Endgroep** / **werknemersgroepen** als van toepassing. Vervolgens opent u de groep die u wilt wijzigen. Ga naar **instellingen** > **uitschalen** of **instellingen** > **omhoog schalen**. De **uitschalen** blade kunt u voor het beheren van exemplaar hoeveelheid. **Omhoog schalen** kunt u voor het beheren van de resourcegrootte van de.  

![Instellingen van gebruikersinterface][7]

## <a name="fault-tolerance-considerations"></a>Overwegingen voor fouttolerantie
U kunt een App Service-omgeving voor het gebruik van maximaal 55 totale compute-resources configureren. Van die rekenresources 55 kan maar 50 worden gebruikt om werkbelastingen te hosten. De reden hiervoor is tweeledig. Er is een minimum van 2 front-compute-resources.  Die tot 53 ter ondersteuning van de toewijzing van de groep met werkrollen verlaat. Om te kunnen bieden fouttolerantie, moet u beschikken over een extra rekenresource zijn die wordt toegewezen op basis van de volgende regels:

* Elke workergroep moet ten minste 1 extra compute-resource die is niet beschikbaar voor een werkbelasting worden toegewezen.
* Wanneer de hoeveelheid rekenresources in een groep met werkrollen boven een bepaalde waarde heeft, klikt u vervolgens is een andere compute-resource vereist voor fouttolerantie. Dit is niet het geval is in de front-endpool.

Binnen een enkele werkgroep zijn de vereisten voor fouttolerantie die voor een bepaalde waarde van de X-resources toegewezen aan een groep met werkrollen:

* Als X tussen 2 en 20, is de hoeveelheid bruikbare compute-resources die u voor werkbelastingen gebruiken kunt X-1.
* Als X zich tussen 21 en 40, is de hoeveelheid bruikbare compute-resources die u voor werkbelastingen gebruiken kunt X-2.
* Als X tussen 41 en 53, is de hoeveelheid bruikbare compute-resources die u voor werkbelastingen gebruiken kunt X-3.

De minimale voetafdruk heeft 2-front-endservers en twee 2 werkers beschikken.  Met de bovenstaande instructies vervolgens volgen hier enkele voorbeelden om te verduidelijken:  

* Als u 30 werknemers in één groep hebt, kan klikt u vervolgens 28 hiervan worden gebruikt om werkbelastingen te hosten.
* Als u twee 2 werkers beschikken in één groep hebt, kan 1 worden gebruikt om werkbelastingen te hosten.
* Als u 20 werknemers in één groep hebt, kan klikt u vervolgens 19 worden gebruikt om werkbelastingen te hosten.  
* Hebt u 21 werknemers in één groep, kan vervolgens nog enige 19 worden gebruikt om werkbelastingen te hosten.  

Het aspect fouttolerantie is belangrijk, maar u moet in waarmee u rekening moet houden wanneer u boven bepaalde drempelwaarden valt schaalt. Als u meer capaciteit van 20 instanties toevoegen wilt, klikt u vervolgens gaat u naar 22 of hoger omdat 21 niet alle meer capaciteit toegevoegd. Hetzelfde geldt gaan boven de 40, waarbij het volgende nummer dat wordt toegevoegd capaciteit 42 is.  

## <a name="deleting-an-app-service-environment"></a>Als u een App Service-omgeving
Als u verwijderen van een App Service Environment wilt, gewoon gebruikt u de **verwijderen** actie aan de bovenkant van de blade App Service-omgeving. Als u dit doet, wordt u gevraagd om in te voeren van de naam van uw App Service-omgeving te bevestigen dat u echt wilt om dit te doen. Houd er rekening mee dat wanneer u een App Service-omgeving verwijdert, u alle van de inhoud van deze ook verwijderen.  

![Een App Service-omgeving gebruikersinterface verwijderen][9]  

## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service-omgevingen, Zie [over het maken van een App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).

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
[Appserviceplans]: ../overview-hosting-plans.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
