---
title: Een installatiekopie van een Windows Azure VM | Microsoft Docs
description: Leg een installatiekopie van een virtuele machine van Azure Windows vast die is gemaakt volgens het klassieke implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 6f7d8e8b3d1ed7c8a06f7e11fea492eec8b60409
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Leg een installatiekopie van een virtuele machine van Azure Windows vast die is gemaakt volgens het klassieke implementatiemodel.
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor informatie over het model van de Resource Manager, [beheerde-installatiekopie van een gegeneraliseerde virtuele machine in Azure](../capture-image-resource.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Dit artikel ziet u het vastleggen van een virtuele machine van Azure waarop Windows wordt uitgevoerd, zodat u deze als een installatiekopie van een gebruiken kunt andere virtuele machines maken. Deze installatiekopie bevat de schijf van het besturingssysteem en eventuele gegevensschijven die zijn gekoppeld aan de virtuele machine. Het bevat geen netwerkconfiguraties, dus u moet voor het instellen van de netwerkconfiguraties bij het maken van de andere virtuele machines die gebruikmaken van de installatiekopie.

Azure slaat de installatiekopie onder **VM-installatiekopieën (klassiek)**, een **Compute** service die wordt vermeld bij het weergeven van alle Azure-services. Dit is de plaats waar alle installatiekopieën die u hebt geüpload worden opgeslagen. Zie voor meer informatie over installatiekopieën [over installatiekopieën voor virtuele machines](about-images.md?toc=%2fazure%2fvirtual-machines%2fWindows%2fclassic%2ftoc.json).

## <a name="before-you-begin"></a>Voordat u begint
Deze stappen wordt ervan uitgegaan dat u hebt al een virtuele machine in Azure gemaakt en geconfigureerd, het besturingssysteem, met inbegrip van eventuele gegevensschijven koppelen. Als u dit nog niet hebt gedaan, raadpleegt u de volgende artikelen voor meer informatie over het maken en de virtuele machine voorbereiden:

* [Een virtuele machine van een installatiekopie maken](createportal.md)
* [Hoe u een gegevensschijf koppelen aan een virtuele machine](attach-disk.md)
* Zorg ervoor dat de server-functies worden ondersteund met Sysprep. Zie voor meer informatie [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!WARNING]
> Dit proces wordt verwijderd van de oorspronkelijke virtuele machine nadat deze vastgelegd.
>
>

Voordat u een installatiekopie van een virtuele machine van Azure vastlegt, kunt u het beste de doel-virtuele machine een back-up. Virtuele machines van Azure kan een back-up maken met Azure Backup. Zie [Back-ups maken van virtuele machines van Azure](../../../backup/backup-azure-arm-vms.md) voor meer informatie. Andere oplossingen zijn beschikbaar van gecertificeerde partners. Als u wilt weten wat er op dit moment beschikbaar is, zoekt u Azure Marketplace.

## <a name="capture-the-virtual-machine"></a>De virtuele machine vastleggen
1. In de [Azure-portal](http://portal.azure.com), **Connect** aan de virtuele machine. Zie voor instructies [hoe u aan te melden bij een virtuele machine met Windows Server][How to sign in to a virtual machine running Windows Server].
2. Open een opdrachtpromptvenster als administrator.
3. Wijzig de map in `%windir%\system32\sysprep`, en voer vervolgens sysprep.exe.
4. De **hulpprogramma voor systeemvoorbereiding** dialoogvenster wordt weergegeven. Ga als volgt te werk:

   * In **System opschonen actie**, selecteer **System Voer Out-of-Box Experience (OOBE)** en zorg ervoor dat **Generalize** is ingeschakeld. Zie voor meer informatie over het gebruik van Sysprep [hoe gebruik Sysprep: An Introduction][How to Use Sysprep: An Introduction].
   * In **afsluitopties**, selecteer **afsluiten**.
   * Klik op **OK**.

   ![Voer Sysprep uit](./media/capture-image/SysprepGeneral.png)
5. Sysprep is afgesloten van de virtuele machine, waardoor de status van de virtuele machine in de Azure portal naar wijzigt **gestopt**.
6. Klik in de Azure-portal op **virtuele Machines (klassiek)** en selecteer de virtuele machine die u wilt vastleggen. De **VM-installatiekopieën (klassiek)** groep wordt vermeld onder **Compute** wanneer u bekijkt **meer services**.

7. Klik op de opdrachtbalk op **vastleggen**.

   ![Virtuele machine vastleggen](./media/capture-image/CaptureVM.png)

   De **vastleggen van de virtuele Machine** dialoogvenster wordt weergegeven.

8. In **installatiekopienaam**, typ een naam voor de nieuwe installatiekopie. In **installatiekopie label**, typt u een label voor de nieuwe installatiekopie.

9. Klik op **ik heb Sysprep uitgevoerd op de virtuele machine**. Dit selectievakje verwijst naar de acties met Sysprep in stap 3-5. Een installatiekopie van een _moet_ worden gegeneraliseerd met Sysprep voordat u een installatiekopie van Windows Server aan uw set aangepaste installatiekopieën toevoegen.

10. Nadat het vastleggen is voltooid, de nieuwe installatiekopie beschikbaar in de **Marketplace**, in de **Compute**, **VM-installatiekopieën (klassiek)** container.

    ![De installatiekopie is geslaagd](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Volgende stappen
De installatiekopie is gereed om te worden gebruikt voor het maken van virtuele machines. Om dit te doen, maakt u een virtuele machine door het selecteren van de **meer services** menuopdracht onderaan in het menu services, klikt u vervolgens **VM-installatiekopieën (klassiek)** in de **Compute** groep. Zie voor instructies [een virtuele machine maken van een installatiekopie van een](createportal.md).

[How to sign in to a virtual machine running Windows Server]:connect-logon.md
[How to Use Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
