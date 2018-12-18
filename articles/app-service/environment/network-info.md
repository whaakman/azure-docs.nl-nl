---
title: Aandachtspunten voor netwerken met App Service Environment - Azure
description: Het netwerkverkeer van de as-omgeving en hoe nsg's en udr's met de as-omgeving instellen
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
ms.date: 08/29/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: d9a0ab84e133863092f68cc949c2b7933bc5da31
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271008"
---
# <a name="networking-considerations-for-an-app-service-environment"></a>Aandachtspunten voor netwerken voor een App Service Environment #

## <a name="overview"></a>Overzicht ##

 Azure [App Service-omgeving] [ Intro] is een implementatie van Azure App Service in een subnet in uw Azure-netwerk (VNet). Er zijn twee implementatietypen voor een App Service environment (ASE):

- **Externe as-omgeving**: Beschrijft de as-omgeving gehoste apps op internet toegankelijk is via het IP-adres. Zie voor meer informatie, [maken van een externe as-omgeving][MakeExternalASE].
- **ILB AS-OMGEVING**: Beschrijft de as-omgeving gehoste apps op een IP-adres in uw VNet. Het interne eindpunt is een interne load balancer (ILB), vandaar dat het een ILB as-omgeving wordt genoemd. Zie voor meer informatie, [maken en gebruiken een ILB as-omgeving][MakeILBASE].

Er zijn twee versies van App Service-omgeving: ASEv1 en ASEv2. Zie voor meer informatie over ASEv1 [Inleiding tot App Service Environment v1][ASEv1Intro]. ASEv1 kan worden geïmplementeerd in een klassieke of Resource Manager-VNet. ASEv2 kan alleen worden geïmplementeerd in een Resource Manager VNet.

Alle aanroepen van een as-omgeving die gaat u naar het internet laat u het VNet via een VIP-adres toegewezen voor de as-omgeving. Het openbare IP-adres van deze VIP is het bron-IP-adres voor alle aanroepen van de as-omgeving die gaat u naar het internet. Als de apps in de as-omgeving aanroepen naar resources in uw VNet of via een VPN kunt, wordt de bron-IP is een van de IP-adressen in het subnet dat wordt gebruikt door de as-omgeving. Omdat de as-omgeving binnen het VNet is, kan het ook toegang tot bronnen binnen het VNet zonder extra configuratie. Als het VNet is verbonden met uw on-premises netwerk, wordt ook toegang tot resources er zonder aanvullende configuratie in apps in de as-omgeving hebben.

![Externe as-omgeving][1] 

Als u een externe as-omgeving hebt, wordt de openbare VIP is ook het eindpunt dat uw ASE-apps worden omgezet naar voor:

* HTTP/S. 
* FTP/S. 
* Webimplementatie.
* Foutopsporing op afstand.

![ILB AS-OMGEVING][2]

Als u een ILB as-omgeving hebt, is het adres van de ILB het eindpunt voor HTTP/S, FTP/S, implementatie van web- en foutopsporing op afstand.

De normale app-poorten zijn:

| Gebruiken | Vanaf | Handeling |
|----------|---------|-------------|
|  HTTP/HTTPS  | Gebruiker worden geconfigureerd |  80, 443 |
|  FTP/FTPS    | Gebruiker worden geconfigureerd |  21, 990, 10001-10020 |
|  Visual Studio fouten opsporen op afstand  |  Gebruiker worden geconfigureerd |  4020, 4022, 4024 |

