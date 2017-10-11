---
title: Het label van een virtuele machine van Windows-resource in Azure | Microsoft Docs
description: Meer informatie over virtuele Windows-machine gemaakt in Azure met het implementatiemodel van Resource Manager-tagging
services: virtual-machines-windows
documentationcenter: 
author: mmccrory
manager: timlt
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
ms.openlocfilehash: 5f00c4265cea3db02dbb09a7f81be636a3fdd3d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Het label van een virtuele Windows-machine in Azure
In dit artikel beschrijft de verschillende manieren voor het taggen van een virtuele Windows-machine in Azure via het Resource Manager-implementatiemodel. Labels zijn de gebruiker gedefinieerde sleutel/waarde-paren die rechtstreeks op een resource of een resourcegroep kunnen worden geplaatst. Azure ondersteunt momenteel maximaal 15 tags per resource en resourcegroep. Labels kunnen worden geplaatst op een bron op het moment van maken of toegevoegd aan een bestaande resource. Houd er rekening mee dat de labels voor resources die zijn gemaakt via het Resource Manager-implementatiemodel alleen worden ondersteund. Als u wilt voor het taggen van een virtuele Linux-machine, Zie [hoe het labelen van een virtuele Linux-machine in Azure](../linux/tag.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Labels met PowerShell
Als u wilt maken, toevoegen en verwijderen van de labels via PowerShell, moet u eerst voor het instellen van uw [PowerShell-omgeving met Azure Resource Manager][PowerShell environment with Azure Resource Manager]. Nadat u de installatie hebt voltooid, kunt u labels kunt plaatsen op Compute, netwerk en opslag bronnen bij het maken of nadat de resource is gemaakt via PowerShell. In dit artikel zal zich concentreren op weergeven/bewerken labels geplaatst op virtuele Machines.

Eerst, gaat u naar een virtuele Machine via de `Get-AzureRmVM` cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Als uw virtuele Machine al labels bevat, klikt u vervolgens ziet u de labels van uw resources:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Als u toevoegen labels via PowerShell wilt, kunt u de `Set-AzureRmResource` opdracht. Houd er rekening mee bij het bijwerken van labels via PowerShell, tags worden bijgewerkt als geheel. Dus als u één label aan een resource die al labels toevoegt is, moet u om op te nemen van de labels die u wilt worden geplaatst op de bron. Hieronder volgt een voorbeeld van andere labels toevoegen aan een resource via PowerShell-Cmdlets.

Deze eerste cmdlet stelt alle labels op geplaatst *MyTestVM* naar de *$tags* variabele, met behulp van de `Get-AzureRmResource` en `Tags` eigenschap.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

De tweede opdracht geeft de labels voor de opgegeven variabele.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment

De derde opdracht voegt een extra tag voor de *$tags* variabele. Let op het gebruik van de  **+=**  toe te voegen van de nieuwe sleutel-waardepaar voor de *$tags* lijst.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

De vierde opdracht stelt alle labels gedefinieerd in de *$tags* variabele met de opgegeven bron. In dit geval is het MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

De vijfde opdracht geeft alle labels op de bron. Zoals u ziet, *locatie* is nu gedefinieerd als een label met *MyLocation* als de waarde.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value        MyDepartment
        Name        Department
        Value        MyApp1
        Name        Application
        Value        MyName
        Name        Created By
        Value        Production
        Name        Environment
        Value        MyLocation
        Name        Location

Bekijk voor meer informatie over labels via PowerShell, de [Azure Resource Cmdlets][Azure Resource Cmdlets].

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over uw Azure-resources te labelen, [overzicht van Azure Resource Manager] [ Azure Resource Manager Overview] en [Tags gebruiken om uw Azure-Resources te organiseren][Using Tags to organize your Azure Resources].
* Zie hoe labels kunnen u helpen uw gebruik van Azure-resources beheren [inzicht in uw Azure-factuur] [ Understanding your Azure Bill] en [inzicht in uw Microsoft Azure-brongebruik][Gain insights into your Microsoft Azure resource consumption].

[PowerShell environment with Azure Resource Manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
[Azure Resource Cmdlets]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
