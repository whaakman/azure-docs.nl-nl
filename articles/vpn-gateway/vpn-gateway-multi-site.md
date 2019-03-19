---
title: 'Een virtueel netwerk verbinden met meerdere sites met behulp van VPN-Gateway en PowerShell: Klassieke | Microsoft Docs'
description: Meerdere lokale on-premises sites verbinden met een klassiek virtueel netwerk met behulp van een VPN-Gateway.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: yushwang
ms.openlocfilehash: 77f8b7094c96e507eef1d360a26240627bc0e350
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57994012"
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Een Site-naar-Site-verbinding toevoegen aan een VNet met een bestaande VPN-gatewayverbinding (klassiek)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure-portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klassiek)](vpn-gateway-multi-site.md)
>
>

In dit artikel begeleidt u bij de Site-naar-Site (S2S) verbindingen toevoegen aan een VPN-gateway met een bestaande verbinding met behulp van PowerShell. Dit type verbinding wordt vaak aangeduid als 'multi-site'-configuratie. De stappen in dit artikel zijn van toepassing op virtuele netwerken die zijn gemaakt met het klassieke implementatiemodel (ook wel bekend als Service Management). Deze stappen niet van toepassing op ExpressRoute/Site-naar-Site-configuraties voor naast elkaar bestaande verbinding.

### <a name="deployment-models-and-methods"></a>Implementatiemodellen en -methoden

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Deze tabel wordt bijgewerkt naarmate nieuwe artikelen en aanvullende hulpprogramma's voor deze configuratie beschikbaar komen. Wanneer een artikel beschikbaar is, koppelen we dit rechtstreeks aan uit deze tabel.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Over het verbinden

U kunt meerdere on-premises sites verbinden met één virtueel netwerk. Dit is vooral geschikt voor het bouwen van hybride cloudoplossingen. Het maken van een multi-site-verbinding met de gateway van uw Azure-netwerk is vergelijkbaar met het maken van andere Site-naar-Site-verbindingen. In feite kunt u een bestaande Azure VPN-gateway, zolang de gateway dynamische is (op een route gebaseerd).

Als u al een statische gateway verbonden met uw virtuele netwerk hebt, kunt u de gateway van het type in dynamisch wijzigen zonder opnieuw opbouwen van het virtuele netwerk om de meerdere locaties. Voordat u het routeringstype wijzigt, zorg dat uw on-premises VPN-gateway biedt ondersteuning voor op route gebaseerde VPN-configuraties.

![Multi-site-diagram](./media/vpn-gateway-multi-site/multisite.png "multi-site")

## <a name="points-to-consider"></a>Punten om in overweging te nemen

**U niet mogelijk het gebruik van de portal wijzigingen aanbrengen in dit virtuele netwerk.** U moet wijzigingen aanbrengen in het netwerkconfiguratiebestand in plaats van de portal. Als u wijzigingen in de portal aanbrengt, wordt deze overschreven uw documentatie over de meerdere site-instellingen voor dit virtuele netwerk.

U ervaring hebt met het netwerkconfiguratiebestand op het moment dat u de procedure voor meerdere sites hebt voltooid. Echter, als er meerdere mensen werken aan uw netwerkconfiguratie, moet u om ervoor te zorgen dat iedereen op de hoogte van deze beperking. Dit betekent niet dat u de portal helemaal niet gebruiken. U kunt deze gebruiken voor alle andere taken, tenzij u configuratiewijzigingen aanbrengt in dit specifieke virtuele netwerk.

## <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u begint met de configuratie, dat u het volgende hebt:

* Compatibele VPN-hardware voor elke on-premises locatie. Controleer [over VPN-apparaten voor verbinding met het virtuele netwerk](vpn-gateway-about-vpn-devices.md) om te controleren of het apparaat dat u wilt gebruiken die bekend is dat deze compatibel is.
* Een extern gericht openbaar IPv4-IP-adres voor elke VPN-apparaat. Het IP-adres kan zich niet achter een NAT bevinden. Dit is vereiste.
* U moet de meest recente versie van de Azure PowerShell-cmdlets installeren. Zorg ervoor dat u de versie SM (Service Management) naast de Resource Manager-versie installeren. Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.
* Iemand die wordt getoond hoe u goed configureren van uw VPN-hardware. Hebt u een wezenlijk inzicht in hoe u uw VPN-apparaat configureren en werken met iemand die wordt hebben.
* De IP-adresbereiken die u gebruiken voor het virtuele netwerk wilt (als u dit nog niet hebt al een hebt gemaakt).
* De IP-adresbereiken voor elk van de lokale netwerksites die u verbinding met maken moet. U moet om ervoor te zorgen dat de IP-bereiken zijn voor elk van de lokale netwerksites die u wilt verbinding maken met elkaar niet overlappen. Anders wordt de portal of de REST-API de configuratie wordt geüpload geweigerd.<br>Hebt u twee lokale netwerksites dat zowel de IP-adresbereik 10.2.3.0/24 bevatten en u een pakket met een doeladres 10.2.3.3 hebt, wouldn't Azure bijvoorbeeld weten in welke site die u het pakket niet verzenden wilt omdat de adresbereiken overlappen. Om te voorkomen dat problemen met routering, toegestaan Azure niet voor het uploaden van een configuratiebestand met overlappende bereiken.