Dit geldt als u op een externe as-omgeving of in een ILB as-omgeving bent. Als u een externe as-omgeving, kunt u deze poorten op de openbare VIP bereikt. Als u een ILB as-omgeving, kunt u deze poorten op de ILB bereikt. Als u poort 443 te vergrendelen, kan het zijn dat er van invloed op sommige functies die beschikbaar zijn in de portal hebt gemaakt. Zie voor meer informatie, [Portal afhankelijkheden](#portaldep).

## <a name="ase-subnet-size"></a>Grootte van de ASE-subnet ##

De grootte van het subnet die wordt gebruikt voor het hosten van een ASE, kan niet worden gewijzigd nadat de ASE is geïmplementeerd.  De as-omgeving maakt gebruik van een adres voor elke infrastructuurrol ook als die voor elk exemplaar geïsoleerd App Service-plan.  Bovendien zijn er 5-adressen die door Azure Networking gebruikt voor elk subnet dat is gemaakt.  Een as-omgeving met geen App Service-plannen helemaal wordt 12 adressen gebruiken voordat u een app maakt.  Als het een ILB as-omgeving wordt het 13 adressen gebruiken voordat u een app in deze as-omgeving maken. Als u uw as-omgeving uitbreiden, kan infrastructuur rollen elke meervoud van 15 en 20 van uw App Service-plan-exemplaren worden toegevoegd.

   > [!NOTE]
   > Niets anders kan zich in het subnet, maar de as-omgeving. Zorg dat u kiest een adresruimte waarmee voor toekomstige groei. U kunt geen deze instelling later wijzigen. We raden een grootte van `/24` met 256-adressen.

Als u omhoog of omlaag schalen, nieuwe rollen van de juiste grootte heeft toegevoegd en vervolgens uw workloads worden gemigreerd van de huidige grootte voor de doelgrootte. Nadat uw apps worden gemigreerd, worden de oorspronkelijke virtuele machines verwijderd. Dit betekent dat als u had een ASE met 100 ASP-instanties zou er een punt waar u dubbele het aantal virtuele machines nodig hebt.  Het is om deze reden die wij het gebruik van adviseren een slash (/ 24) voor alle wijzigingen die u mogelijk nodig hebt.  

## <a name="ase-dependencies"></a>As-omgeving afhankelijkheden ##

### <a name="ase-inbound-dependencies"></a>As-omgeving inkomende afhankelijkheden ###

De as-omgeving binnenkomende toegang afhankelijkheden zijn:

| Gebruiken | Vanaf | Handeling |
|-----|------|----|
| Beheer | Beheeradressen van App Service | ASE-subnet: 454, 455 |
|  Interne communicatie as-omgeving | ASE-subnet: Alle poorten | ASE-subnet: Alle poorten
|  Azure load balancer toegestaan binnenkomend verkeer | Azure Load Balancer | ASE-subnet: Alle poorten
|  App toegewezen IP-adressen | App toegewezen adressen | ASE-subnet: Alle poorten

Het inkomend managementverkeer biedt opdracht en controle van de ASE naast het controleren van het systeem. De bronadressen voor dit verkeer worden vermeld in de [ASE Management adressen] [ ASEManagement] document. De netwerkconfiguratie van de beveiliging moet toegang vanaf alle IP-adressen op de poorten 454 en 455 is toegestaan. Als u toegang vanaf deze adressen blokkeert, wordt de as-omgeving wordt niet in orde en vervolgens worden onderbroken.

Binnen de ASE-subnet er veel poorten die worden gebruikt voor interne onderdeel zijn kunnen communicatie en ze wijzigen.  Hiervoor moet alle poorten in de ASE-subnet voor toegang vanuit het ASE-subnet. 

Voor de communicatie tussen de Azure load balancer en het ASE-subnet zijn de poorten die moeten open zijn minimaal 454 en 455 van 16001. De 16001 poort wordt gebruikt voor keep alive verkeer tussen de load balancer als de as-omgeving. Als u een ILB as-omgeving gebruikt, kunt u verkeer tot alleen de 454, 455, 16001 vergrendelen poorten.  Als u een externe as-omgeving moet u rekening mee dat de poorten van de toegang tot normale app.  Als u app toegewezen adressen die u wilt openen naar alle poorten.  Als u een adres wordt toegewezen aan een specifieke app, gebruikt de load balancer poorten die niet bekend zijn van tevoren HTTP en HTTPS-verkeer verzenden naar de as-omgeving.

Als u app toegewezen IP-adressen die u wilt toestaan dat verkeer van de IP-adressen toegewezen aan uw apps op het ASE-subnet.

De TCP-verkeer binnen via poorten 454 en 455 komt moet gaan van de dezelfde VIP of hebt u een probleem met asymmetrische routering. 

### <a name="ase-outbound-dependencies"></a>Uitgaande ASE-afhankelijkheden ###

Voor uitgaande toegang is een as-omgeving afhankelijk van meerdere externe systemen. Veel van deze systeemafhankelijkheden met DNS-namen zijn gedefinieerd, en niet toewijzen aan een vaste set IP-adressen. De as-omgeving is dus uitgaand verkeer vanuit het ASE-subnet op alle externe IP-adressen in een groot aantal poorten vereist. 

De volledige lijst van de uitgaande afhankelijkheden worden weergegeven in het document dat wordt beschreven [uitgaand verkeer van App Service-omgeving te vergrendelen](./firewall-integration.md). Als de as-omgeving de toegang tot de afhankelijkheden ervan verliest, werkt niet. In dat geval lang genoeg, wordt de as-omgeving is onderbroken. 

### <a name="customer-dns"></a>Klant DNS ###

Als het VNet met een door de klant gedefinieerde DNS-server is geconfigureerd, wordt het door de tenantwerkbelastingen gebruiken. Er moet nog steeds de as-omgeving om te communiceren met Azure DNS voor beheerdoeleinden. 

Als het VNet is geconfigureerd met een klant DNS op de andere kant van een VPN-verbinding, moet de DNS-server bereikbaar is vanaf het subnet waarin de as-omgeving zijn.

Voor het testen van de oplossing van uw web-app kunt u de consoleopdracht *nameresolver*. Ga naar het foutopsporingsvenster in uw scm-site voor uw app of Ga naar de app in de portal en selecteer console. Vanuit de shell-prompt kunt u de opdracht geven *nameresolver* samen met het adres dat u wilt zoeken. Het resultaat dat je weer toegang krijgen is gelijk aan wat uw app krijgt bij het maken van de dezelfde zoekopdracht. Als u nslookup doet u een zoekopdracht met behulp van de Azure DNS in plaats daarvan.

Als u de DNS-instelling van het VNet dat uw ASE in wijzigt, moet u opnieuw opstarten van de as-omgeving. Om te voorkomen dat opnieuw opstarten van de as-omgeving, is het raadzaam dat u uw DNS-instellingen voor uw VNet configureren voordat u uw ASE maken.  

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portal-afhankelijkheden ##

Naast de functionele afhankelijkheden van de as-omgeving zijn er enkele extra items in verband met de portal-ervaring. Enkele van de mogelijkheden in Azure portal zijn afhankelijk van directe toegang tot _SCM-site_. Er zijn twee URL's voor elke app in Azure App Service. De eerste URL is voor toegang tot uw app. De URL van de tweede is voor toegang tot de SCM-site wordt ook wel genoemd de _Kudu-console_. Functies die gebruikmaken van de SCM-site zijn onder andere:

-   Webtaken
-   Functions
-   Logboekstreaming
-   Kudu
-   Extensies
-   Procesverkenner
-   Console

Als u een ILB as-omgeving gebruikt, niet de SCM-site internet toegankelijk van buiten het VNet. Wanneer uw app wordt gehost op een ILB as-omgeving, werken sommige mogelijkheden niet vanuit de portal.  

Veel van de functies die afhankelijk zijn van de SCM-site zijn ook beschikbaar rechtstreeks in de Kudu-console. U kunt verbinding maken op deze rechtstreeks in plaats van met behulp van de portal. Als uw app wordt gehost in een ILB as-omgeving, wordt aan te melden bij uw publicatiereferenties gebruiken. De URL voor toegang tot de SCM-site van een app die wordt gehost in een ILB as-omgeving heeft de volgende indeling: 

```
<appname>.scm.<domain name the ILB ASE was created with> 
```

Als uw ILB as-omgeving de domeinnaam *contoso.net* en de appnaam van uw is *testapp*, de app is bereikt op *testapp.contoso.net*. De SCM-site die u ermee gaat is bereikt op *testapp.scm.contoso.net*.

## <a name="functions-and-web-jobs"></a>Functions en webjobs ##

Zowel Functions als Web taken zijn afhankelijk van de SCM-site, maar worden ondersteund voor gebruik in de portal, zelfs als uw apps in een ILB as-omgeving, zo lang uw browser de SCM-site kunt bereiken.  Als u van een zelfondertekend certificaat met uw ILB as-omgeving gebruikmaakt, moet u uw browser naar het certificaat vertrouwd inschakelen.  Is in het computerarchief van de vertrouwensrelatie voor Internet Explorer en Edge dat betekent het certificaat dat.  Als u met chrome werkt en dat betekent dat u het certificaat in de browser eerder geaccepteerd dat door waarschijnlijk te maken met de scm-site rechtstreeks.  De beste oplossing is het gebruik van een commercieel certificaat in de vertrouwensketen van de browser.  

## <a name="ase-ip-addresses"></a>ASE-IP-adressen ##

Een as-omgeving heeft enkele IP-adressen rekening mee moet houden. Dit zijn:

- **Inkomende openbare IP-adres**: Gebruikt voor appverkeer in een externe as-omgeving en beheer van verkeer in zowel een externe as-omgeving en een ILB as-omgeving.
- **Uitgaande openbare IP-adres**: Gebruikt als de 'van'-IP voor uitgaande verbindingen van de as-omgeving die het VNet, die niet worden doorgestuurd naar beneden een VPN-verbinding.
- **ILB IP-adres**: Als u een ILB as-omgeving.
- **SSL op basis van IP-adressen toegewezen App**: Alleen mogelijk met een externe as-omgeving en wanneer op basis van IP SSL is geconfigureerd.

Deze IP-adressen zijn eenvoudig zichtbaar zijn in een ASEv2 in Azure portal vanuit de gebruikersinterface van de as-omgeving. Als u een ILB as-omgeving hebt, wordt het IP-adres voor de ILB wordt vermeld.

   > [!NOTE]
   > Deze IP-adressen worden niet gewijzigd, zolang de as-omgeving actief en werkend blijft.  Als de as-omgeving wordt onderbroken en hersteld, wijzigt u de adressen die worden gebruikt door de as-omgeving. De normale oorzaak voor een as-omgeving om te worden onderbroken is als u binnenkomende toegang tot of blokkeren van toegang tot een ASE-afhankelijkheid. 

![IP-adressen][3]

### <a name="app-assigned-ip-addresses"></a>App-toegewezen IP-adressen ###

Met een externe as-omgeving, kunt u IP-adressen toewijzen aan afzonderlijke apps. U kunt geen dat doen met een ILB as-omgeving. Zie voor meer informatie over het configureren van uw app een eigen IP-adres hebben [een bestaand aangepast SSL-certificaat binden aan Azure WebApps](../app-service-web-tutorial-custom-ssl.md).

Wanneer een app een eigen SSL op basis van IP-adres heeft, behoudt de as-omgeving zich twee poorten toe te wijzen aan dat IP-adres. Een poort is voor de HTTP-verkeer, en de andere poort voor HTTPS. Deze poorten worden vermeld in de gebruikersinterface van de as-omgeving in het gedeelte van de IP-adressen. Verkeer moeten kunnen deze poorten van de VIP bereiken of de apps zijn niet toegankelijk. Deze vereiste is belangrijk te weten wanneer u Netwerkbeveiligingsgroepen (nsg's) configureren.

## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen ##

[Netwerkbeveiligingsgroepen] [ NSGs] bieden de mogelijkheid voor het beheren van toegang tot het netwerk binnen een VNet. Wanneer u de portal gebruikt, moet u er een regel voor weigeren impliciete is op de laagste prioriteit Alles weigeren. Wat u maken worden uw regels voor toestaan.

In een as-omgeving hebt u geen toegang aan de virtuele machines die worden gebruikt voor het hosten van de as-omgeving zelf. Ze in een beheerd door Microsoft abonnement. Als u beperken van toegang tot de apps op de as-omgeving wilt, stelt u nsg's op het ASE-subnet. In dat geval, betaalt u zorgvuldige aandacht aan de afhankelijkheden van de as-omgeving. Als u eventuele afhankelijkheden blokkeert, werkt niet meer de as-omgeving.

Nsg's kunnen worden geconfigureerd via de Azure-portal of via PowerShell. De informatie hier ziet u de Azure-portal. U maakt en nsg's beheren in de portal als een resource op het hoogste niveau onder **netwerken**.

Wanneer rekening met de vereisten voor binnenkomend en uitgaand gehouden, zijn de nsg's vergelijkbaar met de nsg's in dit voorbeeld wordt getoond. Het VNet-adresbereik _192.168.250.0/23_, en het subnet dat de as-omgeving in _192.168.251.128/25_.

De eerste twee inkomende vereisten voor de as-omgeving naar de functie worden weergegeven aan de bovenkant van de lijst in dit voorbeeld. Ze ASE-beheer inschakelen en de as-omgeving om te communiceren met zichzelf toestaan. De andere vermeldingen alle tenant kunnen worden geconfigureerd zijn en netwerktoegang tot de as-omgeving gehoste toepassingen kunnen beheren. 

![Inkomende beveiligingsregels][4]

Een standaardregel kunt de IP-adressen in het VNet om te communiceren met de ASE-subnet. Een andere standaardregel kunnen de load balancer, ook wel bekend als de openbare VIP en om te communiceren met de as-omgeving. De standaardregels Selecteer **standaardregels** naast de **toevoegen** pictogram. Als u een weigeren alle andere regel nadat de NSG-regels weergegeven, maar u voorkomen dat het verkeer tussen het VIP-adres en de as-omgeving. Om te voorkomen dat verkeer dat afkomstig is van binnen het VNet, Voeg uw eigen regel waarmee inkomend. Een bron die gelijk is aan AzureLoadBalancer gebruiken met een bestemming **eventuele** en een poortbereik van **\*** \*. Omdat de NSG-regel wordt toegepast op het ASE-subnet, moet u geen specifieke worden in het doel.

Als u een IP-adres aan uw app toegewezen, zorg ervoor dat u de poorten openen. De poorten Selecteer **App Service-omgeving** > **IP-adressen**.  

Alle items die wordt weergegeven in de volgende regels voor uitgaand verkeer nodig zijn, met uitzondering van het laatste item. Ze inschakelen netwerktoegang tot de ASE-afhankelijkheden die eerder in dit artikel zijn vermeld. Als u een van deze blokkeert, werkt niet meer de as-omgeving. Het laatste item in de lijst kunt uw as-omgeving om te communiceren met andere resources in uw VNet.

![Uitgaande beveiligingsregels][5]

Nadat uw nsg's zijn gedefinieerd, kunt u ze op het subnet dat uw ASE zich op toewijzen. Als u niet meer de as-omgeving VNet of subnet weet, ziet u deze in de portalpagina as-omgeving. De NSG aan het subnet toewijzen, gaat u naar de gebruikersinterface van het subnet en selecteert u de NSG.

## <a name="routes"></a>Routes ##

Geforceerde tunneling is wanneer u routes in uw VNet instellen, zodat het uitgaande verkeer geen Ga rechtstreeks naar het internet, maar ergens anders, zoals een ExpressRoute-gateway of een virtueel apparaat.  Als u wilt uw ASE zodanig te configureren en vervolgens het document lezen op [uw App Service-omgeving configureren met geforceerde Tunneling][forcedtunnel].  Dit document ziet u de beschikbare opties voor het werken met ExpressRoute en geforceerde tunneling.

Wanneer u een as-omgeving in de portal maken maken we ook een set routetabellen op het subnet dat is gemaakt met de as-omgeving.  Routes die bijvoorbeeld gewoon naar het verzenden van uitgaand verkeer rechtstreeks naar het internet.  
Als u wilt de dezelfde routes handmatig hebt gemaakt, de volgende stappen uit:

1. Ga naar Azure Portal. Selecteer **netwerken** > **routetabellen**.

2. Maak een nieuwe routetabel in dezelfde regio als uw VNet.

3. Vanuit uw routetabel gebruikersinterface, de optie **Routes** > **toevoegen**.

4. Stelt de **volgende hoptype** te **Internet** en de **adresvoorvoegsel** naar **0.0.0.0/0**. Selecteer **Opslaan**.

    Klik er ongeveer als volgt:

    ![Functionele routes][6]

5. Nadat u de nieuwe routetabel gemaakt, gaat u naar het subnet waarin de as-omgeving. Selecteer uw routetabel in de lijst in de portal. Nadat u de wijziging hebt opgeslagen, klikt u vervolgens ziet u de nsg's en de routes die worden vermeld met uw subnet.

    ![Nsg's en routes][7]

## <a name="service-endpoints"></a>Service-eindpunten ##

Met service-eindpunten kunt u de toegang tot multitenant-services beperken tot een reeks virtuele Azure-netwerken en subnetten. In de documentatie [Virtual Network Service Endpoints][serviceendpoints] (Virtuele netwerkservice-eindpunten) vindt u meer informatie over service-eindpunten. 

Wanneer u service-eindpunten voor een bron inschakelt, worden er routes gemaakt die een hogere prioriteit hebben dan alle andere routes. Als u service-eindpunten gebruikt met een ASE met geforceerde tunnels, maakt het managementverkeer van Azure SQL en Azure Storage geen gebruik van geforceerde tunnels. 

Als Service-eindpunten in een subnet met een Azure SQL-exemplaar is ingeschakeld, moet Service-eindpunten zijn ingeschakeld op alle Azure SQL-exemplaren waarmee vanuit dat subnet een verbinding wordt gemaakt. Als u vanuit hetzelfde subnet toegang wilt hebben tot meerdere Azure SQL-exemplaren, is het niet mogelijk om Service-eindpunten wel op het ene Azure SQL-exemplaar in te schakelen en niet op een ander. Azure Storage gedraagt zich niet op dezelfde manier als Azure SQL. Wanneer u Service-eindpunten met Azure Storage inschakelt, kunt u de toegang tot die resource vanuit uw subnet vergrendelen en toegang behouden tot andere Azure Storage-accounts, zelfs als Service-eindpunten op die accounts niet is ingeschakeld.  

![Service-eindpunten][8]

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png
[8]: ./media/network_considerations_with_an_app_service_environment/serviceendpoint.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ASEManagement]: ./management-addresses.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
[forcedtunnel]: ./forced-tunnel-support.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
