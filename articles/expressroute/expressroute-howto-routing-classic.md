---
title: 'Routering (peering) voor een ExpressRoute-circuit configureren: Azure: klassieke | Microsoft Docs'
description: Dit artikel begeleidt u stapsgewijs door de procedure voor het maken en inrichten van de persoonlijke, openbare en Microsoft-peering van een ExpressRoute-circuit. In dit artikel leest u hoe u de status controleert en peerings voor uw circuit bijwerkt of verwijdert.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: cherylmc;ganesr
ms.openlocfilehash: d4d53d2f365e06c58dcbbb70ea168128c364fbfd
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52165217"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a>Peering voor een ExpressRoute-circuit (klassiek) maken en wijzigen
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure-CLI](howto-routing-cli.md)
> * [Video - Privépeering](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - openbare peering](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft-peering](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-routing-classic.md)
> 

In dit artikel leidt u door de stappen voor het maken en beheren van routeringsconfiguratie voor een ExpressRoute-circuit met behulp van PowerShell en het klassieke implementatiemodel. In de volgende stappen ziet u ook hoe u de status van een peering voor een ExpressRoute-circuit controleert en peerings bijwerkt of verwijdert en de inrichting ervan ongedaan maakt. U kunt een, twee of alle drie de peerings (Azure privé, Azure openbaar en Microsoft) voor een ExpressRoute-circuit configureren. U kunt peerings configureren in elke gewenste volgorde. U moet er echter wel voor zorgen dat u de configuratie van elke peering een voor een voltooit. 

Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services voor Layer 2-connectiviteit bieden. Als u een serviceprovider die beheerde laag-3-services (meestal een IPVPN, zoals MPLS), uw connectiviteitsprovider configureren en beheren van routering voor u.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Over Azure-implementatiemodellen**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Configuratievereisten

* Zorg dat u de pagina met [vereisten](expressroute-prerequisites.md), de pagina over [routeringsvereisten](expressroute-routing.md) en de pagina over [werkstromen](expressroute-workflows.md) hebt gelezen voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en laat het circuit ingeschakeld door de connectiviteitsprovider voordat u doorgaat. Het ExpressRoute-circuit moet zijn ingericht en zijn ingeschakeld om de hieronder beschreven cmdlets te kunnen uitvoeren.

### <a name="download-the-latest-powershell-cmdlets"></a>Download de meest recente PowerShell-cmdlets

Installeer de nieuwste versies van de Azure SM (Service Management) PowerShell-modules en de ExpressRoute-module. Wanneer u het volgende voorbeeld, houd er rekening mee dat het versienummer (in dit voorbeeld 5.1.1) worden gewijzigd als nieuwere versies van de cmdlets worden vrijgegeven.

```powershell
Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
```

Zie voor meer informatie, [aan de slag met Azure PowerShell-cmdlets](/powershell/azure/overview) voor stapsgewijze instructies over het configureren van uw computer voor het gebruik van de Azure PowerShell-modules.

### <a name="sign-in"></a>Aanmelden

Als u wilt aanmelden bij uw Azure-account, moet u de volgende voorbeelden gebruiken:

1. Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account.

  ```powershell
  Connect-AzureRmAccount
  ```
2. Controleer de abonnementen voor het account.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Als u meerdere abonnementen hebt, selecteert u het abonnement dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

4. Gebruik vervolgens de volgende cmdlet uit uw Azure-abonnement toevoegen aan PowerShell voor het klassieke implementatiemodel.

  ```powershell
  Add-AzureAccount
  ```

## <a name="azure-private-peering"></a>Persoonlijke Azure-peering

In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een persoonlijke Azure-peering voor een ExpressRoute-circuit. 

### <a name="to-create-azure-private-peering"></a>Persoonlijke Azure-peering maken

1. **Maak een ExpressRoute-circuit.**

  Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om persoonlijke Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt.
2. **Controleer de ExpressRoute-circuit om ervoor te zorgen dat deze is ingericht.**
   
  Controleer, als het ExpressRoute-circuit is ingericht en ook ingeschakeld.

  ```powershell
  Get-AzureDedicatedCircuit -ServiceKey "*********************************"
  ```

  Resultaat:

  ```powershell
  Bandwidth                        : 200
  CircuitName                      : MyTestCircuit
  Location                         : Silicon Valley
  ServiceKey                       : *********************************
  ServiceProviderName              : equinix
  ServiceProviderProvisioningState : Provisioned
  Sku                              : Standard
  Status                           : Enabled
  ```
   
  Zorg ervoor dat het circuit ingericht en ingeschakeld bevat. Als dit niet is, kunt u werken met uw connectiviteitsprovider om op te halen van het circuit met de vereiste status en de status.

  ```powershell
  ServiceProviderProvisioningState : Provisioned
  Status                           : Enabled
  ```
