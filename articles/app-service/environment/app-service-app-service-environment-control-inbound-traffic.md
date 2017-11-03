---
title: Het inkomende verkeer naar een App Service-omgeving beheren
description: Meer informatie over het configureren van netwerk-beveiligingsregels voor binnenkomend verkeer naar een App Service-omgeving beheren.
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: 
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.openlocfilehash: ed72bf3202d6cb2d2161bc0df693d3e6a1fc58ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Het inkomende verkeer naar een App Service-omgeving beheren
## <a name="overview"></a>Overzicht
Een App Service-omgeving kunnen worden gemaakt in **beide** een virtueel netwerk van Azure Resource Manager, **of** een klassieke implementatiemodel [virtueel netwerk] [ virtualnetwork].  Een nieuw virtueel netwerk en een nieuw subnet kunnen worden gedefinieerd op het moment dat een App Service-omgeving wordt gemaakt.  U kunt ook kunnen een App Service-omgeving worden gemaakt in een bestaand virtueel netwerk en de bestaande subnet.  Met een wijziging in juni 2016, worden ASEs ook geïmplementeerd in virtuele netwerken die gebruikmaken van openbare-adresbereiken of RFC1918 adresruimten (dat wil zeggen particuliere adressen).  Zie voor meer informatie over het maken van een App-serviceomgeving [het maken van een App-serviceomgeving][HowToCreateAnAppServiceEnvironment].

Een App-serviceomgeving moet altijd worden gemaakt binnen een subnet omdat een subnet een netwerkgrens bevindt die kan worden gebruikt biedt voor binnenkomend verkeer achter upstream apparaten en services vergrendelen zodat HTTP en HTTPS-verkeer alleen wordt geaccepteerd vanuit specifieke stroomopwaarts IP-adressen.

Binnenkomende en uitgaande netwerkverkeer op een subnet wordt beheerd met behulp van een [netwerkbeveiligingsgroep][NetworkSecurityGroups]. Binnenkomend verkeer te beheren is vereist in een netwerkbeveiligingsgroep netwerkbeveiligingsregels maakt en vervolgens de netwerkbeveiliging toe te wijzen groep het subnet met de App Service-omgeving.

Zodra een netwerkbeveiligingsgroep aan een subnet is toegewezen, wordt binnenkomend verkeer naar apps in App Service-omgeving is toegestaan/geblokkeerd op basis van de toestaan en weigeren van regels die zijn gedefinieerd in de netwerkbeveiligingsgroep.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>Inkomende netwerkpoorten worden gebruikt in een App Service-omgeving
Voordat het vergrendelen van binnenkomend netwerkverkeer koppelen aan een netwerkbeveiligingsgroep, is het belangrijk te weten van de reeks vereiste en optionele netwerkpoorten die wordt gebruikt door een App Service-omgeving.  Per ongeluk uit het verkeer naar bepaalde poorten sluiten, kan dit leiden tot verlies aan functionaliteit in een App Service-omgeving.

Hier volgt een lijst met poorten die worden gebruikt door een App Service-omgeving. Alle poorten zijn **TCP**, tenzij anders wordt duidelijk vermeld:

* 454: **poort vereist** gebruikt door de Azure-infrastructuur voor het beheren en onderhouden van App Service-omgevingen met SSL-beveiliging.  Geen verkeer blokkeert dat naar deze poort.  Deze poort is altijd gebonden aan het openbare VIP van een as-omgeving.
* 455: **poort vereist** gebruikt door de Azure-infrastructuur voor het beheren en onderhouden van App Service-omgevingen met SSL-beveiliging.  Geen verkeer blokkeert dat naar deze poort.  Deze poort is altijd gebonden aan het openbare VIP van een as-omgeving.
* 80: standaard poort voor inkomende HTTP-verkeer naar apps die worden uitgevoerd in App Service-plannen in een App Service-omgeving.  Op een as ingeschakeld met een ILB-omgeving, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.
* 443: standaard poort voor inkomende SSL-verkeer naar apps die worden uitgevoerd in App Service-plannen in een App Service-omgeving.  Op een as ingeschakeld met een ILB-omgeving, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.
* 21: besturingskanaal voor FTP.  Deze poort kan veilig worden geblokkeerd als FTP niet wordt gebruikt.  Op een as ingeschakeld met een ILB-omgeving, kan deze poort worden gebonden aan het adres ILB voor een as-omgeving.
* 990: besturingskanaal voor FTPS.  Deze poort kan veilig worden geblokkeerd als FTPS niet wordt gebruikt.  Op een as ingeschakeld met een ILB-omgeving, kan deze poort worden gebonden aan het adres ILB voor een as-omgeving.
* 10001 10020: kanalen voor FTP-gegevens.  Net als bij het besturingskanaal kunnen deze poorten worden veilig geblokkeerd als FTP niet wordt gebruikt.  Op een as ingeschakeld met een ILB-omgeving, kan deze poort worden gebonden aan de ILB-adres van de as-omgeving.
* 4016: gebruikt voor foutopsporing op afstand met Visual Studio 2012.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Op een as ingeschakeld met een ILB-omgeving, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.
* 4018: gebruikt voor foutopsporing op afstand met Visual Studio 2013.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Op een as ingeschakeld met een ILB-omgeving, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.
* 4020: gebruikt voor foutopsporing op afstand met Visual Studio 2015.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Op een as ingeschakeld met een ILB-omgeving, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.

