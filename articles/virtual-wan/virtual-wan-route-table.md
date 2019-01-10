---
title: Een Azure virtuele WAN virtuele hub-routetabel om door te sturen naar NVA maken | Microsoft Docs
description: Virtuele WAN virtuele hub routetabel sturen verkeer naar een virtueel netwerkapparaat.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to work with routing tables for NVA.
ms.openlocfilehash: 7d5cd8aab0f368ffec636e6dfcacf127c910dafc
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190167"
---
# <a name="create-a-virtual-hub-route-table-to-steer-traffic-to-a-network-virtual-appliance"></a>De routetabel van een virtuele Hub om door te sturen verkeer naar een virtueel netwerkapparaat maken

In dit artikel wordt beschreven hoe u verkeer van een virtuele Hub naar een virtueel netwerkapparaat sturen. 

![Virtual WAN-diagram](./media/virtual-wan-route-table/vwanroute.png)

In dit artikel leert u het volgende:

* Een WAN maken
* Een hub maken
* Hub virtuele netwerkverbindingen maken
* Een hub-route maken
* Een routetabel maken
* De routetabel toepassen op

## <a name="before-you-begin"></a>Voordat u begint

Controleer of dat u voldoet aan de volgende criteria:

1. U hebt een Network Virtual Appliance (NVA) is een software van derden van uw keuze die normaal gesproken is ingericht vanuit Azure Marketplace (koppeling) in een virtueel netwerk.
2. U hebt een privé IP-adres toegewezen aan de netwerkinterface van de NVA. 
3. NVA kan niet worden geïmplementeerd in de virtuele hub. Worden moet geïmplementeerd in een apart VNet. In dit artikel worden de VNet is het VNet van het DMZ genoemd.
4. Het DMZ VNet kan een of meerdere virtuele netwerken zijn verbonden. In dit artikel wordt dit VNet 'Indirecte knooppunt VNet' genoemd. Deze vnet's kunnen worden verbonden met het DMZ-VNet via VNet-peering.
5. Controleer of u 2 VNets die zijn al gemaakt. Deze wordt gebruikt als knooppunt VNets. In dit artikel worden de adresruimten van de VNet-spoke 10.0.2.0/24 en 10.0.3.0/24. Als u informatie over het maken van een VNet, Zie [maken van een virtueel netwerk met behulp van PowerShell](../virtual-network/quick-create-powershell.md).
6. Zorg ervoor dat er geen virtuele netwerkgateways zijn in elk vnet's.

## <a name="signin"></a>1. Aanmelden

Zorg ervoor dat u de nieuwste versie van de Resource Manager PowerShell-cmdlets installeren. Zie [How to install and configure Azure PowerShell](/powershell/azure/azurerm/overview) (Azure PowerShell installeren en configureren) voor meer informatie over het installeren van de PowerShell-cmdlets. Dit is belangrijk omdat eerdere versies van de cmdlets niet de huidige waarden bevatten die u nodig hebt voor deze oefening. De modules in thee volgen voorbeelden zijn Azure RM. In dit artikel wordt in de toekomst worden bijgewerkt naar Azure Az.

1. Open de PowerShell-console met verhoogde bevoegdheden en meld u aan bij uw Azure-account. Deze cmdlet vraagt u om de aanmeldingsreferenties. Na het aanmelden, het instellingen van uw account gedownload zodat ze beschikbaar voor Azure PowerShell zijn.

  ```powershell
  Connect-AzureRmAccount
  ```
2. Haal een lijst met uw Azure-abonnementen op.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Geef het abonnement op dat u wilt gebruiken.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```

## <a name="rg"></a>2. Resources maken

1. Maak een resourcegroep.

  ```powershell
  New-AzureRmResourceGroup -Location "West US" -Name "testRG"
  ```
2. Maak een virtueel WAN.

  ```powershell
  $virtualWan = New-AzureRmVirtualWan -ResourceGroupName "testRG" -Name "myVirtualWAN" -Location "West US"
  ```
3. Maak een virtuele hub.

  ```powershell
  New-AzureRmVirtualHub -VirtualWan $virtualWan -ResourceGroupName "testRG" -Name "westushub" -AddressPrefix "10.0.1.0/24"
  ```

## <a name="connections"></a>3. Verbindingen maken

Hub maken virtuele netwerkverbindingen van indirecte knooppunt VNet en het DMZ-VNet op de virtuele hub.

  ```powershell
  $remoteVirtualNetwork1= Get-AzureRmVirtualNetwork -Name “indirectspoke1” -ResourceGroupName “testRG”
  $remoteVirtualNetwork2= Get-AzureRmVirtualNetwork -Name “indirectspoke2” -ResourceGroupName “testRG”
  $remoteVirtualNetwork3= Get-AzureRmVirtualNetwork -Name “dmzvnet” -ResourceGroupName “testRG”

  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection1” -RemoteVirtualNetwork $remoteVirtualNetwork1
  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection2” -RemoteVirtualNetwork $remoteVirtualNetwork2
  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection3” -RemoteVirtualNetwork $remoteVirtualNetwork3
  ```

## <a name="route"></a>4. Een virtuele hub-route maken

In dit artikel de indirecte knooppunt VNet-adresruimten zijn 10.0.2.0/24 en 10.0.3.0/24 en het DMZ-NVA network interface particulier IP-adres is 10.0.4.5.

```powershell
$route1 = New-AzureRmVirtualHubRoute -AddressPrefix @("10.0.2.0/24", "10.0.3.0/24") -NextHopIpAddress "10.0.4.5"
```

## <a name="applyroute"></a>5. De routetabel van een virtuele hub maken

De routetabel van een virtuele hub maken en vervolgens de gemaakte route van toepassing op het.
 
```powershell
$routeTable = New-AzureRmVirtualHubRouteTable -Route @($route1)
```

## <a name="commit"></a>6. De wijzigingen doorvoeren

Voer de wijzigingen in de virtuele hub.

```powershell
Update-AzureRmVirtualHub -VirtualWanId $virtualWan.Id -ResourceGroupName "testRG" -Name "westushub” -RouteTable $routeTable
```

## <a name="cleanup"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, wanneer u deze niet meer nodig hebt. Vervangen 'myResourceGroup' door de naam van uw resourcegroep en voer de volgende PowerShell-opdracht uit:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Virtual WAN de pagina [Overzicht van Virtual WAN](virtual-wan-about.md).
