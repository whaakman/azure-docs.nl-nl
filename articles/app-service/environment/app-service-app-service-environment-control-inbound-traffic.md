---
title: Inkomend verkeer naar App Service environment - Azure beheren
description: Meer informatie over het configureren van netwerkbeveiligingsregels voor het beheren van inkomend verkeer naar een App Service Environment.
services: app-service
documentationcenter: ''
author: ccompy
manager: erikre
editor: ''
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 84575dcb67845a074ce19cf9d819e1dda3f90e20
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271955"
---
# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Binnenkomend verkeer naar een App Service-omgeving beheren
## <a name="overview"></a>Overzicht
Een App Service Environment kan worden gemaakt in **beide** een virtueel netwerk van Azure Resource Manager, **of** een klassiek implementatiemodel [virtueel netwerk] [ virtualnetwork].  Een nieuw virtueel netwerk en een nieuw subnet kunnen worden gedefinieerd op het moment dat een App Service Environment is gemaakt.  U kunt ook kunnen een App Service-omgeving worden gemaakt in een bestaande virtuele netwerk en een bestaand subnet.  Met een wijziging in juni 2016, kunnen ook as-omgevingen worden geïmplementeerd in virtuele netwerken die gebruikmaken van openbare-adresbereiken of RFC1918 adresruimten (dat wil zeggen particuliere adressen).  Zie voor meer informatie over het maken van een App Service Environment [over het maken van een App Service Environment][HowToCreateAnAppServiceEnvironment].

Een App Service Environment moet altijd worden gemaakt binnen een subnet omdat een subnet een netwerkgrens bevindt die kan worden gebruikt biedt om inkomend verkeer achter upstream-apparaten en services te vergrendelen zodat HTTP en HTTPS-verkeer alleen wordt geaccepteerd vanuit specifieke upstream IP-adressen.

Binnenkomend en uitgaand netwerkverkeer op een subnet wordt beheerd met behulp van een [netwerkbeveiligingsgroep][NetworkSecurityGroups]. Inkomend verkeer beheren is vereist in een netwerkbeveiligingsgroep netwerkbeveiligingsregels maakt en vervolgens de netwerkbeveiliging toe te wijzen groep het subnet met de App Service-omgeving.

Nadat een netwerkbeveiligingsgroep is toegewezen aan een subnet, wordt binnenkomend verkeer naar apps in App Service Environment is toegestaan/geblokkeerd op basis van het toestaan en weigeren van regels die zijn gedefinieerd in de netwerkbeveiligingsgroep.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>Inkomende netwerkpoorten die worden gebruikt in een App Service Environment
Voordat het vergrendelen van binnenkomend netwerkverkeer met een netwerkbeveiligingsgroep, is het belangrijk te weten de reeks vereiste en optionele netwerkpoorten die worden gebruikt door een App Service Environment.  Per ongeluk sluit uit het verkeer naar bepaalde poorten kan leiden tot verlies van de functionaliteit in een App Service Environment.

Hier volgt een lijst met poorten die worden gebruikt door een App Service Environment. Alle poorten zijn **TCP**, tenzij anders wordt duidelijk vermeld:

* 454:  **Vereiste poort** gebruikt door Azure-infrastructuur voor het beheren en onderhouden van App Service-omgevingen met SSL-beveiliging.  Blokkeer niet-verkeer op deze poort.  Deze poort is altijd gekoppeld aan het openbare VIP-adres van een as-omgeving.
* 455:  **Vereiste poort** gebruikt door Azure-infrastructuur voor het beheren en onderhouden van App Service-omgevingen met SSL-beveiliging.  Blokkeer niet-verkeer op deze poort.  Deze poort is altijd gekoppeld aan het openbare VIP-adres van een as-omgeving.
* 80:  De standaardpoort voor inkomende HTTP-verkeer naar apps die worden uitgevoerd in App Service-abonnementen in een App Service Environment.  Op een ASE met ILB ingeschakeld, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.
* 443: De standaardpoort voor inkomende SSL-verkeer naar apps die worden uitgevoerd in App Service-abonnementen in een App Service Environment.  Op een ASE met ILB ingeschakeld, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.
* 21:  Besturingskanaal voor FTP.  Deze poort kan veilig worden geblokkeerd als FTP niet wordt gebruikt.  Op een ASE met ILB ingeschakeld, kan deze poort worden gekoppeld aan het ILB-adres voor een as-omgeving.
* 990:  Besturingskanaal voor FTPS.  Deze poort kan veilig worden geblokkeerd als FTPS niet wordt gebruikt.  Op een ASE met ILB ingeschakeld, kan deze poort worden gekoppeld aan het ILB-adres voor een as-omgeving.
* 10001-10020: Gegevenskanalen voor FTP.  Net als bij het besturingskanaal kunnen deze poorten worden veilig geblokkeerd als FTP niet wordt gebruikt.  Op een ASE met ILB ingeschakeld, kan deze poort worden gekoppeld aan het ILB-adres van de as-omgeving.
* 4016: Gebruikt voor foutopsporing op afstand met Visual Studio 2012.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Op een ASE met ILB ingeschakeld, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.
* 4018: Gebruikt voor foutopsporing op afstand met Visual Studio 2013.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Op een ASE met ILB ingeschakeld, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.
* 4020: Voor foutopsporing op afstand met Visual Studio 2015 gebruikt.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Op een ASE met ILB ingeschakeld, worden deze poort is gebonden aan het ILB-adres van de as-omgeving.

