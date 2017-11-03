---
title: Azure beheerde toepassing MultiStorageAccountCombo UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Storage.MultiStorageAccountCombo UI-element voor beheerde Azure-toepassingen
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
ms.openlocfilehash: e1dda7917988146807ca6cfab10a3a4eac0b7bb2
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a>Microsoft.Storage.MultiStorageAccountCombo UI-element
Een groep besturingselementen voor het maken van meerdere opslagaccounts met namen die met een algemeen voorvoegsel beginnen. Gebruik van dit element wanneer [maken van een Azure-toepassing beheerd](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- De waarde voor `defaultValue.prefix` is samengevoegd met een of meer getallen voor het genereren van de volgorde van de namen van opslagaccounts. Bijvoorbeeld, als `defaultValue.prefix` is **foobar** en `count` is **2**, klikt u vervolgens opslagaccountnamen **foobar1** en **foobar2** worden gegenereerd. Gegenereerde opslagaccountnamen worden gevalideerd voor uniekheid automatisch.
- De namen van opslagaccounts zijn gegenereerd lexicographically op basis van `count`. Bijvoorbeeld, als `count` 10 is, wordt de namen van opslagaccounts eindigen met gehele getallen 2 cijfers (01, 02, 03, enz.).
- De standaardwaarde voor `defaultValue.prefix` is **null**, en voor `defaultValue.type` is **Premium_LRS**.
- Een type dat niet is opgegeven in `constraints.allowedTypes` is verborgen, en een type dat niet is opgegeven in `constraints.excludedTypes` wordt weergegeven.
`constraints.allowedTypes`en `constraints.excludedTypes` zijn beide optioneel, maar niet gelijktijdig worden gebruikt.
- Naast het genereren van de namen van opslagaccounts, `count` wordt gebruikt voor het instellen van de juiste vermenigvuldiger voor het element. Een statische waarde, zoals ondersteunt **2**, of een dynamische waarde van een ander element, zoals `[steps('step1').storageAccountCount]`. De standaardwaarde is **1**.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](overview.md).
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
