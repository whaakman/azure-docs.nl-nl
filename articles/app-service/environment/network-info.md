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
ms.date: 05/31/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 3f80f3c6be747cf84aa9d8b2c386c0568a7511ad
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069385"
---
# <a name="networking-considerations-for-an-app-service-environment"></a>Aandachtspunten voor netwerken voor een App Service Environment #

## <a name="overview"></a>Overzicht ##

 Azure [App Service-omgeving] [ Intro] is een implementatie van Azure App Service in een subnet in uw Azure-netwerk (VNet). Er zijn twee implementatietypen voor een App Service environment (ASE):

- **Externe as-omgeving**: Beschrijft de as-omgeving gehoste apps op internet toegankelijk is via het IP-adres. Zie voor meer informatie, [maken van een externe as-omgeving][MakeExternalASE].
- **ILB ASE**: Beschrijft de as-omgeving gehoste apps op een IP-adres in uw VNet. Het interne eindpunt is een interne load balancer (ILB), vandaar dat het een ILB as-omgeving wordt genoemd. Zie voor meer informatie, [maken en gebruiken een ILB as-omgeving][MakeILBASE].

Alle as-omgevingen, extern en ILB, hebt u een openbare VIP-adres dat wordt gebruikt voor binnenkomend beheerverkeer en als het verzendadres bij het aanroepen van de as-omgeving met internet. Het aanroepen van een as-omgeving die gaat u naar het internet laat u het VNet via het VIP-adres toegewezen voor de as-omgeving. Het openbare IP-adres van deze VIP is het bron-IP-adres voor alle aanroepen van de as-omgeving die gaat u naar het internet. Als de apps in de as-omgeving aanroepen naar resources in uw VNet of via een VPN kunt, wordt de bron-IP is een van de IP-adressen in het subnet dat wordt gebruikt door de as-omgeving. Omdat de as-omgeving binnen het VNet is, kan het ook toegang tot bronnen binnen het VNet zonder extra configuratie. Als het VNet is verbonden met uw on-premises netwerk, wordt ook toegang tot resources er zonder aanvullende configuratie in apps in de as-omgeving hebben.

![Externe as-omgeving][1] 

Als u een externe as-omgeving hebt, wordt de openbare VIP is ook het eindpunt dat uw ASE-apps worden omgezet naar voor:

* HTTP/S 
* FTP/S
* Implementatie van Web
* Foutopsporing op afstand

![ILB AS-OMGEVING][2]

Als u een ILB as-omgeving hebt, is het adres van de ILB-adres van het eindpunt voor HTTP/S, FTP/S, implementatie van web- en foutopsporing op afstand.

## <a name="ase-subnet-size"></a>Grootte van de ASE-subnet ##

De grootte van het subnet dat wordt gebruikt voor het hosten van een as-omgeving kan niet worden gewijzigd nadat de as-omgeving is geïmplementeerd.  De as-omgeving maakt gebruik van een adres voor elke infrastructuurrol ook als die voor elk exemplaar geïsoleerd App Service-plan.  Bovendien zijn er vijf adressen die door het Azure-netwerken wordt gebruikt voor elk subnet dat is gemaakt.  Een as-omgeving met geen App Service-plannen helemaal wordt 12 adressen gebruiken voordat u een app maakt.  Als een ILB as-omgeving is, wordt deze 13-adressen gebruiken voordat u een app in deze as-omgeving maken. Als u uw as-omgeving uitbreiden, kan infrastructuur rollen elke meervoud van 15 en 20 van uw App Service-plan-exemplaren worden toegevoegd.

   > [!NOTE]
   > Niets anders kan zich in het subnet, maar de as-omgeving. Zorg dat u kiest een adresruimte waarmee voor toekomstige groei. U kunt geen deze instelling later wijzigen. We raden een grootte van `/24` met 256-adressen.

Als u omhoog of omlaag schalen, nieuwe rollen van de juiste grootte heeft toegevoegd en vervolgens uw workloads worden gemigreerd van de huidige grootte voor de doelgrootte. De oorspronkelijke VM's verwijderd nadat de werkbelastingen zijn gemigreerd. Als u een as-omgeving met 100 ASP-instanties had, zou er een punt waar u dubbele het aantal virtuele machines nodig hebt.  Het is om deze reden die wij het gebruik van adviseren een slash (/ 24) voor alle wijzigingen die u mogelijk nodig hebt.  

## <a name="ase-dependencies"></a>As-omgeving afhankelijkheden ##

### <a name="ase-inbound-dependencies"></a>As-omgeving inkomende afhankelijkheden ###

Alleen voor de as-omgeving werkt, is de as-omgeving vereist de volgende poorten geopend te zijn:

