---
title: 'Een virtuele netwerkgateway verwijderen: PowerShell: Klassieke Azure-Portal | Microsoft Docs'
description: Verwijder de gateway van een virtueel netwerk met behulp van PowerShell in het klassieke implementatiemodel.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: cherylmc
ms.openlocfilehash: ca014e4f5fbc4a5695dbc5fedc85826c71a2a906
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57767767"
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a>Verwijderen van de gateway van een virtueel netwerk met behulp van PowerShell (klassiek)

> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [Klassiek - PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>

Dit artikel helpt u bij het verwijderen van een VPN-gateway in het klassieke implementatiemodel met behulp van PowerShell. Nadat de virtuele netwerkgateway is verwijderd, wijzigt u het netwerkconfiguratiebestand als u wilt verwijderen van elementen die u niet langer gebruikt.

## <a name="connect"></a>Stap 1: Verbinding maken met Azure

### <a name="1-install-the-latest-powershell-cmdlets"></a>1. Installeer de meest recente PowerShell-cmdlets.

Download en installeer de nieuwste versie van de Azure SM (Service Management) PowerShell-cmdlets. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.

### <a name="2-connect-to-your-azure-account"></a>2. Verbinding maken met uw Azure-account. 

Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Add-AzureAccount
```

## <a name="export"></a>Stap 2: Exporteren en het netwerkconfiguratiebestand weergeven

Maak een map op de computer en exporteer vervolgens het netwerkconfiguratiebestand naar de map. U dit bestand naar beide bekijkt u de huidige configuratie-informatie en om te wijzigen van de netwerkconfiguratie.

In dit voorbeeld wordt het netwerkconfiguratiebestand geëxporteerd naar C:\AzureNet.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Open het bestand met een teksteditor en de weergavenaam voor uw klassieke VNet. Wanneer u een VNet in Azure portal maakt, is de volledige naam die gebruikmaakt van Azure niet zichtbaar in de portal. Een VNet dat wordt weergegeven op de naam 'ClassicVNet1' in de Azure-portal, kan bijvoorbeeld een veel langere naam hebben in het netwerkconfiguratiebestand. De naam kan als volgt uitzien: 'Group ClassicRG1 ClassicVNet1'. Virtueel netwerknamen worden weergegeven als **' VirtualNetworkSite name ='**. De namen in het netwerkconfiguratiebestand gebruiken bij het uitvoeren van uw PowerShell-cmdlets.

## <a name="delete"></a>Stap 3: De virtuele netwerkgateway verwijderen

Wanneer u een virtuele netwerkgateway verwijdert, worden alle verbindingen met het VNet via de gateway verbroken. Als u een P2S-clients die zijn verbonden met het VNet hebt, moeten deze worden beëindigd zonder waarschuwing.

Dit voorbeeld wordt de virtuele netwerkgateway. Zorg ervoor dat u de volledige naam van het virtuele netwerk van het netwerkconfiguratiebestand.

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

Als dit lukt, ziet u het rendement:

```
Status : Successful
```

## <a name="modify"></a>Stap 4: Wijzig het configuratiebestand van het netwerk

Wanneer u een virtuele netwerkgateway verwijdert, wordt in de cmdlet het netwerkconfiguratiebestand niet wijzigen. U moet het bestand als u wilt verwijderen van de elementen die niet meer worden gebruikt te wijzigen. De volgende secties helpen u Wijzig het netwerk-configuratiebestand dat u hebt gedownload.

### <a name="lnsref"></a>Verwijzingen naar lokale netwerksites

Als u wilt verwijderen site referentie-informatie, configuratiewijzigingen aanbrengen in **ConnectionsToLocalNetwork/LocalNetworkSiteRef**. Verwijderen van een lokale site verwijzing triggers Azure te verwijderen wanneer u een tunnel. Afhankelijk van de configuratie die u hebt gemaakt, hebt u mogelijk geen een **LocalNetworkSiteRef** vermeld.

```
<Gateway>
   <ConnectionsToLocalNetwork>
     <LocalNetworkSiteRef name="D1BFC9CB_Site2">
       <Connection type="IPsec" />
     </LocalNetworkSiteRef>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

Voorbeeld:

```
<Gateway>
   <ConnectionsToLocalNetwork>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

### <a name="lns"></a>Lokale netwerksites

Verwijder alle lokale sites die u niet langer gebruikt. Afhankelijk van de configuratie die u hebt gemaakt, is het mogelijk dat u hebt geen een **LocalNetworkSite** vermeld.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
  <LocalNetworkSite name="Site3">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>57.179.18.164</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

In dit voorbeeld wordt verwijderd er alleen Site3.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

### <a name="clientaddresss"></a>Client-adresgroep

Als u een P2S-verbinding met uw VNet hebt, hebt u een **VPNClientAddressPool**. Verwijder de client-adresgroepen die overeenkomen met de gateway van het virtuele netwerk dat u hebt verwijderd.

```
<Gateway>
    <VPNClientAddressPool>
      <AddressPrefix>10.1.0.0/24</AddressPrefix>
    </VPNClientAddressPool>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

Voorbeeld:

```
<Gateway>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

### <a name="gwsub"></a>GatewaySubnet

Verwijder de **GatewaySubnet** die overeenkomt met het VNet.

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
   <Subnet name="GatewaySubnet">
     <AddressPrefix>10.11.1.0/29</AddressPrefix>
   </Subnet>
 </Subnets>
```

Voorbeeld:

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
 </Subnets>
```

## <a name="upload"></a>Stap 5: Uploaden van het netwerkconfiguratiebestand

Sla uw wijzigingen op en upload het netwerkconfiguratiebestand naar Azure. Zorg ervoor dat u het bestandspad zo nodig voor uw omgeving wijzigen.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Als dit lukt, ziet u het rendement ongeveer als in dit voorbeeld:

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded
```
