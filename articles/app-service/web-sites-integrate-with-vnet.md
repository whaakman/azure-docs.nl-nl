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
ms.date: 06/14/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: b269c75be7fec55fb77afecc6d04b86266c74a6f
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147309"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uw app integreren met een Azure-netwerk
Dit document beschrijft de functie voor integratie met virtueel netwerk Azure App Service en hoe u om in te stellen met apps in de [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Virtuele netwerken van Azure] [ VNETOverview] (VNets) kunt u veel van uw Azure-resources in een niet-routeerbare internetnetwerk plaatsen.  

De Azure App Service heeft twee vormen. 

1. De multitenant-systemen die ondersteuning bieden voor het volledige bereik van prijzen en abonnementen, behalve geïsoleerd
2. App Service Environment (ASE), die wordt geïmplementeerd in uw VNet en biedt ondersteuning voor geïsoleerde prijs-plan-apps

Dit document gaat via de twee VNet-integratie-functies, die is bedoeld voor gebruik in de App Service met meerdere tenants. Als uw app in [App Service-omgeving][ASEintro], en vervolgens deze al in een VNet is en geen gebruik van de VNet-integratiefunctie vereist tot resources in hetzelfde VNet. Lees voor meer informatie over alle van de App Service-netwerkfuncties, [App Service-functies voor netwerkbeheer](networking-features.md)

Er zijn twee vormen op de functie voor VNet-integratie

1. Één versie maakt integratie met VNets in dezelfde regio. Deze vorm van de functie vereist een subnet in een VNet in dezelfde regio. Deze functie is nog in preview, maar wordt ondersteund voor productieworkloads voor Windows-app met enkele aanvullende opmerkingen hieronder aangegeven.
2. De andere versie maakt integratie met vnet's in andere regio's of met klassieke VNets. Deze versie van de functie vereist de implementatie van een virtuele netwerkgateway in uw VNet. Dit is de punt-naar-site VPN op basis van functie.

Een app kan alleen een vorm van de VNet-integratiefunctie tegelijk gebruiken. Klik in de vraag is welke functie moet u gebruiken. U kunt gebruiken voor veel dingen. Er zijn echter de duidelijke verschillen:

| Probleem  | Oplossing | 
|----------|----------|
| Een adres RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) bereiken in dezelfde regio wilt | regionale VNet-integratie |
| Doelgroep van een klassiek VNet of een VNet in een andere regio | Gateway vereist een VNet-integratie |
| Wilt u RFC 1918 eindpunten via ExpressRoute bereiken | regionale VNet-integratie |
| Wilt u resources via de service-eindpunten bereiken | regionale VNet-integratie |

Geen van beide functie kunt u bereiken RFC 1918 adressen via ExpressRoute. Als u wilt doen die u wilt gebruiken een as-omgeving voor nu.

Met behulp van de regionale VNet-integratie geen verbinding maken met uw VNet naar on-premises of service-eindpunten configureren. Dat is gescheiden Clusternetwerkconfiguratie. De regionale VNet-integratie kunt gewoon uw app voor aanroepen voor deze verbindingstypen.

Ongeacht de versie die wordt gebruikt, VNet-integratie uw web-apptoegang geeft tot resources in uw virtuele netwerk, maar niet inkomende persoonlijke toegang verleend aan uw web-app vanuit het virtuele netwerk. Toegang tot de persoonlijke site verwijst naar het maken van uw app alleen toegankelijk is vanaf een particulier netwerk zoals uit binnen een virtueel Azure-netwerk. VNet-integratie is alleen voor uitgaande gesprekken vanuit uw app in uw VNet. 

De functie van de VNet-integratie:

* vereist een Standard, Premium of PremiumV2 prijsstelling 
* biedt ondersteuning voor TCP en UDP
* werkt met App Service-apps en functie-apps

Er zijn enkele dingen die VNet-integratie biedt geen ondersteuning voor met inbegrip van:

