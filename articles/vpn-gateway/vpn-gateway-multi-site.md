---
title: 'Een virtueel netwerk verbinden met meerdere sites met behulp van VPN-Gateway en PowerShell: klassieke | Microsoft Docs'
description: In dit artikel begeleidt u stapsgewijs door meerdere lokale on-premises sites met een virtuele netwerk verbindt via een VPN-Gateway voor het klassieke implementatiemodel.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-service-management
ms.assetid: b043df6e-f1e8-4a4d-8467-c06079e2c093
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: yushwang
ms.openlocfilehash: 434f84dc6244eddce9b172a617722b218360ffc2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection-classic"></a>Een Site-naar-Site-verbinding toevoegen aan een VNet met een bestaande VPN-gatewayverbinding (klassiek)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klassiek)](vpn-gateway-multi-site.md)
>
>

Dit artikel begeleidt u bij de Site-naar-Site (S2S)-verbindingen toevoegen aan een VPN-gateway met een bestaande verbinding met behulp van PowerShell. Dit type verbinding is vaak een configuratie 'multi-site' genoemd. De stappen in dit artikel van toepassing op virtuele netwerken die zijn gemaakt met behulp van het klassieke implementatiemodel (ook wel bekend als Service Management). Deze stappen zijn niet van toepassing op ExpressRoute/Site-naar-Site naast elkaar bestaande verbinding configuraties.

### <a name="deployment-models-and-methods"></a>Implementatiemodellen en -methoden

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Deze tabel wordt bijgewerkt als er nieuwe artikelen en aanvullende hulpprogramma's beschikbaar zijn voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we dit rechtstreeks aan uit deze tabel.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="about-connecting"></a>Over het aansluiten van

U kunt meerdere on-premises sites kunt verbinden met één virtueel netwerk. Dit is vooral aantrekkelijke voor het bouwen van oplossingen voor hybride cloud. Meerdere site verbinding maakt met uw Azure virtuele netwerkgateway is vergelijkbaar met het maken van de andere Site-naar-Site-verbindingen. In feite kunt u een bestaande Azure VPN-gateway, zolang de gateway dynamische is (op route gebaseerd).

Als u al een statische gateway verbonden met het virtuele netwerk hebt, kunt u het Gatewaytype naar een dynamische wijzigen zonder opnieuw opbouwen van het virtuele netwerk om te voorzien in meerdere locaties. Voordat u het type routering wijzigt, zorg dat uw on-premises VPN-gateway ondersteuning biedt voor op route gebaseerde VPN-configuraties.

![Multi-site-diagram](./media/vpn-gateway-multi-site/multisite.png "meerdere locaties")

## <a name="points-to-consider"></a>Aandachtspunten

**Het niet mogelijk de portal gebruiken om dit virtuele netwerk te wijzigen.** U moet wijzigingen aanbrengen in het configuratiebestand netwerk in plaats van de portal. Als u wijzigingen in de portal aanbrengt, worden ze uw documentatie over de meerdere site-instellingen voor dit virtuele netwerk hebt overschreven.

U moet vertrouwen, met behulp van het configuratiebestand van het netwerk op het moment dat u de procedure voor meerdere sites hebt voltooid. Echter, als er meerdere mensen werken op uw netwerkconfiguratie, hebt u nodig om ervoor te zorgen dat iedereen op de hoogte van deze beperking. Dit betekent niet dat u de portal helemaal niet gebruiken. U kunt deze gebruiken voor alle andere behalve configuratiewijzigingen aanbrengen in dit bepaalde virtuele netwerk.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met de configuratie, moet u controleren of u het volgende hebt:

* Compatibele VPN-hardware voor elke lokale locatie. Controleer [over VPN-apparaten voor verbinding met het virtuele netwerk](vpn-gateway-about-vpn-devices.md) om te controleren of het apparaat dat u wilt gebruiken iets waarvan bekend is dat compatibel is.
* Een extern gericht openbaar IPv4-IP-adres voor elke VPN-apparaat. Het IP-adres kan zich niet achter een NAT bevinden. Dit is vereiste.
* U moet de meest recente versie van de Azure PowerShell-cmdlets installeren. Zorg ervoor dat u de versie van Service Management (SM) naast de Resource Manager-versie installeert. Zie [installeren en configureren van Azure PowerShell](/powershell/azure/overview) voor meer informatie.
* Iemand die wordt getoond hoe u goed configureren van uw VPN-hardware. U hebt een uitstekend begrip van het configureren van uw VPN-apparaat of werken met iemand die biedt.
* De IP-adresbereiken die u gebruiken voor het virtuele netwerk wilt (als u dit nog niet hebt gemaakt).
* De IP-adresbereiken voor elk van de lokale netwerksites die u een verbinding met maken hebt. U moet ervoor zorgen dat het IP-adres bereiken voor elk van de lokale netwerksites die u wilt verbinding maken met elkaar niet overlappen. Anders wordt wordt de portal of de REST-API de configuratie die u wilt uploaden geweigerd.<br>Als u twee lokale netwerksites hebt dat zowel de IP-adresbereik 10.2.3.0/24 bevatten en u een pakket met een doeladres 10.2.3.3 hebt, wouldn't Azure site waarop u het pakket dat moet worden verzonden omdat de adresbereiken overlappen wilt weten. Om te voorkomen dat problemen met routing, toegestaan Azure niet voor het uploaden van een configuratiebestand met overlappende bereiken.

