---
title: Een virtuele machine maken in de Azure portal | Microsoft Docs
description: Maak een virtuele Windows-machine in de Azure portal.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
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
ms.openlocfilehash: 99a67821ab926983205e2327c428e854d20a0cf5
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Maak een virtuele machine waarop Windows wordt uitgevoerd in de Azure portal
> [!div class="op_single_selector"]
> * [Azure Portal](tutorial.md)
> * [PowerShell: Klassieke implementatie](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Meer informatie over hoe [u deze stappen uitvoert met het implementatiemodel van Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met behulp van de **Azure-portal**.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Deze zelfstudie laat zien hoe u een Azure virtuele machine (VM) waarop Windows wordt uitgevoerd in de Azure portal maken. We een installatiekopie van Windows Server gebruiken als voorbeeld, maar dat is slechts een van de vele installatiekopieën die Azure biedt. Houd er rekening mee dat uw opties in de installatiekopie, afhankelijk van uw abonnement. Windows-bureaublad installatiekopieën kunnen bijvoorbeeld zijn beschikbaar voor MSDN-abonnees.

In deze sectie leest u hoe u de **Dashboard** in de Azure portal om te selecteren en vervolgens de virtuele machine te maken.

U kunt ook maken met virtuele machines met [uw eigen installatiekopieën](createupload-vhd.md). Zie voor meer informatie over deze en andere methoden, [verschillende manieren voor het maken van een virtuele Windows-machine](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a id="createvirtualmachine"></a>Maak de virtuele machine
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [een virtuele machine maken met het implementatiemodel van Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in de Azure portal.
* Meld u bij de virtuele machine. Zie voor instructies [Meld u aan bij een virtuele machine met Windows Server](connect-logon.md).
* Koppel een schijf voor het opslaan van gegevens. U kunt zowel leeg schijven en schijven met gegevens koppelen. Voor instructies raadpleegt u de [een gegevensschijf koppelen aan een virtuele Windows-machine gemaakt met het klassieke implementatiemodel](attach-disk.md).
