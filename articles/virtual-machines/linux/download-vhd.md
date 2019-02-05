---
title: Een Linux VHD downloaden van Azure | Microsoft Docs
description: Download een VHD met Linux met behulp van de Azure CLI en Azure portal.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: 6d827e1142fa17b29633c98c062fd204f7a49e36
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55729725"
---
# <a name="download-a-linux-vhd-from-azure"></a>Een Linux VHD van Azure downloaden

In dit artikel leert u hoe u voor het downloaden van een [Linux virtuele harde schijf (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bestand naar Azure verzenden via de Azure CLI en Azure portal. 

Als u dit nog niet hebt gedaan, installeert u [Azure CLI](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="stop-the-vm"></a>De virtuele machine stoppen

Een VHD kan niet worden gedownload van Azure als deze gekoppeld aan een actieve virtuele machine. U wilt stoppen met de virtuele machine voor het downloaden van een VHD. Als u wilt gebruiken van een VHD als een [installatiekopie](tutorial-custom-images.md) als u wilt andere virtuele machines maken met de nieuwe schijven, moet u de inrichting en generaliseren van het besturingssysteem die is opgenomen in het bestand en de virtuele machine stoppen. Voor het gebruik van de VHD als een schijf voor een nieuw exemplaar van een bestaande virtuele machine of een gegevensschijf, hoeft u alleen om te stoppen en wijs de virtuele machine.

Voor het gebruik van de VHD als een installatiekopie van een andere virtuele machines maken, de volgende stappen uit:

1. Gebruik SSH, de accountnaam en het openbare IP-adres van de virtuele machine verbinding maken met het en inrichting van het ongedaan maken. U vindt het openbare IP-adres met [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show). De + parameter van de gebruiker de laatste ingerichte gebruikersaccount, worden ook verwijderd. Als u de accountreferenties in met de virtuele machine zijn bakken, laat u deze + parameter user. Het volgende voorbeeld verwijdert u de laatste ingerichte gebruikersaccount:

    ```bash
    ssh azureuser@<publicIpAddress>
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Meld u aan bij uw Azure-account met [az login](https://docs.microsoft.com/cli/azure/reference-index).
3. Stop en wijs de virtuele machine.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. De virtuele machine generaliseren. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

Voor het gebruik van de VHD als een schijf voor een nieuw exemplaar van een bestaande virtuele machine of een gegevensschijf, de volgende stappen uit:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com/).
2.  Klik in het menu Hub op **Virtuele machines**.
3.  Selecteer de virtuele machine in de lijst.
4.  Klik op de blade voor de virtuele machine, **stoppen**.

    ![VM stoppen](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Genereren van SAS-URL

Als u wilt het VHD-bestand downloaden, moet u voor het genereren van een [handtekening voor gedeelde toegang (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL. Wanneer de URL wordt gegenereerd, wordt een verlooptijd is toegewezen aan de URL.

1.  Klik op het menu aan de blade voor de virtuele machine, **schijven**.
2.  Selecteer de besturingssysteemschijf voor de virtuele machine en klik vervolgens op **exporteren**.
3.  Klik op **URL genereren**.

    ![URL genereren](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>Downloaden van VHD

1.  De URL die is gegenereerd, klikt u op het VHD-bestand downloaden.

    ![Downloaden van VHD](./media/download-vhd/export-download.png)

2.  U moet mogelijk klikken op **opslaan** in de browser om het downloaden te starten. De standaardnaam voor het VHD-bestand is *abcd*.

    ![Klik op opslaan in de browser](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [uploaden en een Linux-VM maken van een aangepaste schijf met de Azure CLI](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Azure-schijven de Azure CLI beheren](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