## <a name="1-create-a-site-to-site-vpn"></a>1. Een Site-naar-Site VPN-verbinding maken
Als u al een Site-naar-Site-VPN-verbinding met een gateway voor dynamische routering uitstekend! U kunt doorgaan met [exporteren van de configuratie-instellingen van het virtuele netwerk](#export). Als dat niet het geval is, moet u het volgende doen:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Als u al een Site-naar-Site virtueel netwerk hebt, maar een statische routering (op beleid gebaseerd)-gateway heeft:
1. Wijzig de gateway van het type in de dynamische routering. Een multi-site-VPN is een (ook wel bekend als route gebaseerd) gateway voor dynamische routering vereist. Als u wilt uw gateway van het type wijzigen, moet u eerst de bestaande gateway verwijdert en vervolgens een nieuwe maken.
2. De gateway van het nieuwe configureren en maken van uw VPN-tunnel. Zie voor instructies voor instructies [het SKU- en VPN-type opgeven](vpn-gateway-howto-site-to-site-classic-portal.md#sku). Zorg ervoor dat u het Type routering als 'Dynamische' opgeven.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Als u een Site-naar-Site virtueel netwerk hebt:
1. Uw Site-naar-Site virtueel netwerk met behulp van deze instructies maken: [Een virtueel netwerk maken met een Site-naar-Site VPN-verbinding](vpn-gateway-site-to-site-create.md).  
2. Configureer een gateway voor dynamische routering met behulp van deze instructies: [Een VPN-Gateway configureren](vpn-gateway-configure-vpn-gateway-mp.md). Zorg ervoor dat u selecteert **dynamische routering** voor uw Gatewaytype.

## <a name="export"></a>2. Het netwerkconfiguratiebestand exporteert
Uw Azure-netwerk-configuratiebestand exporteren met de volgende opdracht. U kunt de locatie van het bestand om te exporteren naar een andere locatie indien nodig wijzigen.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-the-network-configuration-file"></a>3. Open het netwerkconfiguratiebestand
Open het netwerkconfiguratiebestand dat u hebt gedownload in de vorige stap. Elke xml-editor die u wilt gebruiken. Het bestand moet er ongeveer als volgt uitzien:

        <NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Verwijzingen naar meerdere toevoegen
Wanneer u toevoegen of verwijderen van referentie-informatie voor site, zult u configuratiewijzigingen aanbrengt aan de ConnectionsToLocalNetwork/LocalNetworkSiteRef. Toevoegen van een nieuwe lokale site verwijzing triggers Azure om een nieuwe tunnel te maken. In het onderstaande voorbeeld is de configuratie van het netwerk voor een enkele site-verbinding. Sla het bestand nadat u uw wijzigingen hebt aangebracht.

```xml
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

U wilt toevoegen van aanvullende verwijzingen (Maak een configuratie met meerdere sites), voeg extra 'LocalNetworkSiteRef' regels, zoals wordt weergegeven in het voorbeeld hieronder:

```xml
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-the-network-configuration-file"></a>5. Importeren van het netwerkconfiguratiebestand
Importeer het netwerkconfiguratiebestand. Als u dit bestand met de wijzigingen importeert, wordt de nieuwe tunnels worden toegevoegd. De tunnels gebruikt de dynamische gateway die u eerder hebt gemaakt. U kunt PowerShell gebruiken om het bestand te importeren.

## <a name="6-download-keys"></a>6. Downloaden van sleutels
Zodra uw nieuwe tunnels zijn toegevoegd, gebruikt u de PowerShell-cmdlet 'Get-AzureVNetGatewayKey' om de vooraf gedeelde IPsec/IKE-sleutels voor elke tunnel.

Bijvoorbeeld:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

Als u liever, kunt u ook gebruiken de *ophalen Virtual Network Gateway gedeelde sleutel* REST-API voor het ophalen van de vooraf gedeelde sleutels.

## <a name="7-verify-your-connections"></a>7. Controleer uw verbindingen
Controleer de status van de tunnel voor meerdere locaties. Na het downloaden van de sleutels voor elke tunnel, moet u controleren of verbindingen. Gebruik 'Get-AzureVnetConnection' voor een lijst van virtueel netwerk-tunnels, zoals wordt weergegeven in het onderstaande voorbeeld. VNet1 is de naam van het VNet.

```powershell
Get-AzureVnetConnection -VNetName VNET1
```

Voorbeeld geretourneerd:

```
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over VPN-Gateways, [over VPN-Gateways](vpn-gateway-about-vpngateways.md).
