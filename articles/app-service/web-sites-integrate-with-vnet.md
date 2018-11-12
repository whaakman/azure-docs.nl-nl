---
title: Een app integreren met een Azure-netwerk
description: Ziet u hoe u een app in Azure App Service verbinden met een nieuwe of bestaande Azure-netwerk
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: ccompy
ms.openlocfilehash: 5e7a2150757afeecb6b0b100f3259f1628f42074
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262077"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uw app integreren met een Azure-netwerk
Dit document beschrijft de functie voor integratie met virtueel netwerk Azure App Service en laat zien hoe u om in te stellen met apps in [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). Als u niet bekend bent met Azure-netwerken (VNets), is dit een mogelijkheid waarmee u veel van uw Azure-resources in een niet-internet routeerbare netwerk dat u toegang tot plaatsen. Deze netwerken kunnen vervolgens worden verbonden met uw on-premises netwerken met behulp van verschillende VPN-technologieën. Voor meer informatie over Azure Virtual Networks, beginnen met de informatie hier: [Azure Virtual Network Overview][VNETOverview]. 

De Azure App Service heeft twee vormen. 

1. De multitenant-systemen die ondersteuning bieden voor het volledige bereik van prijzen en abonnementen
2. De App Service Environment (ASE) premium-functie, die wordt geïmplementeerd in uw VNet. 

Dit document gaat via VNet-integratie en geen App Service-omgeving. Als u meer informatie over de functie van de as-omgeving wilt, begint u met de informatie hier: [Inleiding in App Service-omgeving][ASEintro].

VNet-integratie kunt uw web-app-toegang tot resources in uw virtuele netwerk, maar heeft geen persoonlijke toegang verlenen aan uw web-app vanuit het virtuele netwerk. Toegang tot de persoonlijke site verwijst naar het maken van uw app alleen toegankelijk is vanaf een particulier netwerk zoals uit binnen een virtueel Azure-netwerk. Toegang tot de persoonlijke site is alleen beschikbaar met een as-omgeving geconfigureerd met een interne Load Balancer (ILB). Beginnen met het artikel hier voor meer informatie over het gebruik van een ILB as-omgeving: [maken en gebruiken van een ILB as-omgeving][ILBASE]. 

Een veelvoorkomend scenario waar zou u VNet-integratie is het inschakelen van uw web-app toegang heeft tot een database of een webservice die wordt uitgevoerd op een virtuele machine in uw Azure-netwerk. Met VNet-integratie hoeft u geen een openbaar eindpunt beschikbaar maken voor toepassingen op uw virtuele machine maar kan de particuliere niet internet routeerbare adressen in plaats daarvan gebruikt. 

De functie van de VNet-integratie:

* een Standard, Premium of geïsoleerde prijs-abonnement vereist 
* werkt met het klassieke of Resource Manager VNet 
* biedt ondersteuning voor TCP en UDP
* werkt met Web, mobiel, API apps, en functie-apps
* Hiermee kunt een app te verbinden met slechts 1 VNet tegelijk
* Hiermee kunt maximaal vijf VNets kunnen worden geïntegreerd met in een App Service-Plan 
* Hiermee kunt u hetzelfde VNet moet worden gebruikt door meerdere apps in een App Service-Plan
* biedt ondersteuning voor een SLA van 99,9% vanwege de SLA voor de VNet-Gateway

Er zijn enkele dingen die VNet-integratie biedt geen ondersteuning voor het met inbegrip van:

* een schijf koppelen
* AD-integratie 
* NetBios
* toegang tot de persoonlijke site

### <a name="getting-started"></a>Aan de slag
Hier volgen een aantal zaken waarmee u rekening moet houden voordat u uw web-app verbinden met een virtueel netwerk:

