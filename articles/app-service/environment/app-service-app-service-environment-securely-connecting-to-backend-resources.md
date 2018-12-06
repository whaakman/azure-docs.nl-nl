---
title: Veilig verbinding maken met back-Endresources van een App Service Environment
description: Meer informatie over hoe u veilig verbinding maken met back-endresources van een App Service Environment.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 4fccfbb4cbc1b0db0c032d95ad66b497520aa619
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52960937"
---
# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Veilig verbinding maken met back-Endresources van een App Service Environment
## <a name="overview"></a>Overzicht
Nadat een App Service Environment wordt altijd gemaakt **beide** een virtueel netwerk van Azure Resource Manager, **of** een klassiek implementatiemodel [virtueel netwerk] [ virtualnetwork], uitgaande verbindingen van een App Service Environment met andere back-endresources kunnen stromen uitsluitend via het virtuele netwerk.  Met een recente wijziging in juni 2016, kunnen ook as-omgevingen worden geïmplementeerd in virtuele netwerken die gebruikmaken van openbare-adresbereiken of RFC1918 adresruimten (dat wil zeggen particuliere adressen).  

Bijvoorbeeld, kunnen er een SQL-Server die wordt uitgevoerd op een cluster van virtuele machines met poort 1433 vergrendeld.  Het eindpunt mogelijk ACLd zodat u alleen toegang vanaf andere resources in hetzelfde virtuele netwerk.  

Als een ander voorbeeld: gevoelige eindpunten kunnen on-premises uitgevoerd en worden verbonden met Azure via een [Site-naar-Site] [ SiteToSite] of [Azure ExpressRoute] [ ExpressRoute] verbindingen.  Als gevolg hiervan zich alleen resources in virtuele netwerken die zijn verbonden met de Site-naar-Site of een ExpressRoute-tunnels voor toegang tot on-premises eindpunten.

Voor al deze scenario's is apps die worden uitgevoerd op een App Service Environment geen veilig verbinding maken met de verschillende servers en -resources.  Uitgaand verkeer van apps die worden uitgevoerd in een App Service Environment met privé-eindpunten in hetzelfde virtuele netwerk (of die zijn verbonden met hetzelfde virtuele netwerk), wordt alleen flow via het virtuele netwerk.  Uitgaand verkeer naar persoonlijke eindpunten worden niet overgebracht via het openbare Internet.

Een nadeel is van toepassing op het uitgaande verkeer van een App Service Environment naar eindpunten binnen een virtueel netwerk.  App Service-omgevingen tijdelijk niet bereikbaar voor eindpunten van virtuele machines die zich de **dezelfde** subnet als de App Service-omgeving.  Dit mag normaal geen probleem, zolang het App Service-omgevingen zijn geïmplementeerd in een subnet gereserveerd voor exclusief gebruik door alleen de App Service-omgeving.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Uitgaande connectiviteit en DNS-vereisten
Voor een App Service Environment te laten functioneren, is het uitgaande toegang tot verschillende eindpunten vereist. Een volledige lijst van de externe eindpunten die worden gebruikt door een as-omgeving is in de sectie 'Netwerkverbinding vereist' van de [netwerkconfiguratie voor ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.

App Service-omgevingen vereist een geldige DNS-infrastructuur is geconfigureerd voor het virtuele netwerk.  Als voor een of andere reden die de DNS-configuratie wordt gewijzigd nadat een App Service Environment is gemaakt, ontwikkelaars een App Service-omgeving om op te halen de nieuwe DNS-configuratie afdwingt kunnen.  Een rolling omgeving opnieuw opstarten met behulp van het pictogram 'Restart' aan de bovenkant van de App Service Environment management-blade in de portal wordt geactiveerd zorgt ervoor dat de omgeving om op te halen de nieuwe DNS-configuratie.

Het is ook raadzaam dat alle aangepaste DNS-servers op het vnet ingesteld tevoren worden vóór het maken van een App Service Environment.  Als een virtueel netwerk DNS-configuratie is gewijzigd terwijl een App Service-omgeving wordt gemaakt, resulteert dat in het App Service-omgeving maken van het proces mislukt.  In een vergelijkbaar vein als een aangepaste DNS-server op het andere uiteinde van een VPN-gateway bestaat en de DNS-server niet bereikbaar is of niet beschikbaar is, is mislukken het proces voor het maken van App Service-omgeving ook.

## <a name="connecting-to-a-sql-server"></a>Verbinding maken met een SQL-Server
Een algemene SQL Server-configuratie heeft een eindpunt dat luistert op poort 1433:

![Eindpunt voor SQL Server][SqlServerEndpoint]

Er zijn twee methoden voor het beperken van verkeer naar dit eindpunt:

* [Network Access Control Lists] [ NetworkAccessControlLists] (netwerk ACL's)
* [Netwerkbeveiligingsgroepen][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Beperken van toegang tot met een netwerk-ACL
Poort 1433 kan worden beveiligd met behulp van een network access control list.  Het voorbeeld hieronder accounttoewijzing client-adressen die afkomstig zijn van binnen een virtueel netwerk en blokkeert de toegang tot alle andere clients.

![Voorbeeld van een besturingselement toegang netwerk][NetworkAccessControlListExample]

Alle toepassingen die in App Service-omgeving worden uitgevoerd in hetzelfde virtuele netwerk als de SQL-Server geen verbinding maken met de SQL Server-exemplaar met de **VNet interne** IP-adres voor de SQL Server-machine.  

De verbindingsreeks voorbeeld verwijst naar de SQL-Server met behulp van de privé IP-adres.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Hoewel de virtuele machine ook een openbaar eindpunt heeft, wordt verbindingspogingen met het openbare IP-adres wordt geweigerd vanwege de netwerk ACL. 

## <a name="restricting-access-with-a-network-security-group"></a>Beperken van toegang tot met een Netwerkbeveiligingsgroep
Er is een alternatieve methode voor het beveiligen van toegang met een netwerkbeveiligingsgroep.  Netwerkbeveiligingsgroepen kunnen worden toegepast op afzonderlijke virtuele machines of op een subnet met virtuele machines.

Een netwerkbeveiligingsgroep moet eerst worden gemaakt:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Toegang beperken tot alleen intern VNet verkeer is zeer eenvoudig met een netwerkbeveiligingsgroep.  De standaardregels in een netwerkbeveiligingsgroep wordt alleen toegang toestaan vanuit andere netwerkclients in hetzelfde virtuele netwerk.

Als gevolg hiervan vergrendelen van toegang tot SQL Server is net zo eenvoudig als het toepassen van een netwerkbeveiligingsgroep met de standaardregels op een van beide de virtuele machines met SQL Server of het subnet met de virtuele machines.

Het onderstaande voorbeeld van een netwerkbeveiligingsgroep naar het subnet met toepassing:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

Het eindresultaat is een set beveiligingsregels die externe toegang blokkeren terwijl u VNet interne toegang toe te staan:

![Standaard Netwerkbeveiligingsregels][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service-omgevingen, Zie [Inleiding tot App Service-omgeving][IntroToAppServiceEnvironment]

Zie voor meer informatie over het beheren van inkomend verkeer naar uw App Service Environment [binnenkomend verkeer naar een App Service-omgeving beheren][ControlInboundASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SiteToSite]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[ControlInboundASE]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
