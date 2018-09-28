---
title: Diagnostische gegevens over opstarten voor virtuele machines in Azure | Microsoft Docs
description: Overzicht van de twee foutopsporingsfuncties voor virtuele machines in Azure
services: virtual-machines
author: Deland-Han
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: 64641f8acfe7b58763756e2a0707fa799ee804b2
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413663"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>Het gebruik van diagnostische gegevens over opstarten voor virtuele machines in Azure oplossen

Ondersteuning voor twee foutopsporingsfuncties is nu beschikbaar in Azure: ondersteuning voor Console-uitvoer en schermafbeelding voor virtuele machines van Azure Resource Manager-implementatiemodel. 

Wanneer u uw eigen installatiekopie naar Azure of een van de platforminstallatiekopieën van het opstart, kunnen er diverse redenen waarom een virtuele machine in een niet-opstartbare status opgehaald. Deze functies kunnen u eenvoudig analyseren en herstellen van uw virtuele machines opstartfouten.

Voor virtuele Linux-machines, kunt u eenvoudig de uitvoer van uw consolelogboek vanuit de Portal weergeven. Voor zowel Windows als Linux virtuele machines kunt Azure u ook om te zien van een schermopname van de virtuele machine uit de hypervisor. Beide van deze functies worden ondersteund voor virtuele Azure-machines in alle regio's. Houd er rekening mee dat het tot 10 minuten kan duren voordat de schermafbeeldingen en uitvoer worden weergegeven in uw opslagaccount.

## <a name="common-boot-errors"></a>Veelvoorkomende opstartfouten

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Er is geen besturingssysteem gevonden](https://support.microsoft.com/help/4010142)
- [Opstartfout of OPSTARTAPPARAAT_NIET_TOEGANKELIJK](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Diagnostische gegevens op een nieuwe virtuele machine inschakelen
1. Bij het maken van een nieuwe virtuele machine vanuit Azure Portal, selecteert u de **Azure Resource Manager** in de vervolgkeuzelijst deployment-model:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. In **instellingen**, schakel de **diagnostische gegevens over opstarten**, en selecteer vervolgens een storage-account dat u wilt deze diagnostische bestanden moeten worden geplaatst.
 
    ![VM maken](./media/virtual-machines-common-boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > De functie van de diagnostische gegevens over opstarten biedt geen ondersteuning voor premium storage-account. Als u de premium-opslagaccount voor diagnostische gegevens over opstarten gebruiken, ontvangt u mogelijk de StorageAccountTypeNotSupported-fout bij het starten van de virtuele machine.
    >
    > 

3. Als u een Azure Resource Manager-sjabloon implementeert, gaat u naar de resource van uw virtuele machine en de sectie diagnostisch profiel toevoegen. Vergeet niet de API-versieheader '2015-06-15' te gebruiken.

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

Bekijk hier onze opslagplaats voor het implementeren van een voorbeeld van virtuele machine met diagnostische gegevens over opstarten ingeschakeld.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Diagnostische gegevens over opstarten op bestaande virtuele machine inschakelen 

Schakel diagnostische gegevens over opstarten op een bestaande virtuele machine door de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en selecteer vervolgens de virtuele machine.
2. In **ondersteuning en probleemoplossing**, selecteer **diagnostische gegevens over opstarten** > **instellingen**, wijzigt de status van de **op**, en vervolgens Selecteer een opslagaccount. 
4. Zorg ervoor dat de optie voor de diagnostische gegevens over opstarten is geselecteerd en sla de wijziging.

    ![Bestaande VM bijwerken](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

3. Start de VM opnieuw op om de wijzigingen door te voeren.


