---
title: Details van netwerkconfiguratie voor het werken met Express Route
description: Details van netwerkconfiguratie voor het uitvoeren van App Service-omgevingen in een virtueel netwerk is verbonden met een ExpressRoute-Circuit.
services: app-service
documentationcenter: ''
author: stefsch
manager: nirma
editor: ''
ms.assetid: 34b49178-2595-4d32-9b41-110c96dde6bf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2016
ms.author: stefsch
ms.openlocfilehash: 7873192e4a66cd2faed5a1a1255377139d33d750
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51616058"
---
# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Netwerkconfiguratiedetails voor App Service-omgevingen met ExpressRoute
## <a name="overview"></a>Overzicht
Klanten kunnen verbinding maken met een [Azure ExpressRoute] [ ExpressRoute] circuit in hun virtuele netwerken-infrastructuur, waardoor hun on-premises netwerk uitbreiden naar Azure.  Een App Service Environment kan worden gemaakt in een subnet van deze [virtueel netwerk] [ virtualnetwork] infrastructuur.  Apps die worden uitgevoerd op de App Service-omgeving kunnen vervolgens alleen via de ExpressRoute-verbinding tot stand brengen veilige verbindingen met back-end-resources die toegankelijk is.  

Een App Service Environment kan worden gemaakt in **beide** een virtueel netwerk van Azure Resource Manager, **of** een klassieke implementatie model virtueel netwerk.  Met een recente wijziging in juni 2016, kunnen nu ook as-omgevingen worden geïmplementeerd in virtuele netwerken die gebruikmaken van openbare-adresbereiken of RFC1918 adresruimten (dat wil zeggen particuliere adressen). 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Vereiste netwerkverbinding
Er zijn vereisten voor App Service-omgevingen die niet in eerste instantie kan worden voldaan in een virtueel netwerk is verbonden met een ExpressRoute-connectiviteit.  App Service-omgevingen vereisen meer van de volgende alleen juist werken:

