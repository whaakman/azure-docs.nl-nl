---
title: ExpressRoute klant router configuratie voorbeelden | Microsoft Docs
description: Deze pagina vindt u voorbeelden van router config voor Cisco en Juniper routers.
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 564826bc-017a-4683-a385-37c9fa814948
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: 032e584dc5abf59e9e3e8d80673b402f1fbf721b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a>Voorbeelden van de router configuratie instellen en beheren van Routering
Deze pagina bevat de interface en routering configuratie voorbeelden voor IOS-XE van Cisco en Juniper MX reeks routers. Deze zijn bedoeld als voorbeelden voor richtlijnen alleen en moeten niet worden gebruikt. U kunt werken met uw leverancier om te bedenken geschikte configuraties voor uw netwerk. 

> [!IMPORTANT]
> Voorbeelden in deze pagina zijn bedoeld om te worden uitsluitend voor hulp. Bespreek met de leverancier van uw verkoop / technisch team en uw netwerken team actief met relevante configuraties om te voldoen aan uw behoeften. Problemen met configuraties die worden vermeld in deze pagina wordt niet door Microsoft ondersteuning. Neem contact op met de leverancier van uw apparaat voor ondersteuning.
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a>MTU en TCP MSS-instellingen op de interfaces van router
* De MTU van de ExpressRoute-interface is 1500, de gangbare standaard MTU voor een Ethernet-interface op een router. Tenzij uw router een andere MTU standaard heeft, is er niet nodig om op te geven van een waarde op de routerinterface.
* In tegenstelling tot een Azure VPN-Gateway hoeft de TCP-MSS voor een ExpressRoute-circuit niet te worden opgegeven.

Router configuratie voorbeelden van toepassing op alle peerings. Bekijk [ExpressRoute peerings](expressroute-circuit-peerings.md) en [routeringsvereisten voor ExpressRoute](expressroute-routing.md) voor meer informatie over routering.


## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE gebaseerd routers
De voorbeelden in deze sectie voor een router met de IOS-XE-OS-familie van toepassing.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Interfaces en onderliggende interfaces configureren
U moet een sub-interface per in elke router die u verbinding met Microsoft maken-peering. Een sub-interface kan worden geïdentificeerd met een VLAN-ID of een gestapelde paar VLAN-id's en een IP-adres.

**Dot1Q interfacedefinitie**

In dit voorbeeld worden de onderliggende interfacedefinitie voor een onderliggende interface met een enkel VLAN-ID. De VLAN-ID is uniek per peering. Het laatste octet van uw IPv4-adres wordt altijd een oneven getal zijn.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

**QinQ interfacedefinitie**

Dit voorbeeld bevat de submappen interfacedefinitie voor een onderliggende interface met twee VLAN-id. De buitenste VLAN-ID (s-code), als het gebruikt hetzelfde is gebleven via de peerings. De interne (c-code) van de VLAN-ID is uniek per peering. Het laatste octet van uw IPv4-adres wordt altijd een oneven getal zijn.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a>2. Instellen van eBGP-sessies
U moet een BGP-sessie met Microsoft voor elke peering instellen. Het onderstaande voorbeeld kunt u instellen dat een BGP-sessie met Microsoft. Als het IPv4-adres dat u voor uw sub-interface gebruikt a.b.c.d is, worden de IP-adres van de BGP-neighbor (Microsoft) a.b.c.d+1. Het laatste octet van de BGP-neighbor IPv4-adres wordt altijd een even getal zijn.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. Voorvoegsels instellen om te worden geadverteerd via de BGP-sessie
U kunt uw router voor het adverteren Selecteer voorvoegsels naar Microsoft configureren. U kunt dit doen met behulp van het voorbeeld hieronder.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. Route maps
U kunt gebruiken route-kaarten en voorvoegsel lijsten moeten filter voorvoegsels doorgegeven in uw netwerk. Het onderstaande voorbeeld kunt u de taak uit te voeren. Zorg ervoor dat er bijbehorende voorvoegsel een lijst met setup.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Juniper MX reeks routers
De voorbeelden in deze sectie gelden voor alle reeksen Juniper MX routers.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. Interfaces en onderliggende interfaces configureren

**Dot1Q interfacedefinitie**

In dit voorbeeld worden de onderliggende interfacedefinitie voor een onderliggende interface met een enkel VLAN-ID. De VLAN-ID is uniek per peering. Het laatste octet van uw IPv4-adres wordt altijd een oneven getal zijn.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


**QinQ interfacedefinitie**

Dit voorbeeld bevat de submappen interfacedefinitie voor een onderliggende interface met twee VLAN-id. De buitenste VLAN-ID (s-code), als het gebruikt hetzelfde is gebleven via de peerings. De interne (c-code) van de VLAN-ID is uniek per peering. Het laatste octet van uw IPv4-adres wordt altijd een oneven getal zijn.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. Instellen van eBGP-sessies
U moet een BGP-sessie met Microsoft voor elke peering instellen. Het onderstaande voorbeeld kunt u instellen dat een BGP-sessie met Microsoft. Als het IPv4-adres dat u voor uw sub-interface gebruikt a.b.c.d is, worden de IP-adres van de BGP-neighbor (Microsoft) a.b.c.d+1. Het laatste octet van de BGP-neighbor IPv4-adres wordt altijd een even getal zijn.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. Voorvoegsels instellen om te worden geadverteerd via de BGP-sessie
U kunt uw router voor het adverteren Selecteer voorvoegsels naar Microsoft configureren. U kunt dit doen met behulp van het voorbeeld hieronder.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. Route maps
U kunt gebruiken route-kaarten en voorvoegsel lijsten moeten filter voorvoegsels doorgegeven in uw netwerk. Het onderstaande voorbeeld kunt u de taak uit te voeren. Zorg ervoor dat er bijbehorende voorvoegsel een lijst met setup.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Volgende stappen
Zie de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie.

