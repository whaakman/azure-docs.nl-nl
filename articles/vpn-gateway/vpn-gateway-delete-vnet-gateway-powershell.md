---
title: 'Een virtuele netwerkgateway verwijderen: PowerShell: Azure Resource Manager | Microsoft Docs'
description: De gateway van een virtueel netwerk met behulp van PowerShell in het Resource Manager-implementatiemodel verwijderen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.date: 02/07/2019
ms.author: cherylmc
ms.openlocfilehash: 922aa739a42eddbe8cd7e3cabe46681c0c2c6d46
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417071"
---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a>Verwijderen van de gateway van een virtueel netwerk met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klassiek)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

Er zijn een aantal verschillende methoden die u kunt uitvoeren wanneer u wilt verwijderen van een virtuele netwerkgateway voor de configuratie van een VPN-gateway.

- Als u wilt Alles verwijderen en opnieuw beginnen, zoals in het geval van een testomgeving, kunt u de resourcegroep verwijderen. Wanneer u een resourcegroep verwijdert, worden alle resources binnen de groep verwijderd. Deze methode is wordt alleen aanbevolen als u niet wilt dat een van de resources in de resourcegroep. U kunt slechts een paar resources met behulp van deze benadering selectief niet verwijderen.

- Als u wilt dat sommige van de resources in de resourcegroep, wordt het verwijderen van een virtuele netwerkgateway iets gecompliceerdere. Voordat u de virtuele netwerkgateway verwijderen kunt, moet u eerst alle resources die afhankelijk van de gateway zijn verwijderen. De stappen die u volgt, is afhankelijk van het type van de verbindingen die u hebt gemaakt en de afhankelijke resources voor elke verbinding.

## <a name="before-beginning"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a>1. Download de nieuwste Azure Resource Manager PowerShell-cmdlets.

