---
title: 'ARP-tabellen ophalen: klassieke: Azure ExpressRoute probleemoplossing | Microsoft Docs'
description: Deze pagina bevat instructies voor het ophalen van het ARP tabellen voor een ExpressRoute-circuit.
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: b5856acf-03c2-4933-8111-6ce12998d92a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: fcc847b7e30fd55ca759830e0254ab7542e7663e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="getting-arp-tables-in-the-classic-deployment-model"></a>Het ophalen van de ARP tabellen in het klassieke implementatiemodel
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klassiek](expressroute-troubleshooting-arp-classic.md)
> 
> 

Dit artikel begeleidt u bij de stappen voor het ophalen van de tabellen Protocol ARP (Address Resolution) voor uw Azure ExpressRoute-circuit.

> [!IMPORTANT]
> Dit document is bedoeld voor hulp bij het opsporen en oplossen van eenvoudige problemen. Het is niet bedoeld als vervanging voor ondersteuning van Microsoft. Als u op het probleem oplossen kunt met behulp van de volgende richtlijnen, opent u een ondersteuningsaanvraag met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres van de tabellen ARP (Resolution Protocol) en ARP
ARP is een Layer 2-protocol die gedefinieerd in [RFC 826](https://tools.ietf.org/html/rfc826). ARP wordt gebruikt om een Ethernet-adres (MAC-adres) worden toegewezen aan een IP-adres.

Een ARP-tabel bevat een toewijzing van de IPv4-adres en MAC-adres voor een bepaalde peering. De ARP-tabel voor een ExpressRoute-circuitpeering biedt de volgende informatie voor elke interface (primair en secundair):

1. Toewijzing van een lokale router interface IP-adres op een MAC-adres
2. Toewijzing van een ExpressRoute-router interface IP-adres op een MAC-adres
3. De leeftijd van de toewijzing

ARP-tabellen kunnen u met Layer 2-configuratie wordt gevalideerd en basic Layer 2-verbindingsproblemen op te lossen.

Hieronder volgt een voorbeeld van een ARP-tabel:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


De volgende sectie bevat informatie over het weergeven van de ARP-tabellen die zijn zichtbaar voor de ExpressRoute-randrouters.

## <a name="prerequisites-for-using-arp-tables"></a>Vereisten voor het gebruik van ARP-tabellen
Zorg ervoor dat u het volgende hebt voordat u verdergaat:

* Een geldig ExpressRoute-circuit dat geconfigureerd met ten minste één peering. Het circuit moet volledig worden geconfigureerd door de connectiviteitsprovider. U (of uw connectiviteitsprovider) moet configureren ten minste één van de peerings (Azure privé, Azure openbaar of Microsoft) op dit circuit.
* IP-adresbereiken die worden gebruikt voor het configureren van de peerings (Azure privé, Azure openbaar en Microsoft). Controleer de IP-adres toewijzing voorbeelden in de [pagina voor ExpressRoute routing requirements](expressroute-routing.md) ophalen van een goed begrip van hoe IP-adressen zijn toegewezen aan de interfaces op uw aise en de ExpressRoute-zijde. U kunt informatie ophalen over de configuratie van de peering aan de hand van de [configuratiepagina van de ExpressRoute-peering](expressroute-howto-routing-classic.md).
* Gegevens van uw netwerk team of connectiviteit provider over de MAC-adressen van de interfaces die worden gebruikt met deze IP-adressen.
* De meest recente Windows PowerShell-module voor Azure (versie 1,50 of hoger).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP-tabellen voor uw ExpressRoute-circuit
Deze sectie bevat instructies over het weergeven van de ARP-tabellen voor elk type peering met behulp van PowerShell. Voordat u doorgaat, moet u of uw connectiviteitsprovider voor het configureren van de peering. Elk circuit heeft twee paden (primair en secundair). U kunt de ARP-tabel voor elk pad onafhankelijk controleren.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-tabellen voor persoonlijke Azure-peering
De volgende cmdlet geeft de ARP tabellen voor persoonlijke Azure-peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Hieronder volgt een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-tabellen voor openbare Azure-peering:
De volgende cmdlet geeft de ARP tabellen voor openbare Azure-peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Hieronder volgt een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Hieronder volgt een voorbeeld van uitvoer voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-tabellen voor Microsoft-peering
De volgende cmdlet geeft de ARP tabellen voor Microsoft-peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Voorbeeld van uitvoer wordt hieronder weergegeven voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Het gebruik van deze informatie
De ARP-tabel van een peering kan worden gebruikt voor het valideren van Layer 2-configuratie en de verbinding. Deze sectie biedt een overzicht van hoe de ARP-tabellen in verschillende scenario's eruit zien.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>ARP-tabel wanneer een circuit bevindt zich in een operationele status (verwachte)
* De ARP-tabel heeft een vermelding voor de lokale kant met een geldig IP- en MAC-adres en een vergelijkbare vermelding voor de Microsoft-zijde.
* Het laatste octet van het lokale IP-adres is altijd een oneven getal.
* Het laatste octet van het Microsoft-IP-adres is altijd een even getal zijn.
* Hetzelfde MAC-adres wordt weergegeven aan de kant van Microsoft voor alle drie de peerings (primaire en secundaire).

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>ARP-tabel wanneer deze on-premises of wanneer de kant van de connectiviteitsprovider problemen heeft
 Er wordt slechts één vermelding weergegeven in de ARP-tabel. De toewijzing tussen het MAC-adres en het IP-adres dat wordt gebruikt bij Microsoft worden weergegeven.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Als er een probleem als volgt, moet u een ondersteuningsaanvraag openen met uw connectiviteitsprovider te verhelpen.
> 
> 

### <a name="arp-table-when-the-microsoft-side-has-problems"></a>ARP-tabel wanneer de Microsoft-zijde problemen heeft
* U ziet een ARP-tabel wordt weergegeven voor een peering als er problemen bij Microsoft zijn.
* Open een ondersteuningsverzoek met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geef op of er een probleem met Layer 2-connectiviteit.

## <a name="next-steps"></a>Volgende stappen
* Layer 3-configuraties voor uw ExpressRoute-circuit valideren:
  * Een route samenvatting om te bepalen van de status van de BGP-sessies worden opgehaald.
  * Een routetabel om te bepalen welke voorvoegsels worden geadverteerd via ExpressRoute worden opgehaald.
* Valideer de overdracht van gegevens aan de hand van bytes in en uit.
* Open een ondersteuningsverzoek met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.

