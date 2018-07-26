---
title: Azure Virtual WAN gebruiken om een site-naar-site-verbinding te maken met Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Virtual WAN kunt gebruiken om een site-naar-site-VPN-verbinding te maken met Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: ea36a3d4a2471cee6a18d70275aaf2e83ffc6f39
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159648"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan-preview"></a>Zelfstudie: Een site-naar-site-verbinding maken met Azure Virtual WAN (Preview)

In deze zelfstudie leert u hoe u Virtual WAN kunt gebruiken om verbinding te maken met uw resources in Azure via een VPN-verbinding met IPsec/IKE (IKEv2)-beveiliging. Voor dit type verbinding moet er on-premises een VPN-apparaat aanwezig zijn waaraan een extern openbaar IP-adres is toegewezen. Zie voor meer informatie over Virtual WAN het [Overzicht van Virtual WAN](virtual-wan-about.md)

> [!NOTE]
> Als u veel sites hebt, zou u normaal gesproken een [Virtual WAN-partner](https://aka.ms/virtualwan) gebruiken om deze configuratie te maken. U kunt deze configuratie echter ook zelf maken als bekend bent met netwerktechnologie en uw eigen VPN-apparaat kunt configureren.
>

![Virtual WAN-diagram](./media/virtual-wan-about/virtualwan.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een WAN maken
> * Een site maken
> * Een hub maken
> * Een hub verbinden met een site
> * Een VNet verbinden met een hub
> * Een configuratie voor een VPN-apparaat downloaden en toepassen
> * Uw virtuele WAN weergeven
> * Resourcestatus weergeven
> * Een verbinding bewaken

> [!IMPORTANT]
> Azure Virtual WAN is momenteel beschikbaar als een beheerde openbare preview-versie. Als u Virtual WAN wilt gebruiken, moet u zich [inschrijven voor de preview-versie](#enroll).
>
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u aan de volgende criteria hebt voldaan:

* U hebt een compatibel VPN-apparaat nodig dat werkt met routering en IKEv2 en iemand die dit kan configureren. Als u met een Virtual WAN-partner werkt, worden de configuratie-instellingen automatisch gemaakt en hoeft u het apparaat niet zelf handmatig te kunnen configureren.
* Controleer of u een extern gericht openbaar IPv4-adres voor het VPN-apparaat hebt. Dit IP-adres kan zich niet achter een NAT bevinden.
* Als u al een virtueel netwerk hebt waarmee u verbinding wilt maken, dient u te controleren of geen van de subnetten van uw on-premises netwerk overlappen met de virtuele netwerken waarmee u verbinding wilt maken. Uw virtuele netwerk heeft geen gateway-subnet nodig en mag geen virtuele netwerkgateways hebben. Als u geen virtueel netwerk hebt, kunt u er een maken met behulp van de stappen in dit artikel.
* Zorg dat u een IP-adresbereik krijgt voor uw hubregio. De hub is een virtueel netwerk en het adresbereik dat u voor de hubregio opgeeft mag niet overlappen met een van de bestaande virtuele netwerken waarmee u verbinding wilt maken. Dit bereik mag ook niet overlappen met de adresbereiken waarmee u on-premises verbinding wilt maken. Als u de IP-adresbereiken in uw on-premises netwerkconfiguratie niet kent, moet u contact opnemen met iemand die u hierbij kan helpen en de benodigde gegevens kan verstrekken.
* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="enroll"></a>1. Registreren voor de preview-versie

Voordat u Virtual WAN kunt configureren, dient u eerst uw abonnement voor de Preview-versie te registreren. Anders kunt u niet met Virtual WAN werken in de portal. Als u zich wilt inschrijven, stuurt u een e-mail met uw abonnements-id naar **azurevirtualwan@microsoft.com**. U ontvangt een e-mailbevestiging zodra uw abonnement is geregistreerd.

## <a name="vnet"></a>2. Een virtueel netwerk maken

Als u nog geen een VNet hebt, kunt u er eenvoudig een maken met behulp van PowerShell. U kunt ook een virtueel netwerk maken via Azure Portal.

* Controleer wel of de adresruimte voor het VNet dat u maakt niet overlapt met een van de adresbereiken van andere VNet's waarmee u verbinding wilt maken of met de adresruimten van uw on-premises netwerk. 
* Als u al een VNet hebt, dient u te controleren u of dit VNet voldoet aan de vereiste criteria en geen virtuele netwerkgateway heeft.

U kunt eenvoudig een VNet maken door in dit artikel te klikken op 'Try it', waardoor een PowerShell-console wordt geopend. Pas de waarden aan en kopieer en plak de opdrachten in het consolevenster.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Pas de PowerShell-opdrachten aan en maak een resourcegroep.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Een VNet maken

Pas de PowerShell-opdrachten aan om een VNet te maken dat compatibel is met uw omgeving.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```

## <a name="wan"></a>3. Een virtueel WAN maken

1. Open een browser, ga naar [Azure Portal](https://portal.azure.com) en meld u aan met uw Azure-account.
2. Op dit moment vindt u Virtual WAN door te navigeren naar **Alle services** en te zoeken naar Virtual WAN. Ook kunt u in het zoekvak boven in Azure Portal zoeken naar Virtual WAN. Klik op **Virtual WAN** om de pagina te openen.
3. Klik op **Maken** om de pagina **WAN maken** te openen. Als deze pagina niet beschikbaar is, bent u nog niet goedgekeurd voor deze Preview-versie.

  ![WAN maken](./media/virtual-wan-site-to-site-portal/createwan.png)
4. Vul de volgende velden in op de pagina WAN maken.

  * **Naam** - kies een naam voor uw WAN.
  * **Abonnement** - selecteer het abonnement dat u wilt gebruiken.
  * **Resourcegroep** - maak een nieuwe resourcegroep of gebruik een bestaande.
  * **Resourcelocatie** - kies een resourcelocatie uit de vervolgkeuzelijst. Een WAN een globale resource en bevindt zich niet in een bepaalde regio. U moet echter een regio selecteren om de WAN-resource die u maakt eenvoudiger te kunnen beheren en vinden.
5. Klik op **Maken** om het WAN te maken.

## <a name="site"></a>4. Een site maken

Maak het aantal sites dat u nodig hebt voor uw fysieke locaties. Als u bijvoorbeeld een filiaal in NY, een filiaal in Londen en een filiaal in LA hebt, maakt u drie afzonderlijke sites. Deze sites bevatten de eindpunten voor uw on-premises VPN-apparaat. Op dit moment kunt u slechts één privé-adresruimte voor uw site opgeven.

1. Ga naar **Alle resources**.
2. Klik op de virtuele WAN die u hebt gemaakt.
3. Klik op **+Site maken** aan de bovenkant van de pagina om de pagina **Site maken** te openen.

  ![Nieuwe site](media/virtual-wan-site-to-site-portal/createsite.png)
4. Vul de volgende velden in op de pagina **Site maken**:

  *  **Naam** - dit is de naam waarmee u naar uw on-premises site wilt verwijzen.
  *  **Openbaar IP-adres** - dit is het openbare IP-adres van het VPN-apparaat van uw on-premises site.
  *  **Privié-adresruimte** - dit is de IP-adresruimte op uw on-premises site. Verkeer dat bestemd is voor deze adresruimte wordt doorgestuurd naar uw lokale site.
  *  **Abonnement** - controleer of het abonnement klopt.
  *  **Resourcegroep** - selecteer de resourcegroep die u wilt gebruiken.
5. Klik op **Geavanceerd weergeven** om aanvullende instellingen weer te geven. U kunt **BGP inschakelen** (optioneel veld, waarmee u deze functionaliteit inschakelt voor alle verbindingen die voor deze site in Azure worden gemaakt). U kunt ook **Apparaatgegevens** invoeren (optioneel veld). Hierdoor krijgt het Azure-team meer inzicht in uw omgeving, zodat het makkelijker wordt om later aanvullende optimalisatiemogelijkheden toe te voegen of u te helpen bij het oplossen van problemen.

  ![BGP](media/virtual-wan-site-to-site-portal/sitebgp.png)
6. Klik op **Bevestigen** om de site te maken.
7. Herhaal deze stappen voor elke site die u wilt maken.

## <a name="hub"></a>5. Een hub maken en sites koppelen

1. Klik op de pagina voor uw virtuele WAN op **Sites**.
2. Onder **Niet-gekoppelde sites** ziet u een lijst met sites die nog niet aan een hub zijn gekoppeld.
3. Selecteer de sites die u wilt koppelen.
4. Selecteer in de vervolgkeuzelijst de regio waar u uw hub wilt vestigen. Vestig uw hub in de regio waar de VNet's waar u verbinding mee wilt maken zich bevinden.
5. Klik op **Bevestigen**. Als u nog geen hub in deze regio hebt gemaakt, wordt er automatisch een VNet voor de virtuele hub gemaakt. In dit geval wordt de pagina **Regionale hubs maken** weergegeven.
6. Voer op de pagina **Regionale hubs maken** het adresbereik van uw hub-VNet in. Dit is het VNet waarin uw hub-services worden ondergebracht. Het bereik dat u hier opgeeft, moet een privé-IP-adresbereik zijn en mag niet overlappen met een van uw on-premises adresruimten of uw VNet-adresruimten. Er wordt een nieuw VPN-eindpunt gemaakt in het hub-VNet. (Automatisch een hub en gateway maken is alleen beschikbaar in de portal.)
7. Klik op **Create**.

## <a name="vnet"></a>6. Uw VNet verbinden met een hub

In deze stap brengt u de peering-verbinding tussen uw hub en een VNet tot stand. Herhaal deze stappen voor elk VNet dat u wilt verbinden.

1. Klik op de pagina voor uw virtuele WAN op **Virtuele netwerkverbinding**.
2. Klik op de pagina Virtuele netwerkverbinding op **+Verbinding toevoegen**.
3. Vul de volgende velden in op de pagina **Verbinding toevoegen**:

    * **Verbindingsnaam** - voer een naam in voor uw verbinding.
    * **Hubs** - selecteer de hub die u wilt koppelen aan deze verbinding.
    * **Abonnement** - controleer of het abonnement klopt.
    * **Virtueel netwerk** - selecteer het virtuele netwerk dat met deze hub wilt verbinden. Het virtuele netwerk mag geen bestaande virtuele netwerkgateway hebben.

## <a name="device"></a>7. VPN-configuratie downloaden

Gebruik de VPN-apparaatconfiguratie om uw on-premises VPN-apparaat te configureren.

1. Klik op de pagina voor uw virtuele WAN op **Overzicht**.
2. Klik boven aan de pagina Overzicht op **VPN-configuratie downloaden**. Azure maakt een opslagaccount in de resourcegroep 'microsoft-network-[location]', waarbij 'location' wordt vervangen door de locatie van het WAN. Nadat u de configuratie hebt toegepast op uw VPN-apparaten, kunt u dit storage-account verwijderen.
3. Wanneer het bestand gereed is, klikt u op de koppeling om het te downloaden.
4. Pas de configuratie toe op uw VPN-apparaat.

### <a name="understanding-the-vpn-device-configuration-file"></a>Meer informatie over het configuratiebestand voor uw VPN-apparaat

Het apparaatconfiguratiebestand bevat de instellingen die u dient te gebruiken om uw on-premises VPN-apparaat te configureren. Wanneer u dit bestand bekijkt, ziet u de volgende informatie:

* **vpnSiteConfiguration -** in deze sectie vindt u de apparaatgegevens, ingesteld als een site die verbinding maakt met het virtuele WAN. Hier vindt u ook de naam en het openbare ip-adres van het branch-apparaat.
* **vpnSiteConnections -** deze sectie bevat informatie over het volgende:

    * **Adresruimte** van het VNet van de virtuele hub(s)<br>Voorbeeld:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Adresruimte** van de VNets die zijn verbonden met de hub<br>Voorbeeld:

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

### <a name="configuring-your-vpn-device"></a>Uw VPN-apparaat configureren

>[!NOTE]
> Als u met een Virtual WAN-partneroplossing werkt, wordt het VPN-apparaat automatisch geconfigureerd wanneer de apparaatcontroller een configuratiebestand ophaalt van Azure. Dit wordt dan automatisch toegepast op het apparaat om de verbinding met Azure in te stellen. U hoeft in dat geval dus niet te weten hoe u uw VPN-apparaat handmatig configureert.
>

Als u instructies nodig hebt voor het configureren van uw apparaat, kunt u de instructies op de [pagina met configuratiescripts voor VPN-apparaten](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) gebruiken. Houd in dat geval wel rekening met de volgende dingen:

* De instructies op de pagina voor VPN-apparaten zijn niet geschreven voor Virtual WAN, maar u kunt de waarden voor Virtual WAN in het configuratiebestand gebruiken om uw VPN-apparaat handmatig te configureren. 
* De downloadbare apparaatconfiguratiescripts voor VPN Gateway werken niet voor Virtual WAN, omdat de configuratie anders is.
* Virtual WAN werkt alleen met IKEv2, niet met IKEv1.
* Virtual WAN werkt alleen met op route gebaseerde VPN-apparaten en apparaatinstructies.

## <a name="viewwan"></a>8. Uw virtuele WAN weergeven

1. Navigeer naar uw virtuele WAN.
2. Op de pagina Overzicht vertegenwoordigt elk punt op de kaart een hub. Houd de muisaanwijzer boven een punt om de statussamenvatting voor de hub weer te geven.
3. In de sectie Hubs en verbindingen vindt u informatie over de hubstatus, site, regio, VPN-verbindingsstatus en verzonden en ontvangen bytes.

## <a name="viewhealth"></a>9. Resourcestatus bekijken

1. Navigeer naar uw WAN.
2. Klik op de pagina van uw WAN, in de sectie **Ondersteuning en probleemoplossing**, op **Status** en bekijk de status van uw resource.

## <a name="connectmon"></a>10. Een verbinding bewaken

Maak een verbinding om de communicatie tussen een Azure-VM en een externe site te bewaken. Zie voor meer informatie over het instellen van een verbindingscontrole de pagina [Netwerkcommunicatie bewaken](~/articles/network-watcher/connection-monitor.md). Het bronveld is het IP-adres van de VM in Azure en het doel-IP-adres is het IP-adres voor de site.

## <a name="cleanup"></a>11. Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, wanneer u deze niet meer nodig hebt. Vervangen 'myResourceGroup' door de naam van uw resourcegroep en voer de volgende PowerShell-opdracht uit:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="feedback"></a>Feedback voor de preview-versie

Wij stellen uw feedback zeer op prijs. Stuur een e-mail naar <azurevirtualwan@microsoft.com> om problemen te melden of feedback (positief of negatief) te geven over Virtual WAN. Vermeld uw bedrijfsnaam tussen '[ ]' in de onderwerpregel. Vermeld ook uw abonnements-id als u een probleem wilt melden.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een WAN maken
> * Een site maken
> * Een hub maken
> * Een hub verbinden met een site
> * Een VNet verbinden met een hub
> * Een configuratie voor een VPN-apparaat downloaden en toepassen
> * Uw virtuele WAN weergeven
> * Resourcestatus weergeven
> * Een verbinding bewaken

Zie voor meer informatie over Virtual WAN de pagina [Overzicht van Virtual WAN](virtual-wan-about.md).