* VNet-integratie werkt alleen met apps in een **Standard**, **Premium**, of **geïsoleerd** prijsplan. Als u de functie inschakelen en schaal uw App Service-Plan naar een niet-ondersteunde prijsschema uw apps verloren zijn verbindingen naar de vnet's die ze gebruiken. 
* Als uw virtuele doelnetwerk al bestaat, moet de punt-naar-site VPN is ingeschakeld met een gateway voor dynamische routering voordat deze kan worden verbonden met een app hebben. Als uw gateway is geconfigureerd met statische routering, kunt u de punt-naar-site virtueel particulier netwerk (VPN) niet inschakelen.
* Het VNet moet zich in hetzelfde abonnement als uw App Service-Plan(ASP).
* Als uw gateway zich al met punt bevindt-naar-site ingeschakeld en deze zich niet in de basis-SKU, moet u IKEV2 uitgeschakeld in uw punt-naar-site-configuratie.
* De apps die zijn geïntegreerd met een VNet gebruiken de DNS-server die is opgegeven voor dit VNet.
* Standaard routeren uw geïntegreerde apps alleen van verkeer in uw VNet op basis van de routes die zijn gedefinieerd in uw VNet. 

## <a name="enabling-vnet-integration"></a>VNet-integratie inschakelen

U hebt de mogelijkheid uw app kunt koppelen aan een nieuwe of bestaande virtuele netwerk. Als u een nieuw netwerk als onderdeel van uw integratie maakt, naast het maken van het VNet NET, een gateway voor dynamische routering is vooraf geconfigureerd voor u en punt-naar-Site VPN is ingeschakeld. 

> [!NOTE]
> Configureren van een nieuwe integratie van virtuele netwerken kan enkele minuten duren. 
> 
> 

Om in te schakelen VNet-integratie, opent u uw app-instellingen en selecteer Networking. De gebruikersinterface die wordt geopend biedt drie opties voor netwerken. Deze handleiding zal alleen in VNet-integratie via hybride verbindingen en App Service-omgevingen worden verderop in dit document besproken. 

Als uw app zich niet in de juiste prijscategorie plan, wordt de gebruikersinterface kunt u voor het schalen van uw abonnement naar een hogere prijscategorie plan van uw keuze.

![][1]

### <a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>VNet-integratie met een bestaande VNet inschakelen
De gebruikersinterface van de VNet-integratie kunt u selecteren uit een lijst met uw vnet's. De klassieke VNets aangeven dat ze deze met het woord 'Klassieke' tussen haakjes naast de naam van het VNet zijn. De lijst is gesorteerd dat de Resource Manager VNets worden eerst weergegeven. In de afbeelding hieronder wordt weergegeven, ziet u dat slechts één VNet kan worden geselecteerd. Er zijn meerdere redenen die een VNet worden lichter gekleurd weergegeven met inbegrip van kan:

* het VNet is in een ander abonnement dat uw account toegang tot heeft
* het VNet heeft geen punt-naar-Site ingeschakeld
* het VNet heeft geen een gateway voor dynamische routering

![][2]

Integratie in te schakelen, klikt u op de VNet die u integreren wilt in. Nadat u het VNet hebt geselecteerd, wordt uw app automatisch opnieuw gestart om de wijzigingen worden doorgevoerd. 

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Schakel punt-naar-Site in een klassiek VNet

Als uw VNet, heeft geen een gateway en ook geen punt-naar-Site, hebt u het instellen daarvan eerst. Ga naar de portal en Open de lijst met virtuele Networks(classic) voor meer informatie over het configureren van een gateway voor een klassiek VNet. Selecteer het netwerk die u wilt integreren. Selecteer de VPN-verbindingen. Hier kunt kunt u uw punt voor het site-VPN en zelfs laten maken van een gateway maken. De gateway duurt ongeveer 30 minuten actief.

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Inschakelen van punt-naar-Site in een Resource Manager VNet
Als u wilt een Resource Manager VNet configureren met een gateway en een punt-naar-Site, kunt u een van beide PowerShell zoals beschreven hier [configureren van een punt-naar-Site-verbinding met een virtueel netwerk met behulp van PowerShell] [ V2VNETP2S] of de Azure-portal zoals beschreven hier gebruiken [een punt-naar-Site-verbinding met een VNet maken via de Azure-portal configureren][V2VNETPortal]. De gebruikersinterface om uit te voeren van deze mogelijkheid is nog niet beschikbaar. U hoeft te maken van certificaten voor de punt-naar-Site-configuratie. Certificaten worden automatisch gemaakt wanneer u uw Web-App verbinden met het VNet maken via de portal. 

