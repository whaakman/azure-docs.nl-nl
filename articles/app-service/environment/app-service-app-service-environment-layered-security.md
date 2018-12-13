---
title: Gelaagde beveiligingsarchitectuur met App Service-omgevingen - Azure
description: Het implementeren van een gelaagde beveiligingsarchitectuur met App Service-omgevingen.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 5e25de1ad2042ac978c3698165b9d9baba20e816
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274154"
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Een gelaagde beveiligingsarchitectuur met App Service-omgevingen implementeren
## <a name="overview"></a>Overzicht
Omdat App Service-omgevingen een geïsoleerde runtime-omgeving die is geïmplementeerd in een virtueel netwerk bieden, kunnen ontwikkelaars een gelaagde beveiligingsarchitectuur bieden verschillende niveaus van toegang tot het netwerk voor elke fysieke toepassingslaag maken.

Er is een algemene wens verbergen API back-ends van algemene toegang tot Internet en API's om te worden aangeroepen door de upstream-web-apps alleen toestaan.  [Netwerkbeveiligingsgroepen (nsg's)] [ NetworkSecurityGroups] op subnetten met App Service-omgevingen kunnen worden gebruikt om te beperken van openbare toegang tot API-Apps.

Het volgende diagram toont een voorbeeld-architectuur met een op basis van WebAPI-app geïmplementeerd op een App Service Environment.  Drie afzonderlijke web-app-instanties, geïmplementeerd in drie afzonderlijke App Service-omgevingen, maken back-end aanroepen naar de dezelfde WebAPI-app

![Conceptuele architectuur][ConceptualArchitecture] 

Het groen plusteken geven aan dat de netwerkbeveiligingsgroep op het subnet met daarin 'apiase' kunt u inkomende aanroepen van de upstream-web-apps als en aanroepen van zichzelf.  De dezelfde netwerkbeveiligingsgroep weigert echter expliciet toegang tot algemene binnenkomend verkeer van Internet. 

De rest van dit artikel helpt bij de stappen die nodig zijn voor het configureren van de netwerkbeveiligingsgroep op het subnet met daarin "apiase."

## <a name="determining-the-network-behavior"></a>De netwerk-gedrag bepalen
Als u wilt weten welke netwerkbeveiligingsregels nodig zijn, moet u bepalen welke clients in een netwerk kunnen worden bereikt van het App Service-omgeving met de API-app en welke clients worden geblokkeerd.

Aangezien [netwerkbeveiligingsgroepen (nsg's)] [ NetworkSecurityGroups] worden toegepast op subnetten, en App Service-omgevingen in subnetten worden geïmplementeerd, de regels die deel uitmaken van een NSG van toepassing op **alle** apps uitgevoerd op een App Service Environment.  Met behulp van de voorbeeldarchitectuur voor dit artikel, zodra een netwerkbeveiligingsgroep is toegepast op het subnet met daarin "apiase", worden alle apps die worden uitgevoerd op de 'apiase"App Service-omgeving beveiligd door de dezelfde set regels voor beveiliging. 

* **Bepaal de uitgaande IP-adres van de upstream aanroepers:**  Wat is de IP-adres of de adressen van de upstream aanroepers?  Deze adressen moet expliciet toegang zijn toegestaan in de NSG.  Omdat aanroepen tussen App Service-omgevingen worden beschouwd als 'Internet' aanroepen, de uitgaande IP-adres toegewezen aan elk van de drie upstream App Service-omgevingen moet worden toegang is toegestaan in de Netwerkbeveiligingsgroep voor het subnet 'apiase'.   Zie voor meer informatie over het bepalen van de uitgaande IP-adres voor apps die worden uitgevoerd in een App Service Environment, de [netwerkarchitectuur] [ NetworkArchitecture] overzichtsartikel.
* **Moet de back-end API-app om aan te roepen zelf?**  Een punt Soms vergeten en subtiele is het scenario waar de back-endtoepassing moet zijn om aan te roepen zelf.  Als een back-end-API-toepassing op een App Service-omgeving nodig heeft om aan te roepen zelf, wordt dit ook beschouwd als een aanroep van de 'Internet'.  Hiervoor moet de toegang te verlenen vanaf de uitgaande IP-adres van de 'apiase"App Service-omgeving en uit te voeren in de voorbeeldarchitectuur.

## <a name="setting-up-the-network-security-group"></a>Instellen van de Netwerkbeveiligingsgroep
Wanneer de reeks uitgaande IP-adressen zijn bekend is, wordt de volgende stap is om samen te stellen van een netwerkbeveiligingsgroep.  Netwerkbeveiligingsgroepen kunnen worden gemaakt voor beide in Resource Manager virtuele netwerken, evenals een klassieke virtuele netwerken gebaseerde.  De volgende voorbeelden laten zien voor het maken en configureren van een NSG op een klassiek virtueel netwerk met behulp van Powershell.

Voor de voorbeeldarchitectuur staan de omgevingen in Zuid-centraal VS, zodat een lege NSG wordt gemaakt in die regio:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Eerst een expliciete toestaan regel is toegevoegd voor de Azure management-infrastructuur, zoals vermeld in het artikel op [binnenkomend verkeer] [ InboundTraffic] voor App Service-omgevingen.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

Vervolgens twee regels worden toegevoegd aan het toestaan van HTTP en HTTPS-aanroepen van de eerste upstream App Service Environment ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Spoel en herhalen voor de tweede en derde upstream App Service-omgevingen ("fe2ase" en "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Ten slotte toegang verlenen tot de uitgaande IP-adres van de back-end-API-App Service-omgeving zodat deze terug naar zichzelf kan aanroepen.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Er zijn geen andere netwerkbeveiligingsregels zijn vereist, omdat elke NSG bevat een set met standaardregels die inkomend toegang via Internet, standaard geblokkeerd.

De volledige lijst met regels in de netwerkbeveiligingsgroep worden hieronder weergegeven.  Houd er rekening mee hoe de laatste regel die is gemarkeerd, blokkeert binnenkomende toegang vanaf alle aanroepers, dan aanroepers die expliciet toegang hebben gekregen.

![NSG-configuratie][NSGConfiguration] 

De laatste stap is het toepassen van de NSG op het subnet waarin de 'apiase"App Service-omgeving.

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Met de NSG toegepast op het subnet, zijn alleen de drie upstream App Service-omgevingen, en de App Service-omgeving met de API-back-end, aanroep doen naar de omgeving 'apiase' toegestaan.

## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie
Informatie over [netwerkbeveiligingsgroepen](../../virtual-network/security-overview.md).

Informatie over [uitgaande IP-adressen] [ NetworkArchitecture] en App Service-omgevingen.

[Netwerkpoorten] [ InboundTraffic] die door App Service-omgevingen worden gebruikt.

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  app-service-app-service-environment-network-architecture-overview.md
[InboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
