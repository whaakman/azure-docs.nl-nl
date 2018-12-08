---
title: Voorbeelden van routerconfiguratie - NAT - Azure ExpressRoute | Microsoft Docs
description: Deze pagina bevat voorbeelden van routerconfiguratie voor Cisco en Juniper-routers.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 9764a03b0f3a3f70e59097359d5a714da821d3b1
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53105985"
---
# <a name="router-configuration-samples-to-set-up-and-manage-nat"></a>Voorbeelden van routerconfiguratie instellen en beheren van NAT

Deze pagina bevat voorbeelden van de NAT-configuratie voor Cisco ASA en Juniper SRX reeks routers bij het werken met ExpressRoute. Deze zijn bedoeld om te worden voorbeelden voor alleen richtlijnen en mogen niet worden gebruikt als is. U kunt werken met de leverancier van uw zijn beschikbaar met de juiste configuraties voor uw netwerk.

> [!IMPORTANT]
> Voorbeelden in deze pagina zijn bedoeld om de worden alleen voor hulp. U moet werken met uw netwerk team zijn beschikbaar met de juiste configuraties om te voldoen aan uw behoeften en de verkoop- en technische team van uw leverancier. Problemen met betrekking tot de configuraties die worden vermeld op deze pagina wordt niet door Microsoft ondersteuning. Neem contact op met de leverancier van uw apparaat voor ondersteuningsproblemen.
> 
> 

* Voorbeelden van routerconfiguratie hieronder gelden voor openbare Azure- en Microsoft-peerings. U moet NAT niet configureren voor persoonlijke Azure-peering. Beoordeling [ExpressRoute-peerings](expressroute-circuit-peerings.md) en [ExpressRoute NAT-vereisten](expressroute-nat.md) voor meer informatie.

* U moet afzonderlijke NAT IP-adresgroepen gebruiken voor verbinding met internet en ExpressRoute. Met behulp van de dezelfde NAT IP-adresgroep via internet en ExpressRoute leidt asymmetrische Routering en verlies van verbinding.


## <a name="cisco-asa-firewalls"></a>Cisco ASA-firewalls
### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>PAT-configuratie voor verkeer van het klantnetwerk naar Microsoft
    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>


    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>

    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>

    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>

    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2

    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>PAT-configuratie voor verkeer van Microsoft naar klantnetwerk

**Interfaces en de richting:**

    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

**Configuratie:**

NAT-Pool:

    object network outbound-PAT
        host <NAT-IP>

Doelserver:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Object-groep voor klant IP-adressen

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>

    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT-opdrachten:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Juniper SRX reeks routers
### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. Redundante Ethernet-interfaces voor het cluster maken
    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. Twee beveiligingszones maken
* Zone voor het interne netwerk en Untrust Zone voor netwerk extern gerichte Randrouters vertrouwen
* Juiste interfaces toewijzen aan de zones
* Op de interfaces-services toestaan

    beveiliging {zones {beveiligingszone vertrouwensrelatie {-inkomende-hostverkeer {-systeemservices {ping;                   } protocollen {bgp;                   interfaces}} {reth0.100;               }}-beveiligingszone Untrust {-inkomende-hostverkeer {-systeemservices {ping;                   } protocollen {bgp;                   interfaces}} {reth1.100;               }           }       }   }


### <a name="3-create-security-policies-between-zones"></a>3. Beveiligingsbeleid tussen zones maken
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. NAT-beleid configureren
* Maak twee NAT-pools. Een wordt gebruikt voor NAT-verkeer uitgaand naar Microsoft en andere van Microsoft aan de klant.
* Maken van regels voor NAT het betreffende verkeer
  
       security {
           nat {
               source {
                   pool SNAT-To-ExpressRoute {
                       routing-instance {
                           External-ExpressRoute;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   pool SNAT-From-ExpressRoute {
                       routing-instance {
                           Internal;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   rule-set Outbound_NAT {
                       from routing-instance Internal;
                       to routing-instance External-ExpressRoute;
                       rule SNAT-Out {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-To-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
                   rule-set Inbound-NAT {
                       from routing-instance External-ExpressRoute;
                       to routing-instance Internal;
                       rule SNAT-In {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-From-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
               }
           }
       }

### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. BGP voor het adverteren van selectieve voorvoegsels in beide richtingen configureren
Kijk voor voorbeelden in [Routing voorbeelden ](expressroute-config-samples-routing.md) pagina.

### <a name="6-create-policies"></a>6. Beleidsregels maken
    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Volgende stappen
Zie de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md) voor meer informatie.