* een schijf koppelen
* AD-integratie 
* NetBios

## <a name="regional-vnet-integration"></a>regionale VNet-integratie 

Als VNet-integratie met VNets in dezelfde regio als uw app wordt gebruikt, is het vereist het gebruik van een gedelegeerde subnet met ten minste 32 adressen erin. Het subnet kan niet worden gebruikt voor iets anders. Uitgaande oproepen vanuit uw app worden gemaakt vanuit de adressen in de gedelegeerde subnet. Wanneer u deze versie van VNet-integratie, bestaan de aanroepen van adressen in uw VNet. Met behulp van de adressen in uw VNet, kan uw app:

* aanroepen naar service-eindpunt beveiligde services
* toegang tot resources via ExpressRoute-verbindingen
* toegang tot bronnen in het VNet dat u met verbonden bent
* toegang tot resources via gekoppelde verbindingen met inbegrip van ExpressRoute-verbindingen

Deze functie is beschikbaar als preview, maar dit wordt ondersteund voor productieworkloads van Windows-app met de volgende beperkingen:

* u kunt alleen adressen die zich binnen het bereik van RFC 1918 bereiken. De zijn adressen in het 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16-adresblokken.
* u kunt resources niet bereiken over wereldwijde peering-verbindingen
* u kunt routes niet instellen op het verkeer dat afkomstig is van uw app in uw VNet
* de functie is alleen beschikbaar via nieuwe App Service-schaaleenheden die ondersteuning bieden voor PremiumV2 App Service-plannen.
* de functie kan niet worden gebruikt door Isolated-abonnement-apps die zich in een App Service Environment
* de functie moet een niet-gebruikte met ten minste 32 adressen in uw Resource Manager-VNet-subnet.
* De app en het VNet moet zich in dezelfde regio
* Een adres wordt gebruikt voor elk exemplaar van App Service-plan. Omdat de grootte van het gatewaysubnet kan niet worden gewijzigd nadat de toewijzing, gebruikt u een subnet dat meer dan is voor de grootte van uw maximale schaal voldoende. Een/27 met 32 adressen is de aanbevolen grootte, zoals die overeenkomt met een App Service-plan die wordt geschaald naar 20 instanties.
* U kunt een VNet met een geïntegreerde app niet verwijderen. U moet eerst de integratie verwijderen 
* U kunt slechts één regionale VNet-integratie per App Service-plan hebben. Meerdere apps in hetzelfde App Service-plan kunnen gebruiken hetzelfde VNet. 

De functie is in Preview-versie ook voor Linux. Gebruik de functie VNet-integratie met een Resource Manager VNet in dezelfde regio:

1. Ga naar de gebruikersinterface van netwerken in de portal. Als uw app kan de nieuwe functie wilt gebruiken, ziet u een optie voor het VNet toevoegen (preview).  

   ![Selecteer de VNet-integratie][6]

1. Selecteer **VNet toevoegen (preview)** .  

1. Selecteer het Resource Manager VNet die u wilt integreren in en klikt u vervolgens een nieuw subnet maken of kies een bestaand leeg subnet. De integratie van neemt minder dan een minuut in beslag. Uw app wordt tijdens de integratie wordt opnieuw gestart.  Als integratie is voltooid, ziet u details over de VNet die u kunnen worden geïntegreerd in en een banner aan de bovenkant waarin u dat de functie is beschikbaar als preview.

   ![Selecteer het VNet en subnet][7]

Zodra uw app is geïntegreerd met uw VNet, wordt de DNS-server die uw VNet is geconfigureerd met gebruikt. 

Als u wilt verbreken uw app van het VNet, selecteer **verbinding verbreken**. Hiermee wordt uw web-app opnieuw opgestart. 

De nieuwe functie voor VNet-integratie kunt u gebruikmaken van service-eindpunten.  Als u service-eindpunten met uw app, gebruikt u de nieuwe VNet-integratie verbinding maken met een geselecteerde VNet en configureer vervolgens de service-eindpunten op het subnet dat u hebt gebruikt voor de integratie. 

