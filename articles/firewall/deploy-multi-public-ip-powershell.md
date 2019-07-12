---
title: Azure-Firewall implementeren met meerdere openbare IP-adressen met behulp van Azure PowerShell
description: In dit artikel leert u hoe het implementeren van een Azure-Firewall met meerdere openbare IP-adressen met behulp van Azure PowerShell.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/10/2019
ms.author: victorh
ms.openlocfilehash: ce47612f18ee64caa3a053001deb5448f7c27bfd
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703985"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Implementeren van een Azure-Firewall met meerdere openbare IP-adressen met behulp van Azure PowerShell

> [!IMPORTANT]
> Firewall met meerdere openbare IP-adressen van Azure is beschikbaar via Azure PowerShell, Azure CLI, REST en sjablonen. De portal-gebruikersinterface incrementeel aan regio's wordt toegevoegd en is beschikbaar in alle regio's wanneer de implementatie is voltooid.

U kunt een Azure-Firewall met maximaal 100 openbare IP-adressen kunt implementeren.

Met deze functie kunt de volgende scenario's:

- **DNAT** -u kunt meerdere exemplaren van de standaardpoort vertalen naar uw back-endservers. Als u twee openbare IP-adressen hebt, kunt u bijvoorbeeld TCP-poort 3389 (RDP) voor beide IP-adressen vertalen.
- **SNAT** -aanvullende poorten zijn beschikbaar voor uitgaande SNAT-verbindingen, verminderen de kans op poortuitputting SNAT. Op dit moment selecteert Azure Firewall willekeurig het openbare IP-bronadres te gebruiken voor een verbinding. Als u een downstream filteren op uw netwerk hebt, moet u alle openbare IP-adressen die zijn gekoppeld aan uw firewall toestaan.

De volgende Azure PowerShell-voorbeelden laten zien hoe u kunt configureren, toevoegen en verwijderen van openbare IP-adressen voor de Firewall van Azure.

> [!NOTE]
> U kunt de eerste IP-configuratie niet verwijderen uit de Azure-Firewall openbare IP-adres-configuratiepagina. Als u wijzigen van het IP-adres wilt, kunt u Azure PowerShell gebruiken.

## <a name="create-a-firewall-with-two-or-more-public-ip-addresses"></a>Een firewall maken met twee of meer openbare IP-adressen

In dit voorbeeld maakt een firewall die is gekoppeld aan virtueel netwerk *vnet* met twee openbare IP-adressen.

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

$pip2 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp2" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1, $pip2)
```

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Een openbaar IP-adres toevoegen aan een bestaande firewall

In dit voorbeeld wordt het openbare IP-adres *azFwPublicIp1* is gekoppeld aan de firewall.

```azurepowershell
$pip = New-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.AddPublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="remove-a-public-ip-address-from-an-existing-firewall"></a>Verwijderen van een openbaar IP-adres van een bestaande firewall

In dit voorbeeld wordt het openbare IP-adres *azFwPublicIp1* is losgekoppeld van de firewall.

```azurepowershell
$pip = Get-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg"

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.RemovePublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="next-steps"></a>Volgende stappen

* [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)