### <a name="creating-a-pre-configured-vnet"></a>Het maken van een vooraf geconfigureerde VNet
Als u wilt om een nieuw VNet dat is geconfigureerd met een gateway en punt-naar-Site te maken, heeft de App-Service toegang UI de mogelijkheid om dat te doen, maar alleen voor een Resource Manager VNet. Als u een klassiek VNet maken met een gateway en een punt-naar-Site wilt, moet u dit handmatig doen via de gebruikersinterface van netwerken. 

Selecteer voor het maken van een Resource Manager VNet via de gebruikersinterface van VNet-integratie, **nieuw Virtueelnetwerk maken** en geef de:

* Virtuele-netwerknaam
* Virtueel netwerkadresblok
* Subnetnaam
* Subnetadresblok
* Gateway-Adresblok
* Punt-naar-site-adresblok

Als u dit VNet verbinding maken met andere netwerken wilt, moet u vervolgens niet verzamelen van IP-adresruimte die met deze netwerken overlapt. 

> [!NOTE]
> Resource Manager VNet maken met een gateway duurt ongeveer 30 minuten en momenteel is niet geïntegreerd met het VNet met uw app. Nadat u uw VNet is gemaakt met de gateway, moet u keert u terug naar uw app in de gebruikersinterface van de VNet-integratie en selecteert u uw nieuwe VNet.
> 
> 

![][3]

Azure VNets worden normaal gesproken gemaakt binnen een particulier netwerk-adressen. Standaard die de VNet-integratiefunctie verkeer dat bestemd is voor deze IP routeert-adresbereiken in uw VNet. De privé IP-adresbereiken zijn:

* -Dit is hetzelfde als 10.0.0.0 - 10.0.0.0/8 10.255.255.255
* -Dit is hetzelfde als 172.16.0.0 - 172.16.0.0/12 172.31.255.255 
* -Dit is hetzelfde als 192.168.0.0 - 192.168.0.0/16 192.168.255.255

De VNet-adresruimte moet worden opgegeven in CIDR-notatie. Als u niet bekend met CIDR-notatie bent, is een methode voor het opgeven van-adresblokken met een IP-adres en een geheel getal dat het netwerkmasker vertegenwoordigt. Als naslag, houd rekening met dat 10.1.0.0/24 256-adressen en 10.1.0.0/25 128 adressen zou zijn. Een IPv4-adres met een/32 adverteren zou slechts 1-adres zijn. 

Als u hier de informatie over DNS-server ingesteld, wordt die is ingesteld voor uw VNet. U kunt deze informatie van de VNet-gebruikerservaringen bewerken na het maken van VNet. Als u de DNS-server van het VNet wijzigt, moet u een netwerk synchroniseren-bewerking uit te voeren.

Wanneer u een klassiek VNet met behulp van de gebruikersinterface van de VNet-integratie maakt, wordt een VNet gemaakt in dezelfde resourcegroep bevinden als uw app. 

## <a name="how-the-system-works"></a>De werking van het systeem
Op de achtergrond wordt deze functie voortbouwt op punt-naar-Site VPN-technologie waarmee uw app met uw VNet. Apps in Azure App Service hebben een systeem met meerdere tenants architectuur, uitsluit van een app rechtstreeks in een VNet inrichten zoals met virtuele machines wordt uitgevoerd. We beperken netwerktoegang tot alleen de virtuele machine die als host fungeert de app wordt doorgebouwd op de technologie voor punt-naar-site. Toegang tot het netwerk wordt verder beperkt op de app-hosts, zodat uw apps alleen toegang de netwerken die u deze tot configureert voor toegang tot. 

![][4]

Als u een DNS-server nog niet hebt geconfigureerd met het virtuele netwerk, moet uw app IP-adressen gebruiken om te bereiken van bron in het VNet. Tijdens het gebruik van IP-adressen, houd er rekening mee dat het grote voordeel van deze functie is dat Hiermee kunt u de particuliere adressen binnen uw particuliere netwerk gebruiken. Als u uw app tot gebruik openbare IP-voor een van uw virtuele machines, adressen u de VNet-integratie-functie niet gebruikt en communiceren via internet.

## <a name="managing-the-vnet-integrations"></a>Beheren van de VNet-integraties
De mogelijkheid om verbinding te verbreken met een VNet is op het niveau van een app. Bewerkingen die invloed hebben op de VNet-integratie voor meerdere apps zijn op het niveau van een ASP. Vanuit de gebruikersinterface die wordt weergegeven op het niveau van de app, krijgt u meer informatie over uw VNet. Het merendeel van de dezelfde informatie wordt ook weergegeven op het niveau van ASP. 

