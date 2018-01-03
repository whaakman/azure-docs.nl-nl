---
title: 'ARP-tabellen ophalen: Resource Manager: Azure ExpressRoute probleemoplossing | Microsoft Docs'
description: Deze pagina vindt u instructies voor het ophalen van het ARP tabellen voor een ExpressRoute-circuit
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: 0a6bf1d5-6baf-44dd-87d3-1ebd2fd08bdc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: a65b1ba2998eae33b3e73bd2492fbbf025eb5946
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>Het ophalen van de ARP tabellen in het Resource Manager-implementatiemodel
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klassiek](expressroute-troubleshooting-arp-classic.md)
> 
> 

Dit artikel begeleidt u stapsgewijs door de procedure voor meer informatie over de ARP-tabellen voor uw ExpressRoute-circuit. 

> [!IMPORTANT]
> Dit document is bedoeld voor hulp bij het opsporen en oplossen van eenvoudige problemen. Het is niet bedoeld als vervanging voor ondersteuning van Microsoft. U moet een ondersteuningsticket met openen [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u niet voor het oplossen van het probleem op door middel van de instructies die hieronder worden beschreven.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres van de tabellen ARP (Resolution Protocol) en ARP
Protocol ARP (Address Resolution) is een layer 2-protocol gedefinieerd in [RFC 826](https://tools.ietf.org/html/rfc826). ARP wordt gebruikt om het Ethernet-adres (MAC-adres) met een IP-adres toewijzen.

De ARP-tabel bevat een toewijzing van de ipv4-adres en MAC-adres voor een bepaalde peering. De ARP-tabel voor een ExpressRoute-circuitpeering bevat de volgende informatie voor elke interface (primair en secundair)

1. Toewijzing van IP-adres van lokale router interface met de MAC-adres
2. Toewijzing van IP-adres van ExpressRoute router interface met de MAC-adres
3. Leeftijd van de toewijzing

ARP-tabellen kunnen laag 2-configuratie valideren en het oplossen van eenvoudige laag-2 verbindingsproblemen. 

Voorbeeld van de ARP-tabel: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


De volgende sectie bevat informatie over hoe u de ARP-tabellen die zichtbaar zijn voor de ExpressRoute-randrouters kunt bekijken. 

## <a name="prerequisites-for-learning-arp-tables"></a>Vereisten voor het leren van ARP-tabellen
Zorg ervoor dat u het volgende hebt voordat u verder voortgang

* Een geldig ExpressRoute-circuit dat is geconfigureerd met ten minste één peering. Het circuit moet volledig worden geconfigureerd door de connectiviteitsprovider. U (of uw connectiviteitsprovider) moet hebben geconfigureerd ten minste één van de peerings (Azure privé, Azure openbaar en Microsoft) op dit circuit.
* IP-adresbereiken gebruikt voor het configureren van de peerings (Azure privé, Azure openbaar en Microsoft). Controleer de IP-adres toewijzing voorbeelden in de [pagina voor ExpressRoute routing requirements](expressroute-routing.md) ophalen van een goed begrip van hoe IP-adressen zijn toegewezen aan interfaces aan uw kant en de ExpressRoute-zijde. Krijgt u informatie over de configuratie van de peering aan de hand van de [configuratiepagina van de ExpressRoute-peering](expressroute-howto-routing-arm.md).
* Gegevens uit uw netwerken team / connectiviteitsprovider op de MAC-adressen van de interfaces met deze IP-adressen gebruikt.
* U moet de meest recente PowerShell-module voor Azure (1,50 of een nieuwere versie) hebben.

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>De ARP-tabellen ophalen voor uw ExpressRoute-circuit
Deze sectie bevat instructies over hoe u de ARP-tabellen per met behulp van PowerShell-peering kunt bekijken. U of uw connectiviteitsprovider moet hebben geconfigureerd peering voordat u zich verder ontwikkelen. Elk circuit heeft twee paden (primair en secundair). U kunt de ARP-tabel voor elk pad onafhankelijk controleren.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-tabellen voor persoonlijke Azure-peering
De volgende cmdlet biedt het ARP tabellen voor persoonlijke Azure-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Voorbeeld van uitvoer wordt hieronder weergegeven voor een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-tabellen voor openbare Azure-peering
De volgende cmdlet biedt het ARP tabellen voor openbare Azure-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Voorbeeld van uitvoer wordt hieronder weergegeven voor een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-tabellen voor Microsoft-peering
De volgende cmdlet biedt het ARP tabellen voor Microsoft-peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Voorbeeld van uitvoer wordt hieronder weergegeven voor een van de paden

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Het gebruik van deze informatie
De ARP-tabel van een peering kan worden gebruikt om te bepalen valideren laag 2-configuratie en de verbinding. Deze sectie bevat een overzicht van hoe ARP-tabellen onder verschillende scenario's eruit.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP-tabel wanneer een circuit bevindt zich in de operationele status (verwachte status)
* De ARP-tabel heeft een vermelding voor de lokale zijde met een geldig IP-adres en MAC-adres en een vergelijkbare vermelding voor de Microsoft-zijde. 
* Het laatste octet van het lokale IP-adres wordt altijd een oneven getal zijn.
* Het laatste octet van het Microsoft IP-adres wordt altijd een even getal zijn.
* Hetzelfde MAC-adres wordt weergegeven aan de kant van Microsoft voor alle 3 peerings (primaire / secundaire). 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP-tabel wanneer lokale / verbinding-provider heeft problemen
Als er problemen met de on-premises zijn of connectiviteitsprovider die ziet u mogelijk dat beide slechts één vermelding wordt weergegeven in de ARP-tabel of het MAC-adres van het on-premises onvolledige wordt weergegeven. Hier ziet de toewijzing tussen de MAC-adres en het IP-adres dat wordt gebruikt in de Microsoft-kant. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

of
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Een ondersteuningsaanvraag openen met uw connectiviteitsprovider om op te sporen van dergelijke problemen. Als de ARP-tabel geen IP-adressen van de interfaces die zijn toegewezen aan de MAC-adressen heeft, moet u de volgende informatie bekijken:
> 
> 1. Als het eerste IP-adres van het/30 subnet voor de koppeling tussen de MSEE-PR en MSEE wordt gebruikt op de interface van MSEE PR. toegewezen Azure gebruikt altijd het tweede IP-adres voor de msee's.
> 2. Controleer of als de klant (C-code) en VLAN-servicetags (S-Tag) overeenkomt met beide op een combinatie van MSEE PR en MSEE.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-tabel bij Microsoft problemen heeft
* U ziet een ARP-tabel wordt weergegeven voor een peering als er problemen bij Microsoft zijn. 
* Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geef op of er een probleem met laag 2-connectiviteit. 

## <a name="next-steps"></a>Volgende stappen
* Layer 3-configuraties voor uw ExpressRoute-circuit valideren
  * Route samenvatting om te bepalen van de status van de BGP-sessies ophalen 
  * Routetabel om te bepalen welke voorvoegsels worden geadverteerd via ExpressRoute ophalen
* Overdracht van gegevens aan de hand van de bytes in / out-valideren
* Open een ondersteuningsticket met [Microsoft ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.

