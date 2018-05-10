---
title: Linux-installatiekopieën toevoegen aan Azure-Stack
description: Meer informatie over hoe Linux installatiekopieën toevoegen aan Azure-Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 64a860bc925b9c7499363c1fe39d03df88a9a51d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Linux-installatiekopieën toevoegen aan Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt virtuele Linux-machines (VM's) op de Stack Azure implementeren door een Linux-installatiekopie toe te voegen in de Stack Azure Marketplace. De eenvoudigste manier om een Linux-installatiekopie toevoegen aan Azure-Stack is via Marketplace-beheer. Deze installatiekopieën zijn voorbereid en getest op compatibiliteit met de Azure-Stack.

## <a name="marketplace-management"></a>Marketplace-Management

Linux-installatiekopieën downloaden vanuit Azure Marketplace, gebruik de procedures in het volgende artikel. Selecteer de Linux-installatiekopieën die u wilt bieden gebruikers op uw Azure-Stack. 

[Downloaden van de marketplace-items van Azure naar Azure Stack](azure-stack-download-azure-marketplace-item.md).

Houd er rekening mee dat er regelmatig updates zijn voor deze installatiekopieën, dus controleer Marketplace Management vaak om up-to-date te houden.

## <a name="prepare-your-own-image"></a>Uw eigen installatiekopie voorbereiden

 Waar mogelijk, downloaden de afbeeldingen die beschikbaar is via de Marketplace-Management die zijn voorbereid en getest voor Azure-Stack. 
 
 De Azure Linux Agent (doorgaans aangeroepen `WALinuxAgent` of `walinuxagent`) is vereist, en niet alle versies van de agent zal werken op Azure-Stack. Moet u versie 2.2.18 of hoger als u uw eigen installatiekopie maken. Houd er rekening mee dat [cloud init](https://cloud-init.io/) wordt niet ondersteund op Azure-Stack op dit moment.

 U kunt uw eigen Linux-installatiekopie met de volgende instructies voorbereiden:

   * [Op basis van centOS distributies](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

    
## <a name="add-your-image-to-the-marketplace"></a>Uw afbeelding toevoegen aan de marketplace
 
Ga als volgt [de installatiekopie toevoegen aan de Marketplace](azure-stack-add-vm-image.md). Zorg ervoor dat de `OSType` parameter is ingesteld op `Linux`.

Nadat u de installatiekopie aan de Marketplace hebt toegevoegd, een Marketplace-item is gemaakt en gebruikers een virtuele Linux-machine kunnen implementeren.