![][5]

In de pagina Status van de functie Network kunt u zien als uw app is verbonden met uw VNet. Als uw VNet-gateway niet actief om enige reden is, zou dit wordt weergegeven als niet-verbonden. 

De informatie die u hebt nu beschikbaar in de gebruikersinterface van niveau VNet-integratie is hetzelfde als de informatie die u van de ASP krijgt-app. Dit zijn de items:

* Naam van de VNet - deze koppeling wordt het Azure-netwerk gebruikersinterface geopend
* Locatie: dit geeft de locatie van uw VNet. Het is mogelijk om te integreren met een VNet in een andere locatie.
* Status van de certificaat - er zijn certificaten die worden gebruikt voor het beveiligen van de VPN-verbinding tussen de app en het VNet. Dit komt overeen met een test om te controleren of dat ze zijn gesynchroniseerd.
* Status van de gateway - moeten uw gateways niet beschikbaar zijn om enige reden vervolgens uw app geen toegang tot resources in het VNet. 
* VNet-adresruimte - dit is de IP-adresruimte voor uw VNet. 
* Punt-naar-Site-adresruimte - dit is het punt op de site-IP-adresruimte voor uw VNet. Uw app wordt communicatie als die afkomstig zijn van een van de IP-adressen in deze adresruimte. 
* Site naar site-adresruimte - kunt u Site-naar-Site VPN-verbindingen naar uw VNet verbinding maken met uw on-premises bronnen of andere VNet. Hebt u die geconfigureerd en vervolgens de IP-bereiken die worden gedefinieerd met behulp van de VPN-verbinding wordt hier.
* DNS-Servers - als u DNS-Servers geconfigureerd met uw VNet hebt en ze worden hier weergegeven.
* IP-adressen doorgestuurd naar het VNet - er zijn een lijst met IP-adressen die uw VNet is gedefinieerd voor Routering en hier deze adressen weergeven. 

De enige bewerking die u in de app-weergave van uw VNet-integratie uitvoeren kunt is uw app ontkoppelt van het momenteel is verbonden met VNet. Als u wilt verbreken uw app van een VNet, verbreekt u aan de bovenkant. Deze actie wordt uw VNet niet gewijzigd. Het VNet en de configuratie met inbegrip van de gateways blijft ongewijzigd. Als u vervolgens verwijderen van uw VNet wilt, moet u de resources in met inbegrip van de gateways eerst verwijderen. 

De weergave App Service-Plan heeft een aantal aanvullende bewerkingen. Het is ook anders geopend dan vanuit de app. Als u wilt de ASP-netwerken gebruikersinterface is bereikt, open uw ASP-gebruikersinterface en schuif omlaag. Selecteer 'Status netwerkfunctie' te openen van de gebruikersinterface voor het netwerk functie Status. Selecteer 'Klik hier als u wilt beheren' aan de VNet-integratie in deze ASP-lijst.

![][6]

De locatie van de ASP is goed om te onthouden wanneer de locaties van de vnet's die u met integreert kijken. Wanneer het VNet op een andere locatie is kunt u waarschijnlijk veel meer om te zien van latentieproblemen. 

De VNets die zijn geïntegreerd met is een herinnering op het aantal vnet's die uw apps worden geïntegreerd met deze ASP en hoeveel u kunt hebben. 

Voor extra informatie op elk VNet, klikt u op het VNet waarin u geïnteresseerd bent. Naast de details die eerder zijn vastgesteld, kunt u ook een lijst van de apps in deze ASP die van dat VNet gebruikmaken zien. 

Er zijn twee primaire acties met betrekking tot acties. De eerste is de mogelijkheid om toe te voegen routes die uw app in uw VNet uitgaand verkeer van station. De tweede actie is de mogelijkheid om te synchroniseren van certificaten en netwerkgegevens.

![][7]

**Routering** , zoals eerder vermeld de routes die zijn gedefinieerd in uw VNet zijn wat wordt gebruikt voor het routeren van verkeer in uw VNet vanaf uw app. Er zijn enkele gebruikstoepassingen al waar klanten wilt verzenden aanvullende uitgaand verkeer van een app in het VNet en voor hen deze mogelijkheid wordt geboden. Wat gebeurt er met het verkeer nadat die is aan hoe de klant Hiermee configureert u het VNet. 