3. **Configureer persoonlijke Azure-peering voor het circuit.**

  Zorg ervoor dat u de volgende items hebt voordat u verdergaat met de volgende stappen:
   
  * Een /30-subnet voor de primaire koppeling. Dit mag geen deel uitmaken van een adresruimte die is gereserveerd voor virtuele netwerken.
  * Een /30-subnet voor de secundaire koppeling. Dit mag geen deel uitmaken van een adresruimte die is gereserveerd voor virtuele netwerken.
  * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
  * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken. U kunt een persoonlijk AS-nummer voor deze peering gebruiken. Controleer of dat u niet 65515 gebruikt.
  * Een MD5-hash, als u er een wilt gebruiken. **Optioneel**.
     
  Persoonlijke Azure-peering voor uw circuit configureren kunt u het volgende voorbeeld:

  ```powershell
  New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100
  ```    

  Als u wilt een MD5-hash wilt gebruiken, gebruikt u het volgende voorbeeld persoonlijke peering voor uw circuit configureren:

  ```powershell
  New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"
  ```
     
  > [!IMPORTANT]
  > Controleer of dat u uw AS-nummer als peering-ASN, niet als klant-ASN opgeeft.
  > 

### <a name="to-view-azure-private-peering-details"></a>De details van persoonlijke Azure-peering weergeven

Hier vindt u informatie over de configuratie met de volgende cmdlet:

```powershell
Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
```

Resultaat:

```
AdvertisedPublicPrefixes       : 
AdvertisedPublicPrefixesState  : Configured
AzureAsn                       : 12076
CustomerAutonomousSystemNumber : 
PeerAsn                        : 1234
PrimaryAzurePort               : 
PrimaryPeerSubnet              : 10.0.0.0/30
RoutingRegistryName            : 
SecondaryAzurePort             : 
SecondaryPeerSubnet            : 10.0.0.4/30
State                          : Enabled
VlanId                         : 100
```

### <a name="to-update-azure-private-peering-configuration"></a>De configuratie van persoonlijke Azure-peering bijwerken

Met de volgende cmdlet kunt u elk deel van de configuratie bijwerken. In het volgende voorbeeld wordt de VLAN-ID van het circuit wordt bijgewerkt van 100 tot 500.

```powershell
Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"
```

### <a name="to-delete-azure-private-peering"></a>Persoonlijke Azure-peering verwijderen

U kunt een peeringconfiguratie verwijderen door de volgende cmdlet uit te voeren. U moet ervoor zorgen dat alle virtuele netwerken losgekoppeld van het ExpressRoute-circuit zijn voordat u deze cmdlet uitvoert.

```powershell
Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
```

## <a name="azure-public-peering"></a>Openbare Azure-peering

In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een openbare Azure-peering voor een ExpressRoute-circuit.

### <a name="to-create-azure-public-peering"></a>Openbare Azure-peering maken

1. **Een ExpressRoute-circuit maken**

  Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om openbare Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt.
2. **Controleer of ExpressRoute-circuit om te controleren of deze is ingericht**

  Controleer eerst of het ExpressRoute-circuit is ingericht en is ingeschakeld.

  ```powershell
  Get-AzureDedicatedCircuit -ServiceKey "*********************************"
  ```

  Resultaat:

  ```powershell
  Bandwidth                        : 200
  CircuitName                      : MyTestCircuit
  Location                         : Silicon Valley
  ServiceKey                       : *********************************
  ServiceProviderName              : equinix
  ServiceProviderProvisioningState : Provisioned
  Sku                              : Standard
  Status                           : Enabled
  ```
   
  Controleer of dat het circuit ingericht en ingeschakeld bevat. Als dit niet is, kunt u werken met uw connectiviteitsprovider om op te halen van het circuit met de vereiste status en de status.

  ```powershell
  ServiceProviderProvisioningState : Provisioned
  Status                           : Enabled
  ```
4. **Openbare Azure-peering voor het circuit configureren**
   
  Zorg ervoor dat u de volgende informatie voordat u doorgaat:
   
  * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
  * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn.
  * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
  * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
  * Een MD5-hash, als u er een wilt gebruiken. **Optioneel**.

  > [!IMPORTANT]
  > Zorg ervoor dat u uw AS-nummer als peering-ASN en niet als klant-ASN opgeeft.
  >  
     
  Het volgende voorbeeld kunt u openbare Azure-peering voor uw circuit configureren:

  ```powershell
  New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200
  ```
     
  Als u wilt een MD5-hash wilt gebruiken, gebruikt u het volgende voorbeeld uw circuit te configureren:
     
  ```powershell
  New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"
  ```
     
### <a name="to-view-azure-public-peering-details"></a>De details van openbare Azure-peering weergeven

Als u wilt weergeven van informatie over de configuratie, gebruik de volgende cmdlet:

```powershell
Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
```

Resultaat:

```powershell
AdvertisedPublicPrefixes       : 
AdvertisedPublicPrefixesState  : Configured
AzureAsn                       : 12076
CustomerAutonomousSystemNumber : 
PeerAsn                        : 1234
PrimaryAzurePort               : 
PrimaryPeerSubnet              : 131.107.0.0/30
RoutingRegistryName            : 
SecondaryAzurePort             : 
SecondaryPeerSubnet            : 131.107.0.4/30
State                          : Enabled
VlanId                         : 200
```

