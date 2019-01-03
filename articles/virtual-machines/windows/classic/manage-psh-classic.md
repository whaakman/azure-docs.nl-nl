---
title: Uw virtuele machines beheren met behulp van Azure PowerShell | Microsoft Docs
description: Meer informatie over opdrachten die u gebruiken kunt om taken bij het beheren van uw virtuele machines te automatiseren.
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: b7fafa148417ba1667ec0277b414105f95e428ce
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53971781"
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Virtuele machines beheren met Azure PowerShell
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Raadpleeg voor algemene PowerShell-opdrachten met behulp van de Resource Manager-model, [hier](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Veel taken die u elke dag voor het beheren van uw virtuele machines kunnen worden geautomatiseerd met behulp van Azure PowerShell-cmdlets. Dit artikel vindt u van de Voorbeeldopdrachten voor eenvoudigere taken en koppelingen naar artikelen met de opdrachten voor complexere taken weergeven.

> [!NOTE]
> Als u dit nog niet hebt geïnstalleerd en geconfigureerd van Azure PowerShell nog, u instructies in het artikel krijgt [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).
> 
> 

## <a name="how-to-use-the-example-commands"></a>Het gebruik van de voorbeeldopdrachten
U moet enkele van de tekst in de opdrachten vervangen met de tekst die geschikt is voor uw omgeving. De < en > symbolen geven aan tekst die u wilt vervangen. Wanneer u de tekst vervangen, verwijderen van de tekens, maar de aanhalingstekens intact laten.

## <a name="get-a-vm"></a>Een VM ophalen
Dit is een eenvoudige taak die u vaak gebruikt. Gebruik deze informatie ophalen over een virtuele machine, taken uitvoeren op een virtuele machine of ophalen uitvoer op te slaan in een variabele.

Voor informatie over de virtuele machine, voer deze opdracht, vervangt alles in de aanhalingstekens in, met inbegrip van de < en > tekens bevatten:

    Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Voor het opslaan van de uitvoer in een variabele $vm, voert u de volgende uit:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Meld u aan bij een Windows-VM
Voer deze opdrachten uit:

> [!NOTE]
> U kunt de naam van de virtuele machine en cloud-service ophalen uit de weergave van de **Get-AzureVM** opdracht.
> 
> $svcName = `"<cloud service name>"` $vmName = `"<virtual machine name>"` $localPath = `"<drive and folder location to store the downloaded RDP file, example: c:\temp >"` $localFile = $localPath + "\" $vmname +"RDP"get-AzureRemoteDesktopFile - ServiceName $svcName-naam $vmName - LocalPath $localFile-starten
> 
> 

## <a name="stop-a-vm"></a>Een VM stoppen
Voer deze opdracht uit:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Deze parameter gebruiken om het virtuele IP (VIP) van de cloudservice in het geval de laatste virtuele machine in die service in de cloud is. <br><br> Als u de parameter StayProvisioned gebruikt, zult u nog steeds worden gefactureerd voor de virtuele machine.
> 
> 

## <a name="start-a-vm"></a>Een VM starten
Voer deze opdracht uit:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen
Deze taak zijn een paar stappen vereist. Gebruik eerst de ****toevoegen-AzureDataDisk**** cmdlet de schijf toevoegen aan het $vm-object. Vervolgens gebruikt u **Update-AzureVM** cmdlet voor het bijwerken van de configuratie van de virtuele machine.

U moet ook beslissen of u een nieuwe schijf wilt koppelen of een schijf die al gegevens bevat. Voor een nieuwe schijf, de opdracht het VHD-bestand wordt gemaakt en gekoppeld.

Als u een nieuwe schijf wilt koppelen, voert u deze opdracht uit:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Als u een bestaande gegevensschijf wilt koppelen, voert u deze opdracht uit:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Als u wilt gegevensschijven koppelen vanuit een bestaande VHD-bestand in blob-opslag, moet u deze opdracht uitvoeren:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Een Windows-VM maken
Volg de instructies in voor het maken van een nieuwe Windows gebaseerde virtuele machine in Azure, [Azure PowerShell gebruiken om te maken en vooraf configureren van virtuele Windows-machines](create-powershell.md). De stappen van dit onderwerp maakt u het maken van een Azure PowerShell opdracht ingesteld die een Windows-gebaseerde VM die u kunt vooraf worden geconfigureerd:

* Met het lidmaatschap van de Active Directory-domein.
* Met extra schijven.
* Als een lid van een bestaande load balancing instellen.
* Met een statisch IP-adres.

