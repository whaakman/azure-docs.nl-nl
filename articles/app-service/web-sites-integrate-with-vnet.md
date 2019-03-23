---
title: App integreren met Azure Virtual Network - Azure App Service
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
ms.date: 11/12/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 768179f8569eac14166bcbb0a888e1cdbe41d497
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369697"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uw app integreren met een Azure-netwerk
Dit document beschrijft de functie voor integratie met virtueel netwerk Azure App Service en laat zien hoe u om in te stellen met apps in [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Virtuele netwerken van Azure] [ VNETOverview] (VNets) kunt u veel van uw Azure-resources in een niet-routeerbare internetnetwerk plaatsen. Deze netwerken kunnen vervolgens worden verbonden met uw on-premises netwerken met behulp van VPN-technologieën. 

De Azure App Service heeft twee vormen. 

1. De multitenant-systemen die ondersteuning bieden voor het volledige bereik van prijzen en abonnementen, behalve geïsoleerd
2. De App Service Environment (ASE), die wordt geïmplementeerd in uw VNet. 

Dit document gaat via de VNet-integratiefunctie, deze is bedoeld voor gebruik in de App Service met meerdere tenants.  Als uw app in [App Service-omgeving][ASEintro], en vervolgens deze al in een VNet is en geen gebruik van de VNet-integratiefunctie vereist tot resources in hetzelfde VNet.

VNet-integratie kunt uw web-app-toegang tot resources in uw virtuele netwerk, maar geen persoonlijke toegang verleend aan uw web-app vanuit het virtuele netwerk. Toegang tot de persoonlijke site verwijst naar het maken van uw app alleen toegankelijk is vanaf een particulier netwerk zoals uit binnen een virtueel Azure-netwerk. Toegang tot de persoonlijke site is alleen beschikbaar met een as-omgeving geconfigureerd met een interne Load Balancer (ILB). Beginnen met het artikel hier voor meer informatie over het gebruik van een ILB as-omgeving: [Het maken en gebruiken van een ILB as-omgeving][ILBASE]. 

VNet-integratie wordt vaak gebruikt om de toegang van apps met een databases en -services die worden uitgevoerd in uw VNet. Met VNet-integratie hoeft u geen een openbaar eindpunt beschikbaar maken voor toepassingen op uw virtuele machine maar kan de particuliere niet internet routeerbare adressen in plaats daarvan gebruikt. 

De functie van de VNet-integratie:

* vereist een Standard, Premium of PremiumV2 prijsstelling 
* werkt met het klassieke of Resource Manager VNet 
* biedt ondersteuning voor TCP en UDP
* werkt met Web, mobiel, API apps, en functie-apps
* Hiermee kunt een app te verbinden met slechts 1 VNet tegelijk
* Hiermee kunt maximaal vijf VNets kunnen worden geïntegreerd met in een App Service-Plan 
* Hiermee kunt u hetzelfde VNet moet worden gebruikt door meerdere apps in een App Service-Plan
* een virtuele netwerkgateway die is geconfigureerd met een punt bij Site-VPN is vereist
* biedt ondersteuning voor een SLA van 99,9% vanwege de SLA voor de gateway

Er zijn enkele dingen die VNet-integratie biedt geen ondersteuning voor met inbegrip van:

* een schijf koppelen
* AD-integratie 
* NetBios
* toegang tot de persoonlijke site
* resources benaderen via ExpressRoute 
* toegang tot bronnen in de Service-eindpunten 

### <a name="getting-started"></a>Aan de slag
Hier volgen een aantal zaken waarmee u rekening moet houden voordat u uw web-app verbinden met een virtueel netwerk:

* Een doel virtueel netwerk moet punt-naar-site VPN is ingeschakeld met een op route gebaseerde gateway voordat deze kan worden verbonden met de app hebben. 
* Het VNet moet zich in hetzelfde abonnement als uw App Service-Plan(ASP).
* De apps die zijn geïntegreerd met een VNet gebruiken de DNS-server die is opgegeven voor dit VNet.

## <a name="enabling-vnet-integration"></a>VNet-integratie inschakelen

Er is een nieuwe versie van de VNet-integratie-functie die is beschikbaar als preview. Het niet afhankelijk van punt-naar-site VPN en biedt ook ondersteuning voor toegang tot resources via ExpressRoute of Service-eindpunten. Voor meer informatie over de nieuwe preview-functie, gaat u naar het einde van dit document. 

