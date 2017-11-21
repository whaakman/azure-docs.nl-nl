---
title: Een gegevensschijf loskoppelen van een Linux-VM - Azure | Microsoft Docs
description: Meer informatie naar een gegevensschijf loskoppelen van een virtuele machine in Azure met behulp van de CLI 2.0 of de Azure-portal.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: c589dd8c9d597145fd87a00d9a2ba040988cd8ec
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a>Hoe u een gegevensschijf loskoppelen van een virtuele Linux-machine

Wanneer u een gegevensschijf die is gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Hiermee verwijdert u de schijf van de virtuele machine, maar niet verwijderd uit de opslag. 

> [!WARNING]
> Als u een schijf die wordt niet automatisch verwijderd loskoppelen. Als u bent geabonneerd op Premium-opslag, moet u blijft kosten voor opslag voor de schijf. Raadpleeg voor meer informatie [prijzen en facturering wanneer u Premium-opslag](../windows/premium-storage.md#pricing-and-billing). 
> 
> 

Als u de bestaande gegevens op de schijf opnieuw wilt gebruiken, kunt u de schijf opnieuw koppelen aan dezelfde of een andere virtuele machine.  

## <a name="detach-a-data-disk-using-cli-20"></a>Een gegevensschijf met CLI 2.0 loskoppelen

```azurecli
az vm disk detach \
    -g myResourceGroup \
    --vm-name myVm \
    -n myDataDisk
```

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.


## <a name="detach-a-data-disk-using-the-portal"></a>Een gegevensschijf ontkoppelen via de portal
1. Selecteer in het menu links **virtuele Machines**.
2. Selecteer de virtuele machine met de gegevensschijf die u wilt loskoppelen en klik op **stoppen** toewijzing van de virtuele machine.
3. Selecteer in het deelvenster met de virtuele machine **schijven**.
4. Aan de bovenkant van de **schijven** deelvenster **bewerken**.
5. In de **schijven** deelvenster aan de rechterkant van de gegevensschijf die u wilt loskoppelen, klikt u op de ![Detach knopafbeelding](./media/detach-disk/detach.png) knop loskoppelen.
5. Nadat de schijf is verwijderd, klikt u op opslaan boven aan het deelvenster.
6. Klik in het deelvenster virtuele machine op **overzicht** en klik vervolgens op de **Start** knop aan de bovenkant van het deelvenster met de virtuele machine opnieuw opstarten.

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.


## <a name="next-steps"></a>Volgende stappen
Als u de gegevensschijf gebruiken wilt, kunt u zojuist hebt [koppelen aan een andere virtuele machine](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

