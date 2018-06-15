---
title: Opstarten van diagnostische gegevens van virtuele Linux-machines in Azure | Microsoft-document
description: Overzicht van de twee foutopsporing functies voor virtuele Linux-machines in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: delhan
ms.openlocfilehash: 38cc806cb77af60cda10f3aeac2e5ed13b445b8c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33941850"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a>Het gebruik van diagnostische gegevens over opstarten Linux virtuele machines in Azure oplossen

Ondersteuning voor twee foutopsporingsfuncties is nu beschikbaar in Azure: ondersteuning voor Console-uitvoer en Schermafbeelding voor het Azure Virtual Machines Resource Manager-implementatiemodel. 

Wanneer u uw eigen installatiekopie importeert in Azure of een van de installatiekopieën van het platform opstart, kan een VM vanwege verschillende redenen in een status belanden waarin deze niet kan worden opgestart. Met deze functies kunt u uw virtuele machines eenvoudig analyseren en herstellen na fouten bij het opstarten.

Bij virtuele Linux-machines kunt u eenvoudig de uitvoer van uw consolelogboek bekijken via de portal:

![Azure Portal](./media/boot-diagnostics/screenshot1.png)
 
U kunt echter bij zowel virtuele Windows- als Linux-machines een schermopname van de virtuele machine bekijken via de hypervisor van Azure:

![Fout](./media/boot-diagnostics/screenshot2.png)

Beide functies worden ondersteund voor virtuele Azure-machines in alle regio's. Houd er rekening mee dat het tot 10 minuten kan duren voordat de schermafbeeldingen en uitvoer worden weergegeven in uw opslagaccount.

## <a name="common-boot-errors"></a>Veelvoorkomende opstartfouten

- [Bestandssystemen](https://support.microsoft.com/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) 
- [Kernel-problemen](https://support.microsoft.com/help/4091524/how-recovery-azure-linux-vm-from-kernel-related-boot-related-issues/) 
- [FSTAB-fouten](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Diagnostische gegevens op een nieuwe virtuele machine inschakelen
1. Wanneer u een nieuwe virtuele machine maakt vanuit de Azure-portal, selecteer de **Azure Resource Manager** uit de vervolgkeuzelijst van implementatie model:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. In **instellingen**, inschakelen de **opstarten diagnostics**, en selecteer vervolgens een opslagaccount dat u wilt deze diagnostische bestanden moeten worden geplaatst.
 
    ![VM maken](./media/boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > De functie voor Boot diagnostics ondersteunt geen premium-opslagaccount. Als u de premium-opslagaccount voor diagnostische gegevens over opstarten gebruikt, ontvangt u mogelijk de StorageAccountTypeNotSupported-fout bij het starten van de virtuele machine. 
    >
    > 

3. Als u met een Azure Resource Manager-sjabloon implementeert, gaat u naar de bron van de virtuele machine en de sectie diagnostics-profiel toevoegen. Vergeet niet de API-versieheader '2015-06-15' te gebruiken.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Met het diagnostische profiel kunt u het opslagaccount selecteren waarin u deze logboeken wilt opslaan.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Bekijk onze opslagplaats hier voor het implementeren van een steekproef virtuele machine met diagnostische gegevens over opstarten is ingeschakeld.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Diagnostische gegevens over opstarten op bestaande virtuele machine inschakelen 

Schakel diagnostische gegevens over opstarten op een bestaande virtuele machine door de volgende stappen uit:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com), en selecteer vervolgens de virtuele machine.
2. In **ondersteuning + probleemoplossing**, selecteer **diagnostics opstarten** > **instellingen**, de status wijzigen in **op**, en vervolgens Selecteer een opslagaccount. 
4. Zorg ervoor dat de optie opstarten diagnostics is geselecteerd en sla de wijziging.

    ![Bestaande VM bijwerken](./media/boot-diagnostics/enable-for-existing-vm.png)

3. Start de VM opnieuw op om de wijzigingen door te voeren.

## <a name="next-steps"></a>Volgende stappen

Als er een fout 'Kan niet ophalen van inhoud van het logboek' wanneer u diagnostische gegevens voor het opstarten van virtuele machine gebruikt, Zie [kan inhoud van de fout logboek ophalen in VM Boot Diagnostics](https://support.microsoft.com/help/4094480/failed-to-get-contents-of-the-log-error-in-vm-boot-diagnostics-in-azur).