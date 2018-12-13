---
title: Details van netwerkconfiguratie voor Azure ExpressRoute
description: Netwerkconfiguratiedetails voor App Service Environment voor PowerApps in virtuele netwerken die zijn verbonden aan een Azure ExpressRoute-circuit.
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
ms.custom: seodec18
ms.openlocfilehash: 8dc6f595f312326f4082af9f875fefddc3a5fb8d
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53320706"
---
# <a name="network-configuration-details-for-app-service-environment-for-powerapps-with-azure-expressroute"></a>Netwerkconfiguratiedetails voor App Service Environment voor PowerApps met Azure ExpressRoute

Klanten kunnen verbinding maken met een [Azure ExpressRoute] [ ExpressRoute] circuit naar hun virtuele netwerkinfrastructuur hun on-premises netwerk uitbreiden naar Azure. App Service-omgeving is gemaakt in een subnet van de [virtueel netwerk] [ virtualnetwork] infrastructuur. Apps die worden uitgevoerd op App Service-omgeving tot stand brengen van beveiligde verbindingen in de back-end-bronnen die toegankelijk alleen via de ExpressRoute-verbinding zijn.  

App Service-omgeving kan worden gemaakt in deze scenario's:
- Virtuele netwerken van Azure Resource Manager.
- Virtuele netwerken voor de klassieke implementatie-model.
- Virtuele netwerken die gebruikmaken van openbare-adresbereiken of RFC1918-adresruimten (dat wil zeggen, particuliere adressen). 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Vereiste netwerkverbinding

App Service-omgeving heeft netwerkverbindingsvereisten die in eerste instantie niet kunnen worden voldaan in een virtueel netwerk dat verbonden met ExpressRoute.

App Service-omgeving vereist de volgende netwerkinstellingen voor een verbinding te laten functioneren:

* Uitgaande netwerkconnectiviteit met Azure Storage eindpunten over de hele wereld op poort 80 en poort 443. Deze eindpunten bevinden zich in dezelfde regio als de App Service-omgeving, evenals andere Azure-regio's. Azure Storage-eindpunten kunt oplossen door onder de volgende DNS-domeinen: table.core.windows.net, blob.core.windows.net queue.core.windows.net en file.core.windows.net.  

* Uitgaande netwerkconnectiviteit met de Azure Files-service op poort 445.

* Uitgaande netwerkconnectiviteit met Azure SQL Database-eindpunten die zich in dezelfde regio als de App Service-omgeving. SQL Database-eindpunten oplossen onder het domein database.windows.net, waarvoor toegang tot poorten 1433, 11000 11999 en 14000 14999 Zie voor meer informatie over het gebruik van SQL Database V12 poort [poorten boven 1433 voor ADO.NET 4.5](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).

* Uitgaande netwerkconnectiviteit met de Azure-beheerlaag-eindpunten (Azure klassieke implementatiemodel en Azure Resource Manager-eindpunten). Verbinding met deze eindpunten bevat de domeinen management.core.windows.net en management.azure.com. 

* Uitgaande netwerkconnectiviteit met de domeinen ocsp.msocsp.com mscrl.microsoft.com en crl.microsoft.com. Verbinding met deze domeinen is nodig voor de ondersteuning van SSL-functionaliteit.

* De DNS-configuratie voor het virtuele netwerk moet kunnen omzetten van alle eindpunten en domeinen die in dit artikel worden vermeld. Als de eindpunten kunnen niet worden omgezet, mislukt het maken van App Service-omgeving. Een bestaande App Service-omgeving is gemarkeerd als niet in orde zijn.

* Uitgaand verkeer op poort 53 is vereist voor communicatie met de DNS-servers.

* Als een aangepaste DNS-server op het andere uiteinde van een VPN-gateway bestaat, moet de DNS-server bereikbaar is vanaf het subnet waarin App Service-omgeving zijn. 

* Het pad uitgaand netwerkverkeer via de interne zakelijke proxy's kan niet reizen en kan niet worden afdwingen via een tunnel on-premises. Deze acties wijzigen het effectieve NAT-adres van uitgaand netwerkverkeer van App Service-omgeving. Wijzigingen aan de NAT-adres van uitgaand netwerkverkeer App Service-omgeving leiden tot verbindingsfouten in veel van de eindpunten. App Service-omgeving maken is mislukt. Een bestaande App Service-omgeving is gemarkeerd als niet in orde zijn.