### <a name="set-up-a-gateway-in-your-vnet"></a>Een gateway in uw VNet instellen ###

Als u al een gateway zijn geconfigureerd met punt-naar-site-adressen hebt, kunt u overslaan met het configureren van VNet-integratie met uw app.  
Een gateway maken:

1. [Een gatewaysubnet maken] [ creategatewaysubnet] in uw VNet.  

1. [Maken van de VPN-gateway][creategateway]. Selecteer een op route gebaseerde VPN-type.

1. [Het punt ingesteld op siteadressen][setp2saddresses]. Als de gateway niet in de basis-SKU, klikt u vervolgens IKEV2 moet worden uitgeschakeld in de punt-naar-site-configuratie en SSTP moet worden geselecteerd. De adresruimte moet zich in een van de volgende-adresblokken:

* 10.0.0.0/8 - dit betekent dat een IP-adresbereik van 10.0.0.0 naar 10.255.255.255
* 172.16.0.0/12 - dit betekent dat een IP-adresbereik van 172.16.0.0 naar 172.31.255.255 
* 192.168.0.0/16 - dit betekent dat een IP-adresbereik van 192.168.0.0 naar 192.168.255.255

Als u alleen het maken van de gateway voor gebruik met App Service-VNet-integratie, vervolgens u niet nodig hebt om een certificaat te uploaden. Het maken van de gateway kan 30 minuten duren. Niet mogelijk het integreren van uw app met uw VNet totdat de gateway is ingericht. 

### <a name="configure-vnet-integration-with-your-app"></a>VNet-integratie met uw app configureren ###

VNet-integratie inschakelen voor uw app: 

1. Gaat u naar uw app in Azure portal en app-instellingen openen en selecteert u netwerken > VNet-integratie. Schalen van uw ASP een standaard-SKU of beter voordat u VNet-integratie kunt configureren. 
 ![VNet-integratie UI][1]

1. Selecteer **VNet toevoegen**. 
 ![Toevoegen van VNet-integratie][2]

1. Selecteer uw VNet. 
  ![Selecteer uw VNet][8]
  
Uw app wordt opnieuw gestart na deze laatste stap.  

## <a name="how-the-system-works"></a>De werking van het systeem
De functie voor VNet-integratie is gebaseerd op een punt-naar-site VPN-technologie. Apps in Azure App Service worden gehost in een systeem met meerdere tenants, die verhindert het inrichten van een app rechtstreeks in een VNet. De technologie voor punt-naar-site beperkt de toegang tot het netwerk met alleen de virtuele machine die als host fungeert de app. Apps zijn beperkt tot alleen verkeer verzendt uit met het internet, via hybride verbindingen of via VNet-integratie. 

![De werking van VNet-integratie][3]

## <a name="managing-the-vnet-integrations"></a>Beheren van de VNet-integraties
De mogelijkheid om verbinding te verbreken met een VNet is op het niveau van een app. Bewerkingen die invloed hebben op de VNet-integratie voor meerdere apps zijn op het niveau van de App Service-plan. U kunt vanuit de app UID, informatie krijgen op uw VNet. Dezelfde informatie wordt ook weergegeven op het niveau van ASP. 

 ![VNet-details][4]

In de pagina Status van de functie Network kunt u zien als uw app is verbonden met uw VNet. Als uw VNet-gateway niet actief om welke reden dan ook is, wordt de status van uw weergegeven als niet-verbonden. 

De informatie die u hebt nu beschikbaar in de gebruikersinterface van niveau VNet-integratie is hetzelfde als de informatie die u van de ASP krijgt-app. Dit zijn de items:

* Naam van de VNet - koppelingen naar het virtuele netwerk UI
* Locatie - komt overeen met de locatie van uw VNet. Integreren met een VNet in een andere locatie kan leiden tot latentieproblemen met voor uw app. 
* Status van het certificaat - weerspiegelt als uw certificaten gesynchroniseerd tussen uw App Service-plan en uw VNet zijn.
* Status van de gateway - moeten uw gateways niet beschikbaar zijn om enige reden vervolgens uw app geen toegang tot resources in het VNet. 
* VNet-adresruimte - ziet u de IP-adresruimte voor uw VNet. 
* Punt-naar-site-adresruimte - ziet u het punt om te site IP-adresruimte voor uw VNet. Bij het aanroepen in uw VNet, worden de FROM-adres van uw app een van deze adressen. 
* Site-naar-site-adresruimte - kunt u site-naar-site VPN-verbindingen naar uw VNet verbinding maken met uw on-premises bronnen of andere VNet. De IP-bereiken die zijn gedefinieerd met deze VPN-verbinding worden hier weergegeven.
* DNS-Servers - geeft de DNS-Servers geconfigureerd met uw VNet.
* IP-adressen die zijn doorgestuurd naar het VNet - toont de-adresblokken doorgestuurd gebruikt voor verkeer van het station in uw VNet 

