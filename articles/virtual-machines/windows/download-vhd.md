---
title: Downloaden van een Windows VHD vanuit Azure | Microsoft Docs
description: Download een Windows VHD met de Azure-portal.
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
ms.openlocfilehash: 3d44a4a723c39bf9780475a2ac3088da94285f6e
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329367"
---
# <a name="download-a-windows-vhd-from-azure"></a>Een Windows VHD vanuit Azure downloaden

In dit artikel leert u hoe u een Windows-virtuele harde schijf (VHD)-bestand te downloaden van Azure met behulp van de Azure-portal.

## <a name="stop-the-vm"></a>De virtuele machine stoppen

Een VHD kan niet worden gedownload van Azure als deze gekoppeld aan een actieve virtuele machine. U wilt stoppen met de virtuele machine voor het downloaden van een VHD. Als u wilt gebruiken van een VHD als een [installatiekopie](tutorial-custom-images.md) u gebruik voor het maken van andere VM's met nieuwe schijven [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) generaliseren van het besturingssysteem die is opgenomen in het bestand en vervolgens de virtuele machine te stoppen. Voor het gebruik van de VHD als een schijf voor een nieuw exemplaar van een bestaande virtuele machine of een gegevensschijf, hoeft u alleen om te stoppen en wijs de virtuele machine.

Voor het gebruik van de VHD als een installatiekopie van een andere virtuele machines maken, de volgende stappen uit:

1.  Meld u aan bij de [Azure-portal](https://portal.azure.com/) als u dat nog niet hebt gedaan.
2.  [Verbinding maken met de virtuele machine](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
3.  Open op de virtuele machine, het opdrachtpromptvenster als beheerder.
4.  Wijzig de map in *%windir%\system32\sysprep* en sysprep.exe uitvoeren.
5.  Selecteer in het dialoogvenster hulpprogramma voor systeemvoorbereiding **Voer System Out-of-Box Experience (OOBE)**, en zorg ervoor dat **Generalize** is geselecteerd.
6.  Selecteer in de opties voor afsluiten, **afsluiten**, en klik vervolgens op **OK**. 

Voor het gebruik van de VHD als een schijf voor een nieuw exemplaar van een bestaande virtuele machine of een gegevensschijf, de volgende stappen uit:

1.  Klik in het menu Hub in Azure portal op **virtuele Machines**.
2.  Selecteer de virtuele machine in de lijst.
3.  Klik op de blade voor de virtuele machine, **stoppen**.

    ![VM stoppen](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Genereren van SAS-URL

Als u wilt het VHD-bestand downloaden, moet u voor het genereren van een [handtekening voor gedeelde toegang (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL. Wanneer de URL wordt gegenereerd, wordt een verlooptijd is toegewezen aan de URL.

1.  Klik op het menu aan de blade voor de virtuele machine, **schijven**.
2.  Selecteer de besturingssysteemschijf voor de virtuele machine en klik vervolgens op **exporteren**.
3.  Stel de vervaltijd van de URL naar *36000*.
4.  Klik op **URL genereren**.

    ![URL genereren](./media/download-vhd/export-generate.png)

> [!NOTE]
> De verlooptijd wordt van de standaardwaarde voor voldoende tijd voor het downloaden van het grote VHD-bestand voor een Windows Server-besturingssysteem verhoogd. U kunt verwachten dat een VHD-bestand met het Windows Server-besturingssysteem als u wilt downloaden, afhankelijk van de verbinding van enkele uren duren. Als u een VHD voor een gegevensschijf downloaden wilt, is de standaardwaarde voldoende. 
> 
> 

## <a name="download-vhd"></a>Downloaden van VHD

1.  De URL die is gegenereerd, klikt u op het VHD-bestand downloaden.

    ![Downloaden van VHD](./media/download-vhd/export-download.png)

2.  U moet mogelijk klikken op **opslaan** in de browser om het downloaden te starten. De standaardnaam voor het VHD-bestand is *abcd*.

    ![Klik op opslaan in de browser](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [een VHD-bestand uploaden naar Azure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
- [Beheerde schijven maken van niet-beheerde schijven in een opslagaccount](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- [Azure-schijven beheren met PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