| Gebruiken | Van | Naar |
|-----|------|----|
| Beheer | Beheeradressen van App Service | ASE-subnet: 454, 455 |
|  Interne communicatie as-omgeving | ASE-subnet: Alle poorten | ASE-subnet: Alle poorten
|  Azure load balancer toegestaan binnenkomend verkeer | Azure Load Balancer | ASE-subnet: 16001

Er zijn 2 andere poorten op een poort-scan, 7654 en 1221 als open kunnen weergeven. Ze reageren met een IP-adres en niets meer. Ze kunnen worden geblokkeerd als gewenst. 

Het inkomend managementverkeer biedt opdracht en controle van de ASE naast het controleren van het systeem. De bronadressen voor dit verkeer worden vermeld in de [ASE Management adressen] [ ASEManagement] document. De netwerkconfiguratie van de beveiliging moet zodat toegang vanaf de beheeradressen as-omgeving via poorten 454 en 455. Als u toegang vanaf deze adressen blokkeert, wordt de as-omgeving wordt niet in orde en vervolgens worden onderbroken. De TCP-verkeer binnen via poorten 454 en 455 komt moet gaan van de dezelfde VIP of hebt u een probleem met asymmetrische routering. 

Binnen de ASE-subnet, zijn er veel poorten die worden gebruikt voor communicatie met interne onderdelen en ze kunnen wijzigen. Hiervoor moet alle poorten in de ASE-subnet voor toegang vanuit het ASE-subnet. 

Voor de communicatie tussen de Azure load balancer en het ASE-subnet zijn de poorten die moeten open zijn minimaal 454 en 455 van 16001. De 16001 poort wordt gebruikt voor keep alive verkeer tussen de load balancer als de as-omgeving. Als u een ILB as-omgeving gebruikt, dan kunt u verkeer tot alleen de 454, 455, 16001 vergrendelen poorten.  Als u van een externe as-omgeving gebruikmaakt, moet u rekening mee dat de poorten van de toegang tot normale app.  

De andere poorten moet u uzelf met betrekking op zijn de toepassingspoorten:

| Gebruiken | Poorten |
|----------|-------------|
|  HTTP/HTTPS  | 80, 443 |
|  FTP/FTPS    | 21, 990, 10001-10020 |
|  Visual Studio fouten opsporen op afstand  |  4020, 4022, 4024 |
|  Webservice implementeren | 8172 |

Als u de toepassingspoorten te blokkeren, de as-omgeving kan nog steeds werken, maar uw app kan al dan niet.  Als u van app toegewezen IP-adressen met een externe as-omgeving gebruikmaakt, moet u om verkeer van de IP-adressen toegewezen aan uw apps aan de ASE-subnet op de poorten die worden weergegeven in de portal voor as-omgeving > IP-adressen-pagina.

### <a name="ase-outbound-dependencies"></a>Uitgaande ASE-afhankelijkheden ###

Voor uitgaande toegang is een as-omgeving afhankelijk van meerdere externe systemen. Veel van deze systeemafhankelijkheden met DNS-namen zijn gedefinieerd, en niet toewijzen aan een vaste set IP-adressen. De as-omgeving is dus uitgaand verkeer vanuit het ASE-subnet op alle externe IP-adressen in een groot aantal poorten vereist. 

De as-omgeving communiceert van op internet toegankelijk adressen op de volgende poorten:

| Gebruik | Poorten |
|-----|------|
| DNS | 53 |
| NTP | 123 |
| 8CRL, updates van Windows, Linux-afhankelijkheden, Azure-services | 80/443 |
| Azure SQL | 1433 | 
| Bewaking | 12000 |

De uitgaande afhankelijkheden worden weergegeven in het document dat wordt beschreven [uitgaand verkeer van App Service-omgeving te vergrendelen](./firewall-integration.md). Als de as-omgeving de toegang tot de afhankelijkheden ervan verliest, werkt niet. In dat geval lang genoeg, wordt de as-omgeving is onderbroken. 

### <a name="customer-dns"></a>Klant DNS ###

Als het VNet met een door de klant gedefinieerde DNS-server is geconfigureerd, wordt het door de tenantwerkbelastingen gebruiken. De as-omgeving maakt gebruik van Azure DNS voor beheerdoeleinden. Als het VNet is geconfigureerd met een door de klant geselecteerd DNS-server, moet de DNS-server bereikbaar is vanaf het subnet waarin de as-omgeving zijn.

Als u wilt testen DNS-omzetting van uw web-app, kunt u de consoleopdracht *nameresolver*. Ga naar het foutopsporingsvenster in uw scm-site voor uw app of Ga naar de app in de portal en selecteer console. Vanuit de shell-prompt kunt u de opdracht geven *nameresolver* samen met de DNS-naam die u wilt zoeken. Het resultaat dat je weer toegang krijgen is gelijk aan wat uw app krijgt bij het maken van de dezelfde zoekopdracht. Als u nslookup gebruikt, doet u een zoekopdracht met behulp van de Azure DNS in plaats daarvan.

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