### <a name="to-update-azure-public-peering-configuration"></a>De configuratie van openbare Azure-peering bijwerken

Met de volgende cmdlet kunt u elk deel van de configuratie bijwerken. In dit voorbeeld is de VLAN-ID van het circuit wordt bijgewerkt van 200 in 600.

```powershell
Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"
```

Controleer of dat het circuit ingericht en ingeschakeld bevat. 
### <a name="to-delete-azure-public-peering"></a>Openbare Azure-peering verwijderen

U kunt de peeringconfiguratie verwijderen door de volgende cmdlet:

```powershell
Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
```

## <a name="microsoft-peering"></a>Microsoft-peering

In deze sectie vindt u instructies voor het maken, verkrijgen, bijwerken en verwijderen van de configuratie voor een Microsoft-peering voor een ExpressRoute-circuit. 

### <a name="to-create-microsoft-peering"></a>Microsoft-peering maken

1. **Een ExpressRoute-circuit maken**
  
  Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-classic.md) en laat het circuit inrichten door de connectiviteitsprovider. Als uw connectiviteitsprovider beheerde Laag-3-services biedt, kunt u de connectiviteitsprovider vragen om persoonlijke Azure-peering voor u in te schakelen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider routing niet voor u beheert, volgt u onderstaande instructies wanneer u het circuit hebt gemaakt.
2. **Controleer of ExpressRoute-circuit om te controleren of deze is ingericht**

  Controleer of dat het circuit ingericht en ingeschakeld bevat. 
   
  ```powershell
  Get-AzureDedicatedCircuit -ServiceKey "*********************************"
  ```

  Resultaat:
   
  ```powershell
  Bandwidth                        : 200
  CircuitName                      : MyTestCircuit
  Location                         : Silicon Valley
  ServiceKey                       : *********************************
  ServiceProviderName              : equinix
  ServiceProviderProvisioningState : Provisioned
  Sku                              : Standard
  Status                           : Enabled
  ```
   
  Controleer of dat het circuit ingericht en ingeschakeld bevat. Als dit niet is, kunt u werken met uw connectiviteitsprovider om op te halen van het circuit met de vereiste status en de status.

  ```powershell
  ServiceProviderProvisioningState : Provisioned
  Status                           : Enabled
  ```
3. **Microsoft-peering voor het circuit configureren**
   
    Zorg ervoor dat u over de volgende informatie beschikt voordat u verder gaat.
   
   * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
   * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of dat er geen bij andere peering in het circuit dezelfde VLAN-ID gebruikt.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
   * Geadverteerde voorvoegsels: u moet een lijst verstrekken van alle voorvoegsels die u via de BGP-sessie wilt adverteren. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. Als u van plan bent om een set voorvoegsels te verzenden, kunt u een door komma's gescheiden lijst verzenden. Deze voorvoegsels moeten voor u zijn geregistreerd in een RIR/IRR.
   * Klant-ASN: als u voorvoegsels adverteert die niet zijn geregistreerd op het AS-nummer van de peering, kunt u het AS-nummer opgeven waarop ze zijn geregistreerd. **Optioneel**.
   * Naam van routeringsregister: u kunt het RIR/IRR opgeven waarbij het AS-nummer en de voorvoegsels zijn geregistreerd.
   * Een MD5-hash, als u er een wilt gebruiken. **Optioneel.**
     
  Voer de volgende cmdlet voor het configureren van Microsoft-peering voor uw circuit:
 
  ```powershell
  New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"
  ```

### <a name="to-view-microsoft-peering-details"></a>De details van Microsoft-peering weergeven

Hier vindt u informatie over de configuratie met de volgende cmdlet:

```powershell
Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
```
Resultaat:

```powershell
AdvertisedPublicPrefixes       : 123.0.0.0/30
AdvertisedPublicPrefixesState  : Configured
AzureAsn                       : 12076
CustomerAutonomousSystemNumber : 2245
PeerAsn                        : 1234
PrimaryAzurePort               : 
PrimaryPeerSubnet              : 10.0.0.0/30
RoutingRegistryName            : ARIN
SecondaryAzurePort             : 
SecondaryPeerSubnet            : 10.0.0.4/30
State                          : Enabled
VlanId                         : 300
```

### <a name="to-update-microsoft-peering-configuration"></a>Configuratie van Microsoft-peering bijwerken

U kunt een deel van de configuratie met behulp van de volgende cmdlet bijwerken:

```powershell
Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"
```

### <a name="to-delete-microsoft-peering"></a>Microsoft-peering verwijderen

U kunt de peeringconfiguratie verwijderen door de volgende cmdlet:

```powershell
Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
```

## <a name="next-steps"></a>Volgende stappen

Volgende [een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-classic.md).

* Zie voor meer informatie over werkstromen [ExpressRoute-werkstromen](expressroute-workflows.md).
* Voor meer informatie over circuitpeering raadpleegt u [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-circuits en -routeringsdomeinen).