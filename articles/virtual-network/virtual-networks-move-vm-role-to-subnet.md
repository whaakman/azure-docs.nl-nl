---
title: Een VM (klassiek) of een exemplaar van Cloud Services-rol verplaatsen naar een ander subnet - Azure PowerShell | Microsoft Docs
description: Informatie over het verplaatsen van virtuele machines (klassiek) en Cloud Services-rolinstanties naar een ander subnet met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: de4135c7-dc5b-4ffa-84cc-1b8364b7b427
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b094f8338394ef2e84cad3070936d715411326a4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="move-a-vm-classic-or-cloud-services-role-instance-to-a-different-subnet-using-powershell"></a>Een VM (klassiek) of een exemplaar van Cloud Services-rol verplaatsen naar een ander subnet met behulp van PowerShell
U kunt PowerShell gebruiken voor het verplaatsen van uw virtuele machines (klassiek) van één subnet naar de andere in hetzelfde virtuele netwerk (VNet). Rolinstanties kunnen worden verplaatst met het bewerken van het CSCFG-bestand in plaats van met behulp van PowerShell.

> [!NOTE]
> In dit artikel wordt uitgelegd hoe verplaatsen van virtuele machines te implementeren via het klassieke implementatiemodel gebruikt.
> 
> 

Waarom virtuele machines verplaatsen naar een ander subnet? Migratie van subnet is handig wanneer de oudere subnet te klein is en kan niet worden uitgebreid vanwege bestaande actieve virtuele machines in dat subnet. In dat geval kunt u een nieuwe, groter subnet maken en de virtuele machines migreren naar het nieuwe subnet en nadat de migratie is voltooid, kunt u het oude leeg subnet verwijderen.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Een virtuele machine verplaatsen naar een ander subnet
Voer de Set AzureSubnet PowerShell-cmdlet met het onderstaande voorbeeld als sjabloon voor het verplaatsen van een virtuele machine. In het onderstaande voorbeeld verplaatst we TestVM van de huidige subnet naar Subnet 2. Zorg ervoor dat het voorbeeld naar aanleiding van uw omgeving bewerken. Houd er rekening mee dat wanneer u de cmdlet Update-AzureVM als onderdeel van een procedure uitvoert, deze opnieuw wordt opgestart uw virtuele machine als onderdeel van het updateproces.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
    | Set-AzureSubnet –SubnetNames Subnet-2 `
    | Update-AzureVM

Als u een statische interne persoonlijke IP-adres voor de virtuele machine opgegeven, hebt u deze instelling te wissen voordat u de virtuele machine naar een nieuw subnet verplaatsen kunt. In dat geval gebruikt u het volgende:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
    | Set-AzureSubnet -SubnetNames Subnet-2 `
    | Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Een rolinstantie verplaatsen naar een ander subnet
Bewerk het CSCFG-bestand voor het verplaatsen van een rolexemplaar. In het onderstaande voorbeeld verplaatst we 'Role0' in het virtuele netwerk *VNETName* van de aanwezig subnet *Subnet 2*. Omdat de rolinstantie al is geïmplementeerd, moet u alleen de subnetnaam wijzigen = Subnet 2. Zorg ervoor dat het voorbeeld naar aanleiding van uw omgeving bewerken.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