Wanneer u een ILB as-omgeving gebruikt, is de SCM-site niet toegankelijk van buiten het VNet. Bepaalde functies werken niet vanuit de app-portal omdat ze toegang tot de SCM-site van een app nodig hebben. U kunt verbinding maken met de SCM-site rechtstreeks in plaats van de portal. 

Als uw ILB as-omgeving de domeinnaam *contoso.appserviceenvironnment.net* en de appnaam van uw is *testapp*, de app is bereikt op *testapp.contoso.appserviceenvironment.net*. De SCM-site die u ermee gaat is bereikt op *testapp.scm.contoso.appserviceenvironment.net*.

## <a name="ase-ip-addresses"></a>ASE-IP-adressen ##

Een as-omgeving heeft enkele IP-adressen rekening mee moet houden. Dit zijn:

- **Inkomende openbare IP-adres**: Gebruikt voor appverkeer in een externe as-omgeving en beheer van verkeer in zowel een externe as-omgeving en een ILB as-omgeving.
- **Uitgaande openbare IP-adres**: Gebruikt als de 'van'-IP voor uitgaande verbindingen van de as-omgeving die het VNet, die niet worden doorgestuurd naar beneden een VPN-verbinding.
- **ILB IP-adres**: Het ILB IP-adres zit hem alleen in een ILB as-omgeving.
- **SSL op basis van IP-adressen toegewezen App**: Alleen mogelijk met een externe as-omgeving en wanneer op basis van IP SSL is geconfigureerd.

Deze IP-adressen zijn zichtbaar in Azure portal vanuit de gebruikersinterface van de as-omgeving. Als u een ILB as-omgeving hebt, wordt het IP-adres voor de ILB wordt vermeld.

   > [!NOTE]
   > Deze IP-adressen worden niet gewijzigd, zolang de as-omgeving actief en werkend blijft.  Als de as-omgeving wordt onderbroken en hersteld, wijzigt u de adressen die worden gebruikt door de as-omgeving. De normale oorzaak voor een as-omgeving om te worden onderbroken is als u binnenkomende toegang tot of blokkeren van toegang tot een ASE-afhankelijkheid. 

![IP-adressen][3]

### <a name="app-assigned-ip-addresses"></a>App-toegewezen IP-adressen ###

Met een externe as-omgeving, kunt u IP-adressen toewijzen aan afzonderlijke apps. U kunt geen dat doen met een ILB as-omgeving. Zie voor meer informatie over het configureren van uw app een eigen IP-adres hebben [een bestaand aangepast SSL-certificaat binden aan Azure App Service](../app-service-web-tutorial-custom-ssl.md).

