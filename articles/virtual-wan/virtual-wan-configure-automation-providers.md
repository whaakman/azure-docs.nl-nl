---
title: Virtuele WAN-Azure-partners | Microsoft Docs
description: Dit artikel helpt partners Azure virtuele WAN automation instellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: cherylmc
Customer intent: As a Virtual WAN software-defined connectivity provider, I want to set up a provisioning environment.
ms.openlocfilehash: 0ea1b69b788255b1d8d24e17fcc1409d35a4fd36
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814318"
---
# <a name="virtual-wan-partners"></a>Virtuele WAN-partners

In dit artikel vindt u informatie over het instellen van de automatiseringsomgeving te koppelen en een vertakking-apparaat (een klant on-premises VPN-apparaat of SDWAN CPE) te configureren voor Azure virtuele WAN. Als u een provider die vertakking-apparaten die geschikt voor de VPN-verbinding via IPSec-/ IKEv2 of IPSec-/ IKEv1 bevat, wordt dit artikel is voor u.

Een dashboard controller/apparaat een vertakking-apparaat (een klant on-premises VPN-apparaat of SDWAN CPE) doorgaans gebruikt om te worden ingericht. Oplossing SD-WAN-beheerders kunnen vaak gebruiken voor een beheerconsole voor het vooraf inrichten van een apparaat voordat het wordt aangesloten op het netwerk. Deze VPN-apparaat dat compatibel haalt de besturingselement vlak logica van een domeincontroller. Het VPN-apparaat of de SD-WAN controller kunt Azure-API's gebruiken voor het automatiseren van de verbinding met Azure virtuele WAN. Dit type verbinding is vereist voor de on-premises apparaat dat een extern gericht openbaar IP-adres toegewezen.

## <a name ="before"></a>Voordat u begint met het automatiseren van

* Controleer of uw apparaat IPsec IKEv1/IKEv2 ondersteunt. Zie [standaard beleid](#default).
* Zie de [REST-API's](https://docs.microsoft.com/rest/api/azure/) die u gebruikt voor het automatiseren van de verbinding met Azure virtuele WAN.
* Testen van de ervaring van de portal van Azure virtuele WAN.
* Vervolgens kunt u beslissen welk gedeelte van de stappen verbinding u wilt automatiseren. Ten minste, wordt u aangeraden automatiseren:

  * Access Control
  * Het uploaden van gegevens van een apparaat in Azure virtuele WAN vertakking
  * Downloaden van Azure-configuratie en het instellen van de connectiviteit van het apparaat van de vertakking in Azure virtuele WAN

* Krijg inzicht in de verwachte klantervaring in combinatie met Azure virtuele WAN.

  1. Normaal gesproken wordt een virtueel WAN gebruiker het proces gestart door het maken van een virtuele WAN-resource.
  2. De gebruiker wordt ingesteld met een service-principal op basis van resource-groepstoegang voor de on-premises systeem (uw vertakking controller of VPN-apparaat inrichten software) vertakkingsinformatie schrijven in Azure virtuele WAN.
  3. De gebruiker besluiten op dit moment Meld u aan bij uw gebruikersinterface en de referenties voor de service-principal instellen. Eenmaal is voltooid, de controller moet kunnen gegevens van de vertakking met de automatisering die u levert uploaden. Het handmatige equivalent hiervan op de Azure-kant is Site maken.
  4. Nadat de Site (vertakking apparaat)-informatie beschikbaar in Azure is, wordt de gebruiker de site met een hub koppelen. Een virtuele hub is een Microsoft-beheerde virtueel netwerk. De hub bevat verschillende service-eindpunten die verbindingen vanaf uw on-premises netwerk (vpnsite) mogelijk maken. De hub is de kern van uw netwerk in een regio. Er mag alleen bestaan uit één hub per Azure-regio en het vpn-eindpunt (VPN-gateway) in dat is gemaakt tijdens dit proces. De VPN-gateway is een schaalbare gateway welke grootten op de juiste wijze op basis van bandbreedte en verbinding behoeften. U kunt kiezen om virtuele hub- en VPN-gateway maken in uw vertakking apparaat controller-dashboard te automatiseren.
  5. Zodra de virtuele Hub gekoppeld aan de site is, wordt een configuratiebestand wordt gegenereerd voor de gebruiker handmatig te downloaden. Dit is waar uw automation is beschikbaar in en de gebruikerservaring voor naadloze maakt. In plaats van de gebruiker hoeft te handmatig downloaden en configureren van het apparaat vertakking, kunt u de automatisering en minimale doorklikken ervaring bieden op uw gebruikersinterface, waardoor verlichten typische verbindingsproblemen zoals gedeelde sleutel komt niet overeen, IPSec-parameter komt niet overeen, configuratie-bestand leesbaarheid enzovoort.
  6. Aan het einde van deze stap in uw oplossing, wordt de gebruiker een naadloze site-naar-site-verbinding tussen het filiaal apparaat en de virtuele hub hebben. U kunt ook extra verbindingen instellen voor andere hubs. Elke verbinding is een actief / actief-tunnel. Uw klanten kan kiezen met een andere provider voor elk van de koppelingen voor de tunnel.

## <a name ="understand"></a>Informatie over automation-details


###  <a name="access"></a>Toegangsbeheer

Klanten moeten kunnen juiste toegangsbeheer voor virtuele WAN instellen in de gebruikersinterface van het apparaat. Dit wordt aanbevolen met behulp van een Service-Principal voor Azure. Toegang tot service-principal op basis van bevat het juiste apparaat controller-verificatie als u wilt uploaden vertakking informatie. Zie voor meer informatie, [service-principal maken](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Deze functionaliteit is buiten de Azure virtuele WAN aanbieding, wij in de lijst hieronder de gebruikelijke stappen voor het instellen van toegang in Azure, waarna de relevante gegevens in het dashboard voor beheer worden ingevoerde

* Maak een Azure Active Directory-toepassing voor uw on-premises apparaatcontroller.
* Toepassing-ID en verificatiesleutel sleutel ophalen
* Tenant-id ophalen
* Toepassing toewijzen aan rol 'Inzender'

###  <a name="branch"></a>Apparaatgegevens vertakking uploaden

Ontwerp van de gebruikerservaring voor filialen (on-premises site) gegevens uploaden naar Azure. [REST API's](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) voor VPNSite kan worden gebruikt voor het maken van de site-informatie in een virtueel WAN. U kunt alle vertakking SDWAN/VPN-apparaten bieden of apparaat aanpassingen selecteren waar nodig.


### <a name="device"></a>Downloaden van configuratie-apparaat en connectiviteit

Deze stap omvat het downloaden van Azure-configuratie en het instellen van de connectiviteit van het apparaat van de vertakking in Azure virtuele WAN. In deze stap klant die niet van een provider gebruikmaakt handmatig downloaden van de Azure-configuratie en pas deze toe op hun on-premises SDWAN/VPN-apparaat. Als een provider, moet u deze stap te automatiseren. De apparaatcontroller kunt aanroepen 'GetVpnConfiguration' REST-API voor het downloaden van de Azure-configuratie, die meestal als het volgende bestand eruit ziet.

**Opmerkingen bij de configuratie**

  * Als Azure VNets zijn gekoppeld aan de virtuele hub, wordt deze weergegeven als ConnectedSubnets.
  * VPN-verbinding maakt gebruik van op route gebaseerde configuratie en IKEv2/IKEv1.

#### <a name="understanding-the-device-configuration-file"></a>Inzicht krijgen in het configuratiebestand van het apparaat

Het apparaatconfiguratiebestand bevat de instellingen die u dient te gebruiken om uw on-premises VPN-apparaat te configureren. Wanneer u dit bestand bekijkt, ziet u de volgende informatie:

* **vpnSiteConfiguration -** in deze sectie vindt u de apparaatgegevens, ingesteld als een site die verbinding maakt met het virtuele WAN. Hier vindt u ook de naam en het openbare ip-adres van het branch-apparaat.
* **vpnSiteConnections -** deze sectie bevat informatie over het volgende:

    * **Adresruimte** van de virtuele hub VNet.<br>Voorbeeld:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Adresruimte** van de VNets die zijn verbonden met de hub.<br>Voorbeeld:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **IP-adressen** van de VPN-gateway van de virtuele hub. Omdat voor de VPN-gateway elke verbinding is opgebouwd uit 2 tunnels in een actief-/actief-configuratie, worden beide IP-adressen in dit bestand vermeld. In dit voorbeeld ziet u voor elke site 'Instance0' en 'Instance1'.<br>Voorbeeld:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Informatie over verbindingsconfiguratie van VPN-gateway**, zoals BGP, vooraf-gedeelde sleutels, enzovoort. De PSK is de vooraf gedeelde sleutel die automatisch voor u wordt gegenereerd. U kunt altijd de verbinding bewerken op de pagina Overzicht om een aangepaste PSK in te stellen.
  
#### <a name="example-device-configuration-file"></a>Voorbeeld van een apparaatconfiguratiebestand

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

## <a name="default"></a>Standaard-beleid voor IPSec-verbinding

### <a name="initiator"></a>Initiator

De volgende secties worden de combinaties van ondersteunde beleid wanneer Azure de initiator voor de tunnel is.

**Fase 1**

* AES_256, SHA1, DH_GROUP_2
* AES_256, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* 3DES, SHA1, DH_GROUP_2
* 3DES, SHA_256, DH_GROUP_2

**Fase 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256, SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE

### <a name="responder"></a>Responder

De volgende secties worden de combinaties van ondersteunde beleid wanneer Azure het eindpunt voor de tunnel is.

**Fase 1**

* AES_256, SHA1, DH_GROUP_2
* AES_256, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* 3DES, SHA1, DH_GROUP_2
* 3DES, SHA_256, DH_GROUP_2

**Fase 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256, SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE
* CBC_DES, SHA_1, PFS_NONE 
* AES_256, SHA_1, PFS_1
* AES_256, SHA_1, PFS_2
* AES_256, SHA_1, PFS_14
* AES_128, SHA_1, PFS_1
* AES_128, SHA_1, PFS_2
* AES_128, SHA_1, PFS_14
* CBC_3DES, SHA_1, PFS_1
* CBC_3DES, SHA_1, PFS_2
* CBC_3DES, SHA_256, PFS_2
* AES_256, SHA_256, PFS_1
* AES_256, SHA_256, PFS_2
* AES_256, SHA_256, PFS_14
* AES_256, SHA_1, PFS_24
* AES_256, SHA_256, PFS_24
* AES_128, SHA_256, PFS_NONE
* AES_128, SHA_256, PFS_1
* AES_128, SHA_256, PFS_2
* AES_128, SHA_256, PFS_14
* CBC_3DES, SHA_1, PFS_14

### <a name="does-everything-need-to-match-between-the-virtual-hub-vpngateway-policy-and-my-on-premises-sdwanvpn-device-or-sd-wan-configuration"></a>Alles wat moeten overeenkomen met tussen de virtuele hub-beleid voor VPN-gateway en mijn on-premises SDWAN/VPN-apparaat of de SD-WAN configuratie?

Uw on-premises SDWAN/VPN-apparaat of de SD-WAN-configuratie moet overeenkomen met of bevatten de volgende algoritmen en parameters die u in het Azure IPsec/IKE-beleid opgeeft.

* IKE-versleutelingsalgoritme
* IKE-integriteitsalgoritme
* DH-groep
* IPsec-versleutelingsalgoritme
* IPsec-integriteitsalgoritme
* PFS-groep

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over virtuele WAN [over Azure virtuele WAN](virtual-wan-about.md) en de [virtuele WAN-Veelgestelde vragen over Azure](virtual-wan-faq.md).

Voor aanvullende informatie, stuur een e-mail naar <azurevirtualwan@microsoft.com>. Vermeld uw bedrijfsnaam tussen '[ ]' in de onderwerpregel.
