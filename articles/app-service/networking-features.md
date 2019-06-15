---
title: Netwerkfuncties voor implementatie - Azure App Service | Microsoft Docs
description: Het gebruik van de verschillende App Service functies voor netwerkbeheer
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 5c61eed1-1ad1-4191-9f71-906d610ee5b7
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/28/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 666430a11fb95871eb601b2a38eb7b97ad16119f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66498947"
---
# <a name="app-service-networking-features"></a>App Service-functies voor netwerkbeheer

Toepassingen in Azure App Service kunnen op verschillende manieren worden geïmplementeerd. App Service gehoste apps zijn rechtstreeks toegankelijk is via internet en kunnen alleen bereiken standaard eindpunten internet die worden gehost. Veel toepassingen van de klant moeten echter de binnenkomend en uitgaand netwerkverkeer beheren. Er zijn verschillende functies beschikbaar zijn in de App Service om te voldoen aan deze behoeften. De uitdaging is weten met welke functie moet worden gebruikt om een bepaald probleem te verhelpen. Dit document is bedoeld om te bepalen met welke functie moet worden gebruikt op basis van enkele voorbeelden van use cases klanten te helpen.

Er zijn twee primaire implementatietypen voor de Azure App Service. Er is de openbare service met meerdere tenants, die als host fungeert voor App Service-plannen in de gratis, gedeeld, Basic, Standard, Premium en Premiumv2 SKU's prijzen. Dan is de App Service Environment (ase), die als host fungeert voor geïsoleerde SKU App Service-plannen rechtstreeks in uw Azure Virtual Network (VNet) met één tenant. De functies die u gebruikt varieert op als u zich in de service met meerdere tenants of in een as-omgeving. 

## <a name="multi-tenant-app-service-networking-features"></a>Netwerkfuncties multitenant-App Service 

De Azure App Service is een gedistribueerd systeem. De functies die binnenkomende HTTP/HTTPS-aanvragen worden verwerkt, front-ends worden genoemd. De rollen die als host de workload van een klant fungeren heten werknemers. Alle van de rollen in een App Service-implementatie zich in een netwerk met meerdere tenants. Omdat er veel verschillende klanten in de dezelfde App Service-schaaleenheid zijn, kunt u het App Service-netwerk kan geen verbinding maken rechtstreeks op uw netwerk. In plaats van de netwerken met elkaar verbinden, moet u de functies voor het afhandelen van de verschillende aspecten van de communicatie van toepassingen. De functies voor het verwerken van aanvragen naar uw app kunnen niet worden gebruikt voor het oplossen van problemen bij het aanroepen van uw app. De functies voor het oplossen van problemen voor aanroepen vanuit uw app worden niet op dezelfde manier gebruikt voor het oplossen van problemen aan uw app.  

| Inkomende functies | Uitgaande functies |
|---------------------|-------------------|
| App-adres toegewezen | Hybride verbindingen |
| Toegangsbeperkingen | Gateway vereist een VNet-integratie |
| Service-eindpunten | VNet-integratie (preview) |

Tenzij anders vermeld, kunnen alle functies samen worden gebruikt. U kunt de functies voor uw verschillende problemen oplossen kunt combineren.

## <a name="use-case-and-features"></a>Use-case- en -functies

Voor een bepaalde use-case, kan er een aantal manieren om het probleem te verhelpen.  De juiste functie te gebruiken is het soms vanwege redenen dan alleen de use-case zelf. De volgende inkomende gebruiksvoorbeelden voorstellen over het gebruik van App Service-functies voor netwerkbeheer problemen rondom het beheren van het verkeer naar uw app op te lossen. 
 
