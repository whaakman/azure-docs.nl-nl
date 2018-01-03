---
title: Een app integreren met een Azure-netwerk
description: Ziet u hoe u een app in Azure App Service verbinden met een nieuwe of bestaande virtuele Azure-netwerk
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: ccompy
ms.openlocfilehash: b755197af7e8791e01273bcc25f72c0d92ef6bc2
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uw app integreren met een Azure-netwerk
Dit document beschrijft de integratiefunctie van Azure App Service virtueel netwerk en ziet u hoe u met apps in instelt [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Als u niet bekend bent met Azure Virtual Networks (vnet's), is dit een functie waarmee u veel van uw Azure-resources in een internet-routeable netwerk dat u toegang tot te plaatsen. Deze netwerken kunnen vervolgens worden verbonden met uw on-premises netwerken met een aantal VPN-technologieën. Voor meer informatie over Azure Virtual Networks, beginnen met de informatie hier: [Azure Virtual Network-overzicht][VNETOverview]. 

De Azure App Service heeft twee vormen. 

1. De multitenant-systemen die ondersteuning bieden voor de volledige reeks prijsstelling
2. De as-omgeving (App Service omgeving) premium-functie, die wordt geïmplementeerd in uw VNet. 

Dit document doorloopt VNet-integratie en geen App Service-omgeving. Als u meer informatie over de functie as-omgeving wilt, begint u met de informatie hier: [App Service-omgeving inleiding][ASEintro].

Integratie met VNet geeft uw web-app-toegang tot bronnen in het virtuele netwerk, maar heeft geen persoonlijke toegang verlenen aan uw web-app van het virtuele netwerk. Toegang tot persoonlijke site verwijst naar uw app alleen toegankelijk vanaf een particulier netwerk zoals van binnen een virtuele Azure-netwerk maken. Toegang tot persoonlijke site is alleen beschikbaar als er een as-omgeving met een interne Load Balancer (ILB) geconfigureerd. Voor meer informatie over het gebruik van een as ILB-omgeving beginnen met het artikel hier: [maken en gebruiken van een as-omgeving voor de ILB][ILBASE]. 

Een veelvoorkomend scenario waar u de VNet-integratie wilt gebruiken is het inschakelen van uw web-app toegang heeft tot een database of een webservice die wordt uitgevoerd op een virtuele machine in uw Azure virtual network. Met VNet-integratie hoeft u niet te kunnen stellen van een openbaar eindpunt voor toepassingen op uw virtuele machine, maar kan de persoonlijke niet internet routeerbare adressen in plaats daarvan gebruikt. 

De functie VNet integratie:

* vereist een standaard, Premium of geïsoleerd prijzen plannen 
* werkt met de klassieke weergave of Resource Manager VNet 
* biedt ondersteuning voor TCP en UDP
* werkt met Web, mobiel, API-apps en apps van de functie
* Hiermee kan een app tegelijk verbinding maken met slechts 1 VNet
* Hiermee kunt maximaal vijf VNets worden geïntegreerd met in een App Service-Plan 
* Hiermee kunt u hetzelfde VNet moet worden gebruikt door meerdere apps in een App Service-Plan
* ondersteunt een SLA met 99,9% vanwege de SLA op de VNet-Gateway

Er zijn een aantal zaken die VNet integratie biedt geen ondersteuning voor met inbegrip van:

* koppelen van een station
* AD-integratie 
* NetBios
* toegang tot persoonlijke site

### <a name="getting-started"></a>Aan de slag
Hier volgen een aantal zaken rekening moet houden voordat u uw web-app verbinden met een virtueel netwerk:

* VNet-integratie werkt alleen met apps in een **standaard**, **Premium**, of **geïsoleerd** plan prijzen. Als u de functie inschakelen en vervolgens uw App Service Plan een plan voor niet-ondersteunde prijscategorie uw apps de verbinding verbroken aan de VNets die ze gebruiken te schalen. 
* Als het virtuele doelnetwerk al bestaat, moet de punt-naar-site VPN is ingeschakeld met een gateway voor dynamische routering voordat deze kan worden verbonden met een app hebben. Als uw gateway is geconfigureerd met statische routering, kunt u de punt-naar-site virtueel particulier netwerk (VPN) niet inschakelen.
* Het VNet moet zich in hetzelfde abonnement als uw App Service-Plan(ASP). 
* De apps die zijn geïntegreerd met een VNet gebruiken de DNS-server die is opgegeven voor dit VNet.
* Standaard routeren uw integreren apps alleen verkeer in uw VNet op basis van de routes die zijn gedefinieerd in uw VNet. 

## <a name="enabling-vnet-integration"></a>VNet-integratie inschakelen

U hebt de optie voor uw app verbinden met een nieuw of bestaand virtueel netwerk. Als u een nieuw netwerk als onderdeel van uw integratie maakt, niet alleen alleen het VNet maken, een gateway voor dynamische routering is vooraf geconfigureerd voor u en wijst u Site-VPN is ingeschakeld. 

> [!NOTE]
> Configureren van een nieuwe virtuele netwerkintegratie kan enkele minuten duren. 
> 
> 

Integratie in te schakelen VNet, opent u de instellingen van uw app en selecteer vervolgens de netwerken. De gebruikersinterface die wordt geopend biedt drie netwerken opties. Deze handleiding zal in VNet integratie echter hybride verbindingen en App Service-omgevingen worden verderop in dit document besproken. 

Als uw app niet in de juiste plan prijzen is, wordt de gebruikersinterface kunt u uw abonnement op een hogere prijscategorie plan van uw keuze schalen.

![][1]

### <a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>VNet-integratie met een bestaande VNet inschakelen
De gebruikersinterface van de VNet-integratie kunt u selecteren uit een lijst met uw vnet's. De klassieke vnet's geven aan dat ze deze met het woord 'Klassiek' tussen haakjes naast de naam VNet. De lijst is gesorteerd dat de Resource Manager-VNets eerst worden vermeld. In de onderstaande afbeelding ziet u dat er slechts één VNet kan worden geselecteerd. Er zijn meerdere redenen dat een VNet worden lichter gekleurd weergegeven met inbegrip van kan:

* het VNet is in een ander abonnement dat uw account toegang tot heeft
* het VNet heeft geen punt aan de Site is ingeschakeld
* het VNet is geen gateway voor dynamische routering

![][2]

Om in te schakelen integratie klikt u gewoon op de VNet die u wilt integreren. Nadat u het VNet hebt geselecteerd, wordt uw app automatisch opnieuw opgestart om de wijzigingen van kracht te laten worden. 

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Om te kunnen Site in een klassiek VNet
Als uw VNet geen een gateway heeft noch punt naar Site heeft, hebt u die eerst worden ingesteld. Om dit te doen voor een klassiek VNet, gaat u naar de [Azure-portal] [ AzurePortal] en online zetten van de lijst met virtuele Networks(classic). Klik op het netwerk die u wilt integreren in en klik op het grote vak onder Essentials aangeroepen VPN-verbindingen vanaf deze locatie. Hier kunt kunt u uw punt voor het site-VPN en zelfs laten maken van een gateway maken. Nadat u het punt naar site met een gateway maken ervaring doorloopt is ongeveer 30 minuten voordat deze gereed is. 

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Wijs Site in een Resource Manager VNet inschakelen
Als u wilt een Resource Manager VNet met een gateway en verwijs naar de Site configureert, kunt u beide PowerShell zoals beschreven hier [configureren van een punt-naar-Site-verbinding met een virtueel netwerk met behulp van PowerShell] [ V2VNETP2S] of de Azure-portal zoals beschreven hier gebruiken [een punt-naar-Site-verbinding met een VNet met de Azure portal configureren][V2VNETPortal]. De gebruikersinterface voor het uitvoeren van deze mogelijkheid is nog niet beschikbaar. Houd er rekening mee dat u hoeft niet te maken van certificaten voor het punt aan Site-configuratie. Dit wordt automatisch geconfigureerd wanneer u uw Web-App met het VNet verbinden. 

### <a name="creating-a-pre-configured-vnet"></a>Maken van een vooraf geconfigureerde VNet
Als u wilt maken van een nieuw VNet dat is geconfigureerd met een gateway en punt-naar-Site, heeft de mogelijkheid om u te doen maar alleen voor een Resource Manager VNet met de netwerken van UI-App Service. Als u maken van een klassiek VNet met een gateway en punt-naar-Site wilt, moet u dit handmatig doen via de gebruikersinterface van netwerken. 

Voor het maken van een Resource Manager VNet via de gebruikersinterface van de integratie van VNet, selecteert u de **nieuw virtueel netwerk maken** en geef de:

* Virtuele-netwerknaam
* Virtueel netwerkadresblok
* Subnetnaam
* Subnetadresblok
* Gateway-Adresblok
* Punt-naar-site-adresblok

Als u dit VNet verbinding maken met andere netwerken wilt, moet u vervolgens niet verzamelen van IP-adresruimte met deze netwerken overlapt. 

> [!NOTE]
> Maken van een Resource Manager VNet met een gateway duurt ongeveer 30 minuten en momenteel is niet geïntegreerd met het VNet met uw app. Nadat uw VNet is gemaakt met de gateway die u wilt terugkeren naar uw VNet integratie van UI-app en selecteer uw nieuwe VNet.
> 
> 

![][3]

Azure VNets worden gewoonlijk gemaakt binnen een particulier netwerkadressen. Standaard die de VNet-integratiefunctie verkeer dat is bestemd voor deze IP routeert-adresbereiken in uw VNet. De persoonlijke IP-adresbereiken zijn:

* -Dit is hetzelfde als 10.0.0.0 - 10.0.0.0/8 10.255.255.255
* -Dit is hetzelfde als 172.16.0.0 - 172.16.0.0/12 172.31.255.255 
* -Dit is hetzelfde als 192.168.0.0 - 192.168.0.0/16 192.168.255.255

De VNet-adresruimte moet worden opgegeven in CIDR-notatie. Als u niet bekend met CIDR-notatie bent, is dit een methode voor het opgeven van adresblokken met een IP-adres en een geheel getal dat het netwerkmasker vertegenwoordigt. Als een snelle verwijzing rekening mee dat 10.1.0.0/24 zou 256 adressen en 10.1.0.0/25 128 adressen zou zijn. Een IPv4-adres met een /32 zou slechts 1 adres zijn. 

Als u de DNS-servergegevens hier instelt, wordt die is ingesteld voor uw VNet. U kunt deze informatie van de VNet-gebruikerservaringen bewerken na het maken van een VNet. Als u de DNS-server van de VNet wijzigt, moet u een synchronisatie-netwerk-bewerking uit te voeren.

Wanneer u een klassiek VNet via de gebruikersinterface van de integratie VNet maakt, wordt een VNet gemaakt in dezelfde resourcegroep als uw app. 

## <a name="how-the-system-works"></a>De werking van het systeem
Deze functie is gebaseerd onder de dekt boven op het punt-naar-Site VPN-technologie voor uw app verbinden met uw VNet. Apps in Azure App Service beschikken over een multitenant-systeemarchitectuur, die inrichten van een app rechtstreeks in een VNet verhindert, zoals met virtuele machines wordt uitgevoerd. Door voort te bouwen op punt-naar-site-technologie we netwerktoegang beperken tot alleen de virtuele machine die als host fungeert voor de app. Toegang tot het netwerk wordt verder beperkt op deze hosts app zodat uw apps alleen toegang tot de netwerken die u configureert ze toegang krijgen tot. 

![][4]

Als u nog een DNS-server geconfigureerd met het virtuele netwerk, moet uw app IP-adressen gebruiken om te bereiken van resource in het VNet. Tijdens het gebruik van IP-adressen, houd er rekening mee dat het grote voordeel van deze functie is dat Hiermee kunt u de particuliere adressen binnen uw particuliere netwerk gebruiken. Als u uw app instellen maximaal gebruik openbare IP-adressen voor een van uw virtuele machines en u de VNet-integratiefunctie niet gebruikt en communiceren via internet.

## <a name="managing-the-vnet-integrations"></a>Het beheren van de VNet-integraties
De mogelijkheid om te koppelen en Verbreek de verbinding met een vnet is op het niveau van een app. Bewerkingen die invloed hebben op de VNet-integratie voor meerdere apps zijn op een ASP-niveau. Door de gebruikersinterface die wordt weergegeven op het niveau van de app, krijgt u informatie op uw VNet. De meeste dezelfde informatie wordt ook weergegeven op het niveau van ASP. 

![][5]

U kunt zien als uw app is verbonden met uw VNet vanaf de pagina netwerkstatus van de functie. Als uw VNet-gateway is niet actief om welke reden is, zou dit wordt weergegeven als niet-verbonden. 

De informatie die u hebt nu beschikbaar zijn in de gebruikersinterface van niveau VNet-integratie is hetzelfde als de informatie die u van de ASP krijgt-app. Hier volgen die items:

* VNet - naam van deze koppeling opent het Azure-netwerk gebruikersinterface
* Locatie - dit duidt op de locatie van uw VNet. Het is mogelijk om te integreren met een VNet in een andere locatie.
* Certificaatstatus van de - zijn er certificaten worden gebruikt voor het beveiligen van de VPN-verbinding tussen de app en het VNet. Dit duidt op een test om te controleren of dat ze zijn gesynchroniseerd.
* Status van de gateway - uw gateways moet omlaag om welke reden vervolgens uw app geen toegang tot bronnen in het VNet. 
* VNet-adresruimte - dit is de IP-adresruimte voor uw VNet. 
* Wijs aan de adresruimte van de Site - dit is het punt om de site IP-adresruimte voor uw VNet. Uw app bevat communicatie afkomstig is van een van de IP-adressen in deze adresruimte. 
* Site met site-adresruimte - u kunt Site-naar-Site-VPN's gebruiken voor uw VNet verbinding maken met uw lokale bronnen of andere VNet. Hebt u die geconfigureerd en vervolgens de IP-adresbereiken die worden gedefinieerd met behulp van de VPN-verbinding wordt hier.
* DNS-Servers - als u DNS-Servers geconfigureerd met uw VNet hebt en vervolgens worden deze hier weergegeven.
* IP-adressen doorgestuurd naar het VNet - er zijn een lijst met IP-adressen die uw VNet is gedefinieerd voor Routering en hier deze adressen weergeven. 

De enige bewerking die u in de app-weergave van uw VNet-integratie uitvoeren kunt is verbinding met het VNet dat momenteel is verbonden met uw app. U doet dit Klik op verbinding verbreken aan de bovenkant. Deze actie wordt niet gewijzigd voor uw VNet. Het VNet en de configuratie van de gateways waaronder blijft ongewijzigd. Als u vervolgens verwijderen van uw VNet wilt, moet u de resources in met inbegrip van de gateways eerst verwijderen. 

De weergave App Service-Plan heeft een aantal aanvullende bewerkingen. Het is ook anders toegankelijk dan uit de app. Bereiken van open de gebruikersinterface ASP Networking gewoon uw ASP-UI en blader omlaag. Er is een UI-element netwerkstatus van de functie wordt aangeroepen. Dit biedt de details van sommige secundaire rond de integratie van uw VNet. Op deze gebruikersinterface te klikken, wordt de gebruikersinterface voor het netwerk functie Status geopend. Als u vervolgens op 'Klik hier om te beheren' klikt, wordt de gebruikersinterface met een lijst met de VNet-integraties in deze ASP geopend.

![][6]

De locatie van de ASP is klaar om te onthouden wanneer kijken naar de locaties van de VNets die u integreert. Wanneer het VNet op een andere locatie is bent u waarschijnlijk veel meer latentieproblemen tegenkomen. 

De VNets die zijn geïntegreerd met is een herinnering op hoeveel VNets dat uw apps in deze ASP en hoeveel er zijn geïntegreerd met. 

Voor extra informatie op elke VNet, klik op het VNet dat u geïnteresseerd bent in. Naast de details die eerder werden vermeld, kunt u ook een overzicht van de apps in deze ASP die van dit VNet gebruikmaken. 

Er zijn twee primaire acties met betrekking tot acties. De eerste is de mogelijkheid om toe te voegen routes die uw app in uw VNet uitgaand verkeer van station. De tweede actie is de mogelijkheid om te synchroniseren van certificaten en informatie over het netwerk.

![][7]

**Routering** zoals eerder aangegeven in de routes die zijn gedefinieerd in uw VNet zijn wat wordt gebruikt voor het routeren van verkeer naar uw VNet vanaf uw app. Er zijn enkele toepassingen al waar klanten willen aanvullende uitgaand verkeer te verzenden vanuit een app in het VNet en ze deze mogelijkheid is opgegeven. Wat gebeurt er met het verkeer nadat die tot hoe de klant Hiermee configureert u het VNet is. 

**Certificaten** de certificaatstatus duidt op een controle uitgevoerd door de App Service te valideren dat de certificaten die we voor de VPN-verbinding nog steeds goed zijn. Wanneer het VNet-integratie is ingeschakeld, klikt u vervolgens is als dit de eerste integratie aan dit VNet uit alle apps in deze ASP er een vereiste uitwisselen van certificaten om te controleren of de beveiliging van de verbinding. Samen met de certificaten krijgen we de DNS-configuratie, routes en andere vergelijkbare dingen die beschrijven van het netwerk.
Als deze certificaten of informatie over het netwerk is gewijzigd, moet u klikken op 'Sync netwerk'. **Opmerking**: wanneer u klikt op 'Sync netwerk' en zorgt u ervoor een korte uitval in de verbinding tussen uw app en uw VNet dat. Terwijl u uw app niet opnieuw wordt gestart, het verlies van verbinding kan ervoor zorgen dat uw site niet correct. 

## <a name="accessing-on-premises-resources"></a>Toegang tot lokale bronnen
Een van de voordelen van de VNet-integratiefunctie is als uw VNet is verbonden met uw on-premises netwerk met een Site naar Site VPN en uw apps toegang tot uw lokale bronnen van uw app hebben kunnen. Dit werkt, hoewel u wellicht uw on-premises VPN-gateway bijwerken met de routes voor de punt aan IP-adres Site liggen. Wanneer de Site-naar-Site VPN eerst instelt moeten de scripts die wordt gebruikt voor het configureren van het instellen van routes met inbegrip van uw beheerpunt op Site-VPN. Als u de punt aan Site-VPN-toevoegen nadat u uw Site naar Site VPN hebt gemaakt, moet u de routes handmatig bijwerken. Meer informatie over hoe u dat doen verschillen per gateway en worden hier niet beschreven. 

> [!NOTE]
> De functie VNet-integratie is niet geïntegreerd met een app met een VNet met een ExpressRoute-Gateway. Zelfs als de ExpressRoute-Gateway is geconfigureerd in [co-existentie modus] [ VPNERCoex] de VNet-integratie werkt niet. Als u wilt toegang krijgen tot bronnen via een ExpressRoute-verbinding, dan kunt u een [App Service-omgeving][ASE], die wordt uitgevoerd in uw VNet.
> 
> 

## <a name="pricing-details"></a>Prijsdetails
Er zijn slechts enkele nuances waarmee u houden moet rekening bij het gebruik van de VNet-integratiefunctie prijzen. Er zijn 3 gerelateerde kosten voor het gebruik van deze functie:

* ASP prijzen laag vereisten
* Kosten voor de gegevensoverdracht
* Kosten van de VPN-Gateway.

Voor uw apps kunnen deze functie wilt gebruiken, moeten ze zich in een Standard of Premium App Service-Plan. Op deze kosten hier kunt u meer informatie bekijken: [prijzen van App Service][ASPricing]. 

Vanwege de manier waarop punt aan Site-VPN's worden behandeld, altijd hebt u een kosten voor uitgaande gegevens via de integratie van de VNet-verbinding zelfs als het VNet zich in hetzelfde Datacenter. Als u wilt zien wat de kosten zijn, kijkt u hier: [gegevens overbrengen Pricing Details][DataPricing]. 

Het laatste item is de kosten van de VNet-gateways. Als u de gateways voor iets anders zoals Site naar Site VPN-verbindingen hoeft, klikt betaalt u voor gateways voor de ondersteuning van de VNet-integratiefunctie. Er zijn meer informatie op deze kosten hier: [prijzen voor VPN-Gateway][VNETPricing]. 

## <a name="troubleshooting"></a>Problemen oplossen
Wanneer de functie eenvoudig is te installeren, niet dat betekent dat dat uw ervaring probleem beschikbaar zal zijn. Moet u ondervindt zijn problemen bij toegang tot het gewenste eindpunt er een aantal hulpprogramma's die voor het testen van verbinding hebben met de app-console kunt u. Er zijn twee ervaringen van de console die kunt u. Een van de Kudu-console en de andere is de console die u in de Azure portal bereiken kan. Ga naar de Kudu-console van uw app naar Extra -> Kudu. Dit is hetzelfde als het gaat [sitename]. scm.azurewebsites.net. Zodra dat Hiermee opent u simpelweg gaat u naar het foutopsporingstabblad van de console. Als u naar de Azure portal gehoste-console en vervolgens vanuit uw app naar Extra -> Console. 

#### <a name="tools"></a>Hulpprogramma's
De hulpprogramma's voor ping, nslookup en tracert werkt via de console vanwege veiligheidsbeperkingen niet. Void er zijn twee afzonderlijke hulpprogramma's toegevoegd om te vullen. DNS-functionaliteit testen we een hulpprogramma met de naam nameresolver.exe toegevoegd. De syntaxis is:

    nameresolver.exe hostname [optional: DNS Server]

U kunt nameresolver gebruiken om te controleren van de hostnamen die afhankelijk zijn van uw app. Op deze manier kunt u als u iets verkeerd geconfigureerd met uw DNS- of mogelijk geen toegang tot uw DNS-server hebt testen.

De volgende hulpprogramma kunt u testen voor TCP-verbinding met een combinatie van de host en poort. Dit programma heet tcpping.exe en de syntaxis is:

    tcpping.exe hostname [optional: port]

Het hulpprogramma tcpping kunt u zien als u een specifieke host en poort kan bereiken. Alleen succes kan tonen als: Er is een toepassing die luistert op de host en poort combinatie en er is toegang tot het netwerk van uw app naar de opgegeven host en poort.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Toegang tot het VNet foutopsporing gehoste bronnen
Er zijn een aantal dingen die u kunt voorkomen dat uw app in een specifieke host en poort bereikt. De meeste gevallen is een van drie dingen:

* **Er is een firewall in de manier waarop** als er een firewall op de manier, bereikt u de TCP-time. Dat is 21 seconden in dit geval. Het hulpprogramma tcpping gebruiken om te controleren. TCP-outs kunnen worden vanwege een groot aantal dingen voorbij firewalls, maar er starten. 
* **DNS is niet toegankelijk** time-out voor de DNS-is drie seconden per DNS-server. Als u twee DNS-servers hebt, is de time-out 6 seconden. Gebruik nameresolver om te zien als DNS werkt. Houd er rekening mee dat u niet nslookup gebruiken zoals die maakt geen gebruik van uw VNet is geconfigureerd met DNS.
* **Ongeldige P2S-IP-adresbereik** het punt aan site IP-adresbereik moet zich in de RFC 1918 persoonlijke IP-adresbereiken (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Als het bereik IP-adressen buiten die gebruikt, vervolgens werkt dingen niet. 

Als deze items niet beantwoord uw probleem, er eerst voor de eenvoudige zaken: 

* De Gateway is wordt weergegeven als omhoog in de portal?
* Kunnen certificaten dat deze synchroon weergeven?
* Iemand de netwerkconfiguratie wijzigen zonder een 'Sync netwerk' in de betrokken ASP's? 

Als uw gateway actief is, brengt u het back-up. Als uw certificaten niet gesynchroniseerd zijn, gaat u naar de ASP-weergave van uw VNet-integratie en klik op 'Sync netwerk'. Als u vermoedt dat er een wijziging in uw VNet-configuratie is en het is geen synchronisatie zou met uw ASP vervolgens gaat u naar de ASP-weergave van uw VNet-integratie en druk op 'Sync netwerk' net zoals een herinnering, dit zorgt ervoor dat een korte stroomstoring met uw VNet-verbinding en uw apps. 

Als dat goed is, moet u om u te verdiepen iets:

* Zijn er andere VNet integratie met resources in hetzelfde VNet bereiken apps? 
* Kunt u gaat u naar de app-console en tcpping voor alle andere resources in uw VNet bereiken? 

Als een van de bovenstaande voldaan wordt, klikt u vervolgens de integratie van uw VNet functioneert en het probleem zich ergens anders. Dit is waar krijgt moet meer zijn van een uitdaging omdat er is geen eenvoudige manier om te zien waarom u een hostnaam: poort niet bereiken. Sommige van de oorzaken zijn:

* u hebt een firewall van op uw host verhinderen van toegang tot de poort van de toepassing vanaf het punt aan site IP-adresbereik. Vaak overschrijden van subnetten vereist openbare toegang.
* de doelhost is niet actief
* uw toepassing is niet beschikbaar
* u heeft het verkeerde IP- of de hostnaam
* uw toepassing luistert op een andere poort dan wat u verwacht. U kunt dit controleren door te gaan op die host en 'netstat - aon' met behulp van de opdrachtprompt. Hiermee geeft u welk proces ID luistert op poort. 
* de netwerkbeveiligingsgroepen zijn zodanig dat toegang tot uw toepassingshost en poort van uw Wijs IP-adresbereik van site geconfigureerd

Houd er rekening mee dat u welk IP-in uw punt niet naar Site IP-adresbereik die door uw app wordt gebruikt weet, dus moet u toegang toestaan via het hele bereik. 

Extra stappen omvatten:

* Meld u aan bij een andere virtuele machine in uw VNet en probeert te krijgen tot de resource-hostnaam: poort daar. Er zijn een aantal TCP-ping-hulpprogramma's kunt gebruiken voor dit doel uit te voeren of zelfs telnet kunt gebruiken als moeten worden. Het doel hier is zojuist om te bepalen of connectiviteit er van deze andere virtuele machine. 
* online zetten van een toepassing op een andere virtuele machine en test toegang op de betreffende host en poort van de console van uw app

#### <a name="on-premises-resources"></a>Lokale bronnen ####
Als uw app niet kan lokale bereiken, is het eerste wat dat u moet controleren als u een resource in uw VNet kan bereiken. Als dat werkt, probeert u de on-premises toepassing van een virtuele machine in het VNet te bereiken. U kunt telnet of een TCP-ping-hulpprogramma gebruiken. Als uw virtuele machine niet kan van uw lokale resource bereiken, Controleer of dat de Site naar Site VPN-verbinding werkt. Als u werkt, moet u dezelfde mogelijkheden die eerder hebt genoteerd en de on-premises gateway-configuratie en status te controleren. 

Nu zijn als uw VNet gehost VM toegang heeft tot uw on-premises systeem, maar uw app niet en vervolgens de reden waarschijnlijk een van de volgende opties is:

* de routes zijn niet geconfigureerd met uw Wijs IP-adresbereiken site in uw on-premises gateway
* de netwerkbeveiligingsgroepen blokkeren toegang voor uw punt naar Site IP-bereik
* de lokale firewalls blokkeren het verkeer van uw punt naar Site IP-bereik
* hebt u een gebruiker gedefinieerde Route(UDR) in uw VNet waarmee wordt voorkomen uw punt voor verkeer van de Site op basis dat van uw on-premises netwerk bereikt

## <a name="powershell-automation"></a>Automatisering met PowerShell

U kunt de App Service integreren met een Azure-netwerk met behulp van PowerShell. Zie voor een script kant-en-klaar [een app in Azure App Service verbinden met een Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).

## <a name="hybrid-connections-and-app-service-environments"></a>Hybride verbindingen en App Service-omgevingen
Er zijn drie functies waarmee u toegang tot bronnen van de VNet die worden gehost. Ze zijn:

* VNet-integratie
* Hybride verbindingen
* App Service-omgevingen

Hybride verbindingen, moet u voor het installeren van een relay-agent de hybride verbinding Manager(HCM) aangeroepen in uw netwerk. De HCM moet verbinding maken met Azure en uw toepassing. Deze oplossing is met name geweldige van een extern netwerk zoals uw on-premises netwerk of zelfs een andere cloud gehost netwerk omdat het geen een internet toegankelijk eindpunt vereist. De HCM kan alleen worden uitgevoerd op Windows en kunt u maximaal vijf exemplaren die worden uitgevoerd voor maximale beschikbaarheid hebben. Hybride verbindingen biedt alleen ondersteuning voor TCP via en elk CH-eindpunt moet overeenkomen met een combinatie van specifieke hostnaam: poort. 

De functie App Service-omgeving kunt u een exemplaar van de Azure App Service uitvoeren in uw VNet. Hiermee kunt uw apps toegang tot bronnen in uw VNet zonder eventuele extra stappen. Sommige van de andere voordelen van een App Service-omgeving zijn dat u Dv2 op basis van werknemers met maximaal 14 GB RAM-geheugen gebruiken kunt. Een ander voordeel is dat u het systeem om te voldoen aan uw behoeften kunt schalen. In tegenstelling tot de multitenant-omgevingen waarin de ASP beperkt tot 20 exemplaren is, in een as-omgeving kunt u maximaal 100 ASP exemplaren schalen. Een van de dingen die u met een as-omgeving die u niet met VNet-integratie is dat een App Service-omgeving met een ExpressRoute-VPN kunt werken. 

Hoewel er dat enkele case overlapping gebruiken, kunnen geen van deze functie een van de andere vervangen. Weten welke functie u wilt gebruiken, is gekoppeld aan uw behoeften. Bijvoorbeeld:

* Als u een ontwikkelaar bent en u wilt uitvoeren van een site in Azure en laat het toegang tot de database op het werkstation onder uw helpdesk, is het eenvoudigste wat u moet het gebruik van hybride verbindingen. 
* Als u een grote organisatie die een groot aantal wil Webeigenschappen in de openbare cloud en deze op uw eigen netwerk beheren en u wilt gaan met de App Service-omgeving. 
* Als u een aantal App Service hebt gehost apps en wilt gewoon toegang tot bronnen in uw VNet, vervolgens VNet-integratie is de manier om te gaan. 

Afgezien van de toepassingen, zijn sommige eenvoud gerelateerde aspecten. Als uw VNet al met uw on-premises netwerk verbonden is, is vervolgens met behulp van VNet-integratie of een App Service-omgeving een eenvoudige manier om de lokale bronnen gebruiken. Anderzijds, als uw VNet niet is verbonden met uw on-premises netwerk is veel meer overhead voor het instellen van een site naar site VPN met uw VNet vergeleken met de installatie van de HCM. 

Naast de functionele verschillen, er zijn ook prijzen verschillen. De functie App Service-omgeving is een Premium serviceaanbieding maar biedt het hoogste netwerk configuratiemogelijkheden naast andere handige functies. VNet-integratie met Standard of Premium ASP's kunnen worden gebruikt en is ideaal voor het veilig verbruik van bronnen in uw VNet vanaf de multitenant-App Service. Hybride verbindingen is momenteel afhankelijk van een account, wat heeft niveaus die gratis start en vervolgens geleidelijk duurdere op basis van het bedrag u moet BizTalk. Als het gaat om werkt echter veel netwerken, kan niet worden andere zoals hybride verbindingen, waarmee u toegang tot bronnen in afzonderlijke netwerken ook meer dan 100. 

<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
