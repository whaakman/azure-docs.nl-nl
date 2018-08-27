---
title: Configureren van Azure virtuele WAN automation - voor virtuele WAN-partners | Microsoft Docs
description: Dit artikel helpt software gedefinieerde oplossingen connectiviteitspartners Azure virtuele WAN automation instellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: cherylmc
Customer intent: As a Virtual WAN software-defined connectivity provider, I want to set up a provisioning environment.
ms.openlocfilehash: bac728f286c90550107b27da76a070623577ed82
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918897"
---
# <a name="configure-virtual-wan-automation---for-virtual-wan-partners-preview"></a>Virtuele WAN-automation - voor virtuele WAN-partners (Preview) configureren

In dit artikel vindt u informatie over het instellen van de automatiseringsomgeving te koppelen en een vertakking-apparaat (een klant on-premises VPN-apparaat of SDWAN) te configureren voor Azure virtuele WAN. Als u een provider waarmee vertakking-apparaten die geschikt voor de VPN-verbinding via IPsec/IKEv2, wordt dit artikel is voor u.

Met software gedefinieerde connectiviteitsoplossingen gebruiken doorgaans een domeincontroller of een apparaat inrichten center voor het beheren van hun apparaten vertakking. De controller kunt Azure-API's gebruiken voor het automatiseren van de verbinding met Azure virtuele WAN. Dit type verbinding vereist een SDWAN of VPN-apparaat on-premises met een extern gericht openbare IP-adres is toegewezen.

##  <a name="access"></a>Toegangsbeheer

Klanten moeten kunnen juiste toegangsbeheer voor virtuele WAN instellen in de gebruikersinterface van het apparaat. Dit wordt aanbevolen met behulp van een Service-Principal voor Azure. Toegang tot service-principal op basis van bevat het juiste apparaat controller-verificatie als u wilt uploaden vertakking informatie. Zie voor meer informatie, [service-principal maken](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application).

##  <a name="site"></a>Vertakking gegevens uploaden

Ontwerp van de gebruikerservaring voor filialen (on-premises site) gegevens uploaden naar Azure. [REST API's](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) voor **VPNSite** kan worden gebruikt voor het maken van de site-informatie in een virtueel WAN. U kunt alle vertakking SDWAN/VPN-apparaten bieden, of apparaat aanpassingen selecteren waar nodig.

##  <a name="hub"></a>Hub en services

Zodra het apparaat vertakking is geüpload naar Azure, wordt klant doorgaans selecties van hubregio en/of services in Azure portal, die een reeks bewerkingen om te maken van de virtuele hub-netwerk en de VPN-eindpunt in de hub aanroept. De VPN-gateway is een schaalbare gateway welke grootten op de juiste wijze op basis van bandbreedte en verbinding behoeften.

## <a name="device"></a>Apparaatconfiguratie

In deze stap klant die niet van een provider gebruikmaakt handmatig downloaden van de Azure-configuratie en pas deze toe op hun on-premises SDWAN/VPN-apparaat. Als een provider, moet u deze stap te automatiseren. De controller kunt aanroepen **GetVpnConfiguration** REST-API voor het downloaden van de Azure-configuratie, die meestal als het volgende bestand eruit ziet.

**Opmerkingen bij de configuratie**

  * Als Azure VNets zijn gekoppeld aan de virtuele hub, wordt deze weergegeven als ConnectedSubnets.
  * VPN-verbinding maakt gebruik van op route gebaseerde configuratie en IKEv2.

### <a name="understanding-the-device-configuration-file"></a>Inzicht krijgen in het configuratiebestand van het apparaat

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
  
### <a name="example-device-configuration-file"></a>Voorbeeld van een apparaatconfiguratiebestand

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

## <a name="default"></a>Standaard-beleid

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

Uw on-premises SDWAN/VPN-apparaat of de SD-WAN-configuratie moet overeenkomen met of bevatten de volgende algoritmen en parameters die u in het Azure IPsec/IKE-beleid opgeeft. De SA-levensduren zijn alleen lokale specificaties en hoeven niet overeen te.

* IKE-versleutelingsalgoritme
* IKE-integriteitsalgoritme
* DH-groep
* IPsec-versleutelingsalgoritme
* IPsec-integriteitsalgoritme
* PFS-groep

## <a name="feedback"></a>Feedback voor de preview-versie

Wij stellen uw feedback zeer op prijs. Stuur een e-mail naar <azurevirtualwan@microsoft.com> om problemen te melden of feedback (positief of negatief) te geven over Virtual WAN. Vermeld uw bedrijfsnaam tussen '[ ]' in de onderwerpregel. Vermeld ook uw abonnements-id als u een probleem wilt melden.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over virtuele WAN [over Azure virtuele WAN](virtual-wan-about.md) en de [virtuele WAN-Veelgestelde vragen over Azure](virtual-wan-faq.md).