**Certificaten** Status van het certificaat is inclusief een controle wordt uitgevoerd door de App Service om te valideren dat de certificaten die we voor de VPN-verbinding nog steeds prima geschikt zijn. Als de VNet-integratie is ingeschakeld, klikt u vervolgens is als dit de eerste integratie aan dit VNet van alle apps in deze ASP, er een vereiste uitwisselen van certificaten om te controleren of de beveiliging van de verbinding. Samen met de certificaten krijgen we de DNS-configuratie, routes en andere vergelijkbare dingen die worden beschreven van het netwerk.
Als deze certificaten of netwerkgegevens is gewijzigd, moet u klikken op 'Sync netwerk'. **Houd er rekening mee**: wanneer u klikt op 'Sync netwerk' en u een korte stroomstoring in de verbinding tussen uw app en uw VNet veroorzaken. Terwijl uw app niet opnieuw wordt opgestart, het verlies van connectiviteit kan ervoor zorgen dat uw site niet correct. 

## <a name="accessing-on-premises-resources"></a>Toegang tot on-premises bronnen
Een van de voordelen van de functie voor VNet-integratie is dat als uw VNet is verbonden met uw on-premises netwerk met een Site-naar-Site-VPN en uw apps toegang tot uw on-premises resources van uw app hebben kunnen. Bereik voor deze optie om te werken, maar u moet mogelijk uw on-premises VPN-gateway bijwerken met de routes voor uw IP-adres voor de punt-naar-Site. Wanneer de Site-naar-Site VPN is eerst ingesteld moeten de scripts die wordt gebruikt voor het configureren van het instellen van routes met inbegrip van de punt-naar-Site-VPN. Als u de punt-naar-Site VPN-verbinding toevoegen nadat u uw Site-naar-Site-VPN hebt gemaakt, moet u de routes handmatig bijwerken. Informatie over hoe u dat doen variëren per gateway en worden hier niet beschreven. 

> [!NOTE]
> De VNet-integratie-functie is niet geïntegreerd voor een app met een VNet met een ExpressRoute-Gateway. Zelfs als de ExpressRoute-Gateway is geconfigureerd [co-existentie modus] [ VPNERCoex] de VNet-integratie werkt niet. Als u toegang tot resources via een ExpressRoute-verbinding wilt maken, dan kunt u een [App Service-omgeving][ASE], die wordt uitgevoerd in uw VNet.
> 
> 

## <a name="pricing-details"></a>Prijsdetails
Er zijn een aantal aspecten waarmee u houden moet rekening bij het gebruik van de VNet-integratie-functie prijzen. Er zijn 3 gerelateerde kosten voor het gebruik van deze functie:

* ASP voorschriften laag-prijzen
* Kosten voor de gegevensoverdracht
* Kosten van de VPN-Gateway.

Voor uw apps kunnen deze functie wilt gebruiken, moeten ze zich in een Standard of Premium App Service-Plan. U kunt meer details weergeven over deze kosten hier: [prijzen voor App Service][ASPricing]. 

Vanwege de manier waarop de punt-naar-Site VPN-verbindingen worden afgehandeld, hebt u altijd een kosten in rekening gebracht voor uitgaande gegevens via de verbinding van uw VNet-integratie, zelfs als het VNet in hetzelfde Datacenter. Als u wilt zien wat deze kosten zijn, kijkt u hier: [Data Transfer prijsinformatie over][DataPricing]. 

Het laatste item zijn de kosten van de VNet-gateways. Als u niet de gateways voor iets anders, zoals Site-naar-Site VPN-verbindingen nodig hebt, klikt u vervolgens betaalt u voor gateways voor de ondersteuning van de VNet-integratie-functie. Er vindt u informatie over deze kosten hier: [prijzen voor VPN-Gateway][VNETPricing]. 

