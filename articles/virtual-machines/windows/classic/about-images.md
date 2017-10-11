---
title: "Over de installatiekopieën voor virtuele machines van Windows | Microsoft Docs"
description: Meer informatie over hoe afbeeldingen worden gebruikt met Windows virtuele machines in Azure.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cynthn
ms.openlocfilehash: d421cee0becabdf81d865036d0c98b12b077152b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="about-images-for-windows-virtual-machines"></a>Over de installatiekopieën voor virtuele machines van Windows
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor meer informatie over het vinden en gebruiken van afbeeldingen in het Resource Manager-model [hier](../../virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>Werken met installatiekopieën

U kunt de Azure PowerShell-module en de Azure portal gebruiken voor het beheren van de beschikbare installatiekopieën naar uw Azure-abonnement. De Azure PowerShell-module biedt meer opdrachtopties, zodat u kunt achterhalen precies wat u wilt bekijken of te doen. De Azure portal biedt een grafische gebruikersinterface voor veel van de dagelijkse beheertaken.

Hier volgen enkele voorbeelden die gebruikmaken van de Azure PowerShell-module.

* **Ophalen van alle installatiekopieën**:`Get-AzureVMImage`retourneert een lijst met alle installatiekopieën die beschikbaar zijn in uw huidige abonnement: uw afbeeldingen en die worden verstrekt door Azure of partners. De resulterende lijst kan worden grote. De volgende voorbeelden laten zien hoe een kortere lijst.
* **Opvragen van families van de afbeelding**:`Get-AzureVMImage | select ImageFamily` opgehaald van een lijst van families van de installatiekopie door te geven tekenreeksen **ImageFamily** eigenschap.
* **Ophalen van alle afbeeldingen in een specifieke productfamilie**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **VM-installatiekopieën vinden**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` deze cmdlet werkt door het filteren van de eigenschap DataDiskConfiguration, die alleen van toepassing op de VM-installatiekopieën. In dit voorbeeld wordt ook de uitvoer naar alleen de naam label en image filtert.
* **Opslaan van een algemene installatiekopie**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Opslaan van de installatiekopie van een gespecialiseerde**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`

  > [!TIP]
  > De parameter OSState is vereist voor het maken van een VM-installatiekopie, dat bestaat uit de besturingssysteemschijf en gegevensschijven gekoppeld. Als u de parameter niet gebruikt, maakt de cmdlet een installatiekopie van het besturingssysteem. De waarde van de parameter aangeeft of de afbeelding is gegeneraliseerd of speciale, gebaseerd op of de schijf van het besturingssysteem is voorbereid voor hergebruik.

* **Verwijderen van een installatiekopie van een**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Volgende stappen
U kunt ook [maken van een Windows-machine met de Azure portal](tutorial.md).
