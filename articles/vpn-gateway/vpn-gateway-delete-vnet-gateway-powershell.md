---
title: 'Een virtuele netwerkgateway verwijderen: PowerShell: Azure Resource Manager | Microsoft Docs'
description: Verwijder de gateway van een virtueel netwerk met behulp van PowerShell in het Resource Manager-implementatiemodel.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: 4d0f085423d5bd60b24d88649ee1d77bcd1d009f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a>Verwijder de gateway van een virtueel netwerk met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klassiek)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

Er zijn verschillende manieren die kunt u uw als u wilt verwijderen van een virtuele netwerkgateway voor de configuratie van een VPN-gateway.

- Als u wilt Alles verwijderen en opnieuw beginnen, zoals in het geval van een testomgeving kunt u de resourcegroep verwijderen. Wanneer u een resourcegroep verwijdert, worden alle resources binnen de groep verwijderd. Deze methode is wordt alleen aanbevolen als u niet wilt dat de bronnen in de resourcegroep. U kunt alleen een aantal bronnen met deze benadering selectief niet verwijderen.

- Als u wilt dat een aantal van de resources in de resourcegroep, verwijderen van een virtuele netwerkgateway enigszins iets gecompliceerder. Voordat u de virtuele netwerkgateway verwijderen kunt, moet u eerst alle bronnen die afhankelijk van de gateway zijn te verwijderen. De stappen die u volgt, is afhankelijk van het type van de verbindingen die u hebt gemaakt en de afhankelijke resources voor elke verbinding.

## <a name="before-beginning"></a>Voordat u begint

### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a>1. Download de nieuwste Azure Resource Manager PowerShell-cmdlets.

