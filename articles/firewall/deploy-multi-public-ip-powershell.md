---
title: Azure-Firewall implementeren met meerdere openbare IP-adressen met behulp van Azure PowerShell
description: In dit artikel leert u hoe het implementeren van een Azure-Firewall met meerdere openbare IP-adressen met behulp van Azure PowerShell.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/2/2019
ms.author: victorh
ms.openlocfilehash: a5a53766df3338bb36913b589ebda970de55ec94
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491940"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Implementeren van een Azure-Firewall met meerdere openbare IP-adressen met behulp van Azure PowerShell

> [!IMPORTANT]
> Firewall met meerdere openbare IP-adressen van Azure is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt een Azure-Firewall met maximaal 100 openbare IP-adressen kunt implementeren.

Met deze functie kunt de volgende scenario's:

- **DNAT** -u kunt meerdere exemplaren van de standaardpoort vertalen naar uw back-endservers. Als u twee openbare IP-adressen hebt, kunt u bijvoorbeeld TCP-poort 3389 (RDP) voor beide IP-adressen vertalen.
- **SNAT** -aanvullende poorten zijn beschikbaar voor uitgaande SNAT-verbindingen, verminderen de kans op poortuitputting SNAT. Op dit moment selecteert Azure Firewall willekeurig het openbare IP-bronadres te gebruiken voor een verbinding. Als u een downstream filteren op uw netwerk hebt, moet u alle openbare IP-adressen die zijn gekoppeld aan uw firewall toestaan.

De volgende Azure PowerShell-voorbeelden laten zien hoe u kunt toevoegen, verwijderen en configureren van openbare IP-adressen voor de Firewall van Azure.

> [!NOTE]
> Tijdens de preview-versie, als u toevoegen of verwijderen van een openbaar IP-adres aan een actieve firewall bestaande binnenkomende verbindingen met behulp van regels DNAT werkt mogelijk niet voor 40-120 seconden. U kunt de eerste openbare IP-adres toegewezen aan de firewall, tenzij de firewall is ongedaan gemaakt of verwijderd niet verwijderen.

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Een openbaar IP-adres toevoegen aan een bestaande firewall

In dit voorbeeld wordt het openbare IP-adres *azFwPublicIp1* zoals die zijn gekoppeld aan de firewall.

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

In dit voorbeeld wordt het openbare IP-adres *azFwPublicIp1* als losgekoppeld van de firewall.

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

## <a name="next-steps"></a>Volgende stappen

* [Zelfstudie: Azure Firewall-logboeken bewaken](./tutorial-diagnostics.md)