De enige bewerking die u in de app-weergave van uw VNet-integratie uitvoeren kunt is uw app ontkoppelt van het momenteel is verbonden met VNet. Als u wilt verbreken uw app van een VNet, selecteer **verbinding verbreken**. Uw app wordt opnieuw gestart wanneer u een VNet te verbreken. Verbinding verbreken verandert uw VNet niet. Het VNet en de configuratie met inbegrip van de gateways blijft ongewijzigd. Als u vervolgens verwijderen van uw VNet wilt, moet u de resources in met inbegrip van de gateways eerst verwijderen. 

Als u wilt de gebruikersinterface van ASP-VNet-integratie is bereikt, open uw ASP-gebruikersinterface en selecteer **netwerken**.  Selecteer onder de VNet-integratie, **Klik hier om te configureren** openen van de gebruikersinterface voor het netwerk functie Status.

![ASP-VNet-integratie-informatie][5]

De ASP-VNet-integratie-gebruikersinterface ziet u alle van de vnet's die worden gebruikt door de apps in uw ASP. U kunt maximaal 5 VNets die zijn verbonden met door een willekeurig aantal apps in uw App Service-plan hebben. Elke app kan slechts één integration geconfigureerd hebben. Voor informatie over elk VNet, klikt u op het VNet waarin u geïnteresseerd bent. Er zijn twee acties die u hier kunt uitvoeren.

* **Netwerk synchroniseren**. De synchronisatiebewerking van het netwerk zorgt ervoor dat uw certificaten en netwerkgegevens gesynchroniseerd zijn. Als u toevoegen of wijzigen van de DNS-server van uw VNet, moet u om uit te voeren een **netwerk synchroniseren** bewerking. Met deze bewerking wordt opnieuw opgestart alle apps met behulp van dit VNet.
* **Routes toevoegen** routes toe te voegen de basis van uitgaand verkeer in uw VNet.

**Routering** de routes die zijn gedefinieerd in uw VNet worden gebruikt om verkeer te regelen in uw VNet vanaf uw app. Als u aanvullende uitgaand verkeer verzenden naar het VNet wilt, kunt u deze-adresblokken hier toevoegen.   

**Certificaten** als VNet-integratie is ingeschakeld, wordt er een vereiste uitwisselen van certificaten om te controleren of de beveiliging van de verbinding. Samen met de certificaten zijn de DNS-configuratie, routes en andere vergelijkbare dingen die worden beschreven van het netwerk.
Als certificaten of netwerkgegevens wordt gewijzigd, moet u klikken op 'Sync netwerk'. Als u 'Sync netwerk' klikt, zorgt u ervoor dat een korte onderbreking in de verbinding tussen uw app en uw VNet. Terwijl uw app wordt niet opnieuw opgestart, het verlies van connectiviteit kan ervoor zorgen dat uw site niet correct. 

## <a name="accessing-on-premises-resources"></a>Toegang tot on-premises bronnen
Apps toegang hebben tot on-premises bronnen door te integreren met VNets met site-naar-site-verbindingen. Voor toegang tot on-premises bronnen, moet u uw on-premises VPN-gateway routes bijwerken met de punt-naar-site-adresblokken. Wanneer de site-naar-site VPN-verbinding eerst is ingesteld, moeten de scripts die worden gebruikt om deze te configureren routes correct ingesteld. Als u de punt-naar-site-adressen toevoegen nadat u uw site-naar-site-VPN hebt gemaakt, moet u de routes, handmatig bijwerken. Informatie over hoe u dat doen variëren per gateway en worden hier niet beschreven. Bovendien geen BGP is geconfigureerd met een site-naar-site VPN-verbinding.

