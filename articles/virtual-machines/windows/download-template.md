---
title: Download de sjabloon voor een virtuele machine in Azure | Microsoft Docs
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
ms.openlocfilehash: 93ed84cb146119c877c3a143c5f7af9ca8ba0656
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
ms.locfileid: "26055786"
---
# <a name="download-the-template-for-a-vm"></a>De sjabloon voor een VM downloaden
Wanneer u een virtuele machine in Azure met behulp van de portal of PowerShell maakt, wordt een Resource Manager-sjabloon automatisch voor u gemaakt. Deze sjabloon kunt u snel dupliceren van een implementatie. De sjabloon bevat informatie over alle resources in een resourcegroep. Voor een virtuele machine, betekent dit dat de sjabloon bevat alles wat u ter ondersteuning van de virtuele machine in die resourcegroep, met inbegrip van de netwerkbronnen wordt gemaakt.

## <a name="download-the-template-using-the-portal"></a>Download de sjabloon die via de portal
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Een het menu links Selecteer **virtuele Machines**.
3. Selecteer de virtuele machine in de lijst.
4. Selecteer **automatiseringsscript**.
5. Selecteer **downloaden** in het menu aan de bovenkant en sla het ZIP-bestand op uw lokale computer.
6. Open het ZIP-bestand en pak de bestanden naar een map. Het ZIP-bestand bevat:
   
   * Deploy.ps1
   * Deploy.sh 
   * deployer.RB
   * DeploymentHelper.cs
   * parameters.JSON
   * Template.JSON

Het bestand template.json is de sjabloon.

## <a name="download-the-template-using-powershell"></a>Download de sjabloon die met behulp van PowerShell
U kunt ook downloaden .json sjabloon bestand met de [Export-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet. U kunt de `-path` -parameter voor de bestandsnaam en het pad opgeven voor de .json-bestand. Dit voorbeeld ziet u het downloaden van de sjabloon voor de resourcegroep met de naam **myResourceGroup** naar de **C:\users\public\downloads** map op uw lokale computer.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het implementeren van resources met behulp van sjablonen, [overzicht voor Resource Manager-sjabloon](../../azure-resource-manager/resource-manager-template-walkthrough.md).