#### <a name="web-app-for-containers"></a>Web App for Containers

Als u App Service op Linux met de ingebouwde installatiekopieën gebruikt, wordt het regionale VNet-integratiefunctie werkt zonder aanvullende wijzigingen. Als u Web App for Containers gebruikt, moet u uw docker-installatiekopie wijzigen als u wilt gebruiken van VNet-integratie. In uw docker-installatiekopie, gebruikt u de omgevingsvariabele poort als de luisterende poort van de belangrijkste webserver, in plaats van een poortnummer vastgelegd. De omgevingsvariabele poort bij het opstarten van de container automatisch ingesteld door App Service-platform.

### <a name="how-vnet-integration-works"></a>De werking van VNet-integratie

Apps in de App-Service worden gehost op werkrollen. De Basic en hoger prijzen en abonnementen zijn toegewezen die als host fungeert voor abonnementen waarbij er geen andere klanten workloads die worden uitgevoerd op de dezelfde werknemers. VNet-integratie werkt door de virtuele-interfaces met adressen in het gedelegeerde subnet koppelen. Omdat het van-adres is in uw VNet, het toegang heeft tot de meeste dingen in of via uw VNet net zoals een virtuele machine in uw VNet wilt. De VPN-implementatie is anders dan het uitvoeren van een VM in uw VNet, en dat is waarom sommige netwerkfuncties zijn nog niet beschikbaar tijdens het gebruik van deze functie.

![VNet-integratie](media/web-sites-integrate-with-vnet/vnet-integration.png)

Als VNet-integratie is ingeschakeld, wordt uw app nog steeds uitgaande gesprekken op internet via dezelfde kanalen als normale maken. De uitgaande adressen die worden vermeld in de portal voor app-eigenschappen zijn nog steeds de adressen die worden gebruikt door uw app. Wijzigingen voor uw app zijn dat oproepen aan service-eindpunt services beveiligde of RFC 1918-adressen gaat in uw VNet. 

De functie biedt alleen ondersteuning voor een virtuele hostinterface per worker.  Een virtuele hostinterface per worker betekent een regionale VNet-integratie per App Service-plan. Alle apps in hetzelfde App Service-plan het hetzelfde VNet-integratie kunt gebruiken, maar als u een app te verbinden met een extra VNet nodig hebt, moet u een andere App Service-plan maken. De virtuele-interface gebruikt, is geen resource die klanten rechtstreeks toegang tot hebben.

Vanwege de aard van de werking van deze technologie, wordt het verkeer dat wordt gebruikt met VNet-integratie niet weergegeven in Network Watcher of de NSG-stroomlogboeken.  

## <a name="gateway-required-vnet-integration"></a>Gateway vereist een VNet-integratie 

De Gateway vereist een VNet-integratiefunctie:

* kan worden gebruikt om verbinding maken met VNets in elke regio worden ze Resource Manager of klassieke vnet 's
* Hiermee kunt een app te verbinden met slechts 1 VNet tegelijk
* Hiermee kunt maximaal vijf VNets kunnen worden geïntegreerd met in een App Service-Plan 
* Hiermee kunt u hetzelfde VNet moet worden gebruikt door meerdere apps in een App Service-Plan zonder enige impact op het totale aantal die kan worden gebruikt door een App Service-plan.  Hebt u 6 apps met hetzelfde VNet in hetzelfde App Service-plan, telt dat als 1 VNet wordt gebruikt. 
* een virtuele netwerkgateway die is geconfigureerd met een punt bij Site-VPN is vereist
* Wordt niet ondersteund voor gebruik met Linux-apps
* Biedt ondersteuning voor een SLA van 99,9% vanwege de SLA voor de gateway

Deze functie biedt geen ondersteuning:

* Toegang tot resources in ExpressRoute 
* Toegang tot resources in service-eindpunten 

### <a name="getting-started"></a>Aan de slag

Hier volgen een aantal zaken waarmee u rekening moet houden voordat u uw web-app verbinden met een virtueel netwerk:

* Een doel virtueel netwerk moet punt-naar-site VPN is ingeschakeld met een op route gebaseerde gateway voordat deze kan worden verbonden met de app hebben. 
* Het VNet moet zich in hetzelfde abonnement als uw App Service-Plan(ASP).
* De apps die zijn geïntegreerd met een VNet gebruiken de DNS-server die is opgegeven voor dit VNet.

### <a name="set-up-a-gateway-in-your-vnet"></a>Een gateway in uw VNet instellen ###

Als u al een gateway zijn geconfigureerd met punt-naar-site-adressen hebt, kunt u overslaan met het configureren van VNet-integratie met uw app.  
Een gateway maken:

1. [Een gatewaysubnet maken] [ creategatewaysubnet] in uw VNet.  

1. [Maken van de VPN-gateway][creategateway]. Selecteer een op route gebaseerde VPN-type.

1. [Het punt ingesteld op siteadressen][setp2saddresses]. Als de gateway niet in de basis-SKU, klikt u vervolgens IKEV2 moet worden uitgeschakeld in de punt-naar-site-configuratie en SSTP moet worden geselecteerd. De adresruimte moet zich in de RFC 1918-adresblokken, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16

Als u alleen het maken van de gateway voor gebruik met App Service-VNet-integratie, vervolgens u niet nodig hebt om een certificaat te uploaden. Het maken van de gateway kan 30 minuten duren. Niet mogelijk het integreren van uw app met uw VNet totdat de gateway is ingericht. 

### <a name="configure-vnet-integration-with-your-app"></a>VNet-integratie met uw app configureren 

VNet-integratie inschakelen voor uw app: 

1. Gaat u naar uw app in Azure portal en app-instellingen openen en selecteert u netwerken > VNet-integratie. Uw ASP moet zich in een standaard-SKU of beter om een VNet-integratie-functie te gebruiken. 
 ![VNet-integratie UI][1]

1. Selecteer **VNet toevoegen**. 
 ![Toevoegen van VNet-integratie][2]

1. Selecteer uw VNet. 
  ![Selecteer uw VNet][8]
  
Uw app wordt opnieuw gestart na deze laatste stap.  

### <a name="how-the-gateway-required-vnet-integration-feature-works"></a>Hoe de gateway vereist voor VNet-integratiefunctie werkt

De gateway vereist VNet-integratiefunctie is gebaseerd op een punt-naar-site VPN-technologie. De technologie voor punt-naar-site beperkt de toegang tot het netwerk met alleen de virtuele machine die als host fungeert de app. Apps zijn beperkt tot alleen verkeer verzendt uit met het internet, via hybride verbindingen of via VNet-integratie. 

![De werking van VNet-integratie][3]

## <a name="managing-vnet-integration"></a>Beheer van VNet-integratie
De mogelijkheid om verbinding te verbreken met een VNet is op het niveau van een app. Bewerkingen die invloed hebben op de VNet-integratie voor meerdere apps zijn op het niveau van de App Service-plan. Vanuit de app > netwerken > portal voor VNet-integratie kunt u informatie krijgen over uw VNet. U kunt zien dat vergelijkbare informatie op het niveau van de ASP in de ASP-> netwerken > VNet-integratie-portal onder andere voor welke apps in het App Service-plan met behulp van een bepaalde integratie.

 ![VNet-details][4]

De informatie die u beschikbaar zijn voor u in de gebruikersinterface van de VNet-integratie is hetzelfde tussen de app en de ASP-Portal.