## <a name="troubleshooting"></a>Problemen oplossen
Wanneer de functie eenvoudig om in te stellen is, betekent dat niet dat uw ervaring probleem gratis zijn. Moet u ondervindt zijn problemen met het openen van uw gewenste eindpunt er enkele hulpprogramma's die u gebruiken kunt voor het testen van de connectiviteit van de app-console. Er zijn twee console ervaringen die u kunt gebruiken. Een van de Kudu-console is en de andere is de console die u in Azure portal kunt bereiken. Ga naar Extra -> om naar de Kudu-console vanuit uw app Kudu. Dit is hetzelfde als het gaat [sitename]. scm.azurewebsites.net. Zodra dat wordt geopend, gaat u naar het foutopsporingstabblad van de console. Ga naar hulpprogramma's om op te halen voor de Azure portal gehoste-console en vervolgens vanuit uw app-Console >. 

#### <a name="tools"></a>Hulpprogramma's
De hulpprogramma's **ping**, **nslookup** en **tracert** via de console vanwege veiligheidsbeperkingen werkt niet. De void er zijn twee verschillende hulpprogramma's toegevoegd om op te vullen. Als u wilt testen van de DNS-functionaliteit hebben we een hulpprogramma met de naam nameresolver.exe toegevoegd. De syntaxis is:

    nameresolver.exe hostname [optional: DNS Server]

U kunt **nameresolver** om te controleren of de hostnamen die afhankelijk van uw app. Op deze manier kunt u als u niets verkeerd geconfigureerd met uw DNS of mogelijk geen toegang tot de DNS-server hebt testen.

De volgende hulpprogramma kunt u testen voor TCP-verbinding met een combinatie van de host en poort. Dit hulpprogramma wordt aangeroepen **tcpping** en de syntaxis is:

    tcpping.exe hostname [optional: port]

De **tcpping** hulpprogramma geeft u aan als u een specifieke host en poort kunt bereiken. Alleen succes kan weergegeven als: Er is een toepassing die luistert op de combinatie van de host en poort en er is toegang tot het netwerk van uw app naar de opgegeven host en poort.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Toegang tot de VNet-foutopsporing gehoste bronnen
Er zijn een aantal dingen die u kunt voorkomen dat uw app in een specifieke host en poort bereikt. De meeste gevallen is het een van drie dingen:

