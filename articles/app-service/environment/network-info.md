---
title: Aandachtspunten voor netwerken met een Azure App Service-omgeving
description: Legt uit het netwerkverkeer van de as-omgeving en het nsg's en udr's met uw as-omgeving instellen
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ccompy
ms.openlocfilehash: 3ac630982b47f7105feb034982eae070faa72d9e
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="networking-considerations-for-an-app-service-environment"></a>Overwegingen voor een App Service-omgeving netwerken #

## <a name="overview"></a>Overzicht ##

 Azure [App Service-omgeving] [ Intro] een Azure App Service-implementatie in een subnet in uw Azure virtual network (VNet). Er zijn twee implementatietypen voor een App Service-omgeving (as-omgeving):

- **Externe as-omgeving**: beschrijft de as-omgeving gehoste apps op internet toegankelijke IP-adres. Zie voor meer informatie [maken van een externe as-omgeving][MakeExternalASE].
- **As-ILB omgeving**: beschrijft de as-omgeving gehoste apps op een IP-adres binnen uw VNet. Het interne eindpunt, is een interne load balancer (ILB) dat is waarom een ILB-as-omgeving is aangeroepen. Zie voor meer informatie [maken en gebruiken een ILB-as-omgeving][MakeILBASE].

Er zijn nu twee versies van App Service-omgeving: ASEv1 en ASEv2. Zie voor informatie over ASEv1, [Inleiding tot de App Service-omgeving v1][ASEv1Intro]. ASEv1 kan worden geïmplementeerd in een klassiek of Resource Manager VNet. ASEv2 kan alleen worden geïmplementeerd in een Resource Manager VNet.

Alle aanroepen van een as-omgeving gaat u naar het internet laat het VNet via een VIP-adres toegewezen voor de as-omgeving. Het openbare IP-adres van deze VIP wordt vervolgens het bron-IP voor alle aanroepen van de as-omgeving die gaat u naar het internet. Als de apps in uw as-omgeving aanroepen naar resources in uw VNet of via een VPN, wordt de bron-IP is een van de IP-adressen in het subnet dat wordt gebruikt door uw as-omgeving. Omdat de as-omgeving binnen het VNet is, kan het ook toegang tot bronnen binnen het VNet zonder extra configuratie. Als het VNet is verbonden met uw on-premises netwerk, hebben-apps in uw as-omgeving ook toegang tot bronnen er. U hoeft niet te configureren van de as-omgeving of uw app ieder verder.

![Externe as-omgeving][1] 

Als u een externe as-omgeving hebt, wordt het openbare VIP is ook het eindpunt dat uw apps as-omgeving worden omgezet naar voor:

* HTTP/S. 
* FTP/S. 
* Web-implementatie.
* Foutopsporing op afstand.

![ILB AS-OMGEVING][2]

Als u een ILB-as-omgeving hebt, is het IP-adres van de ILB het eindpunt voor HTTP/S, FTP-/ S, web-implementatie en foutopsporing op afstand.

De normale app-poorten zijn:

| Gebruiken | Van | Handeling |
|----------|---------|-------------|
|  HTTP/HTTPS  | Gebruiker worden geconfigureerd |  80, 443 |
|  FTP/FTPS    | Gebruiker worden geconfigureerd |  21, 990, 10001-10020 |
|  Visual Studio foutopsporing op afstand  |  Gebruiker worden geconfigureerd |  4016, 4018, 4020, 4022 |

