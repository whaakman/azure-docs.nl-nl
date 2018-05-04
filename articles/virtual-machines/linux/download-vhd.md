---
title: Downloaden van een VHD Linux van Azure | Microsoft Docs
description: Download een Linux-VHD met behulp van de Azure CLI en de Azure-portal.
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
ms.date: 06/26/2017
ms.author: cynthn
ms.openlocfilehash: 93f165d08813506f9c4d4c0fbb885ddc958e8391
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="download-a-linux-vhd-from-azure"></a>Downloaden van een VHD Linux van Azure

In dit artikel leert u hoe voor het downloaden van een [Linux virtuele harde schijf (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bestand van Azure met behulp van de Azure CLI en de Azure portal. 

Virtuele machines (VM's) in Azure gebruik [schijven](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) als gebruikt voor het opslaan van een besturingssysteem, toepassingen en gegevens. Alle Azure VM's hebben ten minste twee schijven: de schijf van een Windows-besturingssysteem en een tijdelijke schijf. De besturingssysteemschijf in eerste instantie wordt gemaakt van een installatiekopie en de schijf van het besturingssysteem en de installatiekopie zijn beide VHD's opgeslagen in Azure storage-account. Virtuele machines hebben ook een of meer gegevensschijven die ook als virtuele harde schijven zijn opgeslagen.

Als u dit nog niet hebt gedaan, installeert u [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="stop-the-vm"></a>De virtuele machine stoppen

Een VHD kan niet worden gedownload vanuit Azure als deze is gekoppeld aan een actieve virtuele machine. U moet de virtuele machine voor het downloaden van een VHD te stoppen. Als u wilt gebruiken van een VHD als een [installatiekopie](tutorial-custom-images.md) als andere virtuele machines maken met nieuwe schijven, moet u inrichting ervan ongedaan en het besturingssysteem die is opgenomen in het bestand generaliseren en stop de virtuele machine. Voor het gebruik van de VHD als een schijf voor een nieuw exemplaar van een bestaande virtuele machine of de gegevensschijf, hoeft u alleen te stoppen en de VM ongedaan gemaakt.

De VHD gebruiken als een installatiekopie van een andere virtuele machines maken, voert u deze stappen uit:

1. SSH, de accountnaam en het openbare IP-adres van de virtuele machine verbinding maken met het en inrichting ervan ongedaan maakt het gebruik. U vindt het openbare IP-adres met [az netwerk openbare ip-weergeven](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show). De + parameter van de gebruiker de laatste ingerichte gebruiker-account, worden ook verwijderd. Als u de referenties in voor de VM zijn bakken, laat u uit dit + parameter user. Het volgende voorbeeld verwijdert u de laatste ingerichte gebruikersaccount:

    ```bash
    ssh azureuser@<publicIpAddress>
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Aanmelden bij uw Azure-account met [az aanmelding](https://docs.microsoft.com/cli/azure/reference-index#az_login).
3. Stop en toewijzing van de virtuele machine.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. De virtuele machine generalize. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

Voor het gebruik van de VHD als een schijf voor een nieuw exemplaar van een bestaande virtuele machine of de gegevensschijf, voert u deze stappen uit:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com/).
2.  Klik in het menu Hub op **Virtuele machines**.
3.  Selecteer de virtuele machine in de lijst.
4.  Klik op de blade voor de virtuele machine op **stoppen**.

    ![VM stoppen](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Genereren van SAS-URL

Voor het downloaden van het VHD-bestand, moet u voor het genereren van een [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL. Wanneer de URL wordt gegenereerd, wordt een verlooptijd is toegewezen aan de URL.

1.  Klik op het menu aan de blade voor de virtuele machine **schijven**.
2.  De besturingssysteemschijf voor de virtuele machine selecteren en klik vervolgens op **exporteren**.
3.  Klik op **URL genereren**.

    ![URL genereren](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>VHD downloaden

1.  Klik onder de URL die is gegenereerd, Download het VHD-bestand.

    ![VHD downloaden](./media/download-vhd/export-download.png)

2.  Mogelijk moet u op **opslaan** in de browser om het downloaden te starten. De standaardnaam voor het VHD-bestand is *abcd*.

    ![Klik op opslaan in de browser](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [uploaden en Linux-VM te maken van aangepaste schijf met de Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Beheren van Azure-schijven die de Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

