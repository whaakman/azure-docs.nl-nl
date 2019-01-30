---
title: Linux-installatiekopieën toevoegen aan Azure Stack
description: Informatie over hoe Linux-installatiekopieën toevoegen aan Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/16/2018
ms.openlocfilehash: be537d9598eeda902254e56a9202415f4f498d93
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245130"
---
# <a name="add-linux-images-to-azure-stack"></a>Linux-installatiekopieën toevoegen aan Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt virtuele Linux-machines (VM's) in Azure Stack implementeren met het toevoegen van een installatiekopie op basis van Linux in Azure Stack Marketplace. De eenvoudigste manier om een Linux-installatiekopie toevoegen aan Azure Stack is via Marketplace-beheer. Deze installatiekopieën zijn voorbereid en getest op compatibiliteit met Azure Stack.

## <a name="marketplace-management"></a>Marketplace-Management

Gebruik voor het downloaden van Linux-installatiekopieën op Azure Marketplace, de procedures in de [marketplace-items van Azure naar Azure Stack downloaden](azure-stack-download-azure-marketplace-item.md) artikel. Selecteer de Linux-installatiekopieën die u wilt bieden gebruikers in uw Azure Stack. 

Houd er rekening mee dat er zijn regelmatig updates naar deze afbeeldingen, dus kom Marketplace Management vaak om up-to-date te houden.

## <a name="prepare-your-own-image"></a>Uw eigen installatiekopie voorbereiden

Waar mogelijk, downloadt u de installatiekopieën die beschikbaar zijn via Marketplace-beheer die zijn voorbereid en getest voor Azure Stack. 
 
De Azure Linux Agent (ook wel genoemd `WALinuxAgent` of `walinuxagent`) is vereist, en niet alle versies van de agent werkt in Azure Stack. Moet u versie 2.2.18 of hoger als u uw eigen installatiekopie maken. Houd er rekening mee dat [cloud-init](https://cloud-init.io/) wordt niet ondersteund in Azure Stack op dit moment.

U kunt uw eigen Linux-installatiekopie met behulp van de volgende instructies voorbereiden:

* [Op basis van centOS-distributies](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
* [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

    
## <a name="add-your-image-to-the-marketplace"></a>Uw afbeelding toevoegen aan de marketplace
 
Ga als volgt [de installatiekopie toevoegen aan de Marketplace](azure-stack-add-vm-image.md). Zorg ervoor dat de `OSType` parameter is ingesteld op `Linux`.

Nadat u de installatiekopie hebt toegevoegd aan de Marketplace, een Marketplace-item wordt gemaakt en kunnen gebruikers een virtuele Linux-machine implementeren.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie:

- [Marketplace-items van Azure naar Azure Stack downloaden](azure-stack-download-azure-marketplace-item.md)
- [Overzicht van Azure Stack Marketplace](azure-stack-marketplace.md)