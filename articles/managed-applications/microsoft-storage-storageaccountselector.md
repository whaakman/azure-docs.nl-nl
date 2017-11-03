---
title: Azure beheerde toepassing StorageAccountSelector UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Storage.StorageAccountSelector UI-element voor beheerde Azure-toepassingen
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 366a862acc15decf6a8e19f875d5d052695f373c
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Microsoft.Storage.StorageAccountSelector UI-element
Een besturingselement voor het selecteren van een nieuwe of bestaande opslagaccount. Gebruik van dit element wanneer [maken van een Azure-toepassing beheerd](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Indien opgegeven, `defaultValue.name` automatisch wordt gevalideerd voor uniekheid. Als de opslagaccountnaam is niet uniek is, moet de gebruiker Geef een andere naam of kies een bestaand opslagaccount.
- De standaardwaarde voor `defaultValue.type` is **Premium_LRS**.
- Een type dat niet is opgegeven in `constraints.allowedTypes` is verborgen, en een type dat niet is opgegeven in `constraints.excludedTypes` wordt weergegeven.
`constraints.allowedTypes`en `constraints.excludedTypes` zijn beide optioneel, maar niet gelijktijdig worden gebruikt.
- Als `options.hideExisting` is **true**, de gebruiker een bestaand opslagaccount niet kiezen. De standaardwaarde is **false**.


## <a name="sample-output"></a>Voorbeelduitvoer
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](overview.md).
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
