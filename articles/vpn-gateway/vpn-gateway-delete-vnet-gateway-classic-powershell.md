---
title: 'Een virtuele netwerkgateway verwijderen: PowerShell: klassieke Azure Portal | Microsoft Docs'
description: Verwijder de gateway van een virtueel netwerk met PowerShell in het klassieke implementatiemodel.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: cherylmc
ms.openlocfilehash: b1bc18307227a728e2bc8fd95e30fdc1cbdb8c59
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a>Verwijder de gateway van een virtueel netwerk met behulp van PowerShell (klassiek)
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [Klassieke - PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

In dit artikel helpt u bij het verwijderen van een VPN-gateway in het klassieke implementatiemodel met behulp van PowerShell. Nadat de virtuele netwerkgateway is verwijderd, wijzigt u het configuratiebestand netwerk om het verwijderen van elementen die u niet meer gebruikt.

##<a name="connect"></a>Stap 1: Verbinding maken met Azure

### <a name="1-install-the-latest-powershell-cmdlets"></a>1. Installeer de meest recente PowerShell-cmdlets.

Download en installeer de nieuwste versie van de Azure Service Management (SM) PowerShell-cmdlets. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.

### <a name="2-connect-to-your-azure-account"></a>2. Verbinding maken met uw Azure-account. 

Open de PowerShell-console met verhoogde rechten en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Add-AzureAccount
```

## <a name="export"></a>Stap 2: Exporteren en het configuratiebestand van het netwerk weergeven

Maak een map op de computer en exporteer vervolgens het netwerkconfiguratiebestand naar de map. U dit bestand op beide bekijkt u de huidige configuratie-informatie en om te wijzigen van de netwerkconfiguratie.

In dit voorbeeld wordt het netwerkconfiguratiebestand geëxporteerd naar C:\AzureNet.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Open het bestand met een teksteditor en de naam voor het klassieke VNet weergeven. Wanneer u een VNet in de Azure portal maakt, is de volledige naam die gebruikmaakt van Azure niet zichtbaar in de portal. Een VNet dat wordt weergegeven naam 'ClassicVNet1' in de Azure portal kan bijvoorbeeld een veel langer naam hebben in het configuratiebestand van het netwerk. De naam kan als volgt uitzien: 'Groep ClassicRG1 ClassicVNet1'. Virtuele netwerken worden vermeld als **' VirtualNetworkSite name ='**. De namen in het configuratiebestand van het netwerk gebruiken bij het uitvoeren van PowerShell-cmdlets.

## <a name="delete"></a>Stap 3: De virtuele netwerkgateway verwijderen

Wanneer u een virtuele netwerkgateway verwijdert, worden alle verbindingen met het VNet via de gateway verbroken. Als u P2S-clients die zijn verbonden met het VNet hebt, moet het worden beëindigd zonder waarschuwing.

In dit voorbeeld wordt de virtuele netwerkgateway verwijderd. Controleer of u de volledige naam van het virtuele netwerk van het configuratiebestand van het netwerk.

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

Als dit lukt, ziet u het rendement:

```
Status : Successful
```

## <a name="modify"></a>Stap 4: Wijzig het configuratiebestand van het netwerk

Wanneer u een virtuele netwerkgateway verwijdert, wijzig de cmdlet niet het configuratiebestand van het netwerk. U moet het bestand om te verwijderen van de elementen die niet langer worden gebruikt te wijzigen. De volgende secties helpen u Wijzig het netwerk-configuratiebestand dat u hebt gedownload.

### <a name="lnsref"></a>Verwijzingen naar lokale netwerksites

Voor het verwijderen van de site-naslaginformatie configuratiewijzigingen aanbrengen in **ConnectionsToLocalNetwork/LocalNetworkSiteRef**. Verwijderen van een lokale site verwijzing triggers Azure een tunnel verwijderen. Afhankelijk van de configuratie die u hebt gemaakt, hebt u geen een **LocalNetworkSiteRef** vermeld.

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

###<a name="lns"></a>Lokale netwerksites

Verwijder alle lokale sites die u niet meer gebruikt. Afhankelijk van de configuratie die u hebt gemaakt, is het mogelijk dat u hebt geen een **LocalNetworkSite** vermeld.

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

In dit voorbeeld verwijderd we alleen Site3.

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

Als u een P2S-verbinding had uw vnet, hebt u een **VPNClientAddressPool**. Verwijder de client-adresgroepen die overeenkomen met de virtuele netwerkgateway die u hebt verwijderd.

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

## <a name="upload"></a>Stap 5: Upload het configuratiebestand van het netwerk

Sla uw wijzigingen en het configuratiebestand van het netwerk te uploaden naar Azure. Zorg ervoor dat u het bestandspad dat nodig is voor uw omgeving.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Als dit lukt, ziet u het retourtype iets dergelijks zijn op dit voorbeeld:

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded