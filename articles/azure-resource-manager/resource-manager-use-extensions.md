---
title: Configuraties na de implementatie opgeven met behulp van extensies - Azure | Microsoft Docs
description: Informatie over het gebruik van sjabloonextensies van Azure Resource Manager-voor de configuraties na de implementatie.
services: azure-resource-manager
documentationcenter: na
author: mumian
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: eb46966c3a28b3fa4c2b23668109b7c5d23a609b
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53414377"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Configuraties na de implementatie opgeven met behulp van extensies

Sjabloon-uitbreidingen zijn kleine toepassingen die post-implementatieconfiguratie en geautomatiseerde taken op Azure-resources bieden. De meest populaire is extensies voor virtuele machines. Zie [extensies voor virtuele machines en functies voor Windows](../virtual-machines/extensions/features-windows.md), en [extensies en functies voor Linux virtuele machines](../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Extensies

De bestaande uitbreidingen zijn:

- [Microsoft.Compute/virtualMachines/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensies](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Een Microsoft.HDInsight-clusters/extensies](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters/extensions)
- [Microsoft.Sql-servers/databases/extensies](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/sites/siteextensions](https://docs.microsoft.com/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Als u wilt weten de beschikbare uitbreidingen, blader naar de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/). In **filteren op titel**, voer **extensie**.

Voor informatie over het gebruik van deze extensies, Zie:

- [Zelfstudie: Extensies voor virtuele machines met Azure Resource Manager-sjablonen implementeren](./resource-manager-tutorial-deploy-vm-extensions.md).
- [Zelfstudie: SQL BACPAC-bestanden met Azure Resource Manager-sjablonen importeren](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Extensies voor virtuele machines met Azure Resource Manager-sjablonen implementeren](./resource-manager-tutorial-deploy-vm-extensions.md)
