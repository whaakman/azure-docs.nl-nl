---
title: Implementeren van de Firewall van de Azure met Beschikbaarheidszones met Azure PowerShell
description: In dit artikel leert u hoe het implementeren van een Azure-Firewall met Beschikbaarheidszones met Azure PowerShell.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/10/2019
ms.author: victorh
ms.openlocfilehash: 56958eedceeb4602589d65d5e0eb7b10e8a9ff2d
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703998"
---
# <a name="deploy-an-azure-firewall-with-availability-zones-using-azure-powershell"></a>Implementeren van een Firewall van Azure met Beschikbaarheidszones met Azure PowerShell

Firewall van Azure kan worden geconfigureerd tijdens de implementatie in meerdere Beschikbaarheidszones voor verhoogde beschikbaarheid omvatten.

Met deze functie kunt de volgende scenario's:

- U kunt de bedrijfstijd van 99,99% beschikbaarheid verbeteren. Zie voor meer informatie, de Azure-Firewall [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/). De bedrijfstijd van 99,99% SLA wordt aangeboden wanneer twee of meer Beschikbaarheidszones zijn geselecteerd.
- U kunt Azure-Firewall ook koppelen aan een specifieke zone alleen omwille van de nabijheid, met behulp van de service standaard 99,95% SLA.

Zie voor meer informatie over Azure-Beschikbaarheidszones Firewall [wat is Azure Firewall?](overview.md)

De volgende Azure PowerShell-voorbeeld laat zien hoe u een Firewall van Azure met Beschikbaarheidszones kunt implementeren.

## <a name="create-a-firewall-with-availability-zones"></a>Een firewall maken met Beschikbaarheidszones

In dit voorbeeld maakt een firewall in zones 1, 2 en 3.

Als de standaard openbare IP-adres wordt gemaakt, wordt er geen specifieke zone opgegeven. Hiermee maakt u een zone-redundante IP-adres standaard. Standaard openbare IP-adressen kunnen worden geconfigureerd in alle zones, of in een enkele zone.

Het is belangrijk te weten, omdat er een firewall in zone 1 en een IP-adres in zone 2. Maar u kunt een firewall in zone 1 en IP-adres in alle zones, of een firewall en een IP-adres in dezelfde één zone voor de service-toepassing.

```azurepowershell
$rgName = "resourceGroupName"

$vnet = Get-AzVirtualNetwork `
  -Name "vnet" `
  -ResourceGroupName $rgName

$pip1 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1)
  -Zone 1,2,3
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)
