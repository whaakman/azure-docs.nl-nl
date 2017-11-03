---
title: Gelaagde beveiligingsarchitectuur met App Service-omgevingen
description: Het implementeren van een gelaagd beveiligingsarchitectuur met App Service-omgevingen.
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.openlocfilehash: 6c1f62b5e10a625911feea17ae6835e027709790
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Implementatie van een gelaagd beveiligingsarchitectuur met App Service-omgevingen
## <a name="overview"></a>Overzicht
Omdat App Service-omgevingen een geïsoleerde runtime-omgeving in een virtueel netwerk geïmplementeerd bieden, kunnen ontwikkelaars een gelaagd beveiligingsarchitectuur bieden verschillende niveaus van toegang tot het netwerk voor elke toepassingslaag fysieke maken.

Een algemene streven is verbergen API back-ends van algemene toegang tot Internet en dat alleen API's om te worden aangeroepen door de upstream-web-apps.  [Netwerkbeveiligingsgroepen (nsg's)] [ NetworkSecurityGroups] op subnetten met App Service-omgevingen kunnen worden gebruikt voor het openbare toegang tot API toepassingen beperken.

Het volgende diagram toont een voorbeeld-architectuur met een WebAPI op basis van app geïmplementeerd op een App Service-omgeving.  Drie afzonderlijke web app-exemplaren, geïmplementeerd op drie aparte App Service-omgevingen, moeten back-end-aanroepen naar dezelfde WebAPI-app.

![Conceptionele architectuur][ConceptualArchitecture] 

Het groen plusteken aangeven dat de netwerkbeveiligingsgroep op het subnet met 'apiase' inkomende aanroepen van de upstream-web-apps, kunt u als goed aanroepen van zichzelf.  Evenwel de dezelfde netwerkbeveiligingsgroep expliciet geweigerd voor toegang tot algemene binnenkomend verkeer vanaf Internet. 

De rest van dit onderwerp worden de stappen die nodig zijn voor het configureren van de netwerkbeveiligingsgroep op het subnet met 'apiase'.

## <a name="determining-the-network-behavior"></a>Bepalen het gedrag van het netwerk
Als u wilt weten welke netwerkbeveiligingsregels nodig zijn, moet u bepalen welke netwerkclients kunnen worden bereikt van het App Service-omgeving waarin de API-app en welke clients worden geblokkeerd.

Aangezien [netwerkbeveiligingsgroepen (nsg's)] [ NetworkSecurityGroups] worden toegepast op subnetten, en App Service-omgevingen in subnetten worden geïmplementeerd, de regels in een NSG van toepassing op **alle** apps uitgevoerd op een App Service-omgeving.  Gebruik de voorbeeldarchitectuur voor dit artikel nadat een netwerkbeveiligingsgroep is toegepast op het subnet met 'apiase' en worden alle apps die worden uitgevoerd op de App Service-omgeving 'apiase' beveiligd door dezelfde set beveiligingsregels voor verbindingen. 

* **Bepalen van de uitgaande IP-adres van de upstream aanroepers:** wat is de IP-adres of de adressen van de upstream aanroepfuncties?  Deze adressen moet expliciet toegang te krijgen in de NSG.  Omdat aanroepen tussen App Service-omgevingen worden beschouwd als 'Internet' aanroepen, moet dit betekent dat het uitgaande IP-adres toegewezen aan elk van de drie upstream-App Service-omgevingen moet u toegang te krijgen in de NSG voor het subnet 'apiase'.   Voor meer informatie over het bepalen van het uitgaande IP-adres voor apps die worden uitgevoerd in een App Service-omgeving raadpleegt u de [netwerkarchitectuur] [ NetworkArchitecture] overzichtsartikel.
* **De back-end-API-app moet aan te roepen zelf?**  Een punt soms onderbelicht en subtiele is het scenario waarin de back-endtoepassing moet zichzelf.  Als een back-end-API-toepassing op een App-serviceomgeving zichzelf moet, wordt dit ook behandeld als een aanroep van 'Internet'.  Hiervoor moet de toegang te verlenen vanaf de uitgaande IP-adres van de 'apiase' App Service-omgeving ook uit te voeren in de voorbeeldarchitectuur.

## <a name="setting-up-the-network-security-group"></a>Instellen van de Netwerkbeveiligingsgroep
Zodra de set met uitgaande IP-adressen gekend zijn, worden de volgende stap is om een netwerkbeveiligingsgroep samen te stellen.  Netwerkbeveiligingsgroepen kunnen worden gemaakt voor beide Resource Manager virtuele netwerken, evenals klassieke virtuele netwerken gebaseerde.  De volgende voorbeelden tonen maken en configureren van een NSG op een klassiek virtueel netwerk met behulp van Powershell.

Voor de voorbeeldarchitectuur staan de omgevingen in Zuid-centraal VS, zodat een lege NSG wordt gemaakt in deze regio:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Eerst een expliciete toestaan regel wordt toegevoegd voor de Azure management-infrastructuur zoals aangegeven in het artikel op [binnenkomend verkeer] [ InboundTraffic] voor App Service-omgevingen.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

Vervolgens twee regels worden toegevoegd aan het toestaan van HTTP en HTTPS-aanroepen vanuit de eerste upstream-App Service-omgeving ('fe1ase').

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Spoel en Herhaal voor de tweede en derde upstream-App Service-omgevingen ('fe2ase' en 'fe3ase').

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Ten slotte toegang verlenen tot de uitgaande IP-adres van de back-end-API-App Service-omgeving zodat terug naar zichzelf kan worden aangeroepen.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Er zijn geen andere netwerkbeveiligingsregels moeten worden geconfigureerd omdat elke NSG heeft een set met standaardregels die inkomende toegang via het Internet blokkeren standaard.

De volledige lijst met regels in de netwerkbeveiligingsgroep worden hieronder weergegeven.  Houd er rekening mee hoe de laatste regel die is gemarkeerd, blokkeert binnenkomende toegang vanaf alle aanroepfuncties dan degene die expliciet toegang hebben gekregen.

![NSG-configuratie][NSGConfiguration] 

De laatste stap wordt de NSG toegepast op het subnet waarin de 'apiase' App Service-omgeving.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Met het NSG wordt toegepast op het subnet, worden alleen de drie upstream-App Service-omgevingen en de App Service-omgeving met de API-back-end, om aan te roepen in de omgeving 'apiase' toegestaan.

## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie
Informatie over [netwerkbeveiligingsgroepen](../../virtual-network/virtual-networks-nsg.md). 

Informatie over [uitgaande IP-adressen] [ NetworkArchitecture] en App Service-omgevingen.

[Netwerkpoorten] [ InboundTraffic] die wordt gebruikt door App Service-omgevingen.

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  app-service-app-service-environment-network-architecture-overview.md
[InboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
