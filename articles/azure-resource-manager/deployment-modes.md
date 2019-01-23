---
title: Azure Resource Manager-implementatiemodi | Microsoft Docs
description: Beschrijft hoe u om op te geven of u wilt een volledige of incrementele implementatiemodus gebruiken met Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: 105a836f609859825c273ed9fba9dd46237bcaa9
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447935"
---
# <a name="azure-resource-manager-deployment-modes"></a>Azure Resource Manager-implementatiemodi

Bij het implementeren van uw resources, kunt u opgeven dat de implementatie een incrementele update of een volledige update is.  Het belangrijkste verschil tussen deze twee modi is hoe Resource Manager omgaat met bestaande resources in de resourcegroep die zich niet in de sjabloon. De standaardmodus is incrementeel.

## <a name="incremental-and-complete-deployments"></a>Incrementele en volledige implementaties

Bij het implementeren van resources:

* In de volledige modus Resource Manager **verwijdert** resources die aanwezig zijn in de resourcegroep, maar niet zijn opgegeven in de sjabloon. De resources die nog steeds zijn opgegeven, maar worden niet worden geïmplementeerd vanwege een voorwaarde is geëvalueerd als onwaar is, worden niet verwijderd, maar.
* In de Resource Manager-incrementele modus **blijft ongewijzigd** resources die aanwezig zijn in de resourcegroep, maar niet zijn opgegeven in de sjabloon.

Voor beide modi wordt geprobeerd de Resource Manager om alle resources die zijn opgegeven in de sjabloon te maken. Als de resource al in de resourcegroep bestaat en de instellingen niet gewijzigd zijn, wordt de bewerking resulteert in geen wijziging. Als u de waarden van de eigenschappen voor een bron wijzigt, worden de resource wordt bijgewerkt met de nieuwe waarden. Als u probeert om bij te werken van de locatie of het type van een bestaande resource, mislukt de implementatie met een fout. In plaats daarvan implementeert u een nieuwe resource met de locatie of typ dat u nodig hebt.

Wanneer u opnieuw wilt implementeren op een resource in de modus voor incrementele, geeft u alle eigenschapswaarden voor de resource, niet alleen de resources die u bijwerkt. Als u bepaalde eigenschappen niet opgeeft, wordt de update als deze waarden worden overschreven door Resource Manager geïnterpreteerd.

## <a name="example-result"></a>Voorbeeld van resultaat

Ter illustratie van het verschil tussen de modi incrementele en volledige, houd rekening met het volgende scenario.

**Resourcegroep** bevat:

* Resource A
* Resource B
* Resource C

**Sjabloon** bevat:

* Resource A
* Resource B
* Resource D

Wanneer geïmplementeerd in **incrementele** modus, de resourcegroep heeft:

* Resource A
* Resource B
* Resource C
* Resource D

Wanneer geïmplementeerd in **voltooid** Resource C-modus wordt verwijderd. De resourcegroep heeft:

* Resource A
* Resource B
* Resource D

## <a name="set-deployment-mode"></a>Modus instellen voor implementatie

Om in te stellen de implementatiemodus bij het implementeren met PowerShell, gebruikt u de `Mode` parameter.

```azurepowershell-interactive
New-AzureRmResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Om in te stellen de implementatiemodus bij het implementeren met Azure CLI, gebruikt u de `mode` parameter.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Wanneer u een [gekoppelde of geneste sjabloon](resource-group-linked-templates.md), moet u instellen de `mode` eigenschap `Incremental`. Alleen de sjablonen op hoofdniveau ondersteunen de volledige implementatie-modus.

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het maken van Resource Manager-sjablonen, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van resources, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).
* Als u de bewerkingen voor een resourceprovider, Zie [REST API van Azure](/rest/api/).