> [!NOTE]
> De VNet-integratiefunctie integreren niet een app met een VNet met een ExpressRoute-Gateway. Zelfs als de ExpressRoute-Gateway is geconfigureerd [co-existentie modus] [ VPNERCoex] de VNet-integratie werkt niet. Als u toegang tot resources via een ExpressRoute-verbinding wilt maken, dan kunt u een [App Service-omgeving][ASE], die wordt uitgevoerd in uw VNet. 
> 
> 

## <a name="peering"></a>Peering
VNet-integratie kunt u toegang tot bronnen in vnet's die zijn gekoppeld aan het VNet dat u met verbonden bent. Het configureren van peering om te werken met uw app:

1. Voeg dat een peeringverbinding op het VNet uw app verbinding maakt met. Bij het toevoegen van de peeringverbinding inschakelen **toestaan van toegang tot het virtuele netwerk** en Controleer **doorgestuurd verkeer toestaan** en **gatewayoverdracht toestaan**.
1. Een peeringverbinding toevoegen op het VNet dat gekoppeld is met het VNet dat u met verbonden bent. Bij het toevoegen van de peeringverbinding op de doel-VNet, inschakelen **toestaan van toegang tot het virtuele netwerk** en Controleer **doorgestuurd verkeer toestaan** en **toestaan van externe gateways**.
1. Ga naar de App Service-plan > netwerken > gebruikersinterface van de VNet-integratie in de portal.  Selecteer het VNet dat uw app maakt verbinding met. Voeg het adresbereik van het VNet dat is gekoppeld aan het VNet dat uw app is verbonden met onder de sectie routering.  


## <a name="pricing-details"></a>Prijsdetails
Er zijn drie gerelateerde kosten voor het gebruik van de functie voor VNet-integratie:

* ASP voorschriften laag-prijzen
* Kosten voor de gegevensoverdracht
* Kosten van de VPN-Gateway.

Uw apps moeten zich in een Standard, Premium of PremiumV2 App Service-Plan. U kunt meer details weergeven op de kosten die u hier: [App Service-prijzen][ASPricing]. 

Er is een kosten in rekening gebracht voor uitgaande gegevens, zelfs als het VNet in hetzelfde Datacenter. Deze kosten worden beschreven in [Data Transfer prijsinformatie over][DataPricing]. 

Er is een kosten voor de VNet-gateway die is vereist voor de punt-naar-site VPN-verbinding. De gegevens zijn op de [prijzen voor VPN-Gateway] [ VNETPricing] pagina.

## <a name="troubleshooting"></a>Problemen oplossen
Wanneer de functie eenvoudig om in te stellen is, betekent dat niet dat uw ervaring probleem gratis zijn. Moet u ondervindt zijn problemen met het openen van uw gewenste eindpunt er enkele hulpprogramma's die u gebruiken kunt voor het testen van de connectiviteit van de app-console. Er zijn twee consoles die u kunt gebruiken. Is de Kudu-console en de andere is de console in de Azure-portal. Als u wilt de Kudu-console vanuit uw app is bereikt, Ga naar Extra -> Kudu. Dit is hetzelfde als het gaat [sitename]. scm.azurewebsites.net. Zodra dat wordt geopend, gaat u naar het foutopsporingstabblad van de console. Ga naar hulpprogramma's om op te halen voor de Azure portal gehoste-console en vervolgens vanuit uw app-Console >. 

#### <a name="tools"></a>Hulpprogramma's
De hulpprogramma's **ping**, **nslookup** en **tracert** via de console vanwege veiligheidsbeperkingen werkt niet. Om in te vullen de void twee afzonderlijke hulpprogramma's toegevoegd. Als u wilt testen DNS-functionaliteit, hebben we een hulpprogramma met de naam nameresolver.exe toegevoegd. De syntaxis is:

    nameresolver.exe hostname [optional: DNS Server]

U kunt **nameresolver** om te controleren of de hostnamen die afhankelijk van uw app. Op deze manier kunt u als u niets verkeerd geconfigureerd met uw DNS of mogelijk geen toegang tot de DNS-server hebt testen.

De volgende hulpprogramma kunt u testen voor TCP-verbinding met een combinatie van de host en poort. Dit hulpprogramma wordt aangeroepen **tcpping** en de syntaxis is:

    tcpping.exe hostname [optional: port]

De **tcpping** hulpprogramma geeft u aan als u een specifieke host en poort kunt bereiken. Alleen succes kan weergegeven als: Er is een toepassing die luistert op de combinatie van de host en poort en er is toegang tot het netwerk van uw app naar de opgegeven host en poort.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Toegang tot de VNet-foutopsporing gehoste bronnen
Er zijn een aantal dingen die u kunt voorkomen dat uw app in een specifieke host en poort bereikt. De meeste gevallen is het een van drie dingen:

* **Er is een firewall op de manier.** Als u een firewall op de manier hebt, kunt u de time-out voor TCP wordt bereikt. De TCP-time is in dit geval 21 seconden. Gebruik de **tcpping** hulpprogramma connectiviteit testen. TCP-time-outs kan vanwege een groot aantal dingen voorbij firewalls maar het begin beginnen. 
* **DNS is niet toegankelijk is.** De DNS-out is drie seconden per DNS-server. Als u twee DNS-servers hebt, is de time-out 6 seconden. Gebruik nameresolver om te zien of DNS werkt. Vergeet niet dat je niet nslookup gebruiken als die niet van de uw VNet is geconfigureerd gebruikmaken met DNS-server.
* **Het punt-naar-site-adresbereik is ongeldig.** Het IP-adresbereik voor punt-naar-site moet zich in de RFC 1918 privé IP-adresbereiken (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Als het bereik IP-adressen worden gebruikt buiten die, klikt u vervolgens werkt dingen niet. 

Als deze items geen antwoord op uw probleem, zoek eerst de eenvoudige zaken, zoals: 

* Wordt de Gateway weergegeven alsof ze van de portal?
* Certificaten worden weergegeven alsof ze synchroon?
* Iedereen de netwerkconfiguratie wijzigen zonder een 'Sync netwerk' in de betrokken ASP's? 

Als uw gateway niet actief is, brengt u deze back-up. Als uw certificaten niet gesynchroniseerd zijn, gaat u naar de ASP-weergave van uw VNet-integratie en druk op 'Sync netwerk'. Als u vermoedt dat er is een wijziging aangebracht in uw VNet-configuratie die niet is gesynchroniseerd met uw ASP's, drukt u op 'Sync netwerk'.  Een 'Sync netwerk'-bewerking wordt opnieuw opgestart van alle apps in de ASP die kunnen worden geïntegreerd met dit VNet. 

Als dat is prima, moet u om u te verdiepen een en ander:

* Zijn er geen andere apps die met behulp van VNet-integratie tot resources in hetzelfde VNet? 
* Kunt u gaat u naar de app-console en tcpping gebruiken om te bereiken van andere bronnen in uw VNet? 

Als een van bovenstaande voldaan wordt, vervolgens uw VNet-integratie functioneert en het probleem zich ergens anders bevindt. Dit is waar het wordt moet meer zijn van een uitdaging omdat er geen eenvoudige manier om te zien waarom u een host: poort niet bereiken. Enkele van de oorzaken zijn:

* u hebt een firewall van op de host geen toegang tot poort van de toepassing vanaf het punt aan site IP-adresbereik. Vaak overschrijden van subnetten vereist openbare toegang.
* de doelhost is niet actief
* uw toepassing is niet actief
* u hebt het verkeerde IP of hostnaam
* uw toepassing luistert op een andere poort dan wat u verwacht. U kunt uw proces-ID met de poort voor luisteren vergelijken met behulp van 'netstat - aon' op de host van het eindpunt. 
* uw netwerkbeveiligingsgroepen zijn geconfigureerd in zodanig dat ze toegang tot uw toepassingshost en poort van de punt aan site IP-adresbereik voorkomen

Houd er rekening mee dat u niet welk IP in uw punt-naar-Site-IP-adresbereik dat door uw app wordt gebruikt weet, zodat u wilt toestaan dat toegang vanaf het volledige bereik. 

Extra stappen zijn onder andere:

* verbinding maken met een virtuele machine in uw VNet en probeert te krijgen tot de resource-host: poort van daaruit. Als u wilt testen voor TCP-toegang, gebruik de PowerShell-opdracht **test-netconnection**. De syntaxis is:

      test-netconnection hostname [optional: -Port]

* een toepassing op een virtuele machine en test de toegang op die host en poort openen vanuit de console van uw app

#### <a name="on-premises-resources"></a>On-premises bronnen ####

Als uw app niet kan van een resource on-premises bereiken, controleert u of als u de resource van uw VNet kunt bereiken. Gebruik de **test-netconnection** PowerShell-opdracht uit om te controleren op TCP-toegang. Als uw virtuele machine uw on-premises bron niet bereiken kan, Controleer of dat uw Site-naar-Site VPN-verbinding werkt. Als dit werkt, controleert u de dingen die u eerder hebt genoteerd en de on-premises gateway-configuratie en status. 

Als uw VNet die worden gehost virtuele machine kan bereiken uw on-premises systeem, maar uw app kan niet, wordt de oorzaak is waarschijnlijk een van de volgende redenen:

* de routes zijn niet geconfigureerd met de punt aan site IP-adresbereiken in uw on-premises gateway
* uw netwerkbeveiligingsgroepen worden blokkeert de toegang voor uw punt-naar-Site-IP-adresbereik
* uw on-premises firewall blokkeert verkeer van de punt-naar-Site-IP-adresbereik


## <a name="powershell-automation"></a>PowerShell-automatisering

U kunt App Service integreren met een Azure-netwerk met behulp van PowerShell. Zie voor een kant-en-klaar script, [een app in Azure App Service verbinden met een Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).

## <a name="hybrid-connections-and-app-service-environments"></a>Hybride verbindingen en App Service-omgevingen
Er zijn drie functies waarmee toegang tot resources van de VNet die wordt gehost. Dit zijn:

* VNet-integratie
* Hybride verbindingen
* App Service-omgevingen

Hybride verbindingen, moet u voor het installeren van een relay-agent de hybride verbinding Manager(HCM) in uw netwerk genoemd. De HCM moet geen verbinding maken met Azure en ook met uw toepassing. Hybride verbindingen geen vereist een eindpunt dat binnenkomende internet toegankelijk is voor het externe netwerk, zoals is vereist voor een VPN-verbinding. De HCM kan alleen worden uitgevoerd op Windows en u kunt maximaal vijf exemplaren die worden uitgevoerd voor een hoge beschikbaarheid hebben. Hybride verbindingen biedt alleen ondersteuning voor TCP via en elke HC-eindpunt moet overeenkomen met de combinatie van een specifieke host: poort. 

De functie App Service-omgeving kunt u een exemplaar van één tenant van de Azure App Service uitvoeren in uw VNet. Als uw apps zich in een App Service-omgeving, klikt u vervolgens uw apps toegang hebben tot resources in uw VNet zonder eventuele extra stappen. Met een App Service Environment worden uw apps uitgevoerd op krachtiger werknemers en kunnen worden opgeschaald tot 100 ASP-instanties. App Service-omgevingen werken met alle van de functies voor netwerkbeheer met inbegrip van ExpressRoute- en Service-eindpunten.  

Hoewel er dat enkele aanvraag overlapping gebruiken, kunnen geen van deze functies van de andere vervangen. Weten welke functie u wilt gebruiken, is gekoppeld aan uw behoeften. Bijvoorbeeld:

* Als u een ontwikkelaar bent en wilt uitvoeren van een site in Azure die toegang heeft tot een database op het werkstation onder uw werkplek, is het eenvoudigste wat u hybride verbindingen. 
* Als u een grote organisatie die een groot aantal wil Webeigenschappen in de openbare cloud en deze beheren in uw eigen netwerk, klikt u naartoe wilt gaan met de App Service-omgeving. 
* Als u meerdere apps die toegang moeten krijgen tot resources in uw VNet hebt, is de manier om door te gaan met VNet-integratie. 

Als uw VNet is al verbonden met uw on-premises netwerk, klikt u vervolgens met behulp van VNet-integratie of een App Service Environment is een eenvoudige manier te verbruiken on-premises resources. Als uw VNet niet is verbonden met uw on-premises netwerk, is veel meer overhead voor het instellen van een site-naar-site-VPN-verbinding met uw VNet in vergelijking met het installeren van de HCM. 

Naast de functionele verschillen, er zijn ook prijsverschillen. De functie App Service Environment is een Premium serviceaanbieding, maar biedt de meeste netwerk configuratiemogelijkheden naast andere handige functies. VNet-integratie kan worden gebruikt met Standard of Premium ASP's en is ideaal voor veilig gebruik van resources in uw VNet van de App Service met meerdere tenants. Hybride verbindingen is momenteel afhankelijk is van een BizTalk-account, wat heeft niveaus die begin gratis en vervolgens geleidelijk duurder op basis van de hoeveelheid die u nodig hebt. Als het gaat om het werkt maar in veel netwerken, is er geen andere functie, zoals hybride verbindingen, waarmee u toegang krijgt tot resources in afzonderlijke netwerken ook meer dan 100. 

## <a name="new-vnet-integration"></a>Nieuwe VNet-integratie ##

Er is een nieuwe versie van de VNet-integratie-capaciteit die niet afhankelijk van punt-naar-Site VPN-technologie. In tegenstelling tot de bestaande functie werkt de nieuwe Preview-functie met een ExpressRoute- en Service-eindpunten. 

De nieuwe functie is alleen beschikbaar vanuit de nieuwere Azure App Service-schaaleenheden. Als u kan worden geschaald naar PremiumV2, klikt u vervolgens bent u in een nieuwe App Service-schaaleenheid. De gebruikersinterface van de VNet-integratie in de portal ziet u als uw app in de nieuwe VNet-integratie-functie kunt gebruiken. 

De nieuwe versie is in Preview en heeft de volgende kenmerken.

* Er is geen gateway is vereist voor het gebruik van de nieuwe functie voor VNet-integratie
* U kunt toegang tot resources via ExpressRoute-verbindingen zonder extra configuratie dan integreren met de ExpressRoute VNet aangesloten.
* De app en het VNet moet zich in dezelfde regio
* De nieuwe functie moet een niet-gebruikte in uw Resource Manager-VNet-subnet.
* Uw App Service-plan moet een plan Standard, Premium of PremiumV2
* Productieworkloads worden niet ondersteund op de nieuwe functie in Preview-versie
* Uw app moet zich in een Azure App Service-implementatie waarmee naar Premium v2 omhoog te schalen.
* De nieuwe functie voor VNet-integratie werkt niet voor apps in een App Service Environment.
* U kunt een VNet met een geïntegreerde app niet verwijderen.  
* Routetabellen en wereldwijde peering zijn nog niet beschikbaar met de nieuwe VNet-integratie.  
* Een adres wordt gebruikt voor elk exemplaar van App Service-plan. Omdat de grootte van het gatewaysubnet kan niet worden gewijzigd nadat de toewijzing, gebruikt u een subnet dat meer dan is voor de grootte van uw maximale schaal voldoende. Een/27 met 32 adressen is de aanbevolen grootte, zoals die overeenkomt met een App Service-plan die wordt geschaald naar 20 instanties.
* U kunt beveiligde bronnen met behulp van de nieuwe functie van de VNet-integratie van Service-eindpunt verbruiken. Om dit te doen, moet u service-eindpunten op het subnet dat wordt gebruikt voor VNet-integratie inschakelen.

Met de nieuwe functie:

1. Ga naar de gebruikersinterface van netwerken in de portal. Als uw app kan de nieuwe functie wilt gebruiken, ziet u een mogelijkheid om te gebruiken van de nieuwe preview-functie.  

   ![Selecteer de nieuwe preview-VNet-integratie][6]

1. Selecteer **VNet toevoegen (preview)**.  

1. Selecteer het Resource Manager VNet die u wilt integreren in en klikt u vervolgens een nieuw subnet maken of kies een bestaand leeg subnet. De integratie van neemt minder dan een minuut in beslag. Uw app wordt tijdens de integratie wordt opnieuw gestart.  Als integratie is voltooid, ziet u details over de VNet die u kunnen worden geïntegreerd in en een banner aan de bovenkant waarin u dat de functie is beschikbaar als preview.

   ![Selecteer het VNet en subnet][7]

Als u wilt inschakelen voor de app de DNS-server die uw VNet is geconfigureerd met te gebruiken, moet u een toepassingsinstelling voor uw app, waarbij de naam van de WEBSITE_DNS_SERVER en de waarde is het IP-adres van de server maken.  Als u een secundaire DNS-server hebt, maakt u een andere toepassingsinstelling waarbij de naam van de WEBSITE_DNS_ALT_SERVER en de waarde is het IP-adres van de server. 

Als u wilt verbreken uw app van het VNet, selecteer **verbinding verbreken**. Hiermee wordt uw web-app opnieuw opgestart. 

De nieuwe functie voor VNet-integratie kunt u gebruikmaken van service-eindpunten.  Als u service-eindpunten met uw app, gebruikt u de nieuwe VNet-integratie verbinding maken met een geselecteerde VNet en configureer vervolgens de service-eindpunten op het subnet dat u hebt gebruikt voor de integratie. 

<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-details.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-aspdetails.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-newvnet.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-newvnetdetails.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-selectvnet.png

<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#gatewaysubnet
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
