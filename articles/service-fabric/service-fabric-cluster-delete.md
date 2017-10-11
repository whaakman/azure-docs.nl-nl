---
title: Verwijderen van een Azure-cluster en de bijbehorende bronnen | Microsoft Docs
description: Informatie over hoe u een Service Fabric cluster volledig verwijderen van de resourcegroep met het cluster verwijderen of door de bronnen selectief te verwijderen.
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: de422950-2d22-4ddb-ac47-dd663a946a7e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/24/2017
ms.author: chackdan
ms.openlocfilehash: 7672aa12421fbe4ad86e7315d6a7a06c2ff5124d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Verwijderen van een Service Fabric-cluster op Azure en de resources die wordt gebruikt
Een Service Fabric-cluster is opgebouwd uit veel andere Azure-resources naast de clusterbron zelf. Als u een Service Fabric-cluster volledig wilt verwijderen, moet u dus ook alle resources waar het cluster uit bestaat, verwijderen.
Hebt u twee opties: ofwel de resourcegroep die het cluster verwijderen (die wist de clusterbron en alle andere resources in de resourcegroep) of de clusterbron specifiek verwijderen en de bijbehorende resources (maar geen andere resources in de resourcegroep).

> [!NOTE]
> Verwijderen van de clusterbron **heeft geen** verwijderen van alle andere resources die uw Service Fabric-cluster is samengesteld uit.
> 
> 

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Verwijderen van de hele resourcegroep (RG) die de Service Fabric-cluster in gebruik is
Dit is de eenvoudigste manier om ervoor te zorgen dat u alle resources die zijn gekoppeld aan het cluster, met inbegrip van de resourcegroep verwijderen. U kunt de resourcegroep met behulp van PowerShell verwijderen of via de Azure-portal. Als uw resourcegroep resources die niet zijn gerelateerd aan de Service fabric-cluster bevat, kunt u specifieke bronnen verwijderen.

### <a name="delete-the-resource-group-using-azure-powershell"></a>De resourcegroep met Azure PowerShell verwijderen
U kunt ook de resourcegroep verwijderen door het uitvoeren van de volgende Azure PowerShell-cmdlets. Zorg ervoor dat Azure PowerShell 1.0 of hoger is geïnstalleerd op uw computer. Als u dit nog niet hebt gedaan, volgt u de stappen in [installeren en configureren van Azure PowerShell.](/powershell/azure/overview)

Open een PowerShell-venster en voer de volgende PS-cmdlets:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Wordt u gevraagd om bevestiging als u niet gebruikt de *-Force* optie. Op de bevestiging de RG en alle resources daarin verwijderd.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Verwijderen van een resourcegroep in de Azure portal
1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
2. Ga naar het Service Fabric-cluster dat u wilt verwijderen.
3. Klik op de naam van de resourcegroep op de pagina van de essentials cluster.
4. Hiermee wordt de **Resource groep Essentials** pagina.
5. Klik op **Verwijderen**.
6. Volg de instructies op deze pagina om het verwijderen van de resourcegroep te voltooien.

![Verwijderen van resourcegroep][ResourceGroupDelete]

## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>De clusterbron en de resources die wordt gebruikt, maar geen andere resources in de resourcegroep verwijderen
Als uw resourcegroep alleen bronnen die gerelateerd zijn aan de Service Fabric-cluster dat u wilt verwijderen heeft, is het eenvoudiger om de gehele resourcedatabase-groep te verwijderen. Als u de resources één voor één in uw resourcegroep selectief te verwijderen wilt, voert u deze stappen.

Als u uw cluster met behulp van de portal of via een van de Service Fabric-Resource Manager-sjablonen uit de sjablonengalerie hebt geïmplementeerd, zijn de resources die het cluster gebruikt gelabeld met de volgende twee codes. U kunt deze gebruiken om te bepalen welke bronnen die u wilt verwijderen.

***Label #1:*** sleutel = clusternaam, waarde = 'naam van het cluster'

***Label #2:*** sleutel = resourceName, waarde = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Verwijderen van specifieke bronnen in de Azure portal
1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
2. Ga naar het Service Fabric-cluster dat u wilt verwijderen.
3. Ga naar **alle instellingen** op de blade essentials.
4. Klik op **labels** onder **bronbeheer** op de instellingenblade.
5. Klik op een van de **labels** in de blade labels voor een lijst van alle resources met dit label.
   
    ![Resourcelabels][ResourceTags]
6. Zodra u de lijst met gelabelde resources hebt, klikt u op elk van de resources en verwijder deze.
   
    ![Resources met tags][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Verwijderen van de resources met Azure PowerShell
U kunt de resources één voor één verwijderen door het uitvoeren van de volgende Azure PowerShell-cmdlets. Zorg ervoor dat Azure PowerShell 1.0 of hoger is geïnstalleerd op uw computer. Als u dit nog niet hebt gedaan, volgt u de stappen in [installeren en configureren van Azure PowerShell.](/powershell/azure/overview)

Open een PowerShell-venster en voer de volgende PS-cmdlets:

```powershell
Login-AzureRmAccount
```
Voor elk van de resources wilt u verwijderen, voert u de volgende:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Als u wilt verwijderen van de cluster-bron, voert u de volgende:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Volgende stappen
Lees het volgende wanneer u ook meer informatie over het upgraden van een cluster en partitionering services:

* [Meer informatie over de cluster-upgrades](service-fabric-cluster-upgrade.md)
* [Meer informatie over partitioneren stateful services voor maximale schaal](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