## <a name="outbound-connectivity-and-dns-requirements"></a>Uitgaande connectiviteit en DNS-vereisten
Voor een App Service Environment te laten functioneren, is het ook uitgaande toegang tot verschillende eindpunten vereist. Een volledige lijst van de externe eindpunten die worden gebruikt door een as-omgeving is in de sectie 'Netwerkverbinding vereist' van de [netwerkconfiguratie voor ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.

App Service-omgevingen vereist een geldige DNS-infrastructuur is geconfigureerd voor het virtuele netwerk.  Als voor een of andere reden die de DNS-configuratie wordt gewijzigd nadat een App Service Environment is gemaakt, ontwikkelaars een App Service-omgeving om op te halen de nieuwe DNS-configuratie afdwingt kunnen.  Activeren van de rolling omgeving opnieuw worden opgestart met het pictogram "Restart" zich boven aan de blade voor het beheer van App Service-omgeving in de [Azure-portal] [ NewPortal] zorgt ervoor dat de omgeving om op te halen de nieuwe DNS-server de configuratie.

Het is ook raadzaam dat alle aangepaste DNS-servers op het vnet ingesteld tevoren worden vóór het maken van een App Service Environment.  Als een virtueel netwerk DNS-configuratie is gewijzigd terwijl een App Service-omgeving wordt gemaakt, resulteert dat in het App Service-omgeving maken van het proces mislukt.  In een vergelijkbaar vein als een aangepaste DNS-server op het andere uiteinde van een VPN-gateway bestaat en de DNS-server niet bereikbaar is of niet beschikbaar is, is mislukken het proces voor het maken van App Service-omgeving ook.

## <a name="creating-a-network-security-group"></a>Een Netwerkbeveiligingsgroep maken
Voor volledige informatie over hoe de netwerkbeveiligingsgroepen werken, Zie de volgende [informatie][NetworkSecurityGroups].  De Azure-servicebeheer in het volgende voorbeeld is van toepassing op licht van netwerkbeveiligingsgroepen, met nadruk op configureren en toepassen van een netwerkbeveiligingsgroep aan een subnet dat een App Service-omgeving bevat.

**Opmerking:** Netwerkbeveiligingsgroepen kunnen worden geconfigureerd grafisch weergegeven met de [Azure Portal](https://portal.azure.com) of via Azure PowerShell.

Netwerkbeveiligingsgroepen worden eerst gemaakt als een zelfstandige-entiteit die is gekoppeld aan een abonnement. Omdat netwerkbeveiligingsgroepen in een Azure-regio worden gemaakt, moet u ervoor dat de netwerkbeveiligingsgroep is gemaakt in dezelfde regio als de App Service-omgeving.

Het volgende voorbeeld toont het maken van een netwerkbeveiligingsgroep:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Zodra een netwerkbeveiligingsgroep is gemaakt, zijn een of meer netwerkbeveiligingsregels toegevoegd.  Aangezien de set met regels kan worden gewijzigd na verloop van tijd, is het raadzaam om de nummering dat wordt gebruikt voor de regel prioriteiten om gemakkelijk extra regels invoegen na verloop van tijd.

In het volgende voorbeeld ziet u een regel die expliciet toegang tot de poorten die nodig zijn voor de Azure-infrastructuur verleent te beheren en onderhouden van een App Service Environment.  Houd er rekening mee dat alle beheerverkeer via SSL stromen en wordt beveiligd door clientcertificaten, dus zelfs als de poorten zijn geopend door een andere entiteit dan de Azure management-infrastructuur zijn.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP


Wanneer het vergrendelen van toegang tot poort 80 en 443 ' verborgen ' een App Service Environment achter upstream-apparaten of services, moet u weten wat de upstream IP-adres.  Bijvoorbeeld, als u gebruikmaakt van een web application firewall (WAF), de WAF heeft een eigen IP-adres (of adressen) die worden gebruikt bij gebruik van proxy het verkeer naar een downstream-App Service-omgeving.  U moet gebruiken dit IP-adres in de *SourceAddressPrefix* parameter van een regel voor de beveiliging.

In het onderstaande voorbeeld inkomend verkeer van een specifiek IP-adres upstream expliciet zijn toegestaan.  Het adres *1.2.3.4* wordt gebruikt als een tijdelijke aanduiding voor de IP-adres van een upstream WAF.  Wijzig de waarde zodat deze overeenkomt met het adres dat wordt gebruikt door de upstream-apparaat of service.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Als de FTP-ondersteuning vereist is, kunnen de volgende regels worden gebruikt als een sjabloon om toegang te verlenen tot de FTP-besturingselement poort en de gegevens channel-poorten.  Omdat FTP een stateful protocol is, kunt u mogelijk geen FTP-verkeer routeren via een traditionele HTTP/HTTPS-firewall of proxyserver apparaat.  In dit geval moet u instellen de *SourceAddressPrefix* op een andere waarde - bijvoorbeeld het IP-adresbereik van ontwikkelaars of implementatie machines op waarmee FTP-clients worden uitgevoerd. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Opmerking:** de gegevenskanaalpoortbereik kan worden gewijzigd tijdens de preview-periode.)

Als u foutopsporing op afstand met Visual Studio gebruikt, geeft de volgende regels laten zien hoe u toegang verlenen.  Er is een afzonderlijke regel voor alle ondersteunde versies van Visual Studio, omdat elke versie maakt gebruik van een andere poort voor foutopsporing op afstand.  Net als bij de FTP-toegang, mogelijk niet correct externe foutopsporing verkeer stromen via een traditionele WAF of een proxy-apparaat.  De *SourceAddressPrefix* in plaats daarvan kan worden ingesteld op het IP-adresbereik van machines van ontwikkelaars met Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Toewijzen van een Netwerkbeveiligingsgroep aan een Subnet
Een netwerkbeveiligingsgroep is een standaardbeveiligingsregel waarmee toegang tot alle externe verkeer.  Het resultaat van het combineren van de netwerk-beveiligingsregels die hierboven worden beschreven, en de standaardbeveiligingsregel voor binnenkomend verkeer blokkeren is dat alleen verkeer van de bron-adresbereiken die zijn gekoppeld aan een *toestaan* actie is mogelijk om te verzenden verkeer naar apps die worden uitgevoerd in een App Service Environment.

Nadat een netwerkbeveiligingsgroep is gevuld met beveiligingsregels, moet deze worden toegewezen aan het subnet met de App Service-omgeving.  De opdracht toewijzing verwijst naar zowel de naam van het virtuele netwerk waarin de App Service-omgeving zich bevindt, evenals de naam van het subnet waarin de App Service-omgeving is gemaakt.  

Het volgende voorbeeld ziet u een netwerkbeveiligingsgroep wordt toegewezen aan een subnet en een virtueel netwerk:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Zodra de groepstoewijzing van de netwerk-beveiliging is geslaagd (de toewijzing is van een langlopende bewerkingen en kan een paar minuten duren), alleen inkomend verkeer die overeenkomt met *toestaan* regels zal is contact met u apps in de App Service -Omgeving.

Het volgende voorbeeld ziet voor de volledigheid verwijderen en de netwerkbeveiligingsgroep vanuit het subnet dus berekeningsclusters koppelen:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Speciale overwegingen voor expliciete IP-SSL
Als een app is geconfigureerd met een expliciete IP-SSL-adres (van toepassing *alleen* voor as-omgevingen waarvoor een openbare VIP-adres), in plaats van de standaard-IP-adres van de App Service-omgeving, HTTP en HTTPS-verkeer stromen in het subnet via een andere set van poorten dan poort 80 en 443.

De afzonderlijke combinatie van de poorten die worden gebruikt door elk IP-SSL-adres vindt u in de portal-gebruikersinterface van de App Service-omgeving details UX-blade.  Selecteer 'alle instellingen'--> 'IP-adressen.  De blade 'IP-adressen wordt een tabel met alle expliciet geconfigureerde IP-SSL-adressen voor de App Service-omgeving, samen met de combinatie van speciale poort die wordt gebruikt voor het routeren van HTTP en HTTPS-verkeer dat is gekoppeld aan elk IP-SSL-adres.  Het is de combinatie van deze poort die worden gebruikt voor de parameters DestinationPortRange moet bij het configureren van de regels in een netwerkbeveiligingsgroep.

Wanneer een app op een as-omgeving is geconfigureerd voor het gebruik van IP-SSL, is externe klanten niet meer te zien en hoeft niet te hoeven maken over de speciale paar poorttoewijzing.  Verkeer naar de apps worden normaal gesproken worden overgebracht naar het geconfigureerde IP-SSL-adres.  De vertaling op twee speciale poort automatisch gebeurt intern tijdens de laatste fase van het routeren van verkeer in het subnet met daarin de as-omgeving. 

## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service-omgevingen, Zie [Inleiding tot App Service-omgeving][IntroToAppServiceEnvironment]

Zie voor meer informatie over apps die veilig verbinding maken met back-endresource van een App Service Environment [veilig maken van verbinding met back-endresources van een App Service Environment][SecurelyConnecttoBackend]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[SecurelyConnecttoBackend]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->

