---
title: Het Windows-VM-extensies oplossen | Microsoft Docs
description: Meer informatie over het oplossen van Azure Windows VM-extensies oplossen
services: virtual-machines-windows
documentationcenter: ''
author: kundanap
manager: gwallace
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
ms.openlocfilehash: f2b85e9a156d0e6264ec39282b803118963cbbbb
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706649"
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Oplossen van problemen met Azure Windows VM-extensies oplossen
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>De Extensiestatus van de weergeven
Azure Resource Manager-sjablonen kunnen worden uitgevoerd van Azure PowerShell. Nadat de sjabloon wordt uitgevoerd, kan de status van de extensie van Azure Resource Explorer of de opdrachtregelprogramma's worden weergegeven.

Hier volgt een voorbeeld:

Azure PowerShell:

      Get-AzVM -ResourceGroupName $RGName -Name $vmName -Status

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

## <a name="troubleshooting-extension-failures"></a>Oplossen van problemen met extensies oplossen
### <a name="rerun-the-extension-on-the-vm"></a>De extensie op de virtuele machine opnieuw uitvoeren
Als u scripts op de virtuele machine met behulp van de extensie voor aangepaste scripts, kan u soms uitvoeren in een fout waarbij virtuele machine is gemaakt, maar het script is mislukt. In deze omstandigheden vindt is de aanbevolen manier om te herstellen van deze fout voor het verwijderen van de extensie en voert u de sjabloon opnieuw uit.
Opmerking: In de toekomst zou deze functionaliteit wordt verbeterd, zodat de noodzaak voor het verwijderen van de extensie.

#### <a name="remove-the-extension-from-azure-powershell"></a>Verwijder de extensie van Azure PowerShell
    Remove-AzVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Nadat de extensie is verwijderd, kan de sjabloon opnieuw uitgevoerd om uit te voeren van de scripts op de virtuele machine zijn.

