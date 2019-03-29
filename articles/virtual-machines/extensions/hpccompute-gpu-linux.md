---
title: NVIDIA GPU-stuurprogramma-extensie - Azure Linux VM's | Microsoft Docs
description: Microsoft Azure-extensie voor het installeren van NVIDIA GPU-stuurprogramma's op uit de N-serie compute-VM's waarop Linux wordt uitgevoerd.
services: virtual-machines-linux
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/11/2019
ms.author: roiyz
ms.openlocfilehash: 9d9f634d494c3c88146ab1f243d17609cf30bbcd
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620679"
---
# <a name="nvidia-gpu-driver-extension-for-linux"></a>NVIDIA GPU-stuurprogramma-extensie voor Linux

## <a name="overview"></a>Overzicht

Deze extensie installeert NVIDIA GPU-stuurprogramma's op Linux-N-serie VM's. De uitbreiding is afhankelijk van de VM-reeks CUDA- of GRID stuurprogramma's geïnstalleerd. Tijdens de installatie van NVIDIA stuurprogramma's met behulp van deze extensie u accepteren en als u akkoord gaat met de voorwaarden van de [NVIDIA gebruiksrechtovereenkomst](https://go.microsoft.com/fwlink/?linkid=874330). Tijdens de installatie, kan de virtuele machine opnieuw opstarten om de stuurprogramma-installatie te voltooien.

Een uitbreiding is ook beschikbaar voor installatie van NVIDIA GPU-stuurprogramma's op [Windows uit de N-serie VM's](hpccompute-gpu-windows.md).

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

Deze uitbreiding ondersteunt de volgende OS-distributies, afhankelijk van ondersteuning voor stuurprogramma's voor een specifieke versie van het besturingssysteem.

| Distributie | Versie |
|---|---|
| Linux: Ubuntu | 16.04 LTS, 18.04 LTS |
| Linux: Red Hat Enterprise Linux | 7.3, 7.4, 7.5, 7.6 |
| Linux: CentOS | 7.3, 7.4, 7.5, 7.6 |

### <a name="internet-connectivity"></a>Internetconnectiviteit

De Microsoft Azure-extensie voor NVIDIA GPU-stuurprogramma's is vereist dat de doel-VM is verbonden met internet en toegang hebben.

## <a name="extension-schema"></a>Extensieschema

De volgende JSON ziet u het schema voor de extensie.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>Properties

| Name | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| Uitgever | Microsoft.HpcCompute | string |
| type | NvidiaGpuDriverLinux | string |
| typeHandlerVersion | 1.2 | int |

### <a name="settings"></a>Instellingen

Alle instellingen zijn optioneel. Het standaardgedrag is het niet bijwerken de kernel als dit niet vereist voor het installeren van stuurprogramma's, installeert u de nieuwste ondersteunde stuurprogramma en de CUDA-toolkit (indien van toepassing).

| Name | Description | Standaardwaarde | Geldige waarden | Gegevenstype |
| ---- | ---- | ---- | ---- | ---- |
| updateOS | Bijwerken van de kernel, zelfs als niet vereist voor de installatie van stuurprogramma | false | waar of ONWAAR | booleaans |
| driverVersion | NV: De versie van de GRID-stuurprogramma<br> NC/ND: CUDA toolkit versie. De meest recente stuurprogramma's voor de gekozen CUDA worden automatisch geïnstalleerd. | meest recente | GRID: "410.92", "410.71", "390.75", "390.57", "390.42"<br> CUDA: "10.0.130", "9.2.88", "9.1.85" | string |
| installCUDA | CUDA-toolkit installeren. Alleen relevant voor NC/ND-serie VM's. | true | waar of ONWAAR | booleaans |


## <a name="deployment"></a>Implementatie


### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon 

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen. Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor de post-implementatieconfiguratie.

De JSON-configuratie voor een VM-extensie worden genest in de bron van de virtuele machine of geplaatst op de hoofdmap of het hoogste niveau van een Resource Manager JSON-sjabloon. De plaatsing van de JSON-configuratie is van invloed op de waarde van de resourcenaam en het type. Zie voor meer informatie, [naam en type voor de onderliggende resources instellen](../../azure-resource-manager/resource-manager-template-child-resource.md). 

Het volgende voorbeeld wordt ervan uitgegaan dat de extensie is genest in de bron van de virtuele machine. Wanneer het nesten van de extensie-resource, de JSON wordt geplaatst in de `"resources": []` object van de virtuele machine.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.2",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverLinux" `
    -ExtensionType "NvidiaGpuDriverLinux" `
    -TypeHandlerVersion 1.2 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure-CLI

Het volgende voorbeeld komt overeen met de bovenstaande voorbeelden van Azure Resource Manager en PowerShell en aangepaste instellingen ook toegevoegd als voorbeeld voor de installatie van niet-standaard-stuurprogramma. Met name de OS-kernel-updates en een specifieke CUDA toolkit versie stuurprogramma wordt geïnstalleerd.

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverLinux `
  --publisher Microsoft.HpcCompute `
  --version 1.2 `
  --settings '{ `
    "updateOS": true, `
    "driverVersion": "9.1.85", `
  }'
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van Azure PowerShell en Azure CLI. Als u wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, moet u de volgende opdracht uitvoeren.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Extensie uitvoering uitvoer wordt vastgelegd in het volgende bestand:

```bash
/var/log/azure/nvidia-vmext-status
```

### <a name="exit-codes"></a>Afsluitcodes

| Afsluitcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 0 | Bewerking is geslaagd |
| 1 | Onjuist gebruik van de uitbreiding | Uitvoeringslogboek uitvoer controleren |
| 10 | Linux Integration Services voor Hyper-V en Azure niet beschikbaar of is geïnstalleerd | Uitvoer van de controle van lspci |
| 11 | NVIDIA GPU niet gevonden op deze VM-grootte | Gebruik een [ondersteunde VM-grootte en besturingssysteem](../linux/n-series-driver-setup.md) |
| 12 | Installatiekopie-aanbieding niet ondersteund |
| 13 | VM-grootte niet ondersteund | Een VM uit de N-serie gebruiken om te implementeren |
| 14 | De bewerking is mislukt | Uitvoeringslogboek uitvoer controleren |


### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/community/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over extensies [extensies en functies voor Linux virtuele machines](features-linux.md).

Zie voor meer informatie over virtuele machines uit de N-serie, [GPU VM-grootten geoptimaliseerd](../linux/sizes-gpu.md).
