---
title: Een gegevensschijf loskoppelen van een virtuele machine van Windows - Azure | Microsoft Docs
description: Meer informatie naar een gegevensschijf loskoppelen van een virtuele machine in Azure met behulp van het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: bbbd31313db44d32a829e9e4c6c9b5fd9c0e533e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Hoe u een gegevensschijf loskoppelen van een virtuele Windows-computer
Wanneer u een gegevensschijf die is gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Hiermee verwijdert u de schijf van de virtuele machine, maar niet verwijderd uit de opslag.

> [!WARNING]
> Als u een schijf die wordt niet automatisch verwijderd loskoppelen. Als u bent geabonneerd op Premium-opslag, moet u blijft kosten voor opslag voor de schijf. Raadpleeg voor meer informatie [prijzen en facturering wanneer u Premium-opslag](premium-storage.md#pricing-and-billing).
>
>

Als u de bestaande gegevens op de schijf opnieuw wilt gebruiken, kunt u de schijf opnieuw koppelen aan dezelfde of een andere virtuele machine.

## <a name="detach-a-data-disk-using-the-portal"></a>Een gegevensschijf ontkoppelen via de portal
1. Selecteer in de portal hub **virtuele Machines**.
2. Selecteer de virtuele machine met de gegevensschijf die u wilt loskoppelen en klik op **stoppen** toewijzing van de virtuele machine.
3. Selecteer in de virtuele machineblade **schijven**.
4. Aan de bovenkant van de **schijven** blade Selecteer **bewerken**.
5. In de **schijven** blade, aan de rechterkant van de gegevensschijf die u wilt loskoppelen, klikt u op de ![Detach knopafbeelding](./media/detach-disk/detach.png) knop loskoppelen.
5. Nadat de schijf is verwijderd, klikt u op opslaan boven aan de blade.
6. Klik op de blade virtuele machine **overzicht** en klik vervolgens op de **Start** knop aan de bovenkant van de blade opnieuw opstarten van de virtuele machine.



De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.

## <a name="detach-a-data-disk-using-powershell"></a>Een gegevensschijf met behulp van PowerShell loskoppelen
In dit voorbeeld wordt de eerste opdracht wordt de virtuele machine met de naam opgehaald **MyVM07** in de **RG11** resourcegroep met de cmdlet Get-AzureRmVM. De opdracht slaat de virtuele machine in de **$VirtualMachine** variabele.

De tweede opdracht verwijdert u de gegevensschijf DataDisk3 met de naam van de virtuele machine.

De laatste opdracht werkt de status van de virtuele machine om het proces van het verwijderen van de gegevensschijf te voltooien.

```azurepowershell-interactive
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

Zie voor meer informatie [verwijderen AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).

## <a name="next-steps"></a>Volgende stappen
Als u de gegevensschijf gebruiken wilt, kunt u zojuist hebt [koppelen aan een andere virtuele machine](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