* Inkomende netwerktoegang tot de vereiste poorten voor App Service-omgeving moet worden toegestaan. Zie voor meer informatie, [over het beheren van inkomend verkeer naar App Service-omgeving][requiredports].

Om te voldoen aan de DNS-vereisten, zorg ervoor dat een geldig DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk. Als de DNS-configuratie wordt gewijzigd nadat de App Service-omgeving is gemaakt, kunnen ontwikkelaars van App Service-omgeving om op te halen de nieuwe DNS-configuratie forceren. U kunt een rolling omgeving opnieuw opstarten activeren met behulp van de **opnieuw** pictogram onder beheer van de App Service-omgeving in de [Azure portal] [NewPortal]. Het opnieuw opstarten zorgt ervoor dat de omgeving om op te halen de nieuwe DNS-configuratie.

Configureren om te voldoen aan de netwerkvereisten voor inkomende toegang, een [netwerkbeveiligingsgroep (NSG)][NetworkSecurityGroups] op het subnet van de App Service-omgeving. De NSG kan de vereiste toegang [voor het beheren van inkomend verkeer naar App Service-omgeving][requiredports].

## <a name="outbound-network-connectivity"></a>Uitgaande netwerkconnectiviteit

Een nieuwe ExpressRoute-circuit wordt standaard een standaardroute waarmee uitgaande verbinding met internet. App Service-omgeving kunt deze configuratie gebruiken om te verbinden met andere Azure-eindpunten.

Er is een algemene configuratie van de klant voor het definiëren van hun eigen standaardroute (0.0.0.0/0), waardoor het uitgaande internetverkeer naar flow on-premises. Deze verkeersstroom wordt altijd App Service-omgeving. Het uitgaande verkeer wordt geblokkeerd on-premises of NAT wilt een onherkenbare set met adressen die niet meer met verschillende Azure-eindpunten werken.