Download en installeer de nieuwste versie van de Azure Resource Manager PowerShell-cmdlets. Zie voor meer informatie over het downloaden en installeren van PowerShell-cmdlets [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

### <a name="2-connect-to-your-azure-account"></a>2. Verbinding maken met uw Azure-account.

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Connect-AzAccount
```

Controleer de abonnementen voor het account.

```powershell
Get-AzSubscription
```

Als u meer dan één abonnement hebt, geeft u het abonnement dat u wilt gebruiken.

```powershell
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="S2S"></a>Een Site-naar-Site VPN-gateway verwijderen

Als u wilt verwijderen van een virtuele netwerkgateway voor een S2S-configuratie, moet u eerst elke resource die betrekking hebben op de virtuele netwerkgateway verwijderen. Resources moeten worden verwijderd in een bepaalde volgorde vanwege afhankelijkheden. Als u werkt met de voorbeelden hieronder enkele van moeten de waarden worden opgegeven, terwijl andere waarden een resultaat van de uitvoer zijn. We gebruiken de volgende specifieke waarden in de voorbeelden voor demonstratiedoeleinden te gebruiken:

VNet-naam: VNet1<br>
Resourcegroepnaam: RG1<br>
Gatewaynaam van het virtuele netwerk: GW1<br>

De volgende stappen gelden voor het Resource Manager-implementatiemodel.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Ophalen van de virtuele netwerkgateway die u wilt verwijderen.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Controleer of de gateway van het virtuele netwerk geen verbindingen heeft.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a>3. Verwijder alle verbindingen.

U mogelijk gevraagd om de verwijdering van elk van de verbindingen te bevestigen.

```powershell
$Conns | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-delete-the-virtual-network-gateway"></a>4. Verwijder de virtuele netwerkgateway.

U mogelijk gevraagd om de verwijdering van de gateway te bevestigen. Als u een P2S-configuratie in dit VNet naast de S2S-configuratie hebt, wordt er bij het verwijderen van de gateway van virtueel netwerk automatisch alle P2S-clients zonder waarschuwing verbreekt.


```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Op dit moment is uw virtuele netwerkgateway verwijderd. U kunt de volgende stappen gebruiken om alle resources die niet meer worden gebruikt te verwijderen.

### <a name="5-delete-the-local-network-gateways"></a>5 verwijderen de lokale netwerkgateways.

De lijst met de bijbehorende lokale netwerkgateways ophalen.

```powershell
$LNG=Get-AzLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

De lokale netwerkgateways verwijderen. U mogelijk gevraagd om de verwijdering van elk van de lokale netwerkgateway te bevestigen.

```powershell
$LNG | ForEach-Object {Remove-AzLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-public-ip-address-resources"></a>6. De openbare IP-adres-resources verwijderen.

Haal de IP-configuraties van de virtuele netwerkgateway.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

De lijst met openbare IP-adres resources die worden gebruikt voor deze virtuele netwerkgateway ophalen. Als de virtuele netwerkgateway actief / actief is, ziet u twee openbare IP-adressen.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Verwijder de resources voor openbaar IP-adres.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Het gatewaysubnet verwijderen en de configuratie instellen.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="v2v"></a>Een VNet-naar-VNet-VPN-gateway verwijderen

Als u wilt verwijderen van een virtuele netwerkgateway voor een V2V-configuratie, moet u eerst elke resource die betrekking hebben op de virtuele netwerkgateway verwijderen. Resources moeten worden verwijderd in een bepaalde volgorde vanwege afhankelijkheden. Als u werkt met de voorbeelden hieronder enkele van moeten de waarden worden opgegeven, terwijl andere waarden een resultaat van de uitvoer zijn. We gebruiken de volgende specifieke waarden in de voorbeelden voor demonstratiedoeleinden te gebruiken:

VNet-naam: VNet1<br>
Resourcegroepnaam: RG1<br>
Gatewaynaam van het virtuele netwerk: GW1<br>

De volgende stappen gelden voor het Resource Manager-implementatiemodel.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Ophalen van de virtuele netwerkgateway die u wilt verwijderen.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Controleer of de gateway van het virtuele netwerk geen verbindingen heeft.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Mogelijk zijn er andere verbindingen met de virtuele netwerkgateway die deel van een andere resourcegroep uitmaken. Controleer voor extra verbindingen in elke andere resourcegroep. In dit voorbeeld wordt gecontroleerd voor verbindingen van RG2. Deze uitvoeren voor elke resourcegroep die u hebt die een verbinding met de virtuele netwerkgateway kan hebben.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a>3. De lijst met verbindingen in beide richtingen ophalen.

Omdat dit een VNet-naar-VNet-configuratie, moet u de lijst met verbindingen in beide richtingen.

```powershell
$ConnsL=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
In dit voorbeeld wordt gecontroleerd voor verbindingen van RG2. Deze uitvoeren voor elke resourcegroep die u hebt die een verbinding met de virtuele netwerkgateway kan hebben.

```powershell
 $ConnsR=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a>4. Verwijder alle verbindingen.

U mogelijk gevraagd om de verwijdering van elk van de verbindingen te bevestigen.

```powershell
$ConnsL | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a>5. Verwijder de virtuele netwerkgateway.

U mogelijk gevraagd om de verwijdering van de virtuele netwerkgateway te bevestigen. Als u P2S-configuraties in uw VNets naast uw V2V-configuratie hebt, wordt er bij het verwijderen van de virtuele netwerkgateways automatisch alle P2S-clients zonder waarschuwing verbreekt.

```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Op dit moment is uw virtuele netwerkgateway verwijderd. U kunt de volgende stappen gebruiken om alle resources die niet meer worden gebruikt te verwijderen.

### <a name="6-delete-the-public-ip-address-resources"></a>6. De openbare IP-adres-resources verwijderen

Haal de IP-configuraties van de virtuele netwerkgateway.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

De lijst met openbare IP-adres resources die worden gebruikt voor deze virtuele netwerkgateway ophalen. Als de virtuele netwerkgateway actief / actief is, ziet u twee openbare IP-adressen.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Verwijder de resources voor openbaar IP-adres. U mogelijk gevraagd om de verwijdering van het openbare IP-adres te bevestigen.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Het gatewaysubnet verwijderen en de configuratie instellen.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="deletep2s"></a>Een punt-naar-Site VPN-gateway verwijderen

Als u wilt verwijderen van een virtuele netwerkgateway voor een P2S-configuratie, moet u eerst elke resource die betrekking hebben op de virtuele netwerkgateway verwijderen. Resources moeten worden verwijderd in een bepaalde volgorde vanwege afhankelijkheden. Als u werkt met de voorbeelden hieronder enkele van moeten de waarden worden opgegeven, terwijl andere waarden een resultaat van de uitvoer zijn. We gebruiken de volgende specifieke waarden in de voorbeelden voor demonstratiedoeleinden te gebruiken:

VNet-naam: VNet1<br>
Resourcegroepnaam: RG1<br>
Gatewaynaam van het virtuele netwerk: GW1<br>

De volgende stappen gelden voor het Resource Manager-implementatiemodel.


>[!NOTE]
> Wanneer u de VPN-gateway verwijdert, worden alle verbonden clients worden losgekoppeld van het VNet zonder waarschuwing.
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Ophalen van de virtuele netwerkgateway die u wilt verwijderen.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a>2. Verwijder de virtuele netwerkgateway.

U mogelijk gevraagd om de verwijdering van de virtuele netwerkgateway te bevestigen.

```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Op dit moment is uw virtuele netwerkgateway verwijderd. U kunt de volgende stappen gebruiken om alle resources die niet meer worden gebruikt te verwijderen.

### <a name="3-delete-the-public-ip-address-resources"></a>3. De openbare IP-adres-resources verwijderen

Haal de IP-configuraties van de virtuele netwerkgateway.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

De lijst met openbare IP-adressen die worden gebruikt voor deze virtuele netwerkgateway ophalen. Als de virtuele netwerkgateway actief / actief is, ziet u twee openbare IP-adressen.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Verwijder het openbare IP-adressen. U mogelijk gevraagd om de verwijdering van het openbare IP-adres te bevestigen.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="4-delete-the-gateway-subnet-and-set-the-configuration"></a>4. Het gatewaysubnet verwijderen en de configuratie instellen.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete"></a>Een VPN-gateway verwijderen door de resourcegroep te verwijderen

Als u zich geen zorgen over een van uw resources in de resourcegroep te blijven en u alleen wilt beginnen, kunt u een hele resourcegroep verwijderen. Dit is een snelle manier om alles te verwijderen. De volgende stappen gelden alleen voor de Resource Manager-implementatiemodel.

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a>1. Haal een lijst van alle resourcegroepen in uw abonnement.

```powershell
Get-AzResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a>2. Zoek de resourcegroep die u wilt verwijderen.

Zoek de resourcegroep die u wilt verwijderen en weergeven van de lijst met resources in die resourcegroep. In het voorbeeld is de naam van de resourcegroep RG1. Wijzig in het voorbeeld voor het ophalen van een lijst van alle resources.

```powershell
Find-AzResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a>3. Controleer of de resources in de lijst.

Als de lijst wordt geretourneerd, bekijken om te controleren dat u wilt verwijderen van alle resources in de resourcegroep, evenals de resourcegroep zelf. Als u wilt dat sommige van de resources in de resourcegroep, gebruikt u de stappen in de vorige secties van dit artikel uw gateway niet te verwijderen.

### <a name="4-delete-the-resource-group-and-resources"></a>4. Verwijder de resourcegroep en resources.

Als u wilt verwijderen van de resourcegroep en alle resources die zijn opgenomen in de resourcegroep, het voorbeeld wijzigen en uitvoeren.

```powershell
Remove-AzResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a>5. Controleer de status.

Het duurt enige tijd voor Azure om alle resources te verwijderen. U kunt de status van uw resourcegroep controleren met behulp van deze cmdlet.

```powershell
Get-AzResourceGroup -ResourceGroupName RG1
```

Het resultaat dat wordt geretourneerd ziet 'Geslaagd'.

```
ResourceGroupName : RG1
Location          : eastus
ProvisioningState : Succeeded
```
