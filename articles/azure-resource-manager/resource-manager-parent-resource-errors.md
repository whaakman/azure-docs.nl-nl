---
title: Azure bovenliggende resourcefouten | Microsoft Docs
description: Beschrijft hoe u fouten oplossen bij het werken met een bovenliggende resource.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: e59147c4ac18f730f27b9d4aa9c008f219881065
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-parent-resources"></a>Los de fouten voor bovenliggende bronnen

In dit artikel beschrijft de fouten die optreden kunnen bij het implementeren van een resource die afhankelijk is van een bovenliggende resource.

## <a name="symptom"></a>Symptoom

Bij het implementeren van een resource die een onderliggende naar een andere bron, wordt de volgende fout:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

## <a name="cause"></a>Oorzaak

Wanneer een bron een onderliggende naar een andere bron is, moet de bovenliggende resource bestaan voordat u de onderliggende resource maakt. De naam van de onderliggende resource bevat de naam van de bovenliggende. Bijvoorbeeld, kunnen een SQL-Database worden gedefinieerd als:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Maar als u een afhankelijkheid op de server niet opgeeft, de implementatie van de database mogelijk starten voordat de server is geïmplementeerd.

## <a name="solution"></a>Oplossing

U lost deze fout, bevatten een afhankelijkheid.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

Zie voor meer informatie [de volgorde voor het implementeren van resources in Azure Resource Manager-sjablonen definiëren](resource-group-define-dependencies.md).