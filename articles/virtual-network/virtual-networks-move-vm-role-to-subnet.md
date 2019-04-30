---
title: Een VM (klassiek) of een exemplaar van Cloud Services verplaatsen naar een ander subnet - Azure PowerShell | Microsoft Docs
description: Leer hoe u virtuele machines (klassiek) en Cloud Services-rolinstanties verplaatsen naar een ander subnet met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 787a50a0cbf16089cd15f922b494cd12d680cb43
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640392"
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-to-a-different-subnet-using-powershell"></a>Een VM (klassiek) of een exemplaar van Cloud Services verplaatsen naar een ander subnet met behulp van PowerShell
U kunt PowerShell gebruiken om te verplaatsen van uw virtuele machines (klassiek) van het ene subnet naar een andere in hetzelfde virtuele netwerk (VNet). Rolinstanties kunnen worden verplaatst met het bewerken van het CSCFG-bestand, in plaats van met behulp van PowerShell.

> [!NOTE]
> In dit artikel wordt uitgelegd hoe u virtuele machines die zijn geïmplementeerd via het klassieke implementatiemodel alleen verplaatsen.
> 
> 

Waarom virtuele machines verplaatsen naar een ander subnet? Migratie van het subnet is nuttig wanneer de oudere subnet te klein is en kan niet worden uitgebreid vanwege bestaande actieve virtuele machines in dat subnet. In dat geval kunt u een nieuwe, groter subnet maken en de virtuele machines migreren naar het nieuwe subnet en nadat de migratie is voltooid, kunt u de oude leeg subnet verwijderen.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Een virtuele machine verplaatsen naar een ander subnet
Voor het verplaatsen van een virtuele machine, voert u de Set-AzureSubnet PowerShell-cmdlet, met behulp van het voorbeeld hieronder als sjabloon. In het onderstaande voorbeeld zijn we TestVM verplaatsen van een subnet aanwezig is, naar Subnet-2. Zorg ervoor dat het bewerken van het voorbeeld om uw omgeving weer te geven. Houd er rekening mee dat wanneer u de cmdlet Update-AzureVM uitgevoerd als onderdeel van een procedure, deze opnieuw wordt opgestart uw virtuele machine als onderdeel van het updateproces.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

Als u een statische interne privé IP-adres voor uw virtuele machine hebt opgegeven, hebt u deze instelling wijzigen voordat u de virtuele machine naar een nieuw subnet verplaatsen kunt. In dat geval gebruikt u het volgende:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Een rolinstantie verplaatsen naar een ander subnet
Bewerk het CSCFG-bestand voor het verplaatsen van een rolinstantie. In het onderstaande voorbeeld zijn we "Role0" in het virtuele netwerk verplaatsen *VNETName* van de huidige subnet *Subnet 2*. Omdat de rolinstantie is al geïmplementeerd, gaat u alleen de subnetnaam aanpassen = Subnet 2. Zorg ervoor dat het bewerken van het voorbeeld om uw omgeving weer te geven.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