Download en installeer de nieuwste versie van de Azure Resource Manager PowerShell-cmdlets. Zie voor meer informatie over het downloaden en installeren van PowerShell-cmdlets [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

### <a name="2-connect-to-your-azure-account"></a>2. Verbinding maken met uw Azure-account.

Open de PowerShell-console en maak verbinding met uw account. Gebruik het volgende voorbeeld als hulp bij het maken van de verbinding:

```powershell
Login-AzureRmAccount
```

Controleer de abonnementen voor het account.

```powershell
Get-AzureRmSubscription
```

Als u meer dan één abonnement hebt, geeft u het abonnement dat u wilt gebruiken.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="S2S"></a>Verwijderen van een Site-naar-Site VPN-gateway

Als u wilt verwijderen van een virtuele netwerkgateway voor een S2S-configuratie, moet u eerst elke bron die betrekking op de virtuele netwerkgateway hebben verwijderen. Resources moeten worden verwijderd in een bepaalde volgorde vanwege afhankelijkheden. Als u werkt met de voorbeelden hieronder enkele van moeten de waarden worden opgegeven, terwijl andere waarden een uitvoerresultaat zijn. We gebruiken de volgende specifieke waarden in de voorbeelden voor demonstratiedoeleinden:

VNet-naam: VNet1<br>
De naam van resourcegroep: RG1<br>
Naam van virtuele-netwerkgateway: GW1<br>

De volgende stappen van toepassing op het Resource Manager-implementatiemodel.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Haal de virtuele netwerkgateway die u wilt verwijderen.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Controleer of de virtuele netwerkgateway geen verbindingen heeft.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a>3. Verwijder alle verbindingen.

U wordt mogelijk gevraagd de verwijdering van elk van de verbindingen te bevestigen.

```powershell
$Conns | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-delete-the-virtual-network-gateway"></a>4. Verwijder de virtuele netwerkgateway.

U wordt mogelijk gevraagd de verwijdering van de gateway te bevestigen. Als u een P2S-configuratie in dit VNet naast de S2S-configuratie hebt, wordt er bij het verwijderen van de virtuele netwerkgateway automatisch alle P2S-clients zonder waarschuwing verbreekt.


```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Op dit moment is uw virtuele netwerkgateway verwijderd. De volgende stappen kunt u alle resources verwijderen die niet langer worden gebruikt.

### <a name="5-delete-the-local-network-gateways"></a>5 verwijderen uit de lokale netwerkgateways.

De lijst van de bijbehorende lokale netwerkgateways ophalen.

```powershell
$LNG=Get-AzureRmLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

Verwijderen van de lokale netwerkgateways. U wordt mogelijk gevraagd de verwijdering van elk van de lokale netwerkgateway te bevestigen.

```powershell
$LNG | ForEach-Object {Remove-AzureRmLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-public-ip-address-resources"></a>6. Verwijder het openbare IP-adresbronnen.

De IP-configuraties van de virtuele netwerkgateway ophalen.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

De lijst met openbare IP-adresbronnen gebruikt voor deze virtuele netwerkgateway ophalen. Als de virtuele netwerkgateway actief-actief is, ziet u twee openbare IP-adressen.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Het openbare IP-resources verwijderen.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Het gatewaysubnet verwijderen en de configuratie ingesteld.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="v2v"></a>Verwijderen van een VNet-naar-VNet-VPN-gateway

Als u wilt verwijderen van een virtuele netwerkgateway voor een V2V-configuratie, moet u eerst elke bron die betrekking op de virtuele netwerkgateway hebben verwijderen. Resources moeten worden verwijderd in een bepaalde volgorde vanwege afhankelijkheden. Als u werkt met de voorbeelden hieronder enkele van moeten de waarden worden opgegeven, terwijl andere waarden een uitvoerresultaat zijn. We gebruiken de volgende specifieke waarden in de voorbeelden voor demonstratiedoeleinden:

VNet-naam: VNet1<br>
De naam van resourcegroep: RG1<br>
Naam van virtuele-netwerkgateway: GW1<br>

De volgende stappen van toepassing op het Resource Manager-implementatiemodel.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Haal de virtuele netwerkgateway die u wilt verwijderen.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Controleer of de virtuele netwerkgateway geen verbindingen heeft.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Mogelijk zijn er andere verbindingen met de virtuele netwerkgateway die deel van een andere resourcegroep uitmaken. Controleren op aanvullende verbindingen in elke extra resourcegroep. In dit voorbeeld zijn er voor de verbindingen van RG2 gezocht. Deze uitvoeren voor elke resourcegroep dat u hebt die een verbinding met de virtuele netwerkgateway kunnen hebben.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a>3. De lijst met verbindingen opgehaald in beide richtingen.

Omdat dit een VNet-naar-VNet-configuratie, moet u de lijst met verbindingen in beide richtingen.

```powershell
$ConnsL=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
In dit voorbeeld zijn er voor de verbindingen van RG2 gezocht. Deze uitvoeren voor elke resourcegroep dat u hebt die een verbinding met de virtuele netwerkgateway kunnen hebben.

```powershell
 $ConnsR=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a>4. Verwijder alle verbindingen.

U wordt mogelijk gevraagd de verwijdering van elk van de verbindingen te bevestigen.

```powershell
$ConnsL | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a>5. Verwijder de virtuele netwerkgateway.

U wordt mogelijk gevraagd de verwijdering van de virtuele netwerkgateway te bevestigen. Als u P2S-configuraties naar uw vnet's naast de V2V-configuratie hebt, wordt er bij het verwijderen van de virtuele netwerkgateways automatisch alle P2S-clients zonder waarschuwing verbroken.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Op dit moment is uw virtuele netwerkgateway verwijderd. De volgende stappen kunt u alle resources verwijderen die niet langer worden gebruikt.

### <a name="6-delete-the-public-ip-address-resources"></a>6. De resources van openbare IP-adres verwijderen

De IP-configuraties van de virtuele netwerkgateway ophalen.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

De lijst met openbare IP-adresbronnen gebruikt voor deze virtuele netwerkgateway ophalen. Als de virtuele netwerkgateway actief-actief is, ziet u twee openbare IP-adressen.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Het openbare IP-resources verwijderen. U wordt mogelijk gevraagd de verwijdering van het openbare IP-adres te bevestigen.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Het gatewaysubnet verwijderen en de configuratie ingesteld.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="deletep2s"></a>Verwijderen van een punt-naar-Site VPN-gateway

Als u wilt verwijderen van een virtuele netwerkgateway voor een P2S-configuratie, moet u eerst elke bron die betrekking op de virtuele netwerkgateway hebben verwijderen. Resources moeten worden verwijderd in een bepaalde volgorde vanwege afhankelijkheden. Als u werkt met de voorbeelden hieronder enkele van moeten de waarden worden opgegeven, terwijl andere waarden een uitvoerresultaat zijn. We gebruiken de volgende specifieke waarden in de voorbeelden voor demonstratiedoeleinden:

VNet-naam: VNet1<br>
De naam van resourcegroep: RG1<br>
Naam van virtuele-netwerkgateway: GW1<br>

De volgende stappen van toepassing op het Resource Manager-implementatiemodel.


>[!NOTE]
> Wanneer u de VPN-gateway verwijdert, worden alle verbonden clients worden losgekoppeld van het VNet zonder waarschuwing.
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Haal de virtuele netwerkgateway die u wilt verwijderen.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a>2. Verwijder de virtuele netwerkgateway.

U wordt mogelijk gevraagd de verwijdering van de virtuele netwerkgateway te bevestigen.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Op dit moment is uw virtuele netwerkgateway verwijderd. De volgende stappen kunt u alle resources verwijderen die niet langer worden gebruikt.

### <a name="3-delete-the-public-ip-address-resources"></a>3. De resources van openbare IP-adres verwijderen

De IP-configuraties van de virtuele netwerkgateway ophalen.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

De lijst met openbare IP-adressen gebruikt voor deze virtuele netwerkgateway ophalen. Als de virtuele netwerkgateway actief-actief is, ziet u twee openbare IP-adressen.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Verwijder het openbare IP-adressen. U wordt mogelijk gevraagd de verwijdering van het openbare IP-adres te bevestigen.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="4-delete-the-gateway-subnet-and-set-the-configuration"></a>4. Het gatewaysubnet verwijderen en de configuratie ingesteld.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete"></a>Een VPN-gateway verwijderen door het verwijderen van de resourcegroep

Als u zich geen zorgen over het houden van uw resources in de resourcegroep en u wilt beginnen, kunt u een hele resourcegroep verwijderen. Dit is een snelle manier om alles verwijderen. De volgende stappen gelden alleen voor het Resource Manager-implementatiemodel.

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a>1. Een lijst met alle resourcegroepen in uw abonnement ophalen.

```powershell
Get-AzureRmResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a>2. Zoek de resourcegroep die u wilt verwijderen.

Zoek de resourcegroep die u wilt verwijderen en de lijst met resources in die resourcegroep weergeven. In het voorbeeld is de naam van de resourcegroep RG1. Wijzig in het voorbeeld voor het ophalen van een lijst met alle bronnen.

```powershell
Find-AzureRmResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a>3. Controleer of de bronnen in de lijst.

Als de lijst wordt geretourneerd, bekijken om te controleren dat u wilt verwijderen van alle resources in de resourcegroep, evenals de resourcegroep zelf. Als u wilt dat sommige van de resources in de resourcegroep, gebruikt u de stappen in de vorige secties van dit artikel uw gateway verwijderen.

### <a name="4-delete-the-resource-group-and-resources"></a>4. Verwijder de resourcegroep en resources.

Als u wilt verwijderen van de resourcegroep en de resource die zijn opgenomen in de resourcegroep, in het voorbeeld wijzigen en uitvoeren.

```powershell
Remove-AzureRmResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a>5. Controleer de status.

Het duurt enige tijd Azure alle resources verwijderd. U kunt de status van de resourcegroep controleren met behulp van deze cmdlet.

```powershell
Get-AzureRmResourceGroup -ResourceGroupName RG1
```

Het resultaat dat wordt geretourneerd toont 'Voltooid'.

```
ResourceGroupName : RG1
Location          : eastus
ProvisioningState : Succeeded
```