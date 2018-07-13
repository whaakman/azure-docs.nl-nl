---
title: Maak een netwerkbeveiligingsgroep (klassiek) met behulp van PowerShell | Microsoft Docs
description: Meer informatie over het maken en implementeren met behulp van PowerShell een netwerkbeveiligingsgroep (klassiek)
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 86810b0d-0240-46a2-8548-fca22daa56f3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.openlocfilehash: ecb977660ed99a3cea2a71a867f50822b23e568c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38634193"
---
# <a name="create-a-network-security-group-classic-using-powershell"></a>Maak een netwerkbeveiligingsgroep (klassiek) met behulp van PowerShell
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [nsg's maken in het Resource Manager-implementatiemodel](tutorial-filter-network-traffic.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Het voorbeeld PowerShell onderstaande opdrachten kunt verwachten van een eenvoudige omgeving hebt gemaakt, is afhankelijk van het bovenstaande scenario. Als u wilt dat de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst samenstellen van de testomgeving door [het maken van een VNet](virtual-networks-create-vnet-classic-netcfg-ps.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Een NSG voor het front-end-subnet maken

1. Als u Azure PowerShell nog nooit hebt gebruikt, raadpleegt u [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

2. Maak een netwerkbeveiligingsgroep met de naam *NSG-FrontEnd*:

    ```powershell   
    New-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" -Location uswest `
      -Label "Front end subnet NSG"
   ```

3. Maak een beveiligingsregel toegang vanaf internet toe te staan op poort 3389:

    ```powershell   
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name rdp-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
      -SourceAddressPrefix Internet  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '3389'
   ```

4. Maak een beveiligingsregel toegang vanaf internet toe te staan op poort 80:

    ```powershell   
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name web-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 200 `
      -SourceAddressPrefix Internet  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '80'
    ```

## <a name="create-an-nsg-for-the-back-end-subnet"></a>Een NSG voor de back-end-subnet maken

1. Maak een netwerkbeveiligingsgroep met de naam *NSG-back-end*:
   
    ```powershell
    New-AzureNetworkSecurityGroup -Name "NSG-BackEnd" -Location uswest `
      -Label "Back end subnet NSG"
    ```

2. Maak een beveiligingsregel toegang te verlenen vanaf de front-end-subnet op poort 1433 (standaardpoort die wordt gebruikt door SQL Server):
   
    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-FrontEnd" `
      | Set-AzureNetworkSecurityRule -Name rdp-rule `
      -Action Allow -Protocol TCP -Type Inbound -Priority 100 `
      -SourceAddressPrefix 192.168.1.0/24  -SourcePortRange '*' `
      -DestinationAddressPrefix '*' -DestinationPortRange '1433'
    ```

3. Maak een beveiligingsregel blokkeert de toegang van het subnet met het internet:
   
    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-BackEnd" `
      | Set-AzureNetworkSecurityRule -Name block-internet `
      -Action Deny -Protocol '*' -Type Outbound -Priority 200 `
      -SourceAddressPrefix '*'  -SourcePortRange '*' `
      -DestinationAddressPrefix Internet -DestinationPortRange '*'
   ```