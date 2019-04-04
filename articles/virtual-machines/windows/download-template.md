---
title: Download de sjabloon voor een Azure-VM | Microsoft Docs
description: Download de templatefor een virtuele machine om te helpen bij het automatiseren van implementaties in het Resource Manager-implementatiemodel
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: 4ef0c09d2d2ea2ed06708342ab45abcaf149c23e
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58903100"
---
# <a name="download-the-template-for-a-vm"></a>De sjabloon voor een VM downloaden
Wanneer u een virtuele machine in Azure met behulp van de portal of PowerShell maakt, wordt een Resource Manager-sjabloon automatisch voor u gemaakt. U kunt deze sjabloon gebruiken voor het snel dupliceren van een implementatie. De sjabloon bevat informatie over alle resources in een resourcegroep. Voor een virtuele machine, betekent dit dat de sjabloon bevat alles wat u ter ondersteuning van de virtuele machine in die resourcegroep, met inbegrip van de netwerkbronnen wordt gemaakt.

## <a name="download-the-template-using-the-portal"></a>Download de sjabloon met behulp van de portal
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Een menu aan de linkerkant selecteren **virtuele Machines**.
3. Selecteer de virtuele machine in de lijst.
4. Selecteer **sjabloon exporteren**.
5. Selecteer **downloaden** in het menu aan de bovenkant en sla het ZIP-bestand naar uw lokale computer.
6. Open het ZIP-bestand en pak de bestanden naar een map. Het ZIP-bestand bevat:
   
   * Deploy.ps1
   * Deploy.sh 
   * deployer.RB
   * DeploymentHelper.cs
   * parameters.json
   * template.json

Het template.json-bestand is de sjabloon.

## <a name="download-the-template-using-powershell"></a>Download de sjabloon met behulp van PowerShell
U kunt ook downloaden .json sjabloon bestand met de [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup) cmdlet. U kunt de `-path` parameter op te geven van de bestandsnaam en het pad voor het .json-bestand. Dit voorbeeld laat zien hoe u kunt downloaden van de sjabloon voor de resourcegroep met de naam **myResourceGroup** naar de **C:\users\public\downloads** map op uw lokale computer.

```powershell
    Export-AzResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het implementeren van resources met behulp van sjablonen, [walkthrough voor Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-template-walkthrough.md).

