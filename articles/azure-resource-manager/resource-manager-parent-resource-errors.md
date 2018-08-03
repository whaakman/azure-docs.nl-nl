---
title: Azure bovenliggende resourcefouten | Microsoft Docs
description: Beschrijft hoe u fouten oplossen bij het werken met een bovenliggende resource.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/01/2018
ms.author: tomfitz
ms.openlocfilehash: 3042ea1a523f12ae0311545a1b9bc67306f266dd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447299"
---
# <a name="resolve-errors-for-parent-resources"></a>Los fouten voor bovenliggende resources

Dit artikel beschrijft de fouten krijgt u mogelijk bij het implementeren van een resource die afhankelijk is van een bovenliggende resource.

## <a name="symptom"></a>Symptoom

Bij het implementeren van een resource die een onderliggend item naar een andere resource, verschijnt de volgende fout:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

## <a name="cause"></a>Oorzaak

Wanneer een resource een onderliggend element naar een andere resource is, moet de bovenliggende resource aanwezig zijn voordat het maken van de onderliggende resource. De naam van de onderliggende resource definieert de verbinding met de bovenliggende resource. De naam van de onderliggende resource is in de indeling `<parent-resource-name>/<child-resource-name>`. Een SQL-Database kan bijvoorbeeld worden gedefinieerd als:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Als u zowel de server en de database in dezelfde sjabloon implementeren, maar een afhankelijkheid op de server niet opgeeft, de implementatie van de database kan worden gestart voordat de server is geïmplementeerd. 

Als de bovenliggende resource bestaat al en wordt niet geïmplementeerd in dezelfde sjabloon, krijgt u deze fout bij het Resource Manager kan niet koppelen aan de onderliggende resource bovenliggende. Deze fout kan optreden als de onderliggende bron zich niet in de juiste indeling of de onderliggende resource is geïmplementeerd in een resourcegroep die anders is dan de resourcegroep voor de bovenliggende resource.

## <a name="solution"></a>Oplossing

Los deze fout als de bovenliggende en onderliggende resources worden geïmplementeerd in dezelfde sjabloon, bevatten een afhankelijkheid.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

Los deze fout als de bovenliggende resource al eerder is geïmplementeerd in een andere sjabloon, stelt u niet een afhankelijkheid. In plaats daarvan de onderliggende implementeren in dezelfde resourcegroep bevinden en geef de naam van de bovenliggende resource.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "sqlServerName": {
            "type": "string"
        },
        "databaseName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2014-04-01",
            "type": "Microsoft.Sql/servers/databases",
            "location": "[resourceGroup().location]",
            "name": "[concat(parameters('sqlServerName'), '/', parameters('databaseName'))]",
            "properties": {
                "collation": "SQL_Latin1_General_CP1_CI_AS",
                "edition": "Basic"
            }
        }
    ],
    "outputs": {}
}
```

Zie voor meer informatie, [definiëren de volgorde voor het implementeren van resources in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md).