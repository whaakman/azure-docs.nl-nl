---
title: 'ARP-tabellen - probleemoplossing ExpressRoute ophalen: klassiek: Azure | Microsoft Docs'
description: Deze pagina vindt u instructies voor het ophalen van de ARP-tabellen voor een ExpressRoute-circuit - klassieke implementatiemodel.
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: article
ms.date: 01/30/2017
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: 367a79b04a8736e2eafb6851b682f2c244e80522
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53272283"
---
# <a name="getting-arp-tables-in-the-classic-deployment-model"></a>ARP-tabellen ophalen in het klassieke implementatiemodel
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klassiek](expressroute-troubleshooting-arp-classic.md)
> 
> 

In dit artikel leidt u door de stappen voor het ophalen van de tabellen Protocol ARP (Address Resolution) voor uw Azure ExpressRoute-circuit.

> [!IMPORTANT]
> Dit document is bedoeld om u te helpen u eenvoudige problemen vaststellen en oplossen. Het is niet bedoeld om te worden van een vervanging voor ondersteuning van Microsoft. Als u het probleem niet oplossen met behulp van de informatie te volgen, opent u een ondersteuningsaanvraag met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres ARP (Resolution Protocol) en ARP-tabellen
ARP is een Layer 2-protocol die gedefinieerd in [RFC 826](https://tools.ietf.org/html/rfc826). ARP wordt gebruikt om een Ethernet-adres (MAC-adres) toewijzen aan een IP-adres.

Een ARP-tabel bevat een toewijzing van de IPv4-adres en MAC-adres voor een bepaalde peering. De ARP-tabel voor een ExpressRoute-circuitpeering biedt de volgende informatie voor elke interface (primaire en secundaire):

1. Toewijzing van een on-premises router interface IP-adres op een MAC-adres
2. Toewijzing van een ExpressRoute-router interface IP-adres op een MAC-adres
3. De leeftijd van de toewijzing

ARP-tabellen kunnen u met het valideren van laag 2-configuratie en bij het basic-laag-2-verbindingsproblemen oplossen.

Hieronder volgt een voorbeeld van een ARP-tabel:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


De volgende sectie bevat informatie over het weergeven van de ARP-tabellen die zijn zichtbaar voor de ExpressRoute-randrouters.

## <a name="prerequisites-for-using-arp-tables"></a>Vereisten voor het gebruik van de ARP-tabellen
Zorg ervoor dat u het volgende hebt voordat u doorgaat:

* Een geldige ExpressRoute-circuit dat geconfigureerd met ten minste één peering. Het circuit moet volledig worden geconfigureerd door de connectiviteitsprovider. U (of uw connectiviteitsprovider) moet configureren ten minste één van de peerings (Azure privé, Azure openbaar of Microsoft) op dit circuit.
* IP-adresbereiken die worden gebruikt voor het configureren van de peerings (Azure privé, Azure openbaar en Microsoft). Bekijk de voorbeelden IP-adres toewijzing in de [pagina voor ExpressRoute routing requirements](expressroute-routing.md) om op te halen van een goed begrip van hoe IP-adressen zijn toegewezen aan de interfaces op uw aise en aan de ExpressRoute. U krijgt informatie over de configuratie voor peering aan de hand van de [configuratiepagina van de ExpressRoute-peering](expressroute-howto-routing-classic.md).
* Gegevens uit uw netwerken team of connectivity provider over de MAC-adressen van de interfaces die worden gebruikt met deze IP-adressen.
* De meest recente Windows PowerShell-module voor Azure (versie 1,50 of hoger).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP-tabellen voor uw ExpressRoute-circuit
Deze sectie vindt u instructies over het weergeven van de ARP-tabellen voor elk type peering met behulp van PowerShell. Voordat u doorgaat, moet u of uw connectiviteitsprovider het configureren van de peering. Elk circuit heeft twee paden (primaire en secundaire). U kunt de ARP-tabel voor elk pad onafhankelijk van elkaar controleren.

### <a name="arp-tables-for-azure-private-peering"></a>ARP-tabellen voor persoonlijke Azure-peering
De volgende cmdlet biedt de ARP tabellen voor persoonlijke Azure-peering:

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
De volgende cmdlet biedt de ARP tabellen voor openbare Azure-peering:

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
De volgende cmdlet biedt de ARP tabellen voor Microsoft-peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Voorbeeld van de uitvoer wordt hieronder weergegeven voor een van de paden:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Het gebruik van deze informatie
De ARP-tabel van een peering kan worden gebruikt voor het valideren van laag 2-configuratie en connectiviteit. Deze sectie bevat een overzicht van hoe de ARP-tabellen in verschillende scenario's zien.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>ARP-tabel wanneer een circuit zich in de operationele status (verwacht)
* De ARP-tabel heeft een vermelding voor de zijde van de on-premises met een geldig IP-adres en MAC-adres en een soortgelijke vermelding voor de Microsoft-zijde.
* Het laatste achttal werd van het lokale IP-adres is altijd een oneven getal.
* Het laatste achttal werd van het Microsoft-IP-adres is altijd een even getal.
* Hetzelfde MAC-adres wordt weergegeven aan de kant van Microsoft voor alle drie de peerings (primair/secundair).

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>ARP-tabel wanneer deze on-premises of wanneer de kant van de connectiviteitsprovider problemen heeft
 Er wordt slechts één vermelding weergegeven in de ARP-tabel. Hier ziet u de toewijzing tussen het MAC-adres en het IP-adres dat wordt gebruikt bij Microsoft.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Als u een probleem als volgt ondervindt, moet u een ondersteuningsaanvraag openen met uw connectiviteitsprovider om dit te verhelpen.
> 
> 

### <a name="arp-table-when-the-microsoft-side-has-problems"></a>ARP-tabel wanneer de Microsoft-zijde problemen heeft
* Niet ziet u een ARP-tabel die wordt weergegeven voor een peering als er problemen bij Microsoft zijn.
* Open een ondersteuningsaanvraag met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geef op dat er een probleem is met de laag-2-connectiviteit.

## <a name="next-steps"></a>Volgende stappen
* Laag-3 configuraties voor uw ExpressRoute-circuit valideren:
  * Krijg een route overzicht om de status van de BGP-sessies te bepalen.
  * Haal een routetabel om te bepalen welke voorvoegsels worden geadverteerd via ExpressRoute.
* Valideer de overdracht van gegevens aan de hand van bytes in en uit.
* Open een ondersteuningsaanvraag met [Microsoft Azure Help + ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) als u nog steeds problemen ondervindt.

