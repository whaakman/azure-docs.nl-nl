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
ms.date: 10/31/2018
ms.author: delhan
ms.openlocfilehash: 59602977c1b7f6dd0524c6535d8458d3eb1a3f26
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425574"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>Het gebruik van diagnostische gegevens over opstarten voor virtuele machines in Azure oplossen

Er zijn veel oorzaken waardoor dat een virtuele machine wordt de niet-opstartbare status ingeschakeld. Om problemen te verhelpen met uw virtuele machines die zijn gemaakt met behulp van Resource Manager-implementatiemodel kunt u de volgende functies voor foutopsporing: Console-uitvoer en schermafbeelding ondersteuning voor virtuele machines van Azure. 

Voor virtuele Linux-machines, kunt u de uitvoer van uw consolelogboek vanuit de Portal weergeven. Voor zowel Windows als Linux virtuele machines kunt Azure u een schermafbeelding van de virtuele machine uit de hypervisor. Beide functies worden ondersteund voor virtuele Azure-machines in alle regio's. Houd er rekening mee dat het tot 10 minuten kan duren voordat de schermafbeeldingen en uitvoer worden weergegeven in uw opslagaccount.

U kunt selecteren de **diagnostische gegevens over opstarten** optie om het logboek of de schermafbeelding weer te geven.

![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)

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

## <a name="enable-diagnostics-on-a-virtual-machine-created-using-the-azure-portal"></a>Diagnostische gegevens op een virtuele machine gemaakt met behulp van de Azure-Portal inschakelen

De volgende procedure is voor een virtuele machine gemaakt met behulp van de Resource Manager-implementatiemodel.

Op de **Management** tabblad, in **bewaking** sectie, zorg ervoor dat **diagnostische gegevens over opstarten** is ingeschakeld. Uit de **opslagaccount voor diagnostische gegevens** vervolgkeuzelijst, selecteert u een opslagaccount waarin de diagnostische bestanden moeten worden geplaatst.
 
![VM maken](./media/virtual-machines-common-boot-diagnostics/enable-boot-diagnostics-vm.png)

> [!NOTE]
> De functie van de diagnostische gegevens over opstarten biedt geen ondersteuning voor premium storage-account. Als u de premium-opslagaccount voor diagnostische gegevens over opstarten gebruiken, ontvangt u mogelijk de StorageAccountTypeNotSupported-fout bij het starten van de virtuele machine.
>

### <a name="deploying-from-an-azure-resource-manager-template"></a>Implementeren vanuit een Azure Resource Manager-sjabloon

Als u een Azure Resource Manager-sjabloon implementeert, gaat u naar de resource van uw virtuele machine en de sectie diagnostisch profiel toevoegen. Stel de API-versieheader '2015-06-15' of hoger. De meest recente versie is '2018-10-01'.

```json
{
  "apiVersion": "2018-10-01",
  "type": "Microsoft.Compute/virtualMachines",
  … 
```

Met het diagnostische profiel kunt u het opslagaccount selecteren waarin u deze logboeken wilt opslaan.

```json
    "diagnosticsProfile": {
    "bootDiagnostics": {
    "enabled": true,
    "storageUri": "[concat('https://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
    }
    }
    }
}
```

Zie voor meer informatie over het implementeren van resources met behulp van sjablonen [Quick Start: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md).

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Diagnostische gegevens over opstarten op bestaande virtuele machine inschakelen 

Schakel diagnostische gegevens over opstarten op een bestaande virtuele machine door de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en selecteer vervolgens de virtuele machine.
2. In de **ondersteuning en probleemoplossing** sectie, selecteer **diagnostische gegevens over opstarten**en selecteer vervolgens de **instellingen** tabblad.
3. In **diagnostische gegevens over opstarten** instellingen, wijzigt de status van de **op**, en van de **opslagaccount** vervolgkeuzelijst, selecteer een opslagaccount. 
4. Sla de wijziging.

    ![Bestaande VM bijwerken](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

De virtuele machine voor de wijziging door te voeren, moet u opnieuw.

### <a name="enable-boot-diagnostics-using-the-azure-cli"></a>Inschakelen van diagnostische gegevens over opstarten met behulp van de Azure CLI

U kunt de Azure CLI gebruiken om in te schakelen van diagnostische gegevens over opstarten op een bestaande Azure-machine. Zie voor meer informatie, [az vm boot-diagnostics](
https://docs.microsoft.com/cli/azure/vm/boot-diagnostics?view=azure-cli-latest).
