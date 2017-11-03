---
title: Gegevens in de netwerkconfiguratie voor het werken met Express Route
description: Gegevens in de netwerkconfiguratie voor het uitvoeren van App Service-omgevingen in een virtuele netwerken zijn verbonden aan een ExpressRoute-Circuit.
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 34b49178-2595-4d32-9b41-110c96dde6bf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2016
ms.author: stefsch
ms.openlocfilehash: bb3e283e8a9327a9c66c8d8ded037cee5195ffc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Netwerkconfiguratiedetails voor App Service-omgevingen met ExpressRoute
## <a name="overview"></a>Overzicht
Klanten kunnen verbinding maken met een [Azure ExpressRoute] [ ExpressRoute] circuit met hun virtuele netwerkinfrastructuur, waardoor hun on-premises netwerk uitbreiden naar Azure.  Een App Service-omgeving kunnen worden gemaakt in een subnet van deze [virtueel netwerk] [ virtualnetwork] infrastructuur.  Apps die worden uitgevoerd op de App Service-omgeving kunnen vervolgens alleen via de ExpressRoute-verbinding tot stand brengen beveiligde verbindingen met back-end-resources die toegankelijk is.  

Een App Service-omgeving kunnen worden gemaakt in **beide** een virtueel netwerk van Azure Resource Manager, **of** classic deployment model virtueel netwerk.  Met een recente wijziging in juni 2016, worden ASEs nu ook geïmplementeerd in virtuele netwerken die gebruikmaken van openbare-adresbereiken of RFC1918 adresruimten (dat wil zeggen particuliere adressen). 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Vereiste netwerkverbinding
Er zijn verbindingsproblemen netwerkvereisten voor App Service-omgevingen die niet in eerste instantie kan worden voldaan in een virtueel netwerk is verbonden met een ExpressRoute.  App Service-omgevingen moeten alle volgende juist werken als:

* Uitgaande netwerkverbinding met Azure Storage-eindpunten die wereldwijd zijn op beide poorten 80 en 443.  Dit omvat eindpunten zich in dezelfde regio bevinden als de App Service-omgeving, evenals de storage-eindpunten zich in **andere** Azure-regio's.  Azure Storage-eindpunten omgezet onder de volgende DNS-domeinen: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* en *file.core.windows.net*.  
* Uitgaande netwerkverbinding met de service Azure-bestanden op poort 445.
* Uitgaande netwerkverbinding met Sql DB-eindpunten zich in dezelfde regio bevinden als de App Service-omgeving.  Eindpunten van de SQL-database los onder het volgende domein: *database.windows.net*.  Hiervoor moet de toegang tot poorten 1433, 11000 11999 en 14000 14999 openen.  Zie voor meer informatie [in dit artikel over het gebruik van Sql Database V12-poort van](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
* Uitgaande netwerkverbinding met de Azure management vlak eindpunten (ASM- en ARM-eindpunten).  Dit omvat uitgaande verbinding met zowel *management.core.windows.net* en *management.azure.com*. 
* Uitgaande netwerkverbinding met *ocsp.msocsp.com*, *mscrl.microsoft.com* en *crl.microsoft.com*.  Dit is nodig om SSL-functionaliteit te ondersteunen.
* De DNS-configuratie voor het virtuele netwerk moet geschikt is voor het oplossen van alle eindpunten en domeinen die worden vermeld in de eerdere punten.  Als deze eindpunten kunnen niet worden omgezet, mislukken pogingen tot het maken van App Service-omgeving en bestaande App Service-omgevingen worden gemarkeerd als beschadigd.
* Uitgaand verkeer op poort 53 is vereist voor communicatie met de DNS-servers.
* Als een aangepaste DNS-server op het andere einde van een VPN-gateway bestaat, moet de DNS-server bereikbaar zijn vanaf het subnet met de App Service-omgeving. 
* Het uitgaande pad kan niet worden getransporteerd via interne zakelijke proxy's en niet kan worden geforceerd worden via een tunnel met on-premises.  In dat geval het effectieve NAT-adres van uitgaand netwerkverkeer wordt gewijzigd van App Service-omgeving.  Het NAT-adres van een App Service-omgeving uitgaand netwerkverkeer wijzigt, worden verbindingsproblemen op-veel van de eindpunten die hierboven worden genoemd.  Dit leidt tot mislukte pogingen voor App Service-omgeving maken, evenals eerder orde App Service-omgevingen worden gemarkeerd als beschadigd.  
* Inkomende netwerktoegang tot de vereiste poorten voor App Service-omgevingen moet worden toegestaan, zoals beschreven in dit [artikel][requiredports].

De DNS-vereisten kunnen worden voldaan door te zorgen voor een geldig DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk.  Als voor een bepaalde reden worden de DNS-configuratie is gewijzigd nadat een App-serviceomgeving is gemaakt, kunnen ontwikkelaars een App Service-omgeving naar de nieuwe DNS-configuratie worden opgepikt afdwingen.  Activering van rolling omgeving opgestart met het pictogram "Restart" zich boven aan de blade voor het beheer van App Service-omgeving in de [Azure-portal] [ NewPortal] , wordt de omgeving voor de nieuwe DNS-configuratie worden opgepikt.

De netwerkvereisten voor binnenkomende toegang kunnen worden voldaan door het configureren van een [netwerkbeveiligingsgroep] [ NetworkSecurityGroups] op subnet van de App Service-omgeving om toe te staan de vereiste toegang zoals is beschreven in dit [artikel][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Uitgaande netwerkverbinding inschakelen voor een App Service-omgeving
Een nieuwe ExpressRoute-circuit kondigt standaard een standaardroute waarmee uitgaande internetverbinding.  Met deze configuratie is een App-serviceomgeving verbinding kunnen maken met andere Azure-eindpunten.

Een algemene configuratie van de klant is echter voor het definiëren van hun eigen standaardroute (0.0.0.0/0), waardoor uitgaand internetverkeer in plaats daarvan lokale stromen.  App Service-omgevingen verbroken dit netwerkverkeer altijd omdat het uitgaande verkeer geblokkeerd on-premises wordt of NAT zou een onherkenbare set adressen die niet langer met verschillende Azure-eindpunten werken.

De oplossing is voor het definiëren van een (of meer) door de gebruiker gedefinieerde routes (udr's) op het subnet waarin de App Service-omgeving.  Een UDR definieert subnet-specifieke routes die in plaats van de standaardroute gehonoreerd.

Het verdient indien mogelijk, gebruik de volgende configuratie:

* Configuratie van de ExpressRoute kondigt 0.0.0.0/0 en standaard force alle uitgaande verkeer lokale tunnels.
* De UDR toegepast op het subnet met de App-serviceomgeving definieert 0.0.0.0/0 met een volgend hoptype Internet (een voorbeeld hiervan is verder omlaag in dit artikel).

Het gecombineerde effect van deze stappen is dat subnetniveau UDR voorrang boven de ExpressRoute geforceerde tunneling, zodat uitgaande toegang tot Internet van de App Service-omgeving.

> [!IMPORTANT]
> De routes die zijn gedefinieerd in een UDR **moet** specifiek genoeg is, hebben voorrang boven alle routes die worden geadverteerd door de ExpressRoute-configuratie.  Het onderstaande voorbeeld het adresbereik brede 0.0.0.0/0 gebruikt en als zodanig kan mogelijk worden per ongeluk genegeerd door route-advertisements met behulp van specifieke adresbereiken.
> 
> App Service-omgevingen worden niet ondersteund met ExpressRoute-configuraties die **cross adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering**.  ExpressRoute-configuraties waarvoor openbare peering is geconfigureerd, ontvangen route-advertisements van Microsoft voor een groot aantal Microsoft Azure-IP-adresbereiken.  Als deze adresbereiken cross aangekondigd op het pad voor persoonlijke peering, wordt het eindresultaat is dat alle uitgaande pakketten van de App Service-omgeving subnet zal force-tunneled van een klant on-premises netwerkinfrastructuur.  Deze stroom netwerk wordt momenteel niet ondersteund met App Service-omgevingen.  Er is een oplossing voor dit probleem cross-adverteert routes van het pad voor openbare peering naar het pad voor persoonlijke peering stoppen.
> 
> 

Achtergrondinformatie over de gebruiker gedefinieerde routes zijn beschikbaar in deze [overzicht][UDROverview].  

Meer informatie over het maken en configureren van de gebruiker gedefinieerde routes zijn beschikbaar in deze [hoe naar handleiding][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Voorbeeld UDR configuratie voor een App Service-omgeving
**Vereisten**

1. Installeer Azure Powershell uit de [pagina Azure Downloads] [ AzureDownloads] (met de datum juni 2015 of hoger).  Onder "Opdrachtregelprogramma's" is er een koppeling 'Install' onder 'Windows Powershell' waarop de meest recente Powershell-cmdlets wordt geïnstalleerd.
2. Het is raadzaam een uniek subnet wordt gemaakt voor exclusief gebruik door een App Service-omgeving.  Dit zorgt ervoor dat de udr's toegepast op het subnet alleen uitgaand verkeer voor de App Service-omgeving wordt geopend.
3. **Belangrijke**: implementeer de App-serviceomgeving tot geen **nadat** de volgende configuratiestappen uit worden gevolgd.  Dit zorgt ervoor dat de uitgaande netwerkverbinding beschikbaar is voordat u een App Service-omgeving te implementeren.

**Stap 1: Een benoemde routetabel maken**

Het volgende codefragment maakt een routetabel met de naam 'DirectInternetRouteTable' in de regio West ons Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Stap 2: Een of meer routes in de routetabel maken**

U moet een of meer routes toevoegen aan de routetabel om in te schakelen uitgaande toegang tot Internet.  

De aanbevolen aanpak voor het configureren van uitgaande toegang tot het Internet is voor het definiëren van een route voor 0.0.0.0/0 zoals hieronder wordt weergegeven.

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Houd er rekening mee dat 0.0.0.0/0 is een breed bereik, en als zodanig wordt overschreven door meer specifieke adresbereiken die zijn aangekondigd door de ExpressRoute.  Als u wilt herhalen opnieuw de eerdere aanbeveling, moet een UDR met een route 0.0.0.0/0 worden gebruikt in combinatie met een configuratie voor ExressRoute die alleen ook 0.0.0.0/0 kondigt. 

Als alternatief kunt kunt u een uitgebreide en bijgewerkte lijst met CIDR-bereiken in gebruik door Azure downloaden.  Het Xml-bestand met alle van de Azure-IP-adresbereiken is beschikbaar via de [Microsoft Download Center][DownloadCenterAddressRanges].  

Let echter deze bereiken wijzigen gedurende een periode, dus waardoor periodieke handmatige updates voor de gebruiker gedefinieerde routes om synchroon te houden.  Bovendien moet u de Azure-IP-adresbereiken om aan te passen binnen de limiet van 100 route 'samenvatten' omdat er een standaard bovenlimiet van 100 routes in een enkel UDR, houd in gedachten dat UDR routes moeten specifieker zijn dan de routes gedefinieerd geadverteerd door uw ExpressRoute.  

**Stap 3: De routetabel aan het subnet met de App Service-omgeving koppelen**

De laatste stap in de configuratie is om te koppelen van de routetabel aan het subnet waarop de App Service-omgeving wordt geïmplementeerd.  De volgende opdracht wordt gekoppeld aan 'DirectInternetRouteTable' aan de 'ASESubnet' waarin u uiteindelijk een App Service-omgeving.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Stap 4: Laatste stappen**

Zodra de routetabel is gebonden aan het subnet, is het raadzaam eerst te testen en Bevestig het gewenste effect.  Bijvoorbeeld: een virtuele machine implementeren in het subnet en Bevestig dat:

* Uitgaand verkeer naar de Azure- en niet-Azure-eindpunten vermeld eerder in dit artikel is **niet** stromende omlaag het ExpressRoute-circuit.  Het is belangrijk om te controleren of dit gedrag omdat als uitgaand verkeer van het subnet is nog steeds gedwongen via een tunnel on-premises App Service-omgeving altijd het maken mislukt. 
* DNS-zoekacties voor de eindpunten eerder zijn alle correct kan omzetten. 

Zodra de bovenstaande stappen zijn bevestigd, moet u de virtuele machine niet verwijderen omdat het subnet moet 'leeg' op het moment dat de App Service-omgeving wordt gemaakt.

Ga vervolgens verder met het maken van een App-serviceomgeving!

## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgevingen, Zie [Inleiding tot de App Service-omgeving][IntroToAppServiceEnvironment]

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: app-service-app-service-environment-control-inbound-traffic.md
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[NewPortal]:  https://portal.azure.com


<!-- IMAGES -->