| Inkomende use-cases | Functie |
|---------------------|-------------------|
| Ondersteuning voor SSL op basis van IP-behoeften voor uw app | App-adres toegewezen |
| Gedeeld, niet toegewezen inkomende adres voor uw app | App-adres toegewezen |
| Toegang tot uw app uit een reeks goed gedefinieerde adressen beperken | Toegangsbeperkingen |
| Beschikbaar maken van mijn app voor privé-IP's in mijn VNet | ILB AS-OMGEVING </br> Application Gateway met service-eindpunten |
| Toegang tot mijn app beperken van resources in een VNet | Service-eindpunten </br> ILB AS-OMGEVING |
| Beschikbaar maken van mijn app op een privé IP-adres in mijn VNet | ILB AS-OMGEVING </br> privé IP-adres voor inkomend in een toepassingsgateway met service-eindpunten |
| Mijn app met een WAF beveiligen | Application Gateway + ILB as-omgeving </br> Application Gateway met service-eindpunten </br> Azure voordeur met toegangsbeperkingen |
| Verkeer met gelijke taakverdeling naar Mijn apps in verschillende regio 's | Azure voordeur met toegangsbeperkingen | 
| Verkeer met gelijke taakverdeling in dezelfde regio | Application Gateway met service-eindpunten | 

De volgende uitgaande gebruiksvoorbeelden voorstellen hoe u App-Service netwerkfuncties gebruikt om op te lossen uitgaande toegang behoeften voor uw app. 

| Uitgaande use-cases | Functie |
|---------------------|-------------------|
| Toegang tot bronnen in een Azure-netwerk in dezelfde regio | VNet-integratie </br> ASE |
| Toegang tot bronnen in een Azure-netwerk in een andere regio | Gateway vereist een VNet-integratie </br> As-omgeving en VNet-peering |
| Toegang tot resources die zijn beveiligd met service-eindpunten | VNet-integratie </br> ASE |
| Toegang tot bronnen in een particulier netwerk niet is verbonden met Azure | Hybride verbindingen |
| Toegang tot resources via ExpressRoute-circuits | VNet-integratie (beperkt tot RFC 1918 adressen voor deze oefening) </br> ASE | 


### <a name="default-networking-behavior"></a>Standaardgedrag voor netwerken

De Azure App Service-schaaleenheden ondersteuning voor veel klanten in elke implementatie. De abonnementen gratis en gedeelde SKU hosten klantwerkbelastingen voor meerdere tenants werknemers. De Basic en hoger plannen host klantwerkbelastingen die zijn toegewezen aan slechts één App Service-plan (ASP). Als u een Standard-App Service-abonnement hebt, wordt klikt u vervolgens alle apps in het plan uitgevoerd op dezelfde werknemer. Als u de schaal van de werknemer, klikt u vervolgens alle apps in ASP gerepliceerd op een nieuwe werknemer voor elk exemplaar in uw ASP. De werknemers die worden gebruikt voor Premiumv2 verschillen van de werknemers die worden gebruikt voor de andere abonnementen. Elke App Service-implementatie heeft één IP-adres dat wordt gebruikt voor al het inkomende verkeer op de apps in deze App Service-implementatie. Er zijn echter een willekeurige plaats van 4 tot 11 adressen die worden gebruikt voor het aanroepen van uitgaande. Deze adressen worden gedeeld door alle apps in deze App Service-implementatie. De uitgaande adressen anders zijn op basis van de andere werkrol-typen. Dit betekent dat dat de adressen die worden gebruikt door de Free, Shared, Basic, Standard en Premium ASP's anders dan de adressen die worden gebruikt voor uitgaande-aanroepen van de Premiumv2 ASP's zijn. Als u in de eigenschappen voor uw app, ziet u de binnenkomende en uitgaande adressen die worden gebruikt door uw app. Als u vergrendelen van een afhankelijkheid met een IP-ACL wilt, gebruikt u de possibleOutboundAddresses. 

![Eigenschappen van de App](media/networking-features/app-properties.png)

App Service heeft een aantal eindpunten die worden gebruikt voor het beheren van de service.  De adressen die worden gepubliceerd in een afzonderlijk document en zijn ook in de servicetag AppServiceManagement IP. De tag AppServiceManagement wordt alleen gebruikt met een App Service Environment (ASE) waar u wilt toestaan dat dit verkeer. De App Service inkomende adressen in de servicetag AppService IP bijgehouden. Er is geen IP-servicetag dat de uitgaande adressen die worden gebruikt door App Service bevat. 

![Diagram voor het binnenkomende en uitgaande van App Service](media/networking-features/default-behavior.png)

### <a name="app-assigned-address"></a>App-adres toegewezen 

De functie voor app toegewezen adres is een offshoot van de mogelijkheid IP gebaseerde SSL en door het instellen van SSL met uw app wordt geopend. Deze functie kan worden gebruikt voor IP-gebaseerd SSL-aanroepen, maar kan ook worden gebruikt zodat uw app in een adres dat alleen heeft. 