## <a name="outbound-connectivity-and-dns-requirements"></a>Uitgaande verbinding en DNS-vereisten
Voor een App Service-omgeving te laten functioneren, moet deze ook uitgaande toegang tot verschillende eindpunten vereist. Een volledige lijst met de externe eindpunten die worden gebruikt door een as-omgeving is in de sectie 'Netwerkverbinding vereist' van de [netwerkconfiguratie voor ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.

App Service-omgevingen moeten een geldige DNS-infrastructuur is geconfigureerd voor het virtuele netwerk.  Als voor een bepaalde reden worden de DNS-configuratie is gewijzigd nadat een App-serviceomgeving is gemaakt, kunnen ontwikkelaars een App Service-omgeving naar de nieuwe DNS-configuratie worden opgepikt afdwingen.  Activering van rolling omgeving opgestart met het pictogram "Restart" zich boven aan de blade voor het beheer van App Service-omgeving in de [Azure-portal] [ NewPortal] , wordt de omgeving voor de nieuwe DNS-configuratie worden opgepikt.

Het is ook raadzaam dat alle aangepaste DNS-servers op het vnet ingesteld worden voordat u een App-serviceomgeving tevoren.  Als een virtueel netwerk DNS-configuratie wordt gewijzigd terwijl een App Service-omgeving wordt gemaakt, wordt die leiden tot het mislukken van de App Service-omgeving maken van het proces.  In een vergelijkbare vein als een aangepaste DNS-server op het andere einde van een VPN-gateway bestaat en de DNS-server onbereikbaar is of niet beschikbaar is, is mislukt het proces voor het maken van App Service-omgeving ook.

## <a name="creating-a-network-security-group"></a>Een Netwerkbeveiligingsgroep maken
Voor volledige informatie over hoe netwerk werk beveiligingsgroepen raadpleegt u de volgende [informatie][NetworkSecurityGroups].  Het onderstaande voorbeeld raakt op Azure-servicebeheer licht van netwerkbeveiligingsgroepen en een focus over het configureren en toepassen van een netwerkbeveiligingsgroep aan een subnet met een App Service-omgeving.

**Opmerking:** netwerkbeveiligingsgroepen kunnen worden geconfigureerd grafisch weergegeven met de [Azure Portal](https://portal.azure.com) of via Azure PowerShell.

Netwerkbeveiligingsgroepen worden eerst gemaakt als een zelfstandige entiteit die is gekoppeld aan een abonnement. Aangezien netwerkbeveiligingsgroepen in een Azure-regio worden gemaakt, moet u ervoor dat de netwerkbeveiligingsgroep is gemaakt in dezelfde regio bevinden als de App Service-omgeving.

Het volgende voorbeeld toont een netwerkbeveiligingsgroep maken:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Zodra een netwerkbeveiligingsgroep is gemaakt, zijn een of meer netwerkbeveiligingsregels toegevoegd.  Aangezien de set regels kan worden gewijzigd na verloop van tijd, wordt u aangeraden om de nummering dat wordt gebruikt voor prioriteiten om gemakkelijk extra regels invoegen na verloop van tijd.

Het volgende voorbeeld ziet u een regel die expliciet toegang tot de poorten die nodig is voor de Azure-infrastructuur verleent te beheren en onderhouden van een App Service-omgeving.  Houd er rekening mee dat alle beheerverkeer via SSL loopt en wordt beveiligd door clientcertificaten, zodat zelfs als de poorten zijn geopend ze niet toegankelijk door een andere entiteit dan Azure beheerinfrastructuur zijn.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP


Wanneer het vergrendelen van toegang tot poort 80 en 443 ' verbergen ' een App-serviceomgeving achter upstream apparaten of services, moet u de upstream IP-adres op te geven.  Bijvoorbeeld, als u gebruikmaakt van een web application firewall (WAF), de WAF heeft een eigen IP-adres (of adressen) die worden gebruikt bij via een proxy-verkeer naar een downstream-App Service-omgeving.  U moet deze IP-adres in de *SourceAddressPrefix* parameter van een netwerkbeveiligingsregel.

In het onderstaande voorbeeld is binnenkomend verkeer van een specifiek IP-adres upstream expliciet toegestaan.  Het adres *1.2.3.4* wordt gebruikt als een tijdelijke aanduiding voor de IP-adres van een upstream WAF.  Wijzig de waarde zodat deze overeenkomt met het adres dat wordt gebruikt door een upstream-apparaat of service.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Als u FTP-ondersteuning, kunnen de volgende regels worden gebruikt als een sjabloon om toegang te verlenen tot de FTP-besturingselement poort en de gegevens channel-poorten.  Omdat FTP een stateful protocol, kunt u mogelijk geen FTP-verkeer routeren via een traditionele HTTP/HTTPS-firewall of proxyserver apparaat.  In dit geval moet u om in te stellen de *SourceAddressPrefix* op een andere waarde - bijvoorbeeld het IP-adresbereik van ontwikkelaars of implementatie machines op waarmee FTP-clients worden uitgevoerd. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Opmerking:** de gegevenskanaalpoortbereik kan worden gewijzigd tijdens de preview-periode.)

Als u foutopsporing op afstand met Visual Studio gebruikt, ziet u de volgende regels hoe om toegang te verlenen.  Er is een afzonderlijke regel voor elke ondersteunde versie van Visual Studio aangezien elke versie een andere poort voor foutopsporing op afstand gebruikt.  Net zoals bij FTP-toegang mogelijk niet correct externe foutopsporing verkeer stromen via een traditionele WAF of de proxyapparaat.  De *SourceAddressPrefix* in plaats daarvan kan worden ingesteld op het IP-adresbereik van Visual Studio developer machines.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Een Netwerkbeveiligingsgroep toewijzen aan een Subnet
Een netwerkbeveiligingsgroep heeft een standaardbeveiligingsregel die de toegang aan alle externe verkeer weigert.  Het resultaat van het combineren van de beveiligingsregels voor netwerken die hierboven worden beschreven en de standaardregel voor beveiliging blokkeren van binnenkomend verkeer is dat alleen het verkeer van de bron-adresbereiken die zijn gekoppeld aan een *toestaan* actie kunnen verzenden het verkeer naar apps die worden uitgevoerd in een App Service-omgeving.

Nadat een netwerkbeveiligingsgroep is gevuld met beveiligingsregels, moet deze worden toegewezen aan het subnet met de App Service-omgeving.  De opdracht toewijzing verwijst naar zowel de naam van het virtuele netwerk waarin de App Service-omgeving zich bevindt, evenals de naam van het subnet waarin de App-serviceomgeving is gemaakt.  

Het volgende voorbeeld ziet een netwerkbeveiligingsgroep wordt toegewezen aan een subnet en het virtuele netwerk:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Zodra de groepstoewijzing voor netwerk-beveiliging is geslaagd (de toewijzing is van een langlopende bewerkingen en kan een paar minuten duren), alleen inkomend verkeer die overeenkomt met *toestaan* regels wordt met succes bereiken apps in App Service -Omgeving.

Het volgende voorbeeld ziet voor de volledigheid verwijderen en de netwerkbeveiligingsgroep van het subnet dus reke‑ koppelen:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Speciale overwegingen voor expliciete IP-SSL
Als een app is geconfigureerd met een expliciete SSL IP-adres (toepasselijke *alleen* naar ASEs waarvoor een openbare VIP), in plaats van het standaard IP-adres van de App Service-omgeving, HTTP en HTTPS-verkeer stroomt in het subnet via een andere set van poorten dan de poorten 80 en 443.

Het paar afzonderlijke van poorten gebruikt door elk IP-SSL-adres gevonden in de portal-gebruikersinterface van de App Service-omgeving details UX-blade.  Selecteer 'alle instellingen'--> 'IP-adressen'.  Het tabblad 'IP-adressen' bevat een tabel met alle expliciet geconfigureerd IP-SSL-adressen voor de App Service-omgeving, samen met het paar speciale poort die wordt gebruikt voor het routeren van HTTP en HTTPS-verkeer die zijn gekoppeld aan elk IP-SSL-adres.  Deze poort toe dat worden gebruikt voor de parameters DestinationPortRange moet bij het configureren van regels in een netwerkbeveiligingsgroep is.

Wanneer een app op een as-omgeving is geconfigureerd voor gebruik van IP-SSL, is externe klanten niet zien en hoeft niet te hoeven maken over het toewijzen van de paar speciale poort.  Het verkeer naar de apps worden normaal gesproken overgebracht naar de geconfigureerde IP-SSL-adres.  De vertaling naar het paar speciale poort automatisch gebeurt intern tijdens de laatste fase van het routeren van verkeer in het subnet met de as-omgeving. 

## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgevingen, Zie [Inleiding tot de App Service-omgeving][IntroToAppServiceEnvironment]

Zie voor meer informatie om apps veilig verbinding maakt met back-end-bron van een App-serviceomgeving [veilig verbinding probeert te maken met back-endresources van een App-serviceomgeving][SecurelyConnecttoBackend]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[SecurelyConnecttoBackend]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->