* Uitgaande netwerkconnectiviteit met Azure Storage-eindpunten die over de hele wereld zijn op beide poorten 80 en 443.  Dit omvat eindpunten die zich in dezelfde regio als de App Service-omgeving, evenals de storage-eindpunten die zich in **andere** Azure-regio's.  Azure Storage-eindpunten kunt oplossen door onder de volgende DNS-domeinen: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* en *file.core.windows.net*.  
* Uitgaande netwerkconnectiviteit met de Azure Files-service op poort 445.
* Uitgaande netwerkconnectiviteit met Sql DB-eindpunten die zich in dezelfde regio als de App Service-omgeving.  SQL DB-eindpunten onder het volgende domein oplossen: *database.windows.net*.  Hiervoor wordt de toegang tot poorten 1433, 11000 11999 en 14000 14999 te openen.  Zie voor meer informatie [in dit artikel over het gebruik van Sql Database V12 poort](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
* Uitgaande netwerkconnectiviteit met de Azure management-gegevenslaag-eindpunten (zowel ASM en ARM-eindpunten).  Dit omvat uitgaande connectiviteit voor beide *management.core.windows.net* en *management.azure.com*. 
* Uitgaande netwerkconnectiviteit te *ocsp.msocsp.com*, *mscrl.microsoft.com* en *crl.microsoft.com*.  Dit is nodig voor de ondersteuning van SSL-functionaliteit.
* De DNS-configuratie voor het virtuele netwerk moet zijn geschikt voor het oplossen van alle eindpunten en domeinen die worden vermeld in de vorige punten.  Als deze eindpunten kunnen niet worden omgezet, mislukken pogingen tot het maken van App Service-omgeving en bestaande App Service-omgevingen worden gemarkeerd als niet in orde.
* Uitgaand verkeer op poort 53 is vereist voor communicatie met de DNS-servers.
* Als een aangepaste DNS-server op het andere uiteinde van een VPN-gateway bestaat, moet de DNS-server bereikbaar is vanaf het subnet met de App Service-omgeving zijn. 
* Het pad uitgaand netwerkverkeer via de interne zakelijke proxy's kan niet reizen, noch geforceerde tunnels naar on-premises te gebruiken zijn.  In dat geval het effectieve NAT-adres van uitgaand netwerkverkeer wordt gewijzigd van de App Service-omgeving.  Het NAT-adres van een App Service-omgeving uitgaand netwerkverkeer wijzigt, worden fouten in de netwerkconnectiviteit op veel van de hierboven vermelde eindpunten.  Dit resulteert in mislukte pogingen van de App Service-omgeving maken, evenals eerder in orde App Service-omgevingen wordt gemarkeerd als niet in orde.  
* Binnenkomende toegang tot het netwerk naar de vereiste poorten voor App Service-omgevingen moet worden toegestaan, zoals beschreven in dit [artikel][requiredports].

De DNS-vereisten kunnen worden voldaan door ervoor te zorgen dat een geldig DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk.  Als voor een of andere reden die de DNS-configuratie wordt gewijzigd nadat een App Service Environment is gemaakt, ontwikkelaars een App Service-omgeving om op te halen de nieuwe DNS-configuratie afdwingt kunnen.  Activeren van de rolling omgeving opnieuw worden opgestart met het pictogram "Restart" zich boven aan de blade voor het beheer van App Service-omgeving in de [Azure-portal] [ NewPortal] zorgt ervoor dat de omgeving om op te halen de nieuwe DNS-server de configuratie.

De netwerkvereisten voor inkomende toegang kunnen worden voldaan door het configureren van een [netwerkbeveiligingsgroep] [ NetworkSecurityGroups] op van de ase-subnet waarmee de vereiste toegang zoals beschreven in dit [ artikel][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Uitgaande netwerkconnectiviteit inschakelen voor een App Service Environment
Een nieuwe ExpressRoute-circuit wordt standaard een standaardroute waarmee uitgaande verbinding met Internet.  Met deze configuratie wordt een App Service Environment worden geen verbinding maken met andere Azure-eindpunten.

Een veelvoorkomende configuratie van de klant is echter voor het definiëren van hun eigen standaardroute (0.0.0.0/0) die ervoor zorgt uitgaand internetverkeer dat naar in plaats daarvan flow on-premises.  App Service-omgevingen verbroken met deze verkeersstroom altijd omdat het uitgaande verkeer geblokkeerd on-premises wordt en NAT wilt een onherkenbare set met adressen die niet meer met verschillende Azure-eindpunten werken.

De oplossing is voor het definiëren van een (of meer) door de gebruiker gedefinieerde routes (udr's) op het subnet waarin de App Service-omgeving.  Een UDR definieert de subnet-specifieke routes die worden gebruikt in plaats van de standaardroute.

Het verdient indien mogelijk, gebruik de volgende configuratie:

* De ExpressRoute-configuratie adverteert 0.0.0.0/0 en standaard geforceerde tunnels al het uitgaande verkeer on-premises.
* De UDR toegepast op het subnet met de App Service Environment definieert 0.0.0.0/0 met een volgend hoptype van Internet (een voorbeeld hiervan is verder omlaag in dit artikel).

Het gecombineerde effect van deze stappen is dat het subnetniveau UDR voorrang krijgt boven de ExpressRoute geforceerde tunneling, dus ervoor te zorgen dat uitgaande toegang tot Internet van de App Service Environment.

> [!IMPORTANT]
> De routes zijn gedefinieerd in een UDR **moet** specifiek genoeg voor voorrang boven alle routes die door de configuratie van ExpressRoute aangekondigd.  In het volgende voorbeeld wordt het brede adresbereik 0.0.0.0/0 gebruikt, en als zodanig kan mogelijk per ongeluk worden overschreven door routeadvertenties met behulp van specifiekere adresbereiken.
> 
> App Service-omgevingen worden niet ondersteund met ExpressRoute-configuraties die **advertentieoverschrijdende routes van de openbare-peeringpad naar het pad voor persoonlijke peering**.  ExpressRoute-configuraties waarvoor openbare peering is geconfigureerd, ontvangen routeadvertenties van Microsoft voor een groot aantal Microsoft Azure IP-adresbereiken.  Als deze adresbereiken advertentieoverschrijdend op het pad voor persoonlijke peering, wordt het eindresultaat is dat alle uitgaande netwerkpakketten vanuit de App Service-omgeving subnet zal geforceerde aan van een klant on-premises netwerkinfrastructuur.  Deze netwerkstroom wordt momenteel niet ondersteund met App Service-omgevingen.  Een mogelijke oplossing voor dit probleem is om te stoppen advertentieoverschrijdende routes van het pad voor openbare peering naar het pad voor persoonlijke peering.
> 
> 

Achtergrondinformatie over de gebruiker gedefinieerde routes is beschikbaar in deze [overzicht][UDROverview].  

Meer informatie over het maken en configureren van de gebruiker gedefinieerde routes is beschikbaar in deze [hoe naar handleiding][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Voorbeeld van de UDR-configuratie voor een App Service Environment
**Vereisten**

1. Installeer Azure Powershell via de [pagina Azure Downloads] [ AzureDownloads] (gedateerd juni 2015 of hoger).  Onder "Opdrachtregelprogramma's" is er een koppeling 'Installeren' onder 'Windows Powershell' waarop de meest recente Powershell-cmdlets wordt geïnstalleerd.
2. Het verdient aanbeveling een unieke subnet wordt gemaakt voor exclusief gebruik door een App Service Environment.  Dit zorgt ervoor dat de udr's toegepast op het subnet alleen uitgaand verkeer voor de App Service Environment wordt geopend.
3. **Belangrijke**: implementeer de App Service-omgeving tot geen **nadat** de volgende configuratiestappen uit worden gevolgd.  Dit zorgt ervoor dat de uitgaande netwerkconnectiviteit beschikbaar is voor het implementeren van een App Service Environment.

**Stap 1: Een benoemde routetabel maken**

Het volgende codefragment maakt u een routetabel met de naam 'DirectInternetRouteTable' in de regio West ons Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Stap 2: Een of meer routes in de routetabel maken**

U moet een of meer routes toevoegen aan de routetabel, zodat uitgaande toegang tot Internet.  

De aanbevolen aanpak voor het configureren van uitgaande toegang tot Internet is voor het definiëren van een route voor 0.0.0.0/0 zoals hieronder wordt weergegeven.

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Houd er rekening mee dat 0.0.0.0/0 is een brede adresbereik en als zodanig wordt overschreven door specifiekere adresbereiken door de ExpressRoute aangekondigd.  Als u wilt herhalen opnieuw de eerdere aanbeveling, moet een UDR met een route 0.0.0.0/0 worden gebruikt in combinatie met een ExpressRoute-configuratie die alleen ook 0.0.0.0/0 kondigt. 

Als alternatief kunt kunt u een uitgebreide en bijgewerkte lijst met CIDR-bereiken die wordt gebruikt door Azure downloaden.  De Xml-bestand met alle van de Azure-IP-adresbereiken is beschikbaar via de [Microsoft Download Center][DownloadCenterAddressRanges].  

Houd er echter rekening mee dat u deze bereiken wijzigen gedurende een periode, dus blijkt periodieke handmatige updates voor de gebruiker gedefinieerde routes om synchroon te houden.  Ook, omdat er een standaard bovenste limiet van 100 routes in een enkele UDR, moet u de Azure-IP-adresbereiken om aan te passen binnen de limiet van 100 route 'summarize' in gedachten dat UDR routes hoeven te zijn dan de routes specifieker gedefinieerd aangekondigd door uw ExpressRo ute.  

**Stap 3: De routetabel aan het subnet met de App Service-omgeving koppelen**

De laatste configuratiestap is om te koppelen van de routetabel aan het subnet waarop de App Service Environment wordt geïmplementeerd.  De volgende opdracht wordt gekoppeld aan de 'DirectInternetRouteTable' de 'ASESubnet"die uiteindelijk een App Service-omgeving bevat.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Stap 4: Laatste stap**

Nadat de routetabel is gekoppeld aan het subnet, is het aanbevolen om eerst te testen en controleer of het gewenste effect.  Bijvoorbeeld, een virtuele machine implementeren in het subnet en Bevestig dat:

* Uitgaand verkeer naar Azure en niet-Azure-eindpunten die worden vermeld eerder in dit artikel is **niet** het ExpressRoute-circuit stromen.  Het is heel belangrijk om te controleren of dit probleem, omdat als uitgaande verkeer vanuit het subnet is nog steeds wordt gedwongen on-premises App Service-omgeving altijd mislukt het maken van tunnels. 
* DNS-zoekacties voor de eindpunten die eerder vermeld worden alle correct kan omzetten. 

Zodra de bovenstaande stappen zijn bevestigd, moet u de virtuele machine niet verwijderen omdat het subnet moet worden 'lege' op het moment dat de App Service-omgeving is gemaakt.

Ga daarna verder met het maken van een App Service Environment!

## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service-omgevingen, Zie [Inleiding tot App Service-omgeving][IntroToAppServiceEnvironment]

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: app-service-app-service-environment-control-inbound-traffic.md
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[AzureDownloads]: http://azure.microsoft.com/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[NewPortal]:  https://portal.azure.com


<!-- IMAGES -->