![App toegewezen adres diagram](media/networking-features/app-assigned-address.png)

Wanneer u een app-adres toegewezen, wordt uw verkeer nog steeds wordt gerouteerd via dezelfde front-rollen die al het binnenkomende verkeer in de App Service-schaaleenheid verwerkt. Het adres dat is toegewezen aan uw app, wordt alleen gebruikt door uw app. De use cases voor deze functie zijn:

* Ondersteuning voor SSL op basis van IP-behoeften voor uw app
* Instellen van een toegewezen adres voor uw app, die niet wordt gedeeld met iets anders

U kunt informatie over het instellen van een adres van uw app met de zelfstudie op [SSL op basis van IP-configuratie][appassignedaddress]. 

### <a name="access-restrictions"></a>Toegangsbeperkingen 

De beperkingen voor Computertoegang mogelijkheid kunt u filteren **inkomende** aanvragen op basis van het oorspronkelijke IP-adres. De filters gebruiken om actie vindt plaats op de front-rollen die zijn upstream van de worker-rollen waarbij uw apps worden uitgevoerd. Aangezien de front-rollen upstream van de werknemers, kan de mogelijkheid toegangsbeperkingen worden beschouwd als beveiliging op netwerkniveau voor uw apps. De functie kunt u een lijst toestaan en weigeren-adresblokken die in volgorde van prioriteit worden geëvalueerd. Het is vergelijkbaar met de Netwerkbeveiligingsgroep (NSG)-functie die deel uitmaakt van Azure Networking.  U kunt deze functie kunt gebruiken in een as-omgeving of in de service met meerdere tenants. Gebruikt in combinatie met een ILB as-omgeving, kunt u de toegang beperken van privé-adresblokken.

![Toegangsbeperkingen](media/networking-features/access-restrictions.png)

De functie toegangsbeperkingen helpt in scenario's waarin u beperken van de IP-adressen die kunnen worden gebruikt wilt om uw app bereiken. Aanvragen voor deze functie zijn onder andere het gebruik:

* Toegang tot uw app uit een reeks goed gedefinieerde adressen beperken 
* Toegang tot het afkomstig is van een service-taakverdeling, zoals Azure voordeur beperken. Als u uw binnenkomend verkeer naar Azure voordeur wilt, maakt u regels voor het toestaan van verkeer van 147.243.0.0/16 en 2a01:111:2050:: / 44. 

![Beperkingen voor Computertoegang met de voordeur](media/networking-features/access-restrictions-afd.png)

Als u wilt vergrendelen van toegang tot uw app zodat deze alleen kan worden bereikt vanaf de resources in uw Azure Virtual Network (VNet), moet u een statisch openbaar adres op ongeacht de bron in uw VNet is. Als de resources die geen een openbaar adres heeft, moet u de functie voor Service-eindpunten in plaats daarvan gebruiken. Meer informatie over het inschakelen van deze functie met de zelfstudie op [toegangsbeperkingen configureren][iprestrictions].

### <a name="service-endpoints"></a>Service-eindpunten

Service-eindpunten kunt u vergrendelen **inkomende** toegang tot uw app zodat de bron-adres moet afkomstig zijn van een set van subnetten die u selecteert. Deze functie werkt in combinatie met de IP-toegangsbeperkingen. Service-eindpunten worden ingesteld in dezelfde gebruikerservaring als de IP-toegangsbeperkingen. U kunt een toestaan/weigeren lijst met regels voor toegang met openbare adressen, evenals subnetten in uw vnet's bouwen. Deze functie ondersteunt scenario's zoals:

![Service-eindpunten](media/networking-features/service-endpoints.png)

* Instellen van een toepassingsgateway met uw app voor binnenkomend verkeer naar uw app
* Testricting toegang tot uw app naar resources in uw VNet. Dit kan zijn virtuele machines, as-omgevingen of zelfs andere apps die gebruikmaken van VNet-integratie 

![Service-eindpunten met application gateway](media/networking-features/service-endpoints-appgw.png)

U kunt meer informatie over het configureren van service-eindpunten met uw app in de zelfstudie op [toegangsbeperkingen van de Service-eindpunt configureren][serviceendpoints]
 