* Naam van de VNet - koppelingen naar het virtuele netwerk UI
* Locatie - komt overeen met de locatie van uw VNet. Integreren met een VNet in een andere locatie kan leiden tot latentieproblemen met voor uw app. 
* Status van het certificaat - weerspiegelt als uw certificaten gesynchroniseerd tussen uw App Service-plan en uw VNet zijn.
* Status van de gateway - moet u de gateway vereist een VNet-integratie, ziet u de status van de gateway.
* VNet-adresruimte - ziet u de IP-adresruimte voor uw VNet. 
* Punt-naar-site-adresruimte - ziet u het punt om te site IP-adresruimte voor uw VNet. Bij het aanroepen in uw VNet tijdens het gebruik van de functie voor de gateway vereist, worden de FROM-adres van uw app een van deze adressen. 
* Site-naar-site-adresruimte - kunt u site-naar-site VPN-verbindingen naar uw VNet verbinding maken met uw on-premises bronnen of andere VNet. De IP-bereiken die zijn gedefinieerd met deze VPN-verbinding worden hier weergegeven.
* DNS-Servers - geeft de DNS-Servers geconfigureerd met uw VNet.
* IP-adressen die zijn doorgestuurd naar het VNet - toont de-adresblokken doorgestuurd gebruikt voor verkeer van het station in uw VNet 

De enige bewerking die u in de app-weergave van uw VNet-integratie uitvoeren kunt is uw app ontkoppelt van het momenteel is verbonden met VNet. Als u wilt verbreken uw app van een VNet, selecteer **verbinding verbreken**. Uw app wordt opnieuw gestart wanneer u een VNet te verbreken. Verbinding verbreken verandert uw VNet niet. Het subnet of de gateway is niet verwijderd. Als u vervolgens verwijderen van uw VNet wilt, moet u eerst uw app ontkoppelt van het VNet en verwijderen van de resources, zoals gateways. 

Als u wilt de gebruikersinterface van ASP-VNet-integratie is bereikt, open uw ASP-gebruikersinterface en selecteer **netwerken**.  Selecteer onder de VNet-integratie, **Klik hier om te configureren** openen van de gebruikersinterface voor het netwerk functie Status.

![ASP-VNet-integratie-informatie][5]

De ASP-VNet-integratie-gebruikersinterface ziet u alle van de vnet's die worden gebruikt door de apps in uw ASP. Voor informatie over elk VNet, klikt u op het VNet waarin u geïnteresseerd bent. Er zijn twee acties die u hier kunt uitvoeren.

* **Netwerk synchroniseren**. De synchronisatiebewerking van het netwerk is alleen voor de gateway-functie voor afhankelijke VNet-integratie. Uitvoeren van een synchronisatiebewerking van het netwerk zorgt ervoor dat uw certificaten en netwerkgegevens gesynchroniseerd zijn. Als u toevoegen of wijzigen van de DNS-server van uw VNet, moet u om uit te voeren een **netwerk synchroniseren** bewerking. Met deze bewerking wordt opnieuw opgestart alle apps met behulp van dit VNet.
* **Routes toevoegen** routes toe te voegen de basis van uitgaand verkeer in uw VNet.

**Routering** de routes die zijn gedefinieerd in uw VNet worden gebruikt om verkeer te regelen in uw VNet vanaf uw app. Als u aanvullende uitgaand verkeer verzenden naar het VNet wilt, kunt u deze-adresblokken hier toevoegen. Deze mogelijkheid alleen werkt met gateway vereist een VNet-integratie.

**Certificaten** wanneer de gateway vereist een VNet-integratie is ingeschakeld, er is een vereiste uitwisselen van certificaten om te controleren of de beveiliging van de verbinding. Samen met de certificaten zijn de DNS-configuratie, routes en andere vergelijkbare dingen die worden beschreven van het netwerk.
Als certificaten of netwerkgegevens wordt gewijzigd, moet u klikken op 'Sync netwerk'. Als u 'Sync netwerk' klikt, zorgt u ervoor dat een korte onderbreking in de verbinding tussen uw app en uw VNet. Terwijl uw app wordt niet opnieuw opgestart, het verlies van connectiviteit kan ervoor zorgen dat uw site niet correct. 