* **Er is een firewall op de manier** hebt u een firewall op de manier, bereikt u de TCP-time. Dat is 21 seconden in dit geval. Gebruik de **tcpping** hulpprogramma connectiviteit testen. TCP-time-outs kan vanwege een groot aantal dingen voorbij firewalls maar het begin beginnen. 
* **DNS is niet toegankelijk** time-out voor de DNS-is drie seconden per DNS-server. Als u twee DNS-servers hebt, is de time-out 6 seconden. Gebruik nameresolver om te zien of DNS werkt. Vergeet niet dat je niet nslookup gebruiken als die maakt geen gebruik van de uw VNet is geconfigureerd met DNS-server.
* **Ongeldige P2S-IP-adresbereik** het punt aan site IP-adresbereik moet zich in de RFC 1918 privé IP-adresbereiken (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Als het bereik IP-adressen worden gebruikt buiten die, klikt u vervolgens werkt dingen niet. 

Als deze items geen antwoord op uw probleem, zoek eerst de eenvoudige zaken, zoals: 

* Wordt de Gateway weergegeven alsof ze van de portal?
* Certificaten worden weergegeven alsof ze synchroon?
* Iedereen de netwerkconfiguratie wijzigen zonder een 'Sync netwerk' in de betrokken ASP's? 

Als uw gateway niet actief is, brengt u deze back-up. Als uw certificaten niet gesynchroniseerd zijn, gaat u naar de ASP-weergave van uw VNet-integratie en druk op 'Sync netwerk'. Als u vermoedt dat er een wijziging aangebracht in uw VNet-configuratie is en het is niet synchroniseren net als bij uw ASP's, Ga naar de ASP-weergave van uw VNet-integratie en druk op 'Sync netwerk' net zoals een herinnering, omdat dit tot een korte onderbreking met uw VNet-verbinding en uw apps. 

Als dat is prima, moet u om u te verdiepen een en ander:

* Zijn er geen andere apps die met behulp van VNet-integratie tot resources in hetzelfde VNet? 
* Kunt u gaat u naar de app-console en tcpping gebruiken om te bereiken van andere bronnen in uw VNet? 

Als een van bovenstaande voldaan wordt, vervolgens uw VNet-integratie functioneert en het probleem zich ergens anders bevindt. Dit is waar het wordt moet meer zijn van een uitdaging omdat er geen eenvoudige manier om te zien waarom u een host: poort niet bereiken. Enkele van de oorzaken zijn:

* u hebt een firewall van op de host geen toegang tot poort van de toepassing vanaf het punt aan site IP-adresbereik. Vaak overschrijden van subnetten vereist openbare toegang.
* de doelhost is niet actief
* uw toepassing is niet actief
* u hebt het verkeerde IP of hostnaam
* uw toepassing luistert op een andere poort dan wat u verwacht. U kunt dit controleren door te gaan op die host en gebruik 'netstat - aon' vanaf de opdrachtprompt. Dit ziet u welk proces ID luistert op poort. 
* uw netwerkbeveiligingsgroepen zijn geconfigureerd in zodanig dat ze toegang tot uw toepassingshost en poort van de punt aan site IP-adresbereik voorkomen

Houd er rekening mee dat u niet welk IP in uw punt-naar-Site-IP-adresbereik dat door uw app wordt gebruikt weet, zodat u wilt toestaan dat toegang vanaf het volledige bereik. 

Extra stappen zijn onder andere:

* verbinding maken met een virtuele machine in uw VNet en probeert te krijgen tot de resource-host: poort van daaruit. Om te testen voor TCP-toegang tot de PowerShell-opdracht gebruiken **test-netconnection**. De syntaxis is:

      test-netconnection hostname [optional: -Port]

* een toepassing op een virtuele machine en test de toegang op die host en poort openen vanuit de console van uw app

#### <a name="on-premises-resources"></a>On-premises bronnen ####

Als uw app niet kan van een resource on-premises bereiken, controleert u of als u de resource van uw VNet kunt bereiken. Gebruik de **test-netconnection** PowerShell-opdracht uit om dit te doen. Als uw virtuele machine uw on-premises bron niet bereiken kan, Controleer of dat uw Site-naar-Site VPN-verbinding werkt. Als dit werkt, controleert u de dingen die u eerder hebt genoteerd en de on-premises gateway-configuratie en status. 

Als uw VNet wordt gehost kan virtuele machine kan bereiken uw on-premises systeem echter uw app niet en vervolgens de reden waarschijnlijk een van de volgende opties is:

* de routes zijn niet geconfigureerd met de punt aan site IP-adresbereiken in uw on-premises gateway
* uw netwerkbeveiligingsgroepen worden blokkeert de toegang voor uw punt-naar-Site-IP-adresbereik
* uw on-premises firewall blokkeert verkeer van de punt-naar-Site-IP-adresbereik
* u hebt een udr gebruiker gedefinieerd in uw VNet waarmee wordt voorkomen dat uw verkeer punt-naar-Site op basis van uw on-premises netwerk wordt bereikt

## <a name="powershell-automation"></a>PowerShell-automatisering

U kunt App Service integreren met een Azure-netwerk met behulp van PowerShell. Zie voor een kant-en-klaar script, [een app in Azure App Service verbinden met een Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).

## <a name="hybrid-connections-and-app-service-environments"></a>Hybride verbindingen en App Service-omgevingen
Er zijn drie functies waarmee toegang tot resources van de VNet die wordt gehost. Dit zijn:

* VNet-integratie
* Hybride verbindingen
* App Service-omgevingen

Hybride verbindingen, moet u voor het installeren van een relay-agent de hybride verbinding Manager(HCM) in uw netwerk genoemd. De HCM moet geen verbinding maken met Azure en ook met uw toepassing. Deze oplossing is met name handig in een extern netwerk, zoals uw on-premises netwerk of zelfs een andere cloud gehost netwerk omdat het geen een eindpunt dat internet toegankelijk is vereist. De HCM kan alleen worden uitgevoerd op Windows en u kunt maximaal vijf exemplaren die worden uitgevoerd voor een hoge beschikbaarheid hebben. Hybride verbindingen biedt alleen ondersteuning voor TCP via en elke HC-eindpunt moet overeenkomen met de combinatie van een specifieke host: poort. 

De functie App Service-omgeving kunt u een exemplaar van de Azure App Service wordt uitgevoerd in uw VNet. Hiermee kunt uw apps toegang tot bronnen in uw VNet zonder eventuele extra stappen. Sommige van de andere voordelen van een App Service Environment zijn die u op basis van Dv2 werknemers met maximaal 14 GB aan RAM-geheugen gebruiken kunt. Een ander voordeel is dat u het systeem om te voldoen aan uw behoeften kunt schalen. In tegenstelling tot de multitenant-omgevingen waar uw ASP beperkt tot 20 exemplaren is, in een as-omgeving kunt u maximaal 100 ASP-instanties schalen. Een van de voordelen van een as-omgeving die u niet met VNet-integratie is dat een App Service Environment met een ExpressRoute-VPN kunt werken. 

Hoewel er dat enkele aanvraag overlapping gebruiken, kunnen geen van deze functies van de andere vervangen. Weten welke functie u wilt gebruiken, is gekoppeld aan uw behoeften. Bijvoorbeeld:

* Als u een ontwikkelaar bent en u wilt uitvoeren van een site in Azure en deze toegang tot de database op het werkstation onder uw bureau hebt, is het eenvoudigste wat u hybride verbindingen. 
* Als u een grote organisatie die een groot aantal wil Webeigenschappen in de openbare cloud en deze beheren in uw eigen netwerk, klikt u naartoe wilt gaan met de App Service-omgeving. 
* Als u meerdere apps die toegang moeten krijgen tot resources in uw VNet hebt, is de manier om door te gaan met VNet-integratie. 

Als uw VNet al met uw on-premises netwerk verbonden is, is klikt u vervolgens met behulp van VNet-integratie of een App Service Environment een eenvoudige manier om on-premises bronnen te verbruiken. Als uw VNet niet met uw on-premises netwerk verbonden is, is veel meer overhead voor het instellen van een site-naar-site-VPN-verbinding met uw VNet in vergelijking met het installeren van de HCM. 

Naast de functionele verschillen, er zijn ook prijsverschillen. De functie App Service Environment is een Premium serviceaanbieding, maar biedt de meeste netwerk configuratiemogelijkheden naast andere handige functies. VNet-integratie kan worden gebruikt met Standard of Premium ASP's en is ideaal voor veilig gebruik van resources in uw VNet van de App Service met meerdere tenants. Hybride verbindingen is momenteel afhankelijk is van een BizTalk-account, wat heeft niveaus die begin gratis en vervolgens geleidelijk duurder op basis van de hoeveelheid die u nodig hebt. Als het gaat om het werkt maar in veel netwerken, is er geen andere functie, zoals hybride verbindingen, waarmee u toegang krijgt tot resources in afzonderlijke netwerken ook meer dan 100. 

## <a name="new-vnet-integration"></a>Nieuwe VNet-integratie ##

Er is een nieuwe versie van de VNet-integratie-capaciteit die niet afhankelijk van punt-naar-Site VPN-technologie. Deze nieuwe versie is in Preview. De nieuwe VNet-integratie-functie heeft de volgende kenmerken.

- De nieuwe functie vereist een niet-gebruikte in uw Resource Manager-VNet-subnet
- Een adres wordt gebruikt voor elk exemplaar van App Service-plan. Omdat de grootte van het gatewaysubnet kan niet worden gewijzigd nadat de toewijzing, gebruikt u een subnet dat meer dan is voor de grootte van uw maximale schaal voldoende. Een/27 met 32 adressen is de aanbevolen grootte, zoals die overeenkomt met een App Service-plan die wordt geschaald naar 20 instanties.
- U kunt beveiligde bronnen met behulp van de nieuwe functie van de VNet-integratie van Service-eindpunt verbruiken. Toegang vanaf het subnet dat is toegewezen aan uw app Service-eindpunten configureren met uw app inschakelen
- U kunt toegang tot resources via ExpressRoute-verbindingen zonder extra configuratie
- Er is geen gateway is vereist voor het gebruik van de nieuwe functie voor VNet-integratie
- Uw App Service-plan moet een plan Standard, Premium of PremiumV2
- De nieuwe functie is alleen beschikbaar vanuit de nieuwere Azure App Service-schaaleenheden. De portal ziet u als uw app in de nieuwe VNet-integratie-functie kunt gebruiken. 
- De app en het VNet moet zich in dezelfde regio

De nieuwe functie voor VNet-integratie is in eerste instantie alleen beschikbaar in regio's Noord-Europa en VS-Oost.


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