### <a name="hybrid-connections"></a>Hybride verbindingen

App Service Hybrid Connections kunt u uw apps die moeten worden **uitgaande** aanroepen naar de opgegeven TCP-eindpunten. Het eindpunt kan on-premises of in een VNet of een willekeurige plaats waarmee uitgaand verkeer naar Azure op poort 443. De functie vereist de installatie van een relay-agent de Hybrid Connection Manager (HCM) op een Windows Server 2012 of nieuwer host genoemd. De HCM moet kunnen bereiken van Azure Relay op poort 443. De HCM kan worden gedownload vanuit de gebruikersinterface van App Service hybride verbindingen in de portal. 

![Netwerkstroom voor hybride verbindingen](media/networking-features/hybrid-connections.png)

De functie hybride verbindingen van App Service is gebouwd op de mogelijkheid hybride Azure Relay-verbindingen. App Service maakt gebruik van een speciale vorm van de functie die alleen ondersteuning biedt voor die uitgaande oproepen vanuit uw app naar een TCP-host en poort. Deze host en poort alleen moeten oplossen op de host waarop de HCM is geïnstalleerd. Wanneer de app in App Service, een DNS-zoekopdracht op de host en de poort die in uw hybride verbinding zijn gedefinieerd wordt, wordt het verkeer automatisch omgeleid om te gaan via de hybride verbinding en van de Hybrid Connection Manager. Lees voor meer informatie over hybride verbindingen, de documentatie op [hybride verbindingen van App Service][hybridconn]

Deze functie wordt vaak gebruikt om:

* Toegang tot bronnen in particuliere netwerken die niet zijn verbonden met Azure met een VPN of ExpressRoute
* Ondersteuning voor lift- and -shift van on-premises toepassingen naar App Service zonder ook ondersteunende om databases te verplaatsen  
* Veilig toegang verlenen tot een afzonderlijke host en poort per hybride verbinding. De meeste netwerkfuncties open toegang tot een netwerk en met hybride verbindingen alleen hebt u het één host en de poort die u kunt bereiken.
* Scenario's niet wordt gedekt door andere methoden uitgaande connectiviteit
* Ontwikkeling in App Service waar de apps kunnen eenvoudig gebruikmaken van on-premises bronnen uitvoeren 

Omdat de functie kunt toegang tot on-premises bronnen zonder een gat inkomende firewall, is het populaire met ontwikkelaars. De andere uitgaande App Service netwerkfuncties zijn zeer Azure virtuele netwerken die betrekking hebben. Hybride verbindingen heeft geen een afhankelijkheid van een VNet te doorlopen en kan worden gebruikt voor een groter aantal netwerken behoeften. Het is belangrijk te weten dat de App Service Hybrid Connections-functie niet van belang of weten wat u doet boven op het. Dat wil zeggen dat u deze kunt gebruiken voor toegang tot een database, een webservice of een willekeurige TCP-socket op een mainframe is. De functie tunnels in feite TCP-pakketten. 

Hybride verbindingen wordt gebruikt voor ontwikkeling, wordt maar ook gebruikt in talloze productietoepassingen ook. Het is erg handig voor het openen van een webservice of de database, maar is niet geschikt voor situaties met betrekking tot een geweldige veel verbindingen worden gemaakt. 

### <a name="gateway-required-vnet-integration"></a>Gateway vereist een VNet-integratie 

De gateway vereist App Service-VNet-integratie-functie kunnen uw app **uitgaande** aanvragen in een Azure-netwerk. De functie werkt door de host die uw app wordt uitgevoerd op een virtuele netwerkgateway voor uw VNet met een punt-naar-site-VPN verbinding te maken. Wanneer u de functie configureert, krijgt uw app in een van de punt-naar-site-adressen toegewezen aan elke instantie. Deze functie kunt u toegang tot bronnen in het klassieke of Resource Manager VNets in elke regio. 

![Gateway vereist een VNet-integratie](media/networking-features/gw-vnet-integration.png)

Deze functie is het probleem van toegang tot resources in andere VNets opgelost en kan ook worden gebruikt om verbinding maken via een VNet naar andere VNets of zelfs on-premises. Werkt niet met ExpressRoute, verbonden VNets maar matcht met Site-naar-site VPN verbonden netwerken. Het is doorgaans niet geschikt is voor het gebruik van deze functie van een app in een App Service Environment (ASE) omdat de as-omgeving al in uw VNet is. De use cases die deze functie is opgelost zijn:

* Toegang tot bronnen op privé-IP's in uw Azure-netwerken 
* Toegang tot bronnen ter plaatse als er een site-naar-site-VPN is 
* Toegang tot bronnen in gekoppelde VNets 

Wanneer deze functie is ingeschakeld, wordt de DNS-server die de bestemming VNet is geconfigureerd met gebruik van uw app. U kunt meer lezen over deze functie in de documentatie op [App Service-VNet-integratie][vnetintegrationp2s]. 

### <a name="vnet-integration"></a>VNet-integratie

De gateway vereist onderdeel van de VNet-integratie is zeer nuttig, maar nog steeds niet kunt oplossen met toegang tot resources via ExpressRoute. Boven op hoeven om via ExpressRoute-verbindingen te bereiken, is het een nodig om apps te kunnen aanroepen naar service-eindpunt beveiligde services. Om op te lossen beide van deze extra behoeften, is nog een VNet-integratie-mogelijkheid toegevoegd. De nieuwe functie voor VNet-integratie kunt u de back-end van uw app in een subnet in een Resource Manager VNet in dezelfde regio te plaatsen. Deze functie is niet beschikbaar is via een App Service-omgeving, die zich al in een VNet. Met deze functie wordt ingeschakeld:

* Toegang tot bronnen in Resource Manager VNets in dezelfde regio
* Toegang tot resources die zijn beveiligd met service-eindpunten 
* Toegang tot bronnen die toegankelijk via ExpressRoute of VPN-verbindingen zijn

![VNet-integratie](media/networking-features/vnet-integration.png)

Deze functie is beschikbaar als preview en mag niet worden gebruikt voor werkbelastingen voor productie. Voor meer informatie over deze functie kunt u de documenten lezen op [App Service-VNet-integratie][vnetintegration].

## <a name="app-service-environment"></a>App Service-omgeving 

Een App Service Environment (ASE) is een implementatie met één tenant van de Azure App Service die wordt uitgevoerd in uw VNet. De as-omgeving kunt gebruiksvoorbeelden zoals:

* Toegang tot bronnen in uw VNet
* Toegang tot resources via ExpressRoute
* Beschikbaar maken van uw apps met een privé-adres in uw VNet 
* Toegang tot resources via de service-eindpunten 

Met een as-omgeving hoeft u niet het gebruik van functies zoals VNet-integratie of service-eindpunten, omdat de as-omgeving al in uw VNet is. Als u wilt toegang krijgen tot bronnen zoals SQL of opslag via service-eindpunten, service-eindpunten op het ASE-subnet inschakelen. Als u wilt toegang krijgen tot bronnen in het VNet, is er geen aanvullende configuratie vereist.  Als u wilt toegang krijgen tot bronnen via ExpressRoute, kunt u zich al in het VNet en geen hoeft te configureren op de as-omgeving of de apps in het. 

Omdat de apps in een ILB as-omgeving kunnen worden weergegeven op een privé IP-adres, kunt u eenvoudig WAF-apparaten als u alleen de apps dat u wilt dat met het internet en de rest te beveiligen beschikbaar wilt toevoegen. Het plan zich eenvoudige ontwikkeling van toepassingen met meerdere lagen. 

Er zijn enkele dingen die nog niet mogelijk in de service met meerdere tenants die afkomstig van een as-omgeving zijn. Deze omvatten zaken zoals:

* Uw apps op een privé IP-adres weergeven
* Beveiligen van al het uitgaande verkeer met ook netwerkbeheervoorzieningen die geen deel van uw app uitmaken 
* Uw apps hosten in een service met één tenant 
* Kan worden uitgebreid naar veel meer exemplaren dan mogelijk zijn in de service met meerdere tenants 
* Persoonlijke certificaten van CA-client voor gebruik door uw apps laden met persoonlijke Certificeringsinstantie beveiligde eindpunten 
* TLS 1.1 afdwingen voor alle van de apps die worden gehost in het systeem zonder een mogelijkheid om uit te schakelen op het niveau van de app 
* Geef een toegewezen uitgaande adres voor alle apps in de as-omgeving die niet wordt gedeeld met klanten 