Dit geldt als u op een externe as-omgeving of op een as ILB-omgeving. Als u op een externe as-omgeving, heeft deze poorten op het openbare VIP. Als u van een as ILB-omgeving gebruikmaakt, heeft deze poorten op de ILB. Als u poort 443 vergrendelen, kunnen er gevolgen heeft voor sommige functies die worden weergegeven in de portal. Zie voor meer informatie [Portal afhankelijkheden](#portaldep).

## <a name="ase-dependencies"></a>Afhankelijkheden van de as-omgeving ##

Een as-omgeving binnenkomende-afhankelijkheid is:

| Gebruiken | Van | Handeling |
|-----|------|----|
| Beheer | App Service management-adressen | As-omgeving subnet: 454, 455 |
|  Interne communicatie as-omgeving | As-omgeving subnet: alle poorten | As-omgeving subnet: alle poorten
|  Azure load balancer toestaan binnenkomende | Azure load balancer | As-omgeving subnet: alle poorten
|  App toegewezen IP-adressen | App toegewezen adressen | As-omgeving subnet: alle poorten

Het binnenkomende verkeer biedt opdracht en controle van de as-omgeving naast het controleren van het systeem. De bron-IP-adressen voor dit verkeer worden vermeld in de [as-omgeving Management adressen] [ ASEManagement] document. De netwerkconfiguratie van de beveiliging moet toegang vanaf alle IP-adressen op de poorten 454 en 455 toestaan.

Binnen het as-omgeving subnet er veel poorten gebruikt voor interne onderdeel zijn kunnen communicatie en ze wijzigen.  Hiervoor moet alle poorten in het subnet van de as-omgeving voor toegang vanuit het subnet van de as-omgeving. 

Voor de communicatie tussen de Azure load balancer en het subnet van de as-omgeving zijn de poorten die moeten worden geopend minimaal 454 en 455 van 16001. De 16001 poort wordt gebruikt voor keep alive verkeer tussen de load balancer en het as-omgeving. Als u van een as ILB-omgeving gebruikmaakt is, kunt u verkeer naar beneden op alleen de 454, 455, 16001 vergrendelen poorten.  Als u van een externe as-omgeving gebruikmaakt moet u rekening houden met het normale app-poorten.  Als u van app toegewezen adressen gebruikmaakt moet u deze naar alle poorten openen.  Als u een adres is toegewezen aan een specifieke app, gebruikt de load balancer-poorten die niet bekend zijn van vooraf HTTP en HTTPS-verkeer verzenden naar de as-omgeving.

Als u app toegewezen IP-adressen die u wilt toestaan dat verkeer van de IP-adressen toegewezen aan uw apps op het subnet van de as-omgeving.

Voor uitgaande toegang is een as-omgeving afhankelijk van meerdere externe systemen. Die afhankelijkheden systeem zijn gedefinieerd met behulp van DNS-namen en niet toewijzen aan een vaste set van IP-adressen. De as-omgeving is dus uitgaand verkeer van het subnet van de as-omgeving op alle externe IP-adressen in een groot aantal poorten vereist. Een as-omgeving heeft de volgende uitgaande afhankelijkheden:

| Gebruiken | Van | Handeling |
|-----|------|----|
| Azure Storage | As-omgeving subnet | Table.Core.Windows.NET, blob.core.windows.net, queue.core.windows.net, file.core.windows.net: 80, 443, 445 (445 is alleen nodig voor ASEv1.) |
| Azure SQL Database | As-omgeving subnet | database.Windows.NET: 1433, 11000 11999, 14000 14999 (Zie voor meer informatie [SQL Database V12 Poortgebruik](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).)|
| Azure management | As-omgeving subnet | Management.Core.Windows.NET, management.azure.com: 443 
| Verificatie van SSL-certificaat |  As-omgeving subnet            |  OCSP.msocsp.com, mscrl.microsoft.com, crl.microsoft.com: 443
| Azure Active Directory        | As-omgeving subnet            |  Internet: 443
| App Service-beheer        | As-omgeving subnet            |  Internet: 443
| Azure DNS                     | As-omgeving subnet            |  Internet: 53
| Interne communicatie as-omgeving    | As-omgeving subnet: alle poorten |  As-omgeving subnet: alle poorten

Als de as-omgeving verliest toegang tot deze afhankelijkheden, werkt niet. Wanneer dit gebeurt lang genoeg hebt geklikt, wordt de as-omgeving is onderbroken.

### <a name="customer-dns"></a>Klant DNS ###

Als het VNet is geconfigureerd met een klant gedefinieerde DNS-server, wordt het door de werkbelasting van de tenant gebruikt. De as-omgeving moet nog steeds communiceren met de Azure DNS voor beheerdoeleinden. 

Als het VNet is geconfigureerd met een klant DNS op de andere zijde van een VPN, moet de DNS-server bereikbaar is vanuit het subnet waarin de as-omgeving zijn.

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portal-afhankelijkheden ##

Naast de functionele afhankelijkheden van de as-omgeving zijn er enkele extra artikelen die betrekking hebben op de ervaring van de portal. Enkele van de functies in de Azure-portal afhankelijk zijn van directe toegang tot _SCM site_. Er zijn twee URL's voor elke app in Azure App Service. De eerste URL is toegang tot uw app. De URL van de tweede is voor toegang tot de site SCM, ook wel genoemd de _Kudu-console_. Functies die gebruikmaken van de site SCM:

-   Webtaken
-   Functies
-   Logboekstreaming
-   Kudu
-   Extensies
-   Procesverkenner
-   Console

Wanneer u een ILB-as-omgeving, wordt in de SCM-site internet toegankelijk van buiten het VNet niet. Wanneer uw app wordt gehost op een as ILB-omgeving, werken bepaalde functies niet vanuit de portal.  

Veel van deze mogelijkheden die afhankelijk van de site SCM zijn zijn eveneens beschikbaar rechtstreeks in de Kudu-console. U kunt verbinding maken voor haar rechtstreeks in plaats van via de portal. Als uw app wordt gehost in een ILB-as-omgeving, gebruikt u de referenties van uw publishing aan te melden. De URL voor toegang tot de SCM-site van een app die wordt gehost in een ILB-as-omgeving heeft de volgende indeling: 

```
<appname>.scm.<domain name the ILB ASE was created with> 
```

Als u uw as ILB-omgeving is de domeinnaam *contoso.net* en de naam van uw app *testapp*, de app is bereikt op *testapp.contoso.net*. De SCM-site die u ermee gaat is bereikt op *testapp.scm.contoso.net*.

## <a name="functions-and-web-jobs"></a>Functies en Web-taken ##

Zowel functies en Web taken afhankelijk zijn van de site SCM maar worden ondersteund voor gebruik in de portal, zelfs als uw apps in een ILB as-omgeving, zolang uw browser de SCM-site kan bereiken.  Als u van een zelfondertekend certificaat met de as ILB-omgeving gebruikmaakt, moet u uw browser te vertrouwen dat certificaat inschakelen.  Voor Internet Explorer en Edge betekent het certificaat dat moet worden in het computerarchief van de vertrouwensrelatie.  Als u Chrome vervolgens dit betekent dat u het certificaat in de browser eerder geaccepteerd dat door waarschijnlijk rechtstreeks roept de scm-site.  De beste oplossing is het gebruik van een commerciële certificaat dat is in de vertrouwensketen browser.  

## <a name="ase-ip-addresses"></a>As-omgeving IP-adressen ##

Een as-omgeving heeft enkele IP-adressen moet houden. Ze zijn:

- **Openbare binnenkomende IP-adres**: gebruikt voor het verkeer van de app in een externe as-omgeving en beheer van verkeer in zowel een externe as-omgeving en een ILB-as-omgeving.
- **Uitgaande openbare IP-adres**: gebruikt als het 'van' IP-adres voor uitgaande verbindingen van de as-omgeving die laat het VNet, die niet worden doorgestuurd naar beneden een VPN.
- **ILB IP-adres**: als u een ILB-as-omgeving.
- **App toegewezen SSL op basis van IP-adressen**: alleen mogelijk met een externe as-omgeving en het IP-gebaseerde SSL is geconfigureerd.

Deze IP-adressen zijn gemakkelijk zichtbaar zijn in een ASEv2 in de Azure portal van de gebruikersinterface van de as-omgeving. Als u een ILB-as-omgeving hebt, wordt het IP-adres voor de ILB vermeld.

![IP-adressen][3]

### <a name="app-assigned-ip-addresses"></a>App-toegewezen IP-adressen ###

Met een externe as-omgeving, kunt u IP-adressen toewijzen aan afzonderlijke apps. U kunt geen dat doen met een ILB-as-omgeving. Zie voor meer informatie over het configureren van uw app om de eigen IP-adres hebben [een bestaande aangepaste SSL-certificaat binden aan Azure-web-apps](../app-service-web-tutorial-custom-ssl.md).

Wanneer een app zijn eigen SSL op basis van IP-adres heeft, behoudt de as-omgeving zich twee poorten toewijzen aan dat IP-adres. Een poort is voor de HTTP-verkeer en de andere poort is voor HTTPS. Deze poorten worden vermeld in de gebruikersinterface van de as-omgeving in het gedeelte van de IP-adressen. Verkeer moet kunnen deze poorten van de VIP bereiken of de apps niet toegankelijk zijn. Deze vereiste is belangrijk te weten bij het configureren van Netwerkbeveiligingsgroepen (nsg's).

## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen ##

[Netwerkbeveiligingsgroepen] [ NSGs] bieden de mogelijkheid om te beheren van toegang tot het netwerk binnen een VNet. Wanneer u de portal gebruikt, moet u er een regel voor weigeren impliciete is op de laagste prioriteit voor het weigeren van alles. Wat u bouwen zijn de regels voor toestaan.

In een as-omgeving hebt u geen toegang tot de virtuele machines gebruikt als host voor de as-omgeving zelf. Ze zich in een abonnement door Microsoft beheerd. Als u wilt toegang tot de apps op de as-omgeving te beperken, stelt u nsg's op het subnet van de as-omgeving. Hierbij moet u zorgvuldig aandacht schenken aan de afhankelijkheden van de as-omgeving. Als u eventuele afhankelijkheden blokkeert, werkt niet meer de as-omgeving.

Nsg's kunnen worden geconfigureerd via de Azure portal of via PowerShell. De informatie hier ziet u de Azure-portal. U maakt en nsg's in de portal beheren als een bron op het hoogste niveau onder **Networking**.

Wanneer u de vereisten voor binnenkomend en uitgaand in aanmerking worden genomen, het nsg's zijn vergelijkbaar met het nsg's in dit voorbeeld wordt getoond. Het adresbereik van het VNet is _192.168.250.0/16_, en het subnet dat u het as-omgeving in _192.168.251.128/25_.

De eerste twee inkomende vereisten voor de as-omgeving naar de functie worden aan de bovenkant van de lijst in dit voorbeeld weergegeven. Deze as-omgeving beheer inschakelen en de as-omgeving om te communiceren met zichzelf toestaan. De andere vermeldingen zijn alle tenant kunnen worden geconfigureerd en toegang tot het netwerk naar de as-omgeving gehoste toepassingen kunnen bepalen. 

![Inkomende beveiligingsregels][4]

Een standaardregel kunt de IP-adressen in het VNet om te communiceren met het subnet van de as-omgeving. Een andere standaardregel kunnen de load balancer, ook wel bekend als het openbare VIP, om te communiceren met de as-omgeving. Overzicht van de standaardregels Selecteer **regels standaard** naast de **toevoegen** pictogram. Als u al het andere nadat de NSG regels weergegeven regel weigeren plaatst, voorkomt u verkeer tussen het VIP en de as-omgeving. Om te voorkomen dat verkeer dat afkomstig is van binnen het VNet, toevoegen inkomende van uw eigen regel om toe te staan. Een bron die gelijk is aan AzureLoadBalancer gebruiken met als bestemming **eventuele** en een poortbereik van  **\*** . Omdat de regel van het NSG wordt toegepast op het subnet van de as-omgeving, hoeft u niet te worden specifieke in het doel.

Als u een IP-adres aan uw app toegewezen, zorg er dan voor dat u de poorten open houden. Selecteer de poorten vindt **App Service-omgeving** > **IP-adressen**.  

Alle items weergegeven in de volgende uitgaande regels nodig zijn, met uitzondering van het laatste item. Hiermee kunt netwerktoegang tot de as-omgeving afhankelijkheden die eerder in dit artikel zijn vermeld. Als u een van deze blokkeert, werkt niet meer uw as-omgeving. Het laatste item in de lijst kunt uw as-omgeving om te communiceren met andere resources in uw VNet.

![Uitgaande beveiligingsregels][5]

Nadat uw nsg's zijn gedefinieerd, kunt u ze aan het subnet dat u uw as-omgeving op toewijzen. Als je het as-omgeving VNet of een subnet, kunt u het bekijken van de as-omgeving portal-pagina. Het NSG toewijzen aan uw subnet, gaat u naar de gebruikersinterface van het subnet en het NSG selecteren.

## <a name="routes"></a>Routes ##

Routes worden meest problematisch wanneer u uw VNet met Azure ExpressRoute configureren. Er zijn drie soorten routes in een VNet:

-   Systeemroutes
-   BGP-routes
-   Gebruiker gedefinieerde routes (udr's)

BGP-routes overschrijven systeemroutes. Udr's overschrijven BGP-routes. Zie voor meer informatie over routes in virtuele netwerken van Azure [overzicht van de gebruiker gedefinieerde routes][UDRs].

De Azure SQL-database die gebruikmaakt van de as-omgeving voor het beheren van het systeem is een firewall. Is er communicatie afkomstig te zijn van het openbare VIP as-omgeving. Verbindingen met de SQL-database van de as-omgeving wordt geweigerd als deze onder de ExpressRoute-verbinding en u een ander IP-adres worden verzonden.

Als antwoorden op binnenkomende management-aanvragen worden verzonden naar beneden de ExpressRoute, is het antwoordadres anders dan de oorspronkelijke doelhost. Dit verschil verbreekt de TCP-communicatie.

Voor de as werken terwijl uw VNet is geconfigureerd met een ExpressRoute-omgeving, is het eenvoudigste wat te doen:

-   Configureren van ExpressRoute adverteren _0.0.0.0/0_. Standaard deze geforceerd tunnels alle uitgaande verkeer van de lokale.
-   Maak een UDR. Toepassen op het subnet waarin de as-omgeving met een adresvoorvoegsel van _0.0.0.0/0_ en type van een volgende hop _Internet_.

Als u deze twee wijzigingen aanbrengt, wordt niet internet bestemd verkeer dat afkomstig van het subnet van de as-omgeving is uitgevoerd op de ExpressRoute- en de as-omgeving werkt. 

> [!IMPORTANT]
> De routes die zijn gedefinieerd in een UDR moet specifieke voorrang op alle routes die worden geadverteerd door de ExpressRoute-configuratie. Het adresbereik brede 0.0.0.0/0 maakt gebruik van het vorige voorbeeld. Mogelijk kan worden per ongeluk overschreven door de route-advertisements met meer specifieke adresbereiken.
>
> ASEs worden niet ondersteund met ExpressRoute-configuraties die cross routes van het openbare peering pad naar het pad voor persoonlijke peering adverteert. ExpressRoute-configuraties met openbare peering geconfigureerd ontvangen route-advertisements van Microsoft. De aankondigingen bevatten een groot aantal Microsoft Azure-IP-adresbereiken. Als de adresbereiken cross aangekondigd op het pad voor persoonlijke peering, zijn alle uitgaande pakketten van subnet van de as-omgeving force tunneled aan een klant on-premises netwerkinfrastructuur. Deze stroom netwerk wordt momenteel niet ondersteund met ASEs. Er is een oplossing voor dit probleem cross-adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering stoppen.

Volg deze stappen voor het maken van een UDR:

1. Ga naar de Azure-portal. Selecteer **Networking** > **routetabellen**.

2. Maak een nieuwe routetabel in dezelfde regio bevinden als uw VNet.

3. In de routetabel gebruikersinterface, selecteer **Routes** > **toevoegen**.

4. Instellen de **volgende hop type** naar **Internet** en de **adresvoorvoegsel** naar **0.0.0.0/0**. Selecteer **Opslaan**.

    Klik er ongeveer als volgt:

    ![Functionele routes][6]

5. Nadat u de nieuwe routetabel hebt gemaakt, gaat u naar het subnet waarin uw as-omgeving. Selecteer de routetabel in de lijst in de portal. Nadat u de wijziging opslaat, klikt u vervolgens ziet u de nsg's en de routes die worden vermeld met uw subnet.

    ![Nsg's en -routes][7]

### <a name="deploy-into-existing-azure-virtual-networks-that-are-integrated-with-expressroute"></a>Implementeren in een bestaande virtuele Azure-netwerken die zijn geïntegreerd met ExpressRoute ###

Als u wilt uw as-omgeving implementeren in een VNet dat geïntegreerd met ExpressRoute, moet u het subnet waar u het as-omgeving geïmplementeerd configureert. Gebruik vervolgens een Resource Manager-sjabloon te implementeren. Voor het maken van een as-omgeving in een VNet dat al is ExpressRoute geconfigureerd:

- Maak een subnet voor het hosten van de as-omgeving.

    > [!NOTE]
    > Niets anders kan niet in het subnet, maar de as-omgeving. Zorg dat u kiest een adresruimte waarmee voor toekomstige groei. U kan niet deze instelling later wijzigen. Een grootte van het is raadzaam `/25` met 128 adressen.

- Maken udr's (bijvoorbeeld routetabellen) zoals eerder beschreven, en die op het subnet.
- De as-omgeving maken met behulp van een Resource Manager-sjabloon, zoals beschreven in [een as-omgeving maken met behulp van een Resource Manager-sjabloon][MakeASEfromTemplate].

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ASEManagement]: ./management-addresses.md
