---
title: Veilig verbinding te maken met back-Endresources van een App-serviceomgeving
description: Meer informatie over veilige manier verbinding maken met back-endresources van een App Service-omgeving.
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: a43d88d64710b95dd753c19f02582f22accac8b6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Veilig verbinding te maken met back-Endresources van een App-serviceomgeving
## <a name="overview"></a>Overzicht
Nadat een App Service-omgeving wordt altijd gemaakt **beide** een virtueel netwerk van Azure Resource Manager, **of** een klassieke implementatiemodel [virtueel netwerk] [ virtualnetwork], uitgaande verbindingen van een App Service-omgeving naar andere bronnen van de back-end kunnen stromen uitsluitend via het virtuele netwerk.  Met een recente wijziging in juni 2016, worden ASEs ook geïmplementeerd in virtuele netwerken die gebruikmaken van openbare-adresbereiken of RFC1918 adresruimten (dat wil zeggen particuliere adressen).  

Bijvoorbeeld, kan er een SQL-Server uitgevoerd op een cluster van virtuele machines met poort 1433 vergrendeld.  Het eindpunt mogelijk ACLd alleen toegang toestaan via andere bronnen in hetzelfde virtuele netwerk.  

Als een ander voorbeeld gevoelige eindpunten on-premises kan worden uitgevoerd en naar Azure worden verbonden via een [Site-naar-Site] [ SiteToSite] of [Azure ExpressRoute] [ ExpressRoute] verbindingen.  Als gevolg hiervan zich worden alleen de resources in virtuele netwerken die zijn verbonden met de Site-naar-Site of een ExpressRoute-tunnels voor toegang tot lokale eindpunten.

Voor elk van deze scenario's, apps die worden uitgevoerd op een App Service-omgeving kan worden veilig verbinding kunnen maken met de verschillende servers en -bronnen.  Uitgaand verkeer van apps die worden uitgevoerd in een App Service-omgeving naar persoonlijke eindpunten in hetzelfde virtuele netwerk (of verbonden met hetzelfde virtuele netwerk), wordt alleen overdracht via het virtuele netwerk.  Uitgaand verkeer naar persoonlijke eindpunten worden niet overgebracht via het openbare Internet.

Een voorbehoud geldt voor uitgaand verkeer van een App Service-omgeving naar eindpunten binnen een virtueel netwerk.  App Service-omgevingen niet bereiken eindpunten van virtuele machines zich in de **dezelfde** subnet als de App Service-omgeving.  Dit mag normaal gesproken geen probleem, zolang het App Service-omgevingen zijn geïmplementeerd in een subnet is gereserveerd voor exclusief gebruik door alleen de App Service-omgeving.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Uitgaande verbinding en DNS-vereisten
Voor een App Service-omgeving te laten functioneren, vereist deze uitgaande toegang tot verschillende eindpunten. Een volledige lijst met de externe eindpunten die worden gebruikt door een as-omgeving is in de sectie 'Netwerkverbinding vereist' van de [netwerkconfiguratie voor ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.

App Service-omgevingen moeten een geldige DNS-infrastructuur is geconfigureerd voor het virtuele netwerk.  Als voor een bepaalde reden worden de DNS-configuratie is gewijzigd nadat een App-serviceomgeving is gemaakt, kunnen ontwikkelaars een App Service-omgeving naar de nieuwe DNS-configuratie worden opgepikt afdwingen.  Activering van rolling omgeving opgestart met het pictogram 'Opnieuw starten' boven aan de blade voor het beheer van App Service-omgeving in de portal zal de omgeving voor de nieuwe DNS-configuratie worden opgepikt.

Het is ook raadzaam dat alle aangepaste DNS-servers op het vnet ingesteld worden voordat u een App-serviceomgeving tevoren.  Als een virtueel netwerk DNS-configuratie wordt gewijzigd terwijl een App Service-omgeving wordt gemaakt, wordt die leiden tot het mislukken van de App Service-omgeving maken van het proces.  In een vergelijkbare vein als een aangepaste DNS-server op het andere einde van een VPN-gateway bestaat en de DNS-server onbereikbaar is of niet beschikbaar is, is mislukt het proces voor het maken van App Service-omgeving ook.

## <a name="connecting-to-a-sql-server"></a>Verbinding maken met een SQL-Server
Een algemene configuratie van SQL Server heeft een eindpunt luisteren op poort 1433:

![SQL Server-eindpunt][SqlServerEndpoint]

Er zijn twee methoden voor het beperken van verkeer naar dit eindpunt:

* [Network Access Control Lists] [ NetworkAccessControlLists] (netwerk-ACL's)
* [Netwerkbeveiligingsgroepen][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Beperken van toegang tot aan een ACL-netwerk
Poort 1433 kan worden beveiligd met behulp van een ACL (toegangsbeheerlijst) netwerk.  In het voorbeeld hieronder whitelists client adressen die afkomstig zijn van binnen een virtueel netwerk en toegang tot alle andere clients worden geblokkeerd.

![Voorbeeld van een besturingselement toegang netwerk][NetworkAccessControlListExample]

Alle toepassingen die in App Service-omgeving in hetzelfde virtuele netwerk als de SQL-Server verbinding kunnen maken met de SQL Server-exemplaar op met de **VNet interne** IP-adres voor de virtuele machine van SQL Server.  

De verbindingsreeks voorbeeld verwijst naar de SQL-Server de privé IP-adres.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Hoewel de virtuele machine een openbaar eindpunt ook heeft, wordt verbindingspogingen met het openbare IP-adres wordt geweigerd vanwege de ACL van het netwerk. 

## <a name="restricting-access-with-a-network-security-group"></a>Beperken van toegang met een Netwerkbeveiligingsgroep
Er is een alternatieve methode voor het beveiligen van toegang met een netwerkbeveiligingsgroep.  Netwerkbeveiligingsgroepen kunnen worden toegepast op afzonderlijke virtuele machines, of op een subnet met virtuele machines.

Een netwerkbeveiligingsgroep moet eerst worden gemaakt:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Toegang beperken tot alleen VNet interne verkeer is zeer eenvoudig met een netwerkbeveiligingsgroep.  De standaardregels in een netwerkbeveiligingsgroep alleen toestaan toegang van andere netwerkclients in hetzelfde virtuele netwerk.

Als gevolg hiervan vergrendelen toegang tot SQL Server is net zo eenvoudig als het toepassen van een netwerkbeveiligingsgroep met de standaardregels op beide virtuele machines met SQL Server of het subnet met de virtuele machines.

Het onderstaande voorbeeld geldt een netwerkbeveiligingsgroep voor het subnet met:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

Het eindresultaat is een set beveiligingsregels voor verbindingen die externe toegang, terwijl u VNet interne toegang blokkeren:

![Standaard-Netwerkbeveiligingsregels][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgevingen, Zie [Inleiding tot de App Service-omgeving][IntroToAppServiceEnvironment]

Zie voor meer informatie om binnenkomend verkeer naar uw App Service-omgeving beheren [binnenkomend verkeer naar een App-serviceomgeving beheren][ControlInboundASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[ControlInboundASE]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