Wanneer een app een eigen SSL op basis van IP-adres heeft, behoudt de as-omgeving zich twee poorten toe te wijzen aan dat IP-adres. Een poort is voor de HTTP-verkeer, en de andere poort voor HTTPS. Deze poorten worden vermeld in de gebruikersinterface van de as-omgeving in het gedeelte van de IP-adressen. Verkeer moeten kunnen deze poorten van de VIP bereiken of de apps zijn niet toegankelijk. Deze vereiste is belangrijk te weten wanneer u Netwerkbeveiligingsgroepen (nsg's) configureren.

## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen ##

[Netwerkbeveiligingsgroepen] [ NSGs] bieden de mogelijkheid voor het beheren van toegang tot het netwerk binnen een VNet. Wanneer u de portal gebruikt, moet u er een regel voor weigeren impliciete is op de laagste prioriteit Alles weigeren. Wat u maken worden uw regels voor toestaan.

In een as-omgeving hebt u geen toegang aan de virtuele machines die worden gebruikt voor het hosten van de as-omgeving zelf. Ze in een beheerd door Microsoft abonnement. Als u beperken van toegang tot de apps op de as-omgeving wilt, stelt u nsg's op het ASE-subnet. In dat geval, betaalt u zorgvuldige aandacht aan de afhankelijkheden van de as-omgeving. Als u eventuele afhankelijkheden blokkeert, werkt niet meer de as-omgeving.

Nsg's kunnen worden geconfigureerd via de Azure-portal of via PowerShell. De informatie hier ziet u de Azure-portal. U maakt en nsg's beheren in de portal als een resource op het hoogste niveau onder **netwerken**.

De vereiste items in een NSG, voor een as-omgeving aan functie zijn om verkeer te staan:

**Inkomende**
* van het IP-adres servicetag AppServiceManagement op poorten 454,455
* van de load balancer op poort 16001
* vanuit het ASE-subnet aan de ASE-subnet op alle poorten

**Uitgaande**
* voor alle IP-adressen op poort 123
* voor alle IP's op de poorten 80, 443
* aan de IP-adres servicetag SQL op poort 1433
* voor alle IP-adressen op poort 12000
* aan de ASE-subnet op alle poorten

De DNS-poort hoeft niet te worden toegevoegd omdat het verkeer naar DNS wordt niet beïnvloed door NSG-regels. Deze poorten omvatten niet de poorten die door uw apps voor geslaagde gebruik vereist. De normale app-poorten zijn:

| Gebruiken | Poorten |
|----------|-------------|
|  HTTP/HTTPS  | 80, 443 |
|  FTP/FTPS    | 21, 990, 10001-10020 |
|  Visual Studio fouten opsporen op afstand  |  4020, 4022, 4024 |
|  Webservice implementeren | 8172 |

Wanneer rekening met de vereisten voor binnenkomend en uitgaand gehouden, zijn de nsg's vergelijkbaar met de nsg's in dit voorbeeld wordt getoond. 

![Inkomende beveiligingsregels][4]

Een standaardregel kunt de IP-adressen in het VNet om te communiceren met de ASE-subnet. Een andere standaardregel kunnen de load balancer, ook wel bekend als de openbare VIP en om te communiceren met de as-omgeving. De standaardregels Selecteer **standaardregels** naast de **toevoegen** pictogram. Als u een weigeren alle andere voordat u de standaardregels regel plaatst, voorkomt u verkeer tussen de VIP-adres en de as-omgeving. Om te voorkomen dat verkeer dat afkomstig is van binnen het VNet, Voeg uw eigen regel waarmee inkomend. Een bron die gelijk is aan AzureLoadBalancer gebruiken met een bestemming **eventuele** en een poortbereik van **\*** . Omdat de NSG-regel wordt toegepast op het ASE-subnet, moet u geen specifieke worden in het doel.

Als u een IP-adres aan uw app toegewezen, zorg ervoor dat u de poorten openen. De poorten Selecteer **App Service-omgeving** > **IP-adressen**.  

Alle items die wordt weergegeven in de volgende regels voor uitgaand verkeer nodig zijn, met uitzondering van het laatste item. Ze inschakelen netwerktoegang tot de ASE-afhankelijkheden die eerder in dit artikel zijn vermeld. Als u een van deze blokkeert, werkt niet meer de as-omgeving. Het laatste item in de lijst kunt uw as-omgeving om te communiceren met andere resources in uw VNet.

![Uitgaande beveiligingsregels][5]

Nadat uw nsg's zijn gedefinieerd, kunt u ze op het subnet dat uw ASE zich op toewijzen. Als u niet meer de as-omgeving VNet of subnet weet, ziet u deze in de portalpagina as-omgeving. De NSG aan het subnet toewijzen, gaat u naar de gebruikersinterface van het subnet en selecteert u de NSG.

## <a name="routes"></a>Routes ##

Geforceerde tunneling is wanneer u routes in uw VNet instellen, zodat het uitgaande verkeer geen Ga rechtstreeks naar het internet, maar ergens anders, zoals een ExpressRoute-gateway of een virtueel apparaat.  Als u uw ASE zodanig configureren wilt, klikt u vervolgens het document lezen op [uw App Service-omgeving configureren met geforceerde Tunneling][forcedtunnel].  Dit document ziet u de beschikbare opties voor het werken met ExpressRoute en geforceerde tunneling.

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

Met service-eindpunten kunt u de toegang tot multitenant-services beperken tot een reeks virtuele Azure-netwerken en subnetten. In de documentatie [Service-eindpunten voor virtuele netwerken][serviceendpoints] vindt u meer informatie over service-eindpunten. 

Wanneer u service-eindpunten voor een bron inschakelt, worden er routes gemaakt die een hogere prioriteit hebben dan alle andere routes. Als u Service-eindpunten voor elke Azure-service, met een geforceerde tunnel as-omgeving, het verkeer naar deze services niet worden gedwongen via een tunnel. 

Als Service-eindpunten in een subnet met een Azure SQL-exemplaar is ingeschakeld, moet Service-eindpunten zijn ingeschakeld op alle Azure SQL-exemplaren waarmee vanuit dat subnet een verbinding wordt gemaakt. Als u vanuit hetzelfde subnet toegang wilt hebben tot meerdere Azure SQL-exemplaren, is het niet mogelijk om Service-eindpunten wel op het ene Azure SQL-exemplaar in te schakelen en niet op een ander. Er zijn geen andere Azure-service werkt zoals SQL Azure met betrekking tot de Service-eindpunten. Wanneer u Service-eindpunten met Azure Storage inschakelt, kunt u de toegang tot die resource vanuit uw subnet vergrendelen en toegang behouden tot andere Azure Storage-accounts, zelfs als Service-eindpunten op die accounts niet is ingeschakeld.  

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
