---
title: Formaat van een Windows-VM in het klassieke implementatiemodel - Azure | Microsoft Docs
description: Het formaat van een virtuele Windows-computer in het klassieke implementatiemodel met Azure Powershell hebt gemaakt.
services: virtual-machines-windows
documentationcenter: ''
author: Drewm3
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 4277bc8394c7ba140291e9dc776162e87deab96b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
ms.locfileid: "23928242"
---
# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Een Windows-VM gemaakt in het klassieke implementatiemodel vergroten of verkleinen
In dit artikel leest u hoe het formaat van een virtuele machine van Windows, in het klassieke implementatiemodel met Azure Powershell hebt gemaakt.

Wanneer u overweegt de mogelijkheid om te vergroten of verkleinen van een virtuele machine, zijn er twee concepten die het bereik van grootten beschikbaar om het formaat van de virtuele machine te bepalen. Het eerste concept is de regio waarin uw virtuele machine wordt geïmplementeerd. De lijst met VM-grootten die beschikbaar zijn in de regio is op het tabblad Services van de webpagina van de Azure-regio's. Het tweede concept is de fysieke hardware momenteel uw virtuele machine host. De fysieke servers die als host fungeert voor VM's zijn gegroepeerd in clusters van algemene fysieke hardware. De methode van het wijzigen van een VM-grootte verschilt afhankelijk van of de gewenste nieuwe VM-grootte wordt ondersteund door de hardware-cluster momenteel de virtuele machine host.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. U kunt ook [vergroten of verkleinen van een virtuele machine gemaakt in het Resource Manager-implementatiemodel](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="add-your-account"></a>Uw account toevoegen
U moet Azure PowerShell om te werken met de klassieke Azure-resources configureren. Volg onderstaande stappen voor het configureren van Azure PowerShell voor het beheren van klassieke resources.

1. Typ het volgende achter de PowerShell-prompt `Add-AzureAccount` en klik op **Enter**. 
2. Typ in het e-mailadres dat is gekoppeld aan uw Azure-abonnement en klikt u op **doorgaan**. 
3. Typ in het wachtwoord voor uw account. 
4. Klik op **aanmelden**. 

## <a name="resize-in-the-same-hardware-cluster"></a>Vergroten of verkleinen in hetzelfde cluster hardware
Als u een virtuele machine naar een grootte die beschikbaar zijn in de hardware-cluster die als host fungeert voor de virtuele machine, moet u de volgende stappen uitvoeren.

1. Voer de volgende PowerShell-opdracht voor het weergeven van de VM-grootten beschikbaar in de hardware-cluster die als host fungeert voor de cloudservice waarin de virtuele machine.
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. Voer de volgende opdrachten om het formaat van de virtuele machine aan.
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Op een nieuwe hardware cluster vergroten of verkleinen
Het formaat van een virtuele machine naar een formaat is niet beschikbaar in de hardware-cluster die als host fungeert voor de virtuele machine, de cloud-service en alle virtuele machines in de cloudservice moeten opnieuw worden gemaakt. Elke cloudservice wordt gehost op een cluster met één hardware, zodat alle VM's in de cloudservice moet een grootte die wordt ondersteund op een hardware-cluster. De volgende stappen wordt beschreven hoe het formaat van een virtuele machine verwijderd en opnieuw maken van de cloudservice.

1. Voer de volgende PowerShell-opdracht voor het weergeven van de VM-grootten beschikbaar in de regio. 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. Noteer alle configuratie-instellingen voor elke virtuele machine in de cloudservice waarin de virtuele machine worden aangepast. 
3. Verwijder alle VM's in de cloudservice de optie voor het bewaren van de schijven voor elke virtuele machine te selecteren.
4. Maak de virtuele machine worden aangepast met behulp van de gewenste VM-grootte.
5. Maak de andere virtuele machines die zich in de cloudservice met behulp van een VM-grootte beschikbaar zijn in het cluster hardware is nu de host van de cloudservice opnieuw.

Een voorbeeld van een script voor het verwijderen en opnieuw maken van een cloudservice met behulp van een nieuwe VM-grootte vindt [hier](https://github.com/Azure/azure-vm-scripts). 

## <a name="next-steps"></a>Volgende stappen
* [Formaat van een virtuele machine gemaakt in het Resource Manager-implementatiemodel](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

