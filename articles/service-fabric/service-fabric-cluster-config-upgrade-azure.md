---
title: De configuratie van een Azure Service Fabric-cluster upgraden | Microsoft Docs
description: Informatie over het bijwerken van de configuratie die een Service Fabric-cluster in Azure wordt uitgevoerd met behulp van Resource Manager-sjabloon.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 6ddd246057b178119d520bee3bbe58117fbae610
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820974"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Upgrade van de configuratie van een cluster in Azure 

In dit artikel wordt beschreven hoe u de verschillende fabric-instellingen aanpassen voor uw Service Fabric-cluster. Voor clusters die worden gehost in Azure, kunt u instellingen via de [Azure-portal](https://portal.azure.com) of met behulp van een Azure Resource Manager-sjabloon.

> [!NOTE]
> Niet alle instellingen zijn beschikbaar in de portal en is een [best practice om aan te passen met behulp van een Azure Resource Manager-sjabloon](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code); Er is een portal voor Service Fabric Dev\Test alleen voor het scenario van.
> 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Instellingen van het cluster met behulp van Resource Manager-sjablonen aanpassen
Azure-clusters kunnen worden geconfigureerd via de JSON-Resource Manager-sjabloon. Zie voor meer informatie over de verschillende instellingen, [configuratie-instellingen voor clusters](service-fabric-cluster-fabric-settings.md). Bijvoorbeeld de volgende stappen laten zien hoe u een nieuwe instelling toevoegen *MaxDiskQuotaInMB* naar de *Diagnostics* sectie met behulp van Azure Resource Explorer.

1. Ga naar https://resources.azure.com
2. Navigeer naar uw abonnement door uit te vouwen **abonnementen** -> **\<uw abonnement >** -> **resourceGroups**  ->   **\<Uw resourcegroep >** -> **providers** -> **Microsoft.ServiceFabric**  ->  **clusters** -> **\<de naam van uw Cluster >**
3. Selecteer in de rechterbovenhoek, **lezen/schrijven.**
4. Selecteer **bewerken** en werk de `fabricSettings` JSON-element en nieuw-element toe te voegen:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

U kunt ook de instellingen aanpassen in een van de volgende manieren met Azure Resource Manager:

- Gebruik de [Azure-portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template) om te exporteren en het bijwerken van de Resource Manager-sjabloon.
- Gebruik [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell) om te exporteren en het bijwerken van de Resource Manager-sjabloon.
- Gebruik de [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli) om te exporteren en het bijwerken van de Resource Manager-sjabloon.
- Gebruik van Azure DB PowerShell [Set AzureRmServiceFabricSetting](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/Set-AzureRmServiceFabricSetting) en [Remove-AzureRmServiceFabricSetting](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/Remove-AzureRmServiceFabricSetting) opdrachten om de instelling te wijzigen rechtstreeks.
- De Azure CLI gebruiken [az sf cluster instelling](https://docs.microsoft.com/cli/azure/sf/cluster/setting) opdrachten om de instelling te wijzigen rechtstreeks.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [instellingen voor Service Fabric-cluster](service-fabric-cluster-fabric-settings.md).
* Meer informatie over het [in en uit het cluster wordt geschaald](service-fabric-cluster-scale-up-down.md).