## <a name="accessing-on-premises-resources"></a>Toegang tot on-premises bronnen
Apps toegang hebben tot on-premises bronnen door te integreren met VNets met site-naar-site-verbindingen. Als u de gateway vereist een VNet-integratie, moet u uw on-premises VPN-gateway routes bijwerken met de punt-naar-site-adresblokken. Wanneer de site-naar-site VPN-verbinding eerst is ingesteld, moeten de scripts die worden gebruikt om deze te configureren routes correct ingesteld. Als u de punt-naar-site-adressen toevoegen nadat u uw site-naar-site-VPN hebt gemaakt, moet u de routes, handmatig bijwerken. Informatie over hoe u dat doen variëren per gateway en worden hier niet beschreven. U kunt geen BGP is geconfigureerd met een site-naar-site VPN-verbinding.

Er is geen aanvullende configuratie vereist voor de regionale VNet-integratiefunctie te bereiken via uw VNet, en met on-premises. U hoeft uw VNet naar on-premises verbinding met behulp van ExpressRoute of een site-naar-site-VPN. 

> [!NOTE]
> De gateway vereist VNet-integratiefunctie niet geïntegreerd is een app met een VNet met een ExpressRoute-Gateway. Zelfs als de ExpressRoute-Gateway is geconfigureerd [co-existentie modus] [ VPNERCoex] de VNet-integratie werkt niet. Als u nodig hebt voor toegang tot resources via een ExpressRoute-verbinding, kunt u de regionale VNet-integratiefunctie gebruiken of een [App Service-omgeving][ASE], die wordt uitgevoerd in uw VNet. 
> 
> 

## <a name="peering"></a>Peering
Als u met de integratie van regionale VNet-peering gebruikt, hoeft u niet elke extra configuratie. 

Als u de gateway vereist een VNet-integratie met peers, moet u een paar extra items configureren. Het configureren van peering om te werken met uw app:

1. Voeg dat een peeringverbinding op het VNet uw app verbinding maakt met. Bij het toevoegen van de peeringverbinding inschakelen **toestaan van toegang tot het virtuele netwerk** en Controleer **doorgestuurd verkeer toestaan** en **gatewayoverdracht toestaan**.
1. Een peeringverbinding toevoegen op het VNet dat gekoppeld is met het VNet dat u met verbonden bent. Bij het toevoegen van de peeringverbinding op de doel-VNet, inschakelen **toestaan van toegang tot het virtuele netwerk** en Controleer **doorgestuurd verkeer toestaan** en **toestaan van externe gateways**.
1. Ga naar de App Service-plan > netwerken > gebruikersinterface van de VNet-integratie in de portal.  Selecteer het VNet dat uw app maakt verbinding met. Voeg het adresbereik van het VNet dat is gekoppeld aan het VNet dat uw app is verbonden met onder de sectie routering.  


## <a name="pricing-details"></a>Prijsdetails
De regionale VNet-integratie-functie heeft geen extra kosten voor gebruik buiten de ASP prijzen laag kosten in rekening gebracht.

Er zijn drie gerelateerde kosten voor het gebruik van de VNet-integratiefunctie gateway vereist:

* Kosten voor ASP-prijzen laag - uw apps moeten zich in een Standard, Premium of PremiumV2 App Service-Plan. U kunt meer details weergeven op de kosten die u hier: [App Service-prijzen][ASPricing]. 
* Kosten van gegevensoverdracht - er is een kosten in rekening gebracht voor uitgaande gegevens, zelfs als het VNet in hetzelfde Datacenter. Deze kosten worden beschreven in [Data Transfer prijsinformatie over][DataPricing]. 
* Kosten voor VPN-Gateway - er zijn kosten verbonden aan de VNet-gateway die is vereist voor de punt-naar-site VPN-verbinding is. De gegevens zijn op de [prijzen voor VPN-Gateway] [ VNETPricing] pagina.


