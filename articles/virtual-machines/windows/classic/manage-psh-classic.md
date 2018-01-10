---
title: Uw virtuele machines beheren met behulp van Azure PowerShell | Microsoft Docs
description: Meer informatie over opdrachten die u gebruiken kunt voor het automatiseren van taken bij het beheren van uw virtuele machines.
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: timlt
editor: 
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
ms.openlocfilehash: 88ab031778957f20dd42955c2c1311d731e2f588
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Virtuele machines beheren met Azure PowerShell
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Raadpleeg voor algemene PowerShell-opdrachten met het Resource Manager-model, [hier](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Veel taken die u elke dag voor het beheren van uw virtuele machines kunnen worden geautomatiseerd met behulp van Azure PowerShell-cmdlets. In dit artikel krijgt u Voorbeeldopdrachten voor eenvoudigere taken en koppelingen naar artikelen waarin de opdrachten voor complexere taken weergeven.

> [!NOTE]
> Als u dit nog niet hebt geïnstalleerd en geconfigureerd Azure PowerShell nog, kunt u de instructies in het artikel opvragen [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
> 
> 

## <a name="how-to-use-the-example-commands"></a>Het gebruik van de voorbeeldopdrachten
U moet enkele van de tekst in de opdrachten met de tekst die geschikt is voor uw omgeving te vervangen. De < en > symbolen geven de tekst die u wilt vervangen. Wanneer u de tekst vervangen, verwijderen van de symbolen maar laat de aanhalingstekens intact.

## <a name="get-a-vm"></a>Een virtuele machine ophalen
Dit is een eenvoudige taak die u vaak gebruikt. Gebruik deze informatie ophalen over een virtuele machine, taken uitvoeren op een virtuele machine of uitvoer ophalen om op te slaan in een variabele.

Als u informatie over de virtuele machine, voer deze opdracht, vervangt alle aspecten van de aanhalingstekens, met inbegrip van de < en > tekens bevatten:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Voor het opslaan van de uitvoer in een variabele $vm, uitvoeren:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Meld u aan bij een VM op basis van Windows
Deze opdrachten uitvoeren:

> [!NOTE]
> U kunt de naam van de virtuele machine en cloud-service ophalen uit de weergave van de **Get-AzureVM** opdracht.
> 
> $svcName = '<cloud service name>"$vmName = '<virtual machine name>' $localPath =" < station en de map locatie voor het opslaan van het gedownloade RDP-bestand voorbeeld: c:\temp > ' $localFile $localPath = + "\" $vmname +".rdp' Get-AzureRemoteDesktopFile - ServiceName $svcName-naam $vmName - LocalPath $localFile-starten
> 
> 

## <a name="stop-a-vm"></a>Een VM stoppen
Voer deze opdracht uit:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Gebruik deze parameter als u het virtuele IP-adres (VIP) van de cloudservice in het geval de laatste virtuele machine in de cloudservice is. <br><br> Als u de parameter StayProvisioned gebruikt, hebt u nog steeds worden gefactureerd voor de virtuele machine.
> 
> 

## <a name="start-a-vm"></a>Een VM starten
Voer deze opdracht uit:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen
Deze taak zijn een paar stappen vereist. Gebruik eerst de *** toevoegen-AzureDataDisk *** cmdlet de schijf toevoegen aan het $vm-object. Vervolgens gebruikt u **Update-AzureVM** cmdlet bij te werken van de configuratie van de virtuele machine.

U moet ook beslissen of u een nieuwe schijf wilt koppelen of een schijf die al gegevens bevat. Voor een nieuwe schijf, de opdracht wordt het VHD-bestand gemaakt en gekoppeld.

Als u een nieuwe schijf wilt koppelen, voert u deze opdracht uit:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Als u een bestaande gegevensschijf wilt koppelen, voert u deze opdracht uit:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Als u wilt gegevensschijven koppelen vanuit een bestaand VHD-bestand in blob-opslag, moet u deze opdracht uitvoeren:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Maak een VM op basis van Windows
Volg de instructies in voor het maken van een nieuwe Windows gebaseerde virtuele machine in Azure, [Azure PowerShell gebruiken voor het maken en vooraf configureren van virtuele machines op basis van Windows](create-powershell.md). Dit onderwerp stappen maakt u bij het maken van een Azure PowerShell opdracht ingesteld die een VM op basis van Windows die vooraf kan worden geconfigureerd:

* Met het lidmaatschap van de Active Directory-domein.
* Met andere schijven.
* Als een lid van een bestaande Netwerktaakverdeling instellen.
* Met een statisch IP-adres.

