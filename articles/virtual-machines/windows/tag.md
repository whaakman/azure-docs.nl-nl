---
title: Informatie over het taggen van een Windows VM-resource in Azure | Microsoft Docs
description: Meer informatie over het taggen van een Windows-machine hebt gemaakt in Azure met behulp van de Resource Manager-implementatiemodel
services: virtual-machines-windows
documentationcenter: ''
author: mmccrory
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 56d17f45-e4a7-4d84-8022-b40334ae49d2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror
ms.openlocfilehash: 75a6466578808cb5c0dd8d2e32d9445a6e5a5bf8
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140533"
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Informatie over het taggen van een Windows-machine in Azure
Dit artikel beschrijft de verschillende manieren voor het taggen van een Windows-machine in Azure via het Resource Manager-implementatiemodel. Tags zijn de gebruiker gedefinieerde sleutel/waarde-paren die rechtstreeks op een resource of resourcegroep kunnen worden geplaatst. Azure biedt momenteel ondersteuning voor maximaal 15 tags per resource en resourcegroep. Labels kunnen worden geplaatst op een resource op het moment dat wordt gemaakt of toegevoegd aan een bestaande resource. Houd er rekening mee dat de labels voor resources die zijn gemaakt via het Resource Manager-implementatiemodel alleen worden ondersteund. Als u wilt voor het taggen van een virtuele Linux-machine, Zie [informatie over het taggen van een virtuele Linux-machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Labelen met PowerShell
Als u wilt maken, toevoegen en verwijderen van tags via PowerShell, moet u eerst het instellen van uw [PowerShell-omgeving met Azure Resource Manager][PowerShell environment with Azure Resource Manager]. Nadat u de installatie hebt voltooid, kunt u labels kunt plaatsen op Reken-, netwerk- en -resources bij het maken of nadat de resource is gemaakt via PowerShell. In dit artikel zal zich concentreren op weergeven/bewerken tags geplaatst op virtuele Machines.

Navigeer naar een virtuele Machine via de `Get-AzureRmVM` cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Als uw virtuele Machine al labels bevat, klikt u vervolgens ziet u alle tags voor uw resource:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Als u toevoegen van tags via PowerShell wilt, kunt u de `Set-AzureRmResource` opdracht. Houd er rekening mee bij het bijwerken van tags via PowerShell, tags worden bijgewerkt als geheel. Dus als u een tag wilt aan een resource die al tags toevoegen heeft, moet u om op te nemen van de labels die u wilt worden geplaatst op de resource. Hieronder volgt een voorbeeld van het toevoegen van extra labels aan een resource via PowerShell-Cmdlets.

Deze eerste cmdlet worden alle van de labels op geplaatst *MyTestVM* naar de *$tags* variabele, met behulp van de `Get-AzureRmResource` en `Tags` eigenschap.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

De tweede opdracht geeft u de tags voor de opgegeven variabele.

```
    PS C:\> $tags
    
    Key           Value
    ----          -----
    Department    MyDepartment
    Application   MyApp1
    Created By    MyName
    Environment   Production
```

De derde opdracht voegt een extra code aan de *$tags* variabele. Let op het gebruik van de **+=** toe te voegen van de nieuwe sleutel/waarde-paar aan de *$tags* lijst.

        PS C:\> $tags += @{Location="MyLocation"}

De vierde opdracht stelt u alle labels gedefinieerd in de *$tags* variabele aan de opgegeven resource. In dit geval is het MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

De vijfde opdracht geeft alle labels op de resource. Zoals u ziet, *locatie* is nu gedefinieerd als een label met *MyLocation* als de waarde.

```
    PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

    Key           Value
    ----          -----
    Department    MyDepartment
    Application   MyApp1
    Created By    MyName
    Environment   Production
    Location      MyLocation
```

Bekijk voor meer informatie over het taggen via PowerShell, de [Azure Resource-Cmdlets][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het taggen van uw Azure-resources, [overzicht van Azure Resource Manager] [ Azure Resource Manager Overview] en [Tags gebruiken om uw Azure-Resources te organiseren] [ Using Tags to organize your Azure Resources].
* Als u wilt zien hoe labels kunnen u helpen uw gebruik van Azure-resources beheren, Zie [informatie over uw Azure-factuur] [ Understanding your Azure Bill] en [inzicht in het gebruik van de Microsoft Azure-resources] [Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.resources/
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
