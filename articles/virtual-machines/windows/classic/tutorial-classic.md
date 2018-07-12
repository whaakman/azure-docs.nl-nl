---
title: Een virtuele machine maken in Azure portal | Microsoft Docs
description: Maak een virtuele Windows-machine in Azure portal.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 5fd2128ff436d3211f41c7dfdcc4c2b8aabd0eb0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232323"
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Een virtuele machine waarop Windows wordt uitgevoerd in Azure portal maken
> [!div class="op_single_selector"]
> * [Azure Portal](tutorial.md)
> * [PowerShell: Klassieke implementatie](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Meer informatie over het [uitvoeren van deze stappen met het Resource Manager-implementatiemodel](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met behulp van de **Azure-portal**.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Deze zelfstudie leert u hoe u een Azure-machine (VM) waarop Windows wordt uitgevoerd in Azure portal maakt. We een installatiekopie van Windows Server gebruiken als voorbeeld, maar dat is slechts een van de vele installatiekopieën die Azure biedt. Houd er rekening mee dat uw opties voor installatiekopieën, afhankelijk van uw abonnement. Windows-desktop-installatiekopieën mogelijk bijvoorbeeld beschikbaar voor MSDN-abonnees.

Deze sectie leest u hoe u de **Dashboard** in Azure portal om te selecteren en vervolgens de virtuele machine te maken.

U kunt ook maken met behulp van virtuele machines [uw eigen installatiekopieën](createupload-vhd.md). Zie voor meer informatie over deze en andere methoden, [verschillende manieren om te maken van een virtuele machine van Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a id="createvirtualmachine"> </a>De virtuele machine maken
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [maken van een virtuele machine met het Resource Manager-implementatiemodel](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in Azure portal.
* Meld u aan de virtuele machine. Zie voor instructies [Meld u aan bij een virtuele machine met Windows Server](connect-logon.md).
* Koppelen van een schijf voor het opslaan van gegevens. U kunt zowel lege schijven als schijven die gegevens bevatten koppelen. Zie voor instructies de [een gegevensschijf koppelen aan een Windows virtuele machine gemaakt met het klassieke implementatiemodel](attach-disk.md).
