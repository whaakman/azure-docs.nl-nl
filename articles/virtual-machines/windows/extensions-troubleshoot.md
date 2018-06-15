---
title: Het Windows-VM-extensie fouten oplossen | Microsoft Docs
description: Meer informatie over het oplossen van fouten voor virtuele machine van Windows Azure-extensie
services: virtual-machines-windows
documentationcenter: ''
author: kundanap
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 5c99f4a9209595fd58021fd8da9cd67f1e034748
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30914029"
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Virtuele machine van Windows Azure-extensie fouten oplossen
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>De Extensiestatus van weergeven
Azure Resource Manager-sjablonen kunnen in Azure PowerShell worden uitgevoerd. Nadat de sjabloon die wordt uitgevoerd, kan de status van de extensie van Azure Resource Explorer of de opdrachtregel-hulpprogramma's worden weergegeven.

Hier volgt een voorbeeld:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Hier volgt een voorbeeld van uitvoer:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Het extensie-fouten oplossen
### <a name="re-running-the-extension-on-the-vm"></a>De uitbreiding opnieuw uit te voeren op de virtuele machine
Als u scripts op de virtuele machine met behulp van de aangepaste Scriptextensie, kan het soms uitvoeren in een fout waarbij VM is gemaakt, maar het script is mislukt. Onder deze voorwaarden is de aanbevolen manier om deze fout herstellen voor het verwijderen van de extensie en voert u de sjabloon opnieuw.
Opmerking: In de toekomst deze functionaliteit zou worden uitgebreid om te verwijderen van de noodzaak voor het verwijderen van de extensie.

#### <a name="remove-the-extension-from-azure-powershell"></a>Verwijder de extensie van Azure PowerShell
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Wanneer de uitbreiding is verwijderd, is de sjabloon kan worden opnieuw uitgevoerd voor het uitvoeren van de scripts op de virtuele machine.