## <a name="troubleshooting"></a>Problemen oplossen
Wanneer de functie eenvoudig om in te stellen is, betekent dat niet dat uw ervaring probleem gratis zijn. Moet u ondervindt zijn problemen met het openen van uw gewenste eindpunt er enkele hulpprogramma's die u gebruiken kunt voor het testen van de connectiviteit van de app-console. Er zijn twee consoles die u kunt gebruiken. Is de Kudu-console en de andere is de console in de Azure-portal. Als u wilt de Kudu-console vanuit uw app is bereikt, Ga naar Extra -> Kudu. Dit is hetzelfde als het gaat [sitename]. scm.azurewebsites.net. Zodra dat wordt geopend, gaat u naar het foutopsporingstabblad van de console. Ga naar hulpprogramma's om op te halen voor de Azure portal gehoste-console en vervolgens vanuit uw app-Console >. 

#### <a name="tools"></a>Hulpprogramma's
De hulpprogramma's **ping**, **nslookup** en **tracert** via de console vanwege veiligheidsbeperkingen werkt niet. Om in te vullen de void twee afzonderlijke hulpprogramma's toegevoegd. Als u wilt testen DNS-functionaliteit, hebben we een hulpprogramma met de naam nameresolver.exe toegevoegd. De syntaxis is:

    nameresolver.exe hostname [optional: DNS Server]

U kunt **nameresolver** om te controleren of de hostnamen die afhankelijk van uw app. Op deze manier kunt u als u niets verkeerd geconfigureerd met uw DNS of mogelijk geen toegang tot de DNS-server hebt testen. Hier ziet u de DNS-server die uw app wordt gebruikt in de console door te kijken de omgevingsvariabelen WEBSITE_DNS_SERVER en WEBSITE_DNS_ALT_SERVER.

De volgende hulpprogramma kunt u testen voor TCP-verbinding met een combinatie van de host en poort. Dit hulpprogramma wordt aangeroepen **tcpping** en de syntaxis is:

    tcpping.exe hostname [optional: port]

De **tcpping** hulpprogramma geeft u aan als u een specifieke host en poort kunt bereiken. Alleen succes kan weergegeven als: Er is een toepassing die luistert op de combinatie van de host en poort en er is toegang tot het netwerk van uw app naar de opgegeven host en poort.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Toegang tot de VNet-foutopsporing gehoste bronnen
Er zijn een aantal dingen die u kunt voorkomen dat uw app in een specifieke host en poort bereikt. De meeste gevallen is het een van drie dingen:

* **Er is een firewall op de manier.** Als u een firewall op de manier hebt, kunt u de time-out voor TCP wordt bereikt. De TCP-time is in dit geval 21 seconden. Gebruik de **tcpping** hulpprogramma connectiviteit testen. TCP-time-outs kan vanwege een groot aantal dingen voorbij firewalls maar het begin beginnen. 
* **DNS is niet toegankelijk is.** De DNS-out is drie seconden per DNS-server. Als u twee DNS-servers hebt, is de time-out 6 seconden. Gebruik nameresolver om te zien of DNS werkt. Vergeet niet dat je niet nslookup gebruiken als die niet van de uw VNet is geconfigureerd gebruikmaken met DNS-server. Als deze niet toegankelijk is, kan er een firewall of NSG blokkeren van toegang tot de DNS- of deze zijn mogelijk niet beschikbaar.

Als deze items geen antwoord op uw problemen, er eerst voor zaken zoals: 

**regionale VNet-integratie**
* is de bestemming een adres RFC 1918
* is er een NSG geblokkeerd uitgaand verkeer vanuit uw subnet integratie
* Als het gaat via ExpressRoute of een VPN-verbinding, is uw on-premises gateway geconfigureerd routeren van verkeer back-up naar Azure? Als u eindpunten in uw VNet, maar niet on-premises bereiken kunt, is dit goed om te controleren.

