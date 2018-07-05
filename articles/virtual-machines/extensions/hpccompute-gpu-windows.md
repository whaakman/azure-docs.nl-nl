---
title: NVIDIA GPU-stuurprogramma-extensie - Azure Windows VM's | Microsoft Docs
description: Microsoft Azure-extensie voor het installeren van NVIDIA GPU-stuurprogramma's op uit de N-serie compute-VM's waarop Windows wordt uitgevoerd.
services: virtual-machines-windows
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/03/2018
ms.author: danis
ms.openlocfilehash: 16f4dc19f5f1dc065ea303c741deb7d9cffedbe8
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449920"
---
# <a name="nvidia-gpu-driver-extension-for-windows"></a>NVIDIA GPU-stuurprogramma-extensie voor Windows

## <a name="overview"></a>Overzicht

Deze extensie wordt NVIDIA GPU-stuurprogramma's geïnstalleerd op Windows-N-serie VM's. De uitbreiding is afhankelijk van de VM-reeks CUDA- of GRID stuurprogramma's geïnstalleerd. Tijdens de installatie van NVIDIA stuurprogramma's met behulp van deze extensie accepteren en als u akkoord gaat met de voorwaarden van de gebruiksrechtovereenkomst van NVIDIA. Tijdens de installatie, kan uw virtuele machine opnieuw opstarten om de stuurprogramma-installatie te voltooien.

Een uitbreiding is ook beschikbaar voor installatie van NVIDIA GPU-stuurprogramma's op [Linux N-serie VM's](hpccompute-gpu-linux.md).

Voorwaarden van de gebruiksrechtovereenkomst NVIDIA bevinden zich hier: https://go.microsoft.com/fwlink/?linkid=874330

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

Deze uitbreiding ondersteunt de volgende OSs:

| Distributie | Versie |
|---|---|
| Windows 10 | Kern |
| Windows Server 2016 | Kern |
| Windows Server 2012R2 | Kern |

### <a name="internet-connectivity"></a>Internetconnectiviteit

De Microsoft Azure-extensie voor NVIDIA GPU-stuurprogramma's is vereist dat de virtuele doelmachine is verbonden met internet en toegang hebben.

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
    "type": "NvidiaGpuDriverWindows",
    "typeHandlerVersion": "1.0",
    "settings": {
    }
  }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Naam | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | datum |
| Uitgever | Microsoft.HpcCompute | tekenreeks |
| type | NvidiaGpuDriverWindows | tekenreeks |
| typeHandlerVersion | 1.0 | int |


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
    "type": "NvidiaGpuDriverWindows",
    "typeHandlerVersion": "1.0",
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzureRmVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverWindows" `
    -ExtensionType "NvidiaGpuDriverWindows" `
    -TypeHandlerVersion 1.0 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverWindows `
  --publisher Microsoft.HpcCompute `
  --version 1.0 `
  --settings '{ `
  }'
```

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van Azure PowerShell en Azure CLI. Als u wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, moet u de volgende opdracht uitvoeren.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Extensie uitvoering uitvoer wordt vastgelegd in de volgende map:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverMicrosoft\
```

### <a name="error-codes"></a>Foutcodes

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 0 | Bewerking is geslaagd |
| 1 | De bewerking is geslaagd. Opnieuw opstarten vereist. |
| 4, 10 | Time-out van de bewerking. | Voer de bewerking opnieuw uit.
| -1 | Er is een uitzondering opgetreden. | Raadpleeg de logbestanden om de oorzaak van de uitzondering te bepalen. |
| -5 x | De bewerking is onderbroken vanwege opnieuw opstarten. | VM opnieuw. Installatie wordt voortgezet na opnieuw opstarten. Verwijder handmatig moet worden aangeroepen. |


### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/community/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over extensies [extensies voor virtuele machines en functies voor Windows](features-windows.md).

Zie voor meer informatie over virtuele machines uit de N-serie, [GPU VM-grootten geoptimaliseerd](../windows/sizes-gpu.md).