De oplossing is voor het definiëren van een (of meer) gebruiker gedefinieerde routes (udr's) op het subnet waarin App Service-omgeving. Een UDR definieert de subnet-specifieke routes die worden herkend in plaats van de standaardroute.

Gebruik indien mogelijk de volgende configuratie:

* De ExpressRoute-configuratie adverteert 0.0.0.0/0. De configuratie van geforceerde tunnels standaard al het uitgaande verkeer on-premises.
* De UDR toegepast op het subnet waarin App Service-omgeving definieert 0.0.0.0/0 met een volgend hoptype van internet. Een voorbeeld van deze configuratie wordt verderop in dit artikel beschreven.

Het gecombineerde effect van deze configuratie is dat de UDR subnetniveau voorrang hebben op de ExpressRoute geforceerde tunneling. Uitgaande internettoegang van App Service Environment kan worden gegarandeerd.

> [!IMPORTANT]
> De routes zijn gedefinieerd in een UDR moeten specifiek genoeg zijn om voorrang boven alle routes die worden geadverteerd door de ExpressRoute-configuratie te zijn. In het voorbeeld dat wordt beschreven in de volgende sectie wordt het brede adresbereik 0.0.0.0/0 gebruikt. Dit bereik kan per ongeluk worden overschreven door routeadvertenties die gebruikmaken van specifiekere adresbereiken.
> 
> App Service-omgeving wordt niet ondersteund met ExpressRoute-configuraties die routes van de openbare-peeringpad naar het pad voor persoonlijke peering advertentieoverschrijdende. ExpressRoute-configuraties waarvoor openbare peering is geconfigureerd, ontvangen routeadvertenties van Microsoft voor een groot aantal Microsoft Azure IP-adresbereiken. Als deze adresbereiken advertentieoverschrijdend op het pad voor persoonlijke peering, worden alle uitgaande netwerkpakketten vanuit het ase-subnet geforceerde tunnels naar on-premises-netwerkinfrastructuur van de klant te gebruiken. Deze netwerkstroom wordt momenteel niet ondersteund met App Service-omgeving. Eén oplossing is om te stoppen advertentieoverschrijdende routes van het pad voor openbare peering naar het pad voor persoonlijke peering.
> 
> 

Zie voor achtergrondinformatie over door de gebruiker gedefinieerde routes, [routering van verkeer van virtuele netwerken][UDROverview].  

Zie voor meer informatie over het maken en configureren van de gebruiker gedefinieerde routes, [netwerkverkeer routeren met een routetabel met behulp van PowerShell] [UDRHowTo].

## <a name="udr-configuration"></a>UDR-configuratie

In deze sectie bevat een voorbeeld van de UDR-configuratie voor App Service-omgeving.

### <a name="prerequisites"></a>Vereisten

* Installeer Azure PowerShell via de [Azure Downloads page] [AzureDownloads]. Kies een download met een datum van juni 2015 of hoger. Onder **opdrachtregelprogramma's** > **Windows PowerShell**, selecteer **installeren** voor het installeren van de meest recente PowerShell-cmdlets.

* Maak een unieke subnet voor exclusief gebruik door App Service-omgeving. De unieke subnet zorgt ervoor dat de udr's toegepast alleen op het subnet open uitgaand verkeer voor App Service-omgeving.

> [!IMPORTANT]
> App Service-omgeving alleen implementeren nadat u de configuratiestappen hebt voltooid. De stappen zorg uitgaande netwerkverbinding beschikbaar is, voordat u probeert te implementeren App Service-omgeving.

### <a name="step-1-create-a-route-table"></a>Stap 1: Een routetabel maken

Maak een routetabel met de naam **DirectInternetRouteTable** in de regio West ons Azure, zoals wordt weergegeven in dit fragment:

`New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest`

### <a name="step-2-create-routes-in-the-table"></a>Stap 2: Routes in de tabel maken

Routes toevoegen aan de routetabel waarmee uitgaande toegang tot internet.  

Uitgaande toegang tot het internet configureren. Een route voor 0.0.0.0/0 definiëren, zoals wordt weergegeven in dit fragment:

`Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet`

0.0.0.0/0 is een brede adresbereik. Het bereik wordt overschreven door de adresbereiken van ExpressRoute aangekondigd dat specifieker zijn. Een UDR met een route 0.0.0.0/0 moet worden gebruikt in combinatie met een ExpressRoute-configuratie die alleen 0.0.0.0/0 adverteert. 

Als alternatief, een huidige, uitgebreide lijst CIDR-bereiken die wordt gebruikt door Azure te downloaden. Het XML-bestand voor alle Azure-IP-adresbereiken is beschikbaar in de [Microsoft Download Center] [DownloadCenterAddressRanges].  

> [!NOTE]
>
> De Azure-IP-adresbereiken veranderen verloop van tijd. Gebruiker gedefinieerde routes moeten periodieke handmatige updates om synchroon te houden.
>
> Een enkele UDR heeft een standaard bovenste limiet van 100 routes. U moet de Azure-IP-adresbereiken om aan te passen binnen de limiet van 100-route 'summarize'. UDR gedefinieerde routes moeten specifieker dan routes die worden geadverteerd door uw ExpressRoute-verbinding.
> 

### <a name="step-3-associate-the-table-to-the-subnet"></a>Stap 3: De tabel aan het subnet koppelen

De routetabel aan het subnet waar u van plan bent om te implementeren App Service-omgeving koppelen. Met deze opdracht wordt gekoppeld aan de **DirectInternetRouteTable** tabel naar de **ASESubnet** subnet met App Service-omgeving.

`Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'`

### <a name="step-4-test-and-confirm-the-route"></a>Stap 4: Testen en de route bevestigen

Nadat de routetabel is gekoppeld aan het subnet, test en controleer of de route.

Een virtuele machine implementeren in het subnet en bevestig deze voorwaarden:

* Uitgaand verkeer naar de Azure- en niet-Azure-eindpunten die worden beschreven in dit artikel biedt **niet** stroom het ExpressRoute-circuit. Als het uitgaande verkeer vanuit het subnet geforceerd is mislukt via een tunnel on-premises altijd het App Service-omgeving maken.
* DNS-zoekacties voor de eindpunten beschreven in dit artikel alle correct worden omgezet. 

Nadat u de configuratiestappen uit en controleer of de route, verwijder de virtuele machine. Het subnet moet 'lege' wanneer App Service-omgeving wordt gemaakt.

U bent nu klaar om te implementeren App Service-omgeving.

## <a name="next-steps"></a>Volgende stappen

Als u wilt aan de slag met App Service Environment voor PowerApps, Zie [Inleiding tot App Service-omgeving] [IntroToAppServiceEnvironment].

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[requiredports]: app-service-app-service-environment-control-inbound-traffic.md
[networkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
<!-- Old link -- [UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/ --> [UDRHowTo]: https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell [HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md [AzureDownloads]: https://azure.microsoft.com/downloads/ [DownloadCenterAddressRanges]: https://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/ [IntroToAppServiceEnvironment]: app-service-app-service-environment-intro.md [NewPortal]:  https://portal.azure.com


<!-- IMAGES -->