![As-omgeving in een VNet](media/networking-features/app-service-environment.png)

De as-omgeving biedt de beste manier om geïsoleerde en toegewezen app-hosting, maar wordt geleverd met enkele uitdagingen management. Er zijn enkele aandachtspunten voor voordat u een operationele as-omgeving:
 
 * Een as-omgeving wordt uitgevoerd in uw VNet, maar zijn geen afhankelijkheden buiten het VNet. Deze afhankelijkheden moeten worden toegestaan. Meer informatie in [netwerkoverwegingen voor een App Service Environment][networkinfo]
 * Een as-omgeving heeft niet onmiddellijk schalen als de service met meerdere tenants. U hoeft te anticiperen op behoeften schalen, in plaats van reactief schalen. 
 * Een as-omgeving heeft een hogere een kosten die zijn gekoppeld aan deze. Om optimaal gebruikmaken van de as-omgeving, u moet van plan bent veel werkbelastingen in een as-omgeving te brengen in plaats van deze gebruikt voor kleine inspanningen
 * De apps in een as-omgeving kunnen geen toegang beperken tot bepaalde apps in een as-omgeving en andere niet.
 * De as-omgeving is in een subnet en een netwerk van de regels van toepassing op alle verkeer van en naar deze as-omgeving. Als u toewijzen van regels voor binnenkomend verkeer voor slechts één app wilt, gebruikt u toegangsbeperkingen. 

## <a name="combining-features"></a>Functies combineren 

De functies die u hebt genoteerd voor de service met meerdere tenants kunnen samen worden gebruikt om op te lossen uitgebreidere gebruiksvoorbeelden. Twee van de meest voorkomende gebruiksvoorbeelden worden hier beschreven, maar ze zijn slechts voorbeelden. Door te begrijpen wat de verschillende functies doen, kunt u bijna alle van de behoeften van uw systeem architectuur oplossen.

### <a name="inject-app-into-a-vnet"></a>App invoeren in een VNet

Er is een algemene aanvraag over het plaatsen van uw app in een VNet. Als uw app in een VNet, betekent dat de binnenkomende en uitgaande-eindpunten voor een app binnen een VNet zijn. De as-omgeving biedt de beste oplossing is om op te lossen dit probleem, maar krijgt u de meeste van wat u nodig hebt met in de service met meerdere tenants door een combinatie van functies. U kunt bijvoorbeeld alleen intranettoepassingen kunt maken met particuliere binnenkomende en uitgaande adressen door te hosten:

* Het maken van een toepassingsgateway met privé-adres voor binnenkomend en uitgaand
* Binnenkomend verkeer naar uw app service-eindpunten beveiligen 
* De nieuwe VNet-integratie gebruiken om de back-end van uw app in uw VNet 

Deze stijl implementatie zou geen bieden u een toegewezen adres voor uitgaand verkeer naar internet en bieden u de mogelijkheid om al het uitgaande verkeer vanuit uw app.  De stijl van deze implementatie, krijgt u een zeer van wat u zou alleen anders met een as-omgeving. 

### <a name="create-multi-tier-applications"></a>Toepassingen met meerdere lagen maken

Een toepassing met meerdere lagen is een toepassing waarbij de back-end API apps alleen toegankelijk vanuit de front-endlaag. Voor het maken van een toepassing met meerdere lagen, kunt u het volgende doen:

* VNet-integratie gebruiken om de back-end van uw front-end web-app verbinding met een subnet in een VNet te maken
* Service-eindpunten gebruikt voor het beveiligen van inkomend verkeer naar uw API-app voor alleen die afkomstig zijn van het subnet dat wordt gebruikt door uw front-end web-app

![app met meerdere lagen](media/networking-features/multi-tier-app.png)

U kunt meerdere front-apps gebruik van dezelfde API-app met behulp van VNet-integratie van de andere front-apps en service-eindpunten van de API-app met hun subnetten hebben.  

<!--Links-->
[appassignedaddress]: https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl#bind-your-ssl-certificate
[iprestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[serviceendpoints]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[hybridconn]: https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections
[vnetintegrationp2s]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[vnetintegration]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[networkinfo]: https://docs.microsoft.com/azure/app-service/environment/network-info