**Gateway vereist een VNet-integratie**
* is het punt-naar-site-adresbereik in de RFC 1918 bereiken (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255)?
* Wordt de Gateway weergegeven alsof ze van de portal? Als uw gateway niet actief is, brengt u deze back-up.
* Certificaten worden weergegeven alsof ze synchroon of u vermoedt dat de configuratie van het netwerk is gewijzigd?  Als uw certificaten niet gesynchroniseerd zijn of als u vermoedt dat er is een wijziging aangebracht in uw VNet-configuratie die niet is gesynchroniseerd met uw ASP's, drukt u op 'Sync netwerk'.
* Als het gaat via ExpressRoute of een VPN-verbinding, is uw on-premises gateway geconfigureerd routeren van verkeer back-up naar Azure? Als u eindpunten in uw VNet, maar niet on-premises bereiken kunt, is dit goed om te controleren.

Het is een uitdaging netwerkproblemen foutopsporing, omdat er niet wordt weergegeven wat blokkeert de toegang tot de combinatie van een specifieke host: poort. Enkele van de oorzaken zijn:

* u hebt een firewall van op de host geen toegang tot poort van de toepassing vanaf het punt aan site IP-adresbereik. Vaak overschrijden van subnetten vereist openbare toegang.
* de doelhost is niet actief
* uw toepassing is niet actief
* u hebt het verkeerde IP of hostnaam
* uw toepassing luistert op een andere poort dan wat u verwacht. U kunt uw proces-ID met de poort voor luisteren vergelijken met behulp van 'netstat - aon' op de host van het eindpunt. 
* uw netwerkbeveiligingsgroepen zijn geconfigureerd in zodanig dat ze toegang tot uw toepassingshost en poort van de punt aan site IP-adresbereik voorkomen

Houd er rekening mee dat u weet welk adres de app daadwerkelijk wordt gebruikt niet. Dit kan elk adres in het integratie subnet of een punt-naar-site-adresbereik, worden, zodat u wilt toestaan dat toegang vanaf het gehele adresbereik. 

Extra stappen zijn onder andere:

* verbinding maken met een virtuele machine in uw VNet en probeert te krijgen tot de resource-host: poort van daaruit. Als u wilt testen voor TCP-toegang, gebruik de PowerShell-opdracht **test-netconnection**. De syntaxis is:

      test-netconnection hostname [optional: -Port]

* een toepassing op een virtuele machine en test de toegang op die host en poort van de console van uw app met behulp **tcpping**

#### <a name="on-premises-resources"></a>On-premises bronnen ####

Als uw app niet kan van een resource on-premises bereiken, controleert u of als u de resource van uw VNet kunt bereiken. Gebruik de **test-netconnection** PowerShell-opdracht uit om te controleren op TCP-toegang. Als uw virtuele machine tijdelijk niet bereikbaar voor uw on-premises bron, uw VPN of ExpressRoute-verbinding niet juist geconfigureerd.

Als uw VNet die worden gehost virtuele machine kan bereiken uw on-premises systeem, maar uw app kan niet, wordt de oorzaak is waarschijnlijk een van de volgende redenen:

* de routes zijn niet geconfigureerd met het subnet of verwijzen naar de site-adresbereiken in uw on-premises gateway
* uw netwerkbeveiligingsgroepen worden blokkeert de toegang voor uw punt-naar-Site-IP-adresbereik
* uw on-premises firewall blokkeert verkeer van de punt-naar-Site-IP-adresbereik
* u probeert te bereiken een RFC 1918-adres met behulp van de regionale VNet-integratiefunctie


## <a name="powershell-automation"></a>PowerShell-automatisering

U kunt App Service integreren met een Azure-netwerk met behulp van PowerShell. Zie voor een kant-en-klaar script, [een app in Azure App Service verbinden met een Azure Virtual Network](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).


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
