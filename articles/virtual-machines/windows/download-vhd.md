---
title: Een Windows-VHD downloaden vanuit Azure | Microsoft Docs
description: Download een Windows-VHD met de Azure portal.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: d8bf89a4b7c2a158302f9ba09a182a3d8d062adc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="download-a-windows-vhd-from-azure"></a>Een Windows-VHD downloaden van Azure

In dit artikel leert u hoe voor het downloaden van een [Windows virtuele harde schijf (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bestand van Azure met behulp van de Azure-portal. 

Virtuele machines (VM's) in Azure gebruik [schijven](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) als gebruikt voor het opslaan van een besturingssysteem, toepassingen en gegevens. Alle Azure VM's hebben ten minste twee schijven: de schijf van een Windows-besturingssysteem en een tijdelijke schijf. De besturingssysteemschijf in eerste instantie wordt gemaakt van een installatiekopie en de schijf van het besturingssysteem en de installatiekopie zijn beide VHD's opgeslagen in Azure storage-account. Virtuele machines hebben ook een of meer gegevensschijven die ook als virtuele harde schijven zijn opgeslagen.

## <a name="stop-the-vm"></a>De virtuele machine stoppen

Een VHD kan niet worden gedownload vanuit Azure als deze is gekoppeld aan een actieve virtuele machine. U moet de virtuele machine voor het downloaden van een VHD te stoppen. Als u wilt gebruiken van een VHD als een [installatiekopie](tutorial-custom-images.md) als andere virtuele machines maken met nieuwe schijven, gebruikt u [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) te generaliseren van het besturingssysteem die is opgenomen in het bestand en stop de virtuele machine. Voor het gebruik van de VHD als een schijf voor een nieuw exemplaar van een bestaande virtuele machine of de gegevensschijf, hoeft u alleen te stoppen en de VM ongedaan gemaakt.

De VHD gebruiken als een installatiekopie van een andere virtuele machines maken, voert u deze stappen uit:

1.  Meld u aan bij de [Azure-portal](https://portal.azure.com/) als u dat nog niet hebt gedaan.
2.  [Verbinding maken met de virtuele machine](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
3.  Open het opdrachtpromptvenster als beheerder op de virtuele machine.
4.  Wijzig de map in *%windir%\system32\sysprep* en sysprep.exe uitvoeren.
5.  Selecteer in het dialoogvenster hulpprogramma voor systeemvoorbereiding **System Voer Out-of-Box Experience (OOBE)**, en zorg ervoor dat **Generalize** is geselecteerd.
6.  Selecteer in de opties voor afsluiten **afsluiten**, en klik vervolgens op **OK**. 

Voor het gebruik van de VHD als een schijf voor een nieuw exemplaar van een bestaande virtuele machine of de gegevensschijf, voert u deze stappen uit:

1.  Klik in het menu Hub in de Azure portal op **virtuele Machines**.
2.  Selecteer de virtuele machine in de lijst.
3.  Klik op de blade voor de virtuele machine op **stoppen**.

    ![VM stoppen](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Genereren van SAS-URL

Voor het downloaden van het VHD-bestand, moet u voor het genereren van een [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL. Wanneer de URL wordt gegenereerd, wordt een verlooptijd is toegewezen aan de URL.

1.  Klik op het menu aan de blade voor de virtuele machine **schijven**.
2.  De besturingssysteemschijf voor de virtuele machine selecteren en klik vervolgens op **exporteren**.
3.  Stel de vervaltijd van de URL van *36000*.
4.  Klik op **URL genereren**.

    ![URL genereren](./media/download-vhd/export-generate.png)

> [!NOTE]
> De verlooptijd wordt van de standaardwaarde op voldoende tijd biedt voor het downloaden van het grote VHD-bestand voor een besturingssysteem Windows Server verhoogd. Een VHD-bestand met het besturingssysteem Windows Server als u wilt downloaden, afhankelijk van de verbinding van enkele uren duren, kunt u verwachten. Als u een VHD voor een gegevensschijf downloadt, is de standaardwaarde voldoende. 
> 
> 

## <a name="download-vhd"></a>VHD downloaden

1.  Klik onder de URL die is gegenereerd, Download het VHD-bestand.

    ![VHD downloaden](./media/download-vhd/export-download.png)

2.  Mogelijk moet u op **opslaan** in de browser om het downloaden te starten. De standaardnaam voor het VHD-bestand is *abcd*.

    ![Klik op opslaan in de browser](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [een VHD-bestand uploaden naar Azure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
- [Beheerde schijven maken van niet-beheerde schijven in een opslagaccount](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- [Azure-schijven met PowerShell beheren](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