## <a name="1-create-a-site-to-site-vpn"></a>1. Een Site-naar-Site VPN-verbinding maken
Als u al een Site-naar-Site VPN met een gateway voor dynamische routering geweldig! U kunt verdergaan naar [exporteren van de configuratie-instellingen van het virtuele netwerk](#export). Als dat niet het geval is, moet u het volgende doen:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Als u al een virtueel netwerk van de Site-naar-Site hebt, maar een statische routering (op beleid gebaseerd)-gateway heeft:
1. Wijzig het Gatewaytype van uw in het dynamische routering. Een VPN voor meerdere locaties is een (ook wel bekend als op route gebaseerd) gateway voor dynamische routering vereist. Als u wilt wijzigen van het Gatewaytype van uw, moet u eerst de bestaande gateway verwijdert en vervolgens een nieuwe maken.
2. De nieuwe gateway configureren en uw VPN-tunnel maken. Zie voor instructies voor instructies [de SKU- en VPN-type opgeven](vpn-gateway-howto-site-to-site-classic-portal.md#sku). Zorg ervoor dat u het Type routering als 'Dynamische' opgeven.

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Als u geen Site-naar-Site virtueel netwerk hebt:
1. Uw Site-naar-Site virtueel netwerk maken met deze instructies: [een virtueel netwerk maken met een Site-naar-Site VPN-verbinding](vpn-gateway-site-to-site-create.md).  
2. Configureren van een gateway voor dynamische routering met behulp van deze instructies: [configureren van een VPN-Gateway](vpn-gateway-configure-vpn-gateway-mp.md). Selecteer **dynamische routering** voor uw Gatewaytype.

## <a name="export"></a>2. Exporteren van het configuratiebestand van het netwerk
Uw Azure-netwerk-configuratiebestand exporteren door de volgende opdracht uit te voeren. U kunt de locatie van het bestand om te exporteren naar een andere locatie indien nodig wijzigen.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

## <a name="3-open-the-network-configuration-file"></a>3. Open het configuratiebestand van het netwerk
Open het configuratiebestand van het netwerk dat u hebt gedownload in de vorige stap. Een xml-editor die u wilt gebruiken. Het bestand moet er ongeveer als volgt uitzien:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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
Wanneer u toevoegen of verwijderen van de referentie-informatie voor site, zult u configuratiewijzigingen aanbrengt aan de ConnectionsToLocalNetwork/LocalNetworkSiteRef. Toevoegen van een nieuwe lokale site verwijzing triggers Azure om een nieuwe tunnel te maken. In het onderstaande voorbeeld is de netwerkconfiguratie voor een enkele site-verbinding. Sla het bestand nadat u uw wijzigingen hebt aangebracht.

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

Voeg aanvullende verwijzingen (een configuratie met meerdere sites maken), extra 'LocalNetworkSiteRef' regels gewoon toevoegen zoals weergegeven in het voorbeeld hieronder:

```
  <Gateway>
    <ConnectionsToLocalNetwork>
      <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
      <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
    </ConnectionsToLocalNetwork>
  </Gateway>
```

## <a name="5-import-the-network-configuration-file"></a>5. Importeren van het configuratiebestand van het netwerk
Importeer het configuratiebestand van het netwerk. Als u dit bestand met de wijzigingen importeert, kunt u de nieuwe tunnels wordt toegevoegd. De tunnels gebruikt de dynamische gateway die u eerder hebt gemaakt. U kunt PowerShell gebruiken om het bestand te importeren.

## <a name="6-download-keys"></a>6. Downloaden van sleutels
Als uw nieuwe tunnels zijn toegevoegd, gebruikt u de PowerShell-cmdlet 'Get-AzureVNetGatewayKey' om op te halen van de vooraf gedeelde IPsec/IKE-sleutels voor elke tunnel.

Bijvoorbeeld:

```powershell
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"
Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"
```

Als u liever, kunt u ook gebruiken de *ophalen Virtual Network Gateway gedeelde sleutel* REST-API om op te halen van de vooraf gedeelde sleutels.

## <a name="7-verify-your-connections"></a>7. Controleer uw verbindingen
Controleer de status van de tunnel voor meerdere locaties. Na het downloaden van de sleutels voor elke tunnel, moet u controleren of verbindingen. Gebruik 'Get-AzureVnetConnection' voor een lijst van virtueel netwerk tunnels, zoals wordt weergegeven in het onderstaande voorbeeld. VNet1 is de naam van de VNet.

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
