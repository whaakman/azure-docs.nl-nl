---
title: 'ARP-tabellen - problemen oplossen met - ExpressRoute: Azure| Microsoft Docs'
description: Deze pagina vindt u instructies voor de ARP-tabellen ophalen voor een ExpressRoute-circuit
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: article
ms.date: 01/30/2017
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: 1807bda35f6bfcc9dbbb30f054cedb9454a88a7f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158567"
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>ARP-tabellen ophalen in het Resource Manager-implementatiemodel
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klassiek](expressroute-troubleshooting-arp-classic.md)
> 
> 

In dit artikel leidt u door de stappen voor meer informatie over de ARP-tabellen voor uw ExpressRoute-circuit.

> [!IMPORTANT]
> Dit document is bedoeld om u te helpen u eenvoudige problemen vaststellen en oplossen. Het is niet bedoeld om te worden van een vervanging voor ondersteuning van Microsoft. U moet een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u niet de volgens de richtlijnen die hieronder worden beschreven probleem op te lossen.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres ARP (Resolution Protocol) en ARP-tabellen
Protocol ARP (Address Resolution) is een layer 2-protocol die is gedefinieerd in [RFC 826](https://tools.ietf.org/html/rfc826). ARP wordt gebruikt om het Ethernet-adres (MAC-adres) met een ip-adres worden toegewezen.

De ARP-tabel bevat een toewijzing van de ipv4-adres en MAC-adres voor een bepaalde peering. De ARP-tabel voor een ExpressRoute-circuitpeering bevat de volgende informatie voor elke interface (primaire en secundaire)

1. Toewijzing van IP-adres van on-premises router-interface op de MAC-adres
2. Toewijzing van IP-adres van ExpressRoute router-interface op de MAC-adres
3. Leeftijd van de toewijzing

ARP-tabellen kunnen laag-2-configuratie valideren en het oplossen van algemene layer 2 problemen met de netwerkverbinding. 

Voorbeeld van de ARP-tabel: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


De volgende sectie bevat informatie over hoe u de ARP-tabellen zichtbaar voor de ExpressRoute-randrouters kunt bekijken. 

## <a name="prerequisites-for-learning-arp-tables"></a>Vereisten voor het leren van ARP-tabellen
Zorg ervoor dat u het volgende hebt voordat u verder de voortgang

* Een geldige ExpressRoute-circuit dat is geconfigureerd met ten minste één peering. Het circuit moet volledig worden geconfigureerd door de connectiviteitsprovider. U (of uw connectiviteitsprovider) moet hebben geconfigureerd ten minste één van de peerings (Azure privé, Azure openbaar en Microsoft) op dit circuit.
* IP-adresbereiken gebruikt voor het configureren van de peerings (Azure privé, Azure openbaar en Microsoft). Bekijk de voorbeelden IP-adres toewijzing in de [pagina voor ExpressRoute routing requirements](expressroute-routing.md) om op te halen van een goed begrip van hoe ip-adressen zijn toegewezen aan de interfaces op uw kant- en aan de ExpressRoute. Krijgt u informatie over de configuratie voor peering aan de hand van de [configuratiepagina van de ExpressRoute-peering](expressroute-howto-routing-arm.md).
* Gegevens van uw netwerken team / connectiviteitsprovider op de MAC-adressen van de interfaces die worden gebruikt met deze IP-adressen.
* U moet de meest recente PowerShell-module voor Azure (versie 1,50 of hoger) hebben.

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>De ARP-tabellen ophalen voor uw ExpressRoute-circuit
In deze sectie vindt instructies voor hoe u de ARP-tabellen per peering met behulp van PowerShell kunt bekijken. U of uw connectiviteitsprovider moet hebben geconfigureerd de peering voordat u verder uitgevoerd. Elk circuit heeft twee paden (primaire en secundaire). U kunt de ARP-tabel voor elk pad onafhankelijk van elkaar controleren.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-tabellen voor persoonlijke Azure-peering
De volgende cmdlet biedt de ARP tabellen voor persoonlijke Azure-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secondary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Voorbeeld van de uitvoer wordt hieronder weergegeven voor een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-tabellen voor openbare Azure-peering
De volgende cmdlet biedt de ARP tabellen voor openbare Azure-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secondary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Voorbeeld van de uitvoer wordt hieronder weergegeven voor een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-tabellen voor Microsoft-peering
De volgende cmdlet biedt de ARP tabellen voor Microsoft-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secondary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Voorbeeld van de uitvoer wordt hieronder weergegeven voor een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Het gebruik van deze informatie
De ARP-tabel van een peering kan worden gebruikt om te bepalen valideren layer 2-configuratie en connectiviteit. Deze sectie bevat een overzicht van hoe ARP-tabellen in verschillende scenario's eruit.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP-tabel wanneer een circuit operationele status (verwachte status)
* De ARP-tabel heeft een vermelding voor de zijde van de on-premises met een geldig IP-adres en MAC-adres en een soortgelijke vermelding voor de Microsoft-zijde. 
* Het laatste achttal werd van het lokale ip-adres is altijd een oneven getal.
* Het laatste achttal werd van het Microsoft-ip-adres wordt altijd een even getal zijn.
* Hetzelfde MAC-adres worden weergegeven op de Microsoft-zijde van alle 3-peerings (primaire / secundaire). 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP-tabel wanneer on-premises / connectivity provider kant heeft problemen
Als er problemen met de on-premises zijn of u u ziet mogelijk dat beide slechts één vermelding wordt weergegeven in de ARP-tabel of het MAC-adres van het on-premises connectiviteitsprovider onvolledig ziet. Hiermee wordt de toewijzing tussen de MAC-adres en het IP-adres dat wordt gebruikt in de Microsoft-kant weergegeven. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

of
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Open een ondersteuningsaanvraag met uw connectiviteitsprovider om op te sporen van dergelijke problemen. Wanneer de ARP-tabel heeft geen IP-adressen van de interfaces die zijn toegewezen aan de MAC-adressen, controleert u de volgende informatie:
> 
> 1. Als het eerste IP-adres van het/30 subnet toegewezen voor de koppeling tussen de MSEE-pull-aanvraag en de MSEE wordt gebruikt op de interface van de MSEE-pull-aanvraag. Azure maakt altijd gebruik van het tweede IP-adres voor msee's.
> 2. Controleer of als de klant (C-code) en de service (S-code) VLAN-tags overeenkomen met beide op een sleutelpaar met een MSEE-pull-aanvraag en de MSEE.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-tabel bij Microsoft problemen heeft
* Niet ziet u een ARP-tabel die wordt weergegeven voor een peering als er problemen bij Microsoft zijn. 
* Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geef op dat er een probleem is met laag-2-connectiviteit. 

## <a name="next-steps"></a>Volgende stappen
* Laag-3 configuraties voor uw ExpressRoute-circuit valideren
  * Route samenvatting om te bepalen van de status van de BGP-sessies ophalen 
  * Haal de routetabel om te bepalen welke voorvoegsels worden geadverteerd via ExpressRoute
* Overdracht van gegevens valideren aan de hand van de bytes in / uit
* Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.